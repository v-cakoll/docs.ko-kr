---
title: <clear>
ms.date: 03/30/2017
ms.assetid: 54dcd1d1-038f-4fc8-a3a4-56ba7a1ca0fd
author: BrucePerlerMS
ms.openlocfilehash: e96349c72fc4a952e3dc7efeea5f69ebaa1fd0ad
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70252040"
---
# \<clear>
현재 토큰 처리기 컬렉션에서 모든 보안 토큰 처리기를 지웁니다.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<clear>**  
  
## <a name="syntax"></a>구문  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <clear>  
      </clear>  
    </securityTokenHandlers>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>특성 및 요소  
 다음 섹션에서는 특성, 자식 요소 및 부모 요소에 대해 설명합니다.  
  
### <a name="attributes"></a>특성  
 None  
  
### <a name="child-elements"></a>자식 요소  
 None  
  
### <a name="parent-elements"></a>부모 요소  
  
|요소|Description|  
|-------------|-----------------|  
|[\<securityTokenHandlers>](securitytokenhandlers.md)|끝점에 등록 된 보안 토큰 처리기의 컬렉션을 지정 합니다.|
