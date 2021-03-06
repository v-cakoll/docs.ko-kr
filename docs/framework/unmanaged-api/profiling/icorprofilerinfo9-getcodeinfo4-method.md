---
title: ICorProfilerInfo9::GetCodeInfo4
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo9.GetCodeInfo4
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: f65cebff912adeb7afc34434467cf7be72f9be32
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77449766"
---
# <a name="icorprofilerinfo9getcodeinfo4-method"></a>ICorProfilerInfo9:: GetCodeInfo4 메서드

네이티브 코드 시작 주소가 지정 된 경우이 코드를 저장 하는 가상 메모리 블록을 반환 합니다.

## <a name="syntax"></a>구문

```cpp
HRESULT GetCodeInfo4( [in]  UINT_PTR pNativeCodeStartAddress,
                      [in]  ULONG32 cCodeInfos,
                      [out] ULONG32* pcCodeInfos,
                      [out] COR_PRF_CODE_INFO codeInfos[]);
```

## <a name="parameters"></a>매개 변수

- `pNativeCodeStartAddress`

  \[in] 네이티브 함수의 시작 부분에 대 한 포인터입니다.

- `cCodeInfos`

  \[in] `codeInfos` 배열의 크기입니다.

- `pcCodeInfos`

  \[out] 사용할 수 있는 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) 구조체의 총 수에 대 한 포인터입니다.

- `codeInfos`

  \[out] 호출자가 제공한 버퍼입니다. 메서드가 반환된 후에는 각각 네이티브 코드 블록을 설명하는 `COR_PRF_CODE_INFO` 구조체의 배열을 포함합니다.

## <a name="remarks"></a>설명

`GetCodeInfo4` 메서드는 메서드의 여러 다른 네이티브 버전에 대 한 코드 정보를 조회할 수 있다는 점을 제외 하 고 [GetCodeInfo3](icorprofilerinfo4-getcodeinfo3-method.md)와 비슷합니다.

> [!NOTE]
> `GetCodeInfo4` 가비지 수집을 트리거할 수 있습니다.

범위는 CIL(Common Intermediate Language) 오프셋의 오름차순으로 정렬됩니다.

`GetCodeInfo4` 반환한 후에는 `codeInfos` 버퍼가 모든 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) 구조를 포함할 수 있을 만큼 충분히 큰지 확인 해야 합니다. 이렇게 하려면 `cCodeInfos` 값을 `cchName` 매개 변수의 값과 비교합니다. [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) 구조의 크기로 나눈 `cCodeInfos` `pcCodeInfos`보다 작은 경우 더 큰 `codeInfos` 버퍼를 할당 하 고, 더 큰 새 크기를 사용 하 여 `cCodeInfos`를 업데이트 하 고, `GetCodeInfo4`을 다시 호출 합니다.

또는 길이가 0인 `GetCodeInfo4` 버퍼로 `codeInfos`를 먼저 호출하여 올바른 버퍼 크기를 구합니다. 그런 다음 `codeInfos` 버퍼 크기를 `pcCodeInfos`에서 반환 된 값으로 설정 하 고 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) 구조의 크기를 곱하여 `GetCodeInfo4`를 다시 호출할 수 있습니다.

## <a name="requirements"></a>요구 사항

**플랫폼:** [.Net Core 지원 운영 체제](../../../core/install/dependencies.md?pivots=os-windows)를 참조 하세요.

**헤더:** CorProf.idl, CorProf.h

**라이브러리:** CorGuids.lib

**.Net 버전:** [!INCLUDE[net_core_22](../../../../includes/net-core-22-md.md)]

## <a name="see-also"></a>참고 항목

- [ICorProfilerInfo9 인터페이스](ICorProfilerInfo9-interface.md)
