---
title: ICorPublish::GetProcess 메서드
ms.date: 03/30/2017
api_name:
- ICorPublish.GetProcess
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorPublish::GetProcess
helpviewer_keywords:
- ICorPublish::GetProcess method [.NET Framework debugging]
- GetProcess method, ICorPublish interface [.NET Framework debugging]
ms.assetid: c5143805-2eb7-45b8-85ed-c8fb34df1084
topic_type:
- apiref
ms.openlocfilehash: a9d28243e9907fcc6320b2e09a49312bf35a70b4
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73121784"
---
# <a name="icorpublishgetprocess-method"></a>ICorPublish::GetProcess 메서드
지정 된 식별자를 사용 하 여 프로세스를 나타내는 [ICorPublishProcess](../../../../docs/framework/unmanaged-api/debugging/icorpublishprocess-interface.md) 인스턴스를 가져옵니다.  
  
## <a name="syntax"></a>구문  
  
```cpp  
HRESULT GetProcess(  
    [in] unsigned              pid,   
    [out] ICorPublishProcess   **ppProcess  
);  
```  
  
## <a name="parameters"></a>매개 변수  
 `pid`  
 진행 프로세스의 식별자입니다.  
  
 `ppProcess`  
 제한이 프로세스를 나타내는 `ICorPublishProcess` 인스턴스의 주소에 대 한 포인터입니다.  
  
## <a name="remarks"></a>주의  
 프로세스가 존재 하지 않거나 현재 사용자가 디버그할 수 있는 관리 되는 프로세스가 아닌 경우 `GetProcess` 실패 합니다.  
  
## <a name="requirements"></a>요구 사항  
 **플랫폼:** [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)을 참조하세요.  
  
 **헤더:** CorPub .idl, CorPub. h  
  
 **라이브러리:** CorGuids.lib  
  
 **.NET Framework 버전:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>참조

- [ICorPublish 인터페이스](../../../../docs/framework/unmanaged-api/debugging/icorpublish-interface.md)
