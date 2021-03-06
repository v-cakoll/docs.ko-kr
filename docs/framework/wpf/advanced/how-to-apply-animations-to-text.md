---
title: '방법: 텍스트에 애니메이션 적용'
ms.date: 03/30/2017
helpviewer_keywords:
- typography [WPF], animations
- animation [WPF], text
ms.assetid: eec3d26c-0a21-420f-8012-671621c47089
ms.openlocfilehash: ed2f3beb904f724ac93e2c4033aa6b2eb3fa1290
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79174630"
---
# <a name="how-to-apply-animations-to-text"></a>방법: 텍스트에 애니메이션 적용
애니메이션은 애플리케이션의 텍스트 모양과 표시를 변경할 수 있습니다. 다음 예제에서는 컨트롤의 텍스트 표시에 영향을 주는 다양한 <xref:System.Windows.Controls.TextBlock> 유형의 애니메이션을 사용합니다.  
  
## <a name="example"></a>예제  
 다음 예제에서는 <xref:System.Windows.Media.Animation.DoubleAnimation> a를 사용하여 텍스트 블록의 너비를 애니메이션합니다. 10초 동안 너비 값을 텍스트 블록의 너비에서 0으로 변경한 후 다시 너비 값을 반대로 변경하여 계속합니다. 이러한 형식의 애니메이션은 지우기 효과를 생성합니다.  
  
 [!code-xaml[TextAnimationSample#TextAnimationSample1](~/samples/snippets/csharp/VS_Snippets_Wpf/TextAnimationSample/CS/Window1.xaml#textanimationsample1)]  
  
 다음 예제에서는 <xref:System.Windows.Media.Animation.DoubleAnimation> a를 사용하여 텍스트 블록의 불투명도를 애니메이션합니다. 5초 동안 불투명도 값을 1.0에서 0으로 변경한 다음 불투명 값을 반대로 변경하고 계속합니다.  
  
 [!code-xaml[TextAnimationSample#TextAnimationSample2](~/samples/snippets/csharp/VS_Snippets_Wpf/TextAnimationSample/CS/Window1.xaml#textanimationsample2)]  
  
 다음 다이어그램은 <xref:System.Windows.Controls.TextBlock> 컨트롤이 `1.00` `0.00` <xref:System.Windows.Media.Animation.Timeline.Duration%2A>에 의해 정의된 5초 간격동안 불투명도를 변경하는 효과를 보여 주며,  
  
 ![불투명도를 1.00에서 0.00으로 변경하는 텍스트입니다.](./media/how-to-apply-animations-to-text/faded-text-opacity-change.png)  

 다음 예제에서는 <xref:System.Windows.Media.Animation.ColorAnimation> a를 사용하여 텍스트 블록의 전경 색상을 애니메이션합니다. 전경색 값을 5초 동안 한 색상에서 두 번째 색상으로 변경한 다음 색상 값을 반대로 변경하고 계속합니다.  
  
 [!code-xaml[TextAnimationSample#TextAnimationSample3](~/samples/snippets/csharp/VS_Snippets_Wpf/TextAnimationSample/CS/Window1.xaml#textanimationsample3)]  
  
 다음 예제에서는 <xref:System.Windows.Media.Animation.DoubleAnimation> a를 사용하여 텍스트 블록을 회전시킵니다. 텍스트 블록은 20초 동안 전체 회전을 수행한 다음 회전을 계속 반복합니다.  
  
 [!code-xaml[TextAnimationSample#TextAnimationSample4](~/samples/snippets/csharp/VS_Snippets_Wpf/TextAnimationSample/CS/Window1.xaml#textanimationsample4)]  
  
## <a name="see-also"></a>참고 항목

- [애니메이션 개요](../graphics-multimedia/animation-overview.md)
