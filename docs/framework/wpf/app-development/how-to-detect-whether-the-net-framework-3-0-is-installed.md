---
title: '방법: .NET Framework 3.0 설치 여부 확인'
ms.date: 03/30/2017
helpviewer_keywords:
- WinFX Runtime user-agent string
- presence of WPT [WPF], detecting
- detecting WPF presence [WPF]
ms.assetid: 7f71d652-1749-4379-945a-aa2e3994cb43
ms.openlocfilehash: 60868661df442849db3f5421f8ea33f790fd83fc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79187362"
---
# <a name="how-to-detect-whether-the-net-framework-30-is-installed"></a>방법: .NET Framework 3.0 설치 여부 확인
관리자가 시스템에 Microsoft .NET Framework 응용 프로그램을 배포하려면 먼저 .NET Framework 런타임이 있는지 확인해야 합니다. 이 항목에서는 관리자가 .NET Framework가 시스템에 있는지 여부를 확인하는 데 사용할 수 있는 HTML/JavaScript로 작성된 스크립트를 제공합니다.  
  
> [!NOTE]
> Microsoft .NET 프레임워크의 설치, 배포 및 검색에 대한 자세한 내용은 [Microsoft .NET 프레임워크 버전 배포 3.0에서](https://docs.microsoft.com/previous-versions/dotnet/articles/aa480198(v=msdn.10))설명합니다.  
  
<a name="content_expiration"></a>
## <a name="detect-the-net-clr-user-agent-string"></a>".NET CLR" 사용자 에이전트 문자열 검색  
 .NET 프레임워크를 설치하면 MSI는 ".NET CLR" 및 버전 번호를 UserAgent 문자열에 추가합니다. 다음 예제에서는 간단한 HTML 페이지에 포함된 스크립트를 보여 주며 있습니다. 스크립트는 UserAgent 문자열을 검색하여 .NET Framework가 설치되어 있는지 확인하고 검색 결과에 상태 메시지를 표시합니다.  
  
```html  
<HTML>  
  <HEAD>  
    <TITLE>Test for the .NET Framework 3.0</TITLE>  
    <META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=utf-8" />  
    <SCRIPT LANGUAGE="JavaScript">  
    <!--  
    var dotNETRuntimeVersion = "3.0.04425.00";  
  
    function window::onload()  
    {  
      if (HasRuntimeVersion(dotNETRuntimeVersion))  
      {  
        result.innerText =   
          "This machine has the correct version of the .NET Framework 3.0: "   
          + dotNETRuntimeVersion  
      }   
      else  
      {  
        result.innerText =   
          "This machine does not have the correct version of the .NET Framework 3.0."  
      }  
      result.innerText += "\n\nThis machine's userAgent string is: " +   
        navigator.userAgent + ".";  
    }  
  
    //  
    // Retrieve the version from the user agent string and   
    // compare with the specified version.  
    //  
    function HasRuntimeVersion(versionToCheck)  
    {  
      var userAgentString =   
        navigator.userAgent.match(/.NET CLR [0-9.]+/g);  
  
      if (userAgentString != null)  
      {  
        var i;  
  
        for (i = 0; i < userAgentString.length; ++i)  
        {  
          if (CompareVersions(GetVersion(versionToCheck),   
            GetVersion(userAgentString[i])) <= 0)  
            return true;  
        }  
      }  
  
      return false;  
    }  
  
    //  
    // Extract the numeric part of the version string.  
    //  
    function GetVersion(versionString)  
    {  
      var numericString =   
        versionString.match(/([0-9]+)\.([0-9]+)\.([0-9]+)/i);  
      return numericString.slice(1);  
    }  
  
    //  
    // Compare the 2 version strings by converting them to numeric format.  
    //  
    function CompareVersions(version1, version2)  
    {  
      for (i = 0; i < version1.length; ++i)  
      {  
        var number1 = new Number(version1[i]);  
        var number2 = new Number(version2[i]);  
  
        if (number1 < number2)  
          return -1;  
  
        if (number1 > number2)  
          return 1;  
      }  
  
      return 0;  
    }  
  
    -->  
    </SCRIPT>  
  </HEAD>  
  
  <BODY>  
    <div id="result" />  
  </BODY>  
</HTML>  
```  
  
 ".NET CLR" 버전검색이 성공하면 다음과 같은 유형의 상태 메시지가 나타납니다.  
  
 `This machine has the correct version of the .NET Framework 3.0: 3.0.04425.00`  
  
 `This machine's userAgent string is: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; .NET CLR 1.1.4322; InfoPath.1; .NET CLR 2.0.50727; .NET CLR 3.0.04425.00).`  
  
 그렇지 않으면 다음과 같은 유형의 상태 메시지가 나타납니다.  
  
 `This machine does not have correct version of the .NET Framework 3.0.`  
  
 `This machine's userAgent string is: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; .NET CLR 1.1.4322; InfoPath.1; .NET CLR 2.0.50727).`
