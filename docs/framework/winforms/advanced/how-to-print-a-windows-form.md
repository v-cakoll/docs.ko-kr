---
title: '방법: Windows Form 인쇄'
description: CopyFromScreen 메서드를 사용 하 여 프로그래밍 방식으로 현재 Windows Form의 복사본을 인쇄 하는 방법을 알아봅니다.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Forms, printing
- printing [Windows Forms]
- printing a form
- printing [Windows Forms], printing a form
ms.assetid: c8dff5f8-f56a-4c07-ae31-64643b31f8fc
ms.openlocfilehash: b59ea4b5347903b36a166c4f8ac0d8d7db18635e
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174694"
---
# <a name="how-to-print-a-windows-form"></a>방법: Windows Form 인쇄
개발 프로세스의 일부로, 일반적으로 Windows Form의 복사본을 인쇄 합니다. 다음 코드 예제에서는 메서드를 사용 하 여 현재 폼의 복사본을 인쇄 하는 방법을 보여 줍니다 <xref:System.Drawing.Graphics.CopyFromScreen%2A> .  
  
## <a name="example"></a>예제  
 [!code-csharp[System.Drawing.Graphics.CopyFromScreen#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Graphics.CopyFromScreen/CS/Form1.cs#1)]
 [!code-vb[System.Drawing.Graphics.CopyFromScreen#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Graphics.CopyFromScreen/VB/Form1.vb#1)]  
  
## <a name="robust-programming"></a>강력한 프로그래밍  
 다음 조건에서 예외가 발생합니다.  
  
- 프린터에 액세스할 수 있는 권한이 없습니다.  
  
- 설치 된 프린터가 없습니다.  
  
## <a name="net-framework-security"></a>.NET Framework 보안  
 이 코드 예제를 실행 하려면 컴퓨터에서 사용 하는 프린터에 액세스할 수 있는 권한이 있어야 합니다.  
  
## <a name="see-also"></a>참고 항목

- <xref:System.Drawing.Printing.PrintDocument>
- [방법: GDI+를 사용하여 이미지 렌더링](how-to-render-images-with-gdi.md)
- [방법: Windows Forms에서 그래픽 인쇄](how-to-print-graphics-in-windows-forms.md)
