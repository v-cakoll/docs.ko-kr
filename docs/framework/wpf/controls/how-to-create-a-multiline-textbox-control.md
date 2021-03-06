---
title: '방법: 여러 줄 TextBox 컨트롤 만들기'
description: XAML을 사용 하 여 Windows Presentation Foundation 응용 프로그램에서 여러 줄의 텍스트에 맞게 확장 되는 TextBox 컨트롤을 정의 하는 방법을 알아봅니다.
ms.date: 03/30/2017
helpviewer_keywords:
- TextBox control [WPF], multiple lines of text
ms.assetid: 05914a93-d0ea-4a9a-b693-09df7d4e2ac2
ms.openlocfilehash: 0a88d4d768884df135afddb491431650b9ba2d24
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166254"
---
# <a name="how-to-create-a-multiline-textbox-control"></a>방법: 여러 줄 TextBox 컨트롤 만들기
이 예제에서는를 사용 하 여 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] <xref:System.Windows.Controls.TextBox> 여러 줄의 텍스트에 맞게 자동으로 확장 되는 컨트롤을 정의 하는 방법을 보여 줍니다.  
  
## <a name="example"></a>예제  
 특성을 <xref:System.Windows.Controls.TextBox.TextWrapping%2A> **wrap** 으로 설정 하면 컨트롤의 가장자리에 도달할 때 텍스트를 새 줄로 래핑하여 <xref:System.Windows.Controls.TextBox> <xref:System.Windows.Controls.TextBox> 필요한 경우 새 줄의 공간을 포함 하도록 컨트롤을 자동으로 확장 합니다.  
  
 특성을 <xref:System.Windows.Controls.Primitives.TextBoxBase.AcceptsReturn%2A> **true** 로 설정 하면 반환 키를 누를 때 새 줄이 삽입 됩니다 .를 다시 한 번 자동으로 확장 <xref:System.Windows.Controls.TextBox> 하 여 필요한 경우 새 줄의 공간을 포함 합니다.  
  
 특성은에 <xref:System.Windows.Controls.Primitives.TextBoxBase.VerticalScrollBarVisibility%2A> 스크롤 막대를 추가 하 여가 해당 콘텐츠를 포함 <xref:System.Windows.Controls.TextBox> <xref:System.Windows.Controls.TextBox> <xref:System.Windows.Controls.TextBox> 하는 프레임 또는 창의 크기 보다 크게 확장 될 때까지 스크롤할 수 있도록 합니다.  
  
 [!code-xaml[TextBox_MiscCode#_MultilineTextBoxXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml#_multilinetextboxxaml)]  
  
## <a name="see-also"></a>참고 항목

- <xref:System.Windows.TextWrapping>
- [TextBox 개요](textbox-overview.md)
- [RichTextBox 개요](richtextbox-overview.md)
