---
title: ICorProfilerInfo3::GetStringLayout2 메서드
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.GetStringLayout2 Method
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::GetStringLayout2
helpviewer_keywords:
- ICorProfilerInfo3::GetStringLayout2 method [.NET Framework profiling]
- GetStringLayout2 method [.NET Framework profiling]
ms.assetid: 1a268496-ee51-4d84-8700-ee56fd0c499d
topic_type:
- apiref
ms.openlocfilehash: 1e3dc4735af68da7f76fc6fce84d2dd4ac3f576e
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74449659"
---
# <a name="icorprofilerinfo3getstringlayout2-method"></a>ICorProfilerInfo3::GetStringLayout2 메서드
문자열 개체의 레이아웃 정보를 가져옵니다. 이 메서드는 [ICorProfilerInfo2:: GetStringLayout](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo2-getstringlayout-method.md) 메서드를 대체 합니다.  
  
## <a name="syntax"></a>구문  
  
```cpp  
HRESULT GetStringLayout2(  
    [out] ULONG *pStringLengthOffset,  
    [out] ULONG *pBufferOffset);  
```  
  
## <a name="parameters"></a>매개 변수  
 `pStringLengthOffset`  
 제한이 문자열 자체의 길이를 저장 하는 `ObjectID` 포인터를 기준으로 하는 위치의 오프셋에 대 한 포인터입니다. 길이는 `DWORD`로 저장 됩니다.  
  
 `pBufferOffset`  
 제한이 와이드 문자 문자열을 저장 하는 `ObjectID` 포인터를 기준으로 하는 버퍼의 오프셋에 대 한 포인터입니다.  
  
## <a name="remarks"></a>설명  
 문자열이 null로 종료 될 수도 있고 그렇지 않을 수도 있습니다.  
  
## <a name="requirements"></a>요구 사항  
 **플랫폼:** [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)을 참조하세요.  
  
 **헤더:** CorProf.idl, CorProf.h  
  
 **라이브러리:** CorGuids.lib  
  
 **.NET Framework 버전:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>참고자료

- [ICorProfilerInfo3 인터페이스](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo3-interface.md)
- [프로파일링 인터페이스](../../../../docs/framework/unmanaged-api/profiling/profiling-interfaces.md)
