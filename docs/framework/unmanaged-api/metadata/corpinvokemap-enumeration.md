---
title: CorPinvokeMap 열거형
ms.date: 03/30/2017
api_name:
- CorPinvokeMap
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorPinvokeMap
helpviewer_keywords:
- CorPinvokeMap enumeration [.NET Framework metadata]
ms.assetid: f14f986e-f6ce-42bc-aa23-18150c46d28c
topic_type:
- apiref
ms.openlocfilehash: 17b7af7016cf88fd3ae263dd952502d515b0c833
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74441565"
---
# <a name="corpinvokemap-enumeration"></a>CorPinvokeMap 열거형
PInvoke 호출에 대 한 옵션을 지정 합니다.  
  
## <a name="syntax"></a>구문  
  
```cpp  
typedef enum  CorPinvokeMap {  
  
    pmNoMangle          = 0x0001,  
  
    pmCharSetMask       = 0x0006,  
    pmCharSetNotSpec    = 0x0000,  
    pmCharSetAnsi       = 0x0002,  
    pmCharSetUnicode    = 0x0004,  
    pmCharSetAuto       = 0x0006,  
  
    pmBestFitUseAssem   = 0x0000,  
    pmBestFitEnabled    = 0x0010,  
    pmBestFitDisabled   = 0x0020,  
    pmBestFitMask       = 0x0030,  
  
    pmThrowOnUnmappableCharUseAssem   = 0x0000,  
    pmThrowOnUnmappableCharEnabled    = 0x1000,  
    pmThrowOnUnmappableCharDisabled   = 0x2000,  
    pmThrowOnUnmappableCharMask       = 0x3000,  
  
    pmSupportsLastError = 0x0040,   
  
    pmCallConvMask      = 0x0700,  
    pmCallConvWinapi    = 0x0100,  
    pmCallConvCdecl     = 0x0200,  
    pmCallConvStdcall   = 0x0300,  
    pmCallConvThiscall  = 0x0400,  
    pmCallConvFastcall  = 0x0500,  
  
    pmMaxValue          = 0xFFFF  
  
} CorPinvokeMap;  
```  
  
## <a name="members"></a>멤버  
  
|멤버|설명|  
|------------|-----------------|  
|`pmNoMangle`|지정 된 각 멤버 이름을 사용 합니다.|  
|`pmCharSetMask`|예약됨.|  
|`pmCharSetNotSpec`|예약됨.|  
|`pmCharSetAnsi`|문자열을 다중 바이트 문자열로 마샬링하십시오.|  
|`pmCharSetUnicode`|문자열을 유니코드 2 바이트 문자로 마샬링하십시오.|  
|`pmCharSetAuto`|대상 운영 체제에 맞게 문자열을 자동으로 마샬링하십시오. Windows NT, Windows 2000, Windows XP 및 Windows Server 2003 제품군의 기본값은 유니코드입니다. 기본값은 Windows 98 및 Windows Me의 ANSI입니다.|  
|`pmBestFitUseAssem`|예약됨.|  
|`pmBestFitEnabled`|ANSI 문자 집합에서 정확히 일치 하는 항목이 없는 유니코드 문자의 최적 매핑을 수행 합니다.|  
|`pmBestFitDisabled`|유니코드 문자의 최적 매핑을 수행 하지 않습니다. 이 경우 매핑할 수 없는 모든 문자는 '? '로 바뀝니다.|  
|`pmBestFitMask`|예약됨.|  
|`pmThrowOnUnmappableCharUseAssem`|예약됨.|  
|`pmThrowOnUnmappableCharEnabled`|Interop 마샬러에서 매핑할 때 매핑할 문자가 없으면 예외를 throw 합니다.|  
|`pmThrowOnUnmappableCharDisabled`|Interop 마샬러가 매핑할 수 없는 문자를 발견 한 경우 예외를 throw 하지 않습니다.|  
|`pmThrowOnUnmappableCharMask`|예약됨|  
|`pmSupportsLastError`|특성화 된 메서드에서 반환 하기 전에 호출 수신자가 Win32 `SetLastError` 함수를 호출할 수 있도록 허용 합니다.|  
|`pmCallConvMask`|예약됨|  
|`pmCallConvWinapi`|기본 플랫폼 호출 규칙을 사용 합니다. 예를 들어 Windows에서 기본값은 `StdCall` Windows CE .NET은 `Cdecl`됩니다.|  
|`pmCallConvCdecl`|`Cdecl` 호출 규칙을 사용 합니다. 이 경우 호출자는 스택을 정리 합니다. 이렇게 하면 `varargs` (즉, 가변적인 개수의 매개 변수를 허용 하는 함수)를 사용 하 여 함수를 호출할 수 있습니다.|  
|`pmCallConvStdcall`|`StdCall` 호출 규칙을 사용 합니다. 이 경우 호출 수신자는 스택을 정리 합니다. 이 플랫폼을 사용 하 여 관리 되지 않는 함수를 호출할 호출에 대 한 기본 규칙입니다.|  
|`pmCallConvThiscall`|`ThisCall` 호출 규칙을 사용 합니다. 이 경우 첫 번째 매개 변수는 `this` 포인터이 고 레지스터 ECX에 저장 됩니다. 다른 매개 변수는 스택에 푸시됩니다. `ThisCall` 호출 규칙은 관리 되지 않는 DLL에서 내보낸 클래스에서 메서드를 호출 하는 데 사용 됩니다.|  
|`pmCallConvFastcall`|예약됨.|  
|`pmMaxValue`|예약됨.|  
  
## <a name="requirements"></a>요구 사항  
 **플랫폼:** [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)을 참조하세요.  
  
 **헤더:** CorHdr .h  
  
 **.NET Framework 버전:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>참고 항목

- [메타데이터 열거형](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
