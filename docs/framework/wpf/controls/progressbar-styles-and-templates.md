---
title: ProgressBar 스타일 및 템플릿
ms.date: 03/30/2017
helpviewer_keywords:
- parts [WPF], ProgressBar
- ProgressBar [WPF], styles and templates
- styles [WPF], ProgressBar
- ControlTemplate [WPF], ProgressBar
- templates [WPF], ProgressBar
- states [WPF], ProgressBar
ms.assetid: 935aa600-16e6-4947-a905-37a189a583dd
ms.openlocfilehash: 6551701e86dd6abcd42f143f146c7bdadfeabbcf
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74283450"
---
# <a name="progressbar-styles-and-templates"></a>ProgressBar 스타일 및 템플릿
이 항목에서는 <xref:System.Windows.Controls.ProgressBar> 컨트롤의 스타일 및 템플릿에 대해 설명 합니다. 기본 <xref:System.Windows.Controls.ControlTemplate>를 수정 하 여 컨트롤에 고유한 모양을 제공할 수 있습니다. 자세한 내용은 [컨트롤에 대 한 템플릿 만들기](../../../desktop-wpf/themes/how-to-create-apply-template.md)를 참조 하세요.  
  
## <a name="progressbar-parts"></a>ProgressBar 파트  
 다음 표에서는 <xref:System.Windows.Controls.ProgressBar> 컨트롤의 명명 된 파트를 나열 합니다.  
  
|부분|형식|설명|  
|-|-|-|  
|PART_Indicator|<xref:System.Windows.FrameworkElement>|진행률을 나타내는 개체입니다.|  
|PART_Track|<xref:System.Windows.FrameworkElement>|진행률 표시기의 경로를 정의 하는 개체입니다.|  
|PART_GlowRect|<xref:System.Windows.FrameworkElement>|진행률 표시줄을 embellishes 개체입니다.|  
  
## <a name="progressbar-states"></a>ProgressBar 상태  
 다음 표에서는 <xref:System.Windows.Controls.ProgressBar> 컨트롤의 시각적 상태를 보여 줍니다.  
  
|VisualState 이름|VisualStateGroup 이름|설명|  
|----------------------|---------------------------|-----------------|  
|활성화 상태의|CommonStates|<xref:System.Windows.Controls.ProgressBar> <xref:System.Windows.Controls.Primitives.RangeBase.Value%2A> 속성을 기반으로 진행률을 보고 합니다.|  
|비활성화|CommonStates|<xref:System.Windows.Controls.ProgressBar>는 반복 패턴으로 일반적인 진행률을 보고 합니다.|  
|유효|ValidationStates|컨트롤은 <xref:System.Windows.Controls.Validation> 클래스를 사용 하 고 연결 된 <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 속성은 `false`됩니다.|  
|InvalidFocused|ValidationStates|연결 된 <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 속성 `true` 컨트롤에 포커스가 있습니다.|  
|InvalidUnfocused|ValidationStates|연결 된 <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> 속성이 `true` 컨트롤에 포커스가 없는 경우|  
  
## <a name="progressbar-controltemplate-example"></a>ProgressBar ControlTemplate 예제  
 다음 예제에서는 <xref:System.Windows.Controls.ProgressBar> 컨트롤에 대 한 <xref:System.Windows.Controls.ControlTemplate>를 정의 하는 방법을 보여 줍니다.  
  
 [!code-xaml[ControlTemplateExamples#ProgressBar](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/progressbar.xaml#progressbar)]  
  
 앞의 예제에서는 다음 리소스를 하나 이상 사용합니다.  
  
 [!code-xaml[ControlTemplateExamples#Resources](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/shared.xaml#resources)]  
  
 전체 샘플을 보려면 [Styling with ControlTemplates Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating)(ControlTemplate으로 스타일 지정 샘플)을 참조하세요.  
  
## <a name="see-also"></a>참고자료

- <xref:System.Windows.FrameworkElement.Style%2A>
- <xref:System.Windows.Controls.ControlTemplate>
- [Control 스타일 및 템플릿](control-styles-and-templates.md)
- [컨트롤 사용자 지정](control-customization.md)
- [스타일 지정 및 템플릿](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [컨트롤에 대 한 템플릿 만들기](../../../desktop-wpf/themes/how-to-create-apply-template.md)
