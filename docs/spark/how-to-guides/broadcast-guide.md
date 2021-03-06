---
title: Apache Spark용으로 .NET에서 브로드캐스트 변수 사용
description: .NET에서 Apache Spark 애플리케이션용으로 브로드캐스트 변수를 사용하는 방법을 알아봅니다.
ms.date: 06/25/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: d86b160855cc4d3f3a6502f5606d4766b7c06aa0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617858"
---
# <a name="use-broadcast-variables-in-net-for-apache-spark"></a>Apache Spark용으로 .NET에서 브로드캐스트 변수 사용

이 문서에서는 .NET에서 Apache Spark용으로 브로드캐스트 변수를 사용하는 방법을 알아봅니다. [Apache Spark의 브로드캐스트 변수](https://spark.apache.org/docs/2.2.0/rdd-programming-guide.html#broadcast-variables)는 읽기 전용으로 사용할 변수를 실행기에서 공유하는 메커니즘입니다. 브로드캐스트 변수를 사용하면 작업으로 복사본을 전달하는 대신 각 머신에 캐시된 읽기 전용 변수를 유지할 수 있습니다. 브로드캐스트 변수를 사용하여 효율적인 방식으로 모든 노드에 대규모 입력 데이터 세트의 사본을 제공할 수 있습니다.

데이터는 한 번만 전송되기 때문에, 브로드캐스트 변수는 각 작업에서 실행기에 전달되는 지역 변수와 비교할 때 성능상의 이점이 있습니다. 브로드캐스트 변수 및 이 변수를 사용하는 이유를 더 깊이 있게 이해하려면 [공식 브로드캐스트 변수 설명서](https://spark.apache.org/docs/2.2.0/rdd-programming-guide.html#broadcast-variables)를 참조하세요.

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="create-broadcast-variables"></a>브로드캐스트 변수 만들기

브로드캐스트 변수를 만들려면 변수 `v`에 대해 `SparkContext.Broadcast(v)`를 호출합니다. 브로드캐스트 변수는 변수 `v` 주위의 래퍼이며, `Value()` 메서드를 호출하여 이 변수의 값에 액세스할 수 있습니다.

다음 코드 조각에서는 문자열 변수 `v`를 만듭니다. `SparkContext.Broadcast(v)`를 호출하면 브로드캐스트 변수 `bv`를 만듭니다. 문자열 `Broadcast`에 대한 형식 매개 변수는 브로드캐스트 중인 변수의 형식과 일치합니다. UDF(사용자 정의 함수)는 `bv`의 값을 반환합니다.

```csharp
string v = "Variable to be broadcasted";
Broadcast<string> bv = SparkContext.Broadcast(v);

Func<Column, Column> udf = Udf<string, string>(
    str => $"{str}: {bv.Value()}");
```

## <a name="delete-broadcast-variables"></a>브로드캐스트 변수 삭제

`Destroy()` 메서드를 호출하여 모든 실행기에서 브로드캐스트 변수를 삭제할 수 있습니다.

```csharp
bv.Destroy();
```

`Destroy()`는 브로드캐스트 변수와 관련된 모든 데이터 및 메타데이터를 삭제하므로 주의해서 사용해야 합니다. 브로드캐스트 변수가 제거되면 이 변수를 다시 사용할 수 없습니다.

## <a name="limit-broadcast-variable-scope-in-udfs"></a>UDF에서 브로드캐스트 변수 범위 제한

UDF에 브로드캐스트 변수를 사용할 때는 변수의 범위를 변수가 참조하는 UDF로만 제한해야 합니다. [UDF 사용 가이드](udf-guide.md)에서는 이러한 상황을 자세히 설명합니다. 범위는 브로드캐스트 변수에 대해 `Destroy()`를 호출할 때 특히 중요합니다.

제거된 브로드캐스트 변수가 다른 UDF에 표시되거나 다른 UDF에서 액세스 가능한 경우 이 변수를 참조하지 않는 UDF를 포함해 모든 UDF에서 serialization을 위해 이 변수를 선택합니다. .NET for Apache Spark는 제거된 브로드캐스트 변수를 직렬화할 수 없으며 이에 따른 오류가 발생합니다. 다음 코드 조각은 오류를 보여 줍니다.

```csharp
string v = "Variable to be broadcasted";
Broadcast<string> bv = SparkContext.Broadcast(v);

// Using the broadcast variable in a UDF:
Func<Column, Column> udf1 = Udf<string, string>(
    str => $"{str}: {bv.Value()}");

// Destroying bv
bv.Destroy();

// Calling udf1 after destroying bv throws the following expected exception:
// org.apache.spark.SparkException: Attempted to use Broadcast(0) after it was destroyed
df.Select(udf1(df["_1"])).Show();

// Different UDF udf2 that is not referencing bv
Func<Column, Column> udf2 = Udf<string, string>(
    str => $"{str}: not referencing broadcast variable");

// Calling udf2 throws the following (unexpected) exception:
// [Error] [JvmBridge] org.apache.spark.SparkException: Task not serializable
df.Select(udf2(df["_1"])).Show();
```

다음 코드 조각은 `bv`를 제거해도 예기치 않은 serialization 동작으로 인해 `udf2`에 영향을 주지 않도록 하는 방법을 보여 줍니다.

```csharp
string v = "Variable to be broadcasted";
// Restricting the visibility of bv to only the UDF referencing it
{
    Broadcast<string> bv = SparkContext.Broadcast(v);

    // Using the broadcast variable in a UDF:
    Func<Column, Column> udf1 = Udf<string, string>(
        str => $"{str}: {bv.Value()}");

    // Destroying bv
    bv.Destroy();
}

// Different UDF udf2 that is not referencing bv
Func<Column, Column> udf2 = Udf<string, string>(
    str => $"{str}: not referencing broadcast variable");

// Calling udf2 works fine as expected
df.Select(udf2(df["_1"])).Show();
```

## <a name="next-steps"></a>다음 단계

* [.NET for Apache Spark 시작](../tutorials/get-started.md)
* [Windows에서 .NET for Apache Spark 애플리케이션 디버그](debug.md)
* [.NET for Apache Spark 작업자 및 사용자 정의 함수 이진 파일 배포](deploy-worker-udf-binaries.md)
