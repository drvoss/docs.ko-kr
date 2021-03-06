---
title: IMetaDataImport::EnumMembers 메서드
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumMembers
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumMembers
helpviewer_keywords:
- IMetaDataImport::EnumMembers method [.NET Framework metadata]
- EnumMembers method [.NET Framework metadata]
ms.assetid: 3fb8e178-342b-4c89-9bcf-f7f834e6cb77
topic_type:
- apiref
ms.openlocfilehash: acb772a64c8f13405f2836bb5f4f308986dce414
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74447662"
---
# <a name="imetadataimportenummembers-method"></a>IMetaDataImport::EnumMembers 메서드
지정한 형식의 멤버를 나타내는 MemberDef 토큰을 열거합니다.  
  
## <a name="syntax"></a>구문  
  
```cpp  
HRESULT EnumMembers (   
   [in, out]  HCORENUM    *phEnum,   
   [in]  mdTypeDef   cl,   
   [out] mdToken     rMembers[],   
   [in]  ULONG       cMax,   
   [out] ULONG       *pcTokens  
);  
```  
  
## <a name="parameters"></a>매개 변수  
 `phEnum`  
 [in, out] 열거자에 대 한 포인터입니다.  
  
 `cl`  
 진행 멤버가 열거 될 형식을 나타내는 TypeDef 토큰입니다.  
  
 `rMembers`  
 제한이 MemberDef 토큰을 보유 하는 데 사용 되는 배열입니다.  
  
 `cMax`  
 [in] `rMembers` 배열의 최대 크기입니다.  
  
 `pcTokens`  
 제한이 `rMembers`에서 반환 된 실제 MemberDef 토큰 수입니다.  
  
## <a name="return-value"></a>반환 값  
  
|HRESULT|설명|  
|-------------|-----------------|  
|`S_OK`|`EnumMembers` 성공적으로 반환 되었습니다.|  
|`S_FALSE`|열거할 MemberDef 토큰이 없습니다. 이 경우 `pcTokens`은 0입니다.|  
  
## <a name="remarks"></a>주의  
 클래스에 대 한 멤버의 컬렉션을 열거 하는 경우 `EnumMembers`는 클래스에 직접 정의 된 멤버 (필드 및 메서드는 제외)만 반환 하 고 속성 또는 이벤트는 반환 **하지 않습니다** . 클래스가 상속 된 멤버에 대 한 구현을 제공 하는 경우에도 클래스가 상속 하는 멤버를 반환 하지 않습니다. 상속 된 멤버를 열거 하려면 호출자가 상속 체인을 명시적으로 탐색 해야 합니다. 상속 체인에 대 한 규칙은 원래 메타 데이터를 내보낸 언어 또는 컴파일러에 따라 달라질 수 있습니다.
 
 `EnumMembers`에서 속성 및 이벤트를 열거 하지 않습니다. 이를 열거 하려면 [Enumproperties](imetadataimport-enumproperties-method.md) 또는 [enumproperties](imetadataimport-enumevents-method.md)를 사용 합니다.
  
## <a name="requirements"></a>요구 사항  
 **플랫폼:** [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)을 참조하세요.  
  
 **헤더:** Cor  
  
 **라이브러리:** Mscoree.dll에 리소스로 포함 됩니다.  
  
 **.NET Framework 버전:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>참고 항목

- [IMetaDataImport 인터페이스](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 인터페이스](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
