---
title: LoadStringRC 함수
ms.date: 03/30/2017
api_name:
- LoadStringRC
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- LoadStringRC
helpviewer_keywords:
- LoadStringRC function [.NET Framework hosting]
ms.assetid: 752e49b4-987c-4c28-a118-1a0c1ed510c5
topic_type:
- apiref
ms.openlocfilehash: 8bd0292ddf22453f8892ed8bddd10c2144877097
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008519"
---
# <a name="loadstringrc-function"></a>LoadStringRC 함수
현재 스레드의 기본 문화권을 사용 하 여 HRESULT 값을 오류 메시지로 변환 합니다.  
  
 이 함수는 .NET Framework 4에서 더 이상 사용 되지 않습니다.  
  
## <a name="syntax"></a>구문  
  
```cpp  
HRESULT LoadStringRC (  
    [in]  UINT    iResourceID,
    [out] LPWSTR  szBuffer,
    [in]  int     iMax,
    [in]  int     bQuiet  
);  
```  
  
## <a name="parameters"></a>매개 변수  
 `iResourceID`  
 진행 HRESULT입니다.  
  
 `szBuffer`  
 제한이 성공적으로 완료 되 면 오류 메시지를 포함 하는 버퍼입니다.  
  
 `iMax`  
 진행 오류 메시지 버퍼의 크기입니다.  
  
 `bQuiet`  
 진행 무시.  
  
## <a name="return-value"></a>반환 값  
 이 메서드는 Winerror.h에 정의 된 대로 다음 값 외에 표준 COM (구성 요소 개체 모델) 오류 코드를 반환 합니다.  
  
|반환 코드|Description|  
|-----------------|-----------------|  
|S_OK|메서드가 완료되었습니다.|  
|E_INVALIDARG|`szBuffer`가 null 이거나 `iMax` 가 0입니다.|  
  
## <a name="remarks"></a>설명  
 메서드가 성공적으로 완료 되지 않으면에 `szBuffer` 빈 문자열이 포함 됩니다.  
  
## <a name="requirements"></a>요구 사항  
 **플랫폼:**[시스템 요구 사항](../../get-started/system-requirements.md)을 참조하세요.  
  
 **헤더:** Mscoree.dll  
  
 **라이브러리:** Mscoree.dll 및 Mscorwks.dll. Mscorwks.dll 대신 Mscoree.dll을 사용 하 여 올바른 버전의 .NET Framework를 대상으로 하는지 확인 합니다.  
  
 **.NET Framework 버전:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>참고 항목

- [LoadStringRCEx 함수](loadstringrcex-function.md)
- [사용되지 않는 CLR 호스팅 함수](deprecated-clr-hosting-functions.md)
