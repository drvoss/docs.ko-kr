---
title: ICorDebugLoadedModule::GetSize 메서드
ms.date: 03/30/2017
ms.assetid: aaa0e5c0-be9d-4fe1-8418-5295b9b184d6
ms.openlocfilehash: 3f2f8a1721847b8f7b845c42aa3c91e032c2d474
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73122640"
---
# <a name="icordebugloadedmodulegetsize-method"></a>ICorDebugLoadedModule::GetSize 메서드
로드된 모듈의 크기(바이트)를 가져옵니다.  
  
## <a name="syntax"></a>구문  
  
```cpp  
HRESULT GetSize(  
   [out] ULONG32 *pcBytes  
);  
```  
  
## <a name="parameters"></a>매개 변수  
 `pcBytes`  
 [out] 로드된 모듈의 바이트 수에 대한 포인터입니다.  
  
## <a name="remarks"></a>주의  
  
> [!NOTE]
> 이 메서드는 .NET 네이티브에서만 사용할 수 있습니다.  
  
## <a name="requirements"></a>요구 사항  
 **플랫폼:** [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)을 참조하세요.  
  
 **헤더:** CorDebug.idl, CorDebug.h  
  
 **라이브러리:** CorGuids.lib  
  
 **.NET Framework 버전:** [!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>참조

- [ICorDebugLoadedModule 인터페이스](../../../../docs/framework/unmanaged-api/debugging/icordebugloadedmodule-interface.md)
- [디버깅 인터페이스](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)
