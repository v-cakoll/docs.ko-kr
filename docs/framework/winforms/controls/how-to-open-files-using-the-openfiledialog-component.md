---
title: '방법: OpenFileDialog 구성 요소를 사용 하 여 파일 열기'
ms.date: 02/11/2019
description: OpenFileDialog 구성 요소를 사용 하 여 파일을 검색 하 고 선택 하는 Windows 대화 상자를 여는 방법에 대해 알아봅니다.
dev_langs:
- csharp
- vb
helpviewer_keywords:
- OpenFileDialog component [Windows Forms], opening files
- OpenFile method [Windows Forms], OpenFileDialog component
- files [Windows Forms], opening with OpenFileDialog component
ms.assetid: 9d88367a-cc21-4ffd-be74-89fd63767d35
ms.openlocfilehash: d571885011b0f0c723c73a417f294f30f96952f4
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904431"
---
# <a name="how-to-open-files-with-the-openfiledialog"></a>방법: OpenFileDialog를 사용 하 여 파일 열기

<xref:System.Windows.Forms.OpenFileDialog?displayProperty=nameWithType>구성 요소는 파일을 검색 하 고 선택 하는 Windows 대화 상자를 엽니다. 선택한 파일을 열고 읽으려면 메서드를 사용 <xref:System.Windows.Forms.OpenFileDialog.OpenFile%2A?displayProperty=nameWithType> 하거나 클래스의 인스턴스를 만들 수 있습니다 <xref:System.IO.StreamReader?displayProperty=nameWithType> . 다음 예에서는 두 가지 방법을 보여 줍니다.

.NET Framework에서 속성을 가져오거나 설정 하려면 <xref:System.Windows.Forms.FileDialog.FileName%2A> 클래스에서 부여한 권한 수준이 필요 합니다 <xref:System.Security.Permissions.FileIOPermission?displayProperty=nameWithType> . 이 예제에서는 <xref:System.Security.Permissions.FileIOPermission> 권한 검사를 실행 하 고 부분 신뢰 컨텍스트에서 실행 하는 경우 권한 부족으로 인해 예외를 throw 할 수 있습니다. 자세한 내용은 [코드 액세스 보안 기본 사항](../../misc/code-access-security-basics.md)을 참조 하세요.

C # 또는 Visual Basic 명령줄에서 .NET Framework 앱으로 이러한 예제를 빌드하고 실행할 수 있습니다. 자세한 내용은 [csc.exe를 사용 하 여 명령줄 빌드](../../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md) 또는 [명령줄에서 빌드](../../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md)를 참조 하세요.

.NET Core 3.0부터 .NET Core Windows Forms * \<folder name> .csproj* 프로젝트 파일이 있는 폴더에서 Windows .net core 앱으로 예제를 빌드하고 실행할 수도 있습니다.

## <a name="example-read-a-file-as-a-stream-with-streamreader"></a>예: StreamReader를 사용 하 여 파일을 스트림으로 읽기  
  
다음 예제에서는 Windows Forms 컨트롤의 이벤트 처리기를 사용 하 여 메서드를 사용 하 여 <xref:System.Windows.Forms.Button> <xref:System.Windows.Forms.Control.Click> 를 엽니다 <xref:System.Windows.Forms.OpenFileDialog> <xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> . 사용자가 파일을 선택 하 고 **확인**을 선택 하면 클래스의 인스턴스는 <xref:System.IO.StreamReader> 파일을 읽고 폼의 텍스트 상자에 해당 내용을 표시 합니다. 파일 스트림에서 읽는 방법에 대 한 자세한 내용은 및을 참조 하십시오 <xref:System.IO.FileStream.BeginRead%2A?displayProperty=nameWithType> <xref:System.IO.FileStream.Read%2A?displayProperty=nameWithType> .  

 [!code-csharp[OpenFileDialog#1](~/samples/snippets/winforms/open-files/example1/cs/Form1.cs)]
 [!code-vb[OpenFileDialog#1](~/samples/snippets/winforms/open-files/example1/vb/Form1.vb)]  

## <a name="example-open-a-file-from-a-filtered-selection-with-openfile"></a>예: System.windows.forms.openfiledialog.openfile를 사용 하 여 필터링 된 선택에서 파일 열기

다음 예제에서는 <xref:System.Windows.Forms.Button> 컨트롤의 이벤트 처리기를 사용 하 여 <xref:System.Windows.Forms.Control.Click> <xref:System.Windows.Forms.OpenFileDialog> 텍스트 파일만 표시 하는 필터를 사용 하 여를 엽니다. 사용자가 텍스트 파일을 선택 하 고 **확인**을 선택 하면 <xref:System.Windows.Forms.OpenFileDialog.OpenFile%2A> 메서드를 사용 하 여 메모장에서 파일을 엽니다.

 [!code-csharp[OpenFileDialog#2](~/samples/snippets/winforms/open-files/example2/cs/Form1.cs)]
 [!code-vb[OpenFileDialog#2](~/samples/snippets/winforms/open-files/example2/vb/Form1.vb)]  

## <a name="see-also"></a>참고 항목

- <xref:System.Windows.Forms.OpenFileDialog>
- [OpenFileDialog 구성 요소](openfiledialog-component-windows-forms.md)
