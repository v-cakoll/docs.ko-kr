---
title: ForEach를 사용하여 BlockingCollection의 항목 제거
ms.date: 05/04/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- thread-safe collections, how to enumerate blocking collection
ms.assetid: 2096103c-22f7-420d-b631-f102bc33a6dd
ms.openlocfilehash: 46638d2cd8078fefebc0eacc4b8f7798ffe178ff
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288903"
---
# <a name="use-foreach-to-remove-items-in-a-blockingcollection"></a>ForEach를 사용하여 BlockingCollection의 항목 제거

<xref:System.Collections.Concurrent.BlockingCollection%601.Take%2A> 및 <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> 메서드를 사용하여 <xref:System.Collections.Concurrent.BlockingCollection%601>에서 항목을 가져오는 것 외에도, 추가가 완료되고 컬렉션이 빈 상태가 될 때까지 [ForEach](../../../csharp/language-reference/keywords/foreach-in.md)(Visual Basic의 [For Each](../../../visual-basic/language-reference/statements/for-each-next-statement.md))를 <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType>과(와) 함께 사용하여 항목을 제거할 수 있습니다. 일반적인 `foreach`(`For Each`) 루프와 달리 이 열거자는 항목을 제거하여 소스 컬렉션을 수정하기 때문에 이 방식을 *열거형 변경* 또는 *열거형 소비*라고 합니다.

## <a name="example"></a>예제

다음 예제에서는 `foreach`(`For Each`) 루프를 사용하여 <xref:System.Collections.Concurrent.BlockingCollection%601>의 항목을 모두 제거하는 방법을 보여 줍니다.

[!code-csharp[CDS_BlockingCollection#03](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example03.cs#03)]
[!code-vb[CDS_BlockingCollection#03](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/enumeratebc.vb#03)]

이 예제에서는 소비 스레드의 <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType> 메서드에 `foreach` 루프를 사용하는데, 이 루프는 컬렉션을 열거하면서 해당 컬렉션의 각 항목을 제거합니다. <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType>는 항상 컬렉션에 있는 항목의 최대 수를 제한합니다. 이런 방식으로 컬렉션을 열거하는 경우 제공되는 항목이 없거나 컬렉션이 비었을 때 소비자 스레드를 차단합니다. 이 예제에서는 항목이 소비되는 것보다 더 빠르게 공급자 스레드에서 항목을 추가하기 때문에 차단이 발생하지 않습니다.

<xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType>은(는) `IEnumerable<T>`를 반환하므로 순서를 보장할 수 없습니다. 그러나 내부적으로 <xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType>은(는) 기본 컬렉션 형식으로 사용되며 FIFO(선입선출) 순서를 따라 개체를 큐에서 제거합니다. <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType>에 대한 동시 호출이 이루어지면 경쟁이 발생합니다. 한 열거형에서 사용된(큐에서 제거된) 한 항목을 다른 열거형에서 관찰할 수 없습니다.

컬렉션을 수정하지 않고 열거하려면 <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A> 메서드 없이 `foreach`(`For Each`)만 사용하면 됩니다. 그러나 이와 같이 열거한 결과는 특정 시점의 컬렉션을 나타내는 스냅샷에 불과하다는 사실을 이해할 필요가 있습니다. 루프를 실행하는 동시에 다른 스레드를 통해 항목을 추가하거나 제거하고 있는 경우에는 루프에서 컬렉션의 실제 상태를 나타내지 않을 수 있습니다.

## <a name="see-also"></a>참조

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [병렬 프로그래밍](../../parallel-programming/index.md)
