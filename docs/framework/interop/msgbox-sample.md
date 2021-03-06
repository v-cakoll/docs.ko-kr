---
title: MsgBox 샘플
description: MsgBox를 사용하여 In 매개 변수의 값으로 문자열 형식을 전달하는 샘플을 참조하세요. .NET에서 EntryPoint, CharSet 및 ExactSpelling 필드를 사용하는 경우를 보여 줍니다.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- marshaling, MsgBox sample
- data marshaling, MsgBox sample
ms.assetid: 9e0edff6-cc0d-4d5c-a445-aecf283d9c3a
ms.openlocfilehash: ccf882e1f801dd18e5b65a4279fc580d927dd29d
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904093"
---
# <a name="msgbox-sample"></a>MsgBox 샘플
이 샘플에서는 In 매개 변수로 값 형식 문자열 형식을 전달하는 방법과 <xref:System.Runtime.InteropServices.DllImportAttribute.EntryPoint>, <xref:System.Runtime.InteropServices.DllImportAttribute.CharSet> 및 <xref:System.Runtime.InteropServices.DllImportAttribute.ExactSpelling> 필드 사용 시기를 보여 줍니다.  
  
 MsgBox 샘플에서는 원래 함수 선언과 함께 표시되는 다음과 같은 관리되지 않는 함수를 사용합니다.  
  
- User32.dll에서 내보낸 **MessageBox**.  
  
    ```cpp
    int MessageBox(HWND hWnd, LPCTSTR lpText, LPCTSTR lpCaption,
       UINT uType);  
    ```  
  
 이 샘플에서 `NativeMethods` 클래스에는 `MsgBoxSample` 클래스에서 호출하는 관리되지 않는 각 함수의 관리되는 프로토타입이 포함되어 있습니다. 관리되는 프로토타입 메서드 `MsgBox`, `MsgBox2` 및 `MsgBox3`에는 관리되지 않는 동일한 함수에 대한 다른 선언이 있습니다.  
  
 ANSI로 지정된 문자 유형은 Unicode 함수의 이름인 진입점 `MessageBoxW`와 일치하지 않으므로 `MsgBox2`를 선언하면 메시지 상자에 잘못된 출력이 생성됩니다. `MsgBox3`의 선언으로 인해 **EntryPoint**, **CharSet** 및 **ExactSpelling** 필드가 불일치하게 됩니다. 호출되면 `MsgBox3`에서 예외를 throw합니다. 문자열 이름 지정 및 이름 마샬링에 대한 자세한 내용은 [문자 집합 지정](specifying-a-character-set.md)을 참조하세요.  
  
## <a name="declaring-prototypes"></a>프로토타입 선언  
 [!code-cpp[Conceptual.Interop.Marshaling#5](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/msgbox.cpp#5)]
 [!code-csharp[Conceptual.Interop.Marshaling#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/msgbox.cs#5)]
 [!code-vb[Conceptual.Interop.Marshaling#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/msgbox.vb#5)]  
  
## <a name="calling-functions"></a>함수 호출  
 [!code-cpp[Conceptual.Interop.Marshaling#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/msgbox.cpp#6)]
 [!code-csharp[Conceptual.Interop.Marshaling#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/msgbox.cs#6)]
 [!code-vb[Conceptual.Interop.Marshaling#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/msgbox.vb#6)]  
  
## <a name="see-also"></a>참조

- [문자열 마샬링](marshaling-strings.md)
- [문자열에 대한 기본 마샬링](default-marshaling-for-strings.md)
- [관리 코드에서 프로토타입 만들기](creating-prototypes-in-managed-code.md)
- [문자 집합 지정](specifying-a-character-set.md)
