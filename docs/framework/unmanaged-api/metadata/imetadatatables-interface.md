---
title: IMetaDataTables 인터페이스
ms.date: 03/30/2017
api_name:
- IMetaDataTables
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataTables
helpviewer_keywords:
- IMetaDataTables interface [.NET Framework metadata]
ms.assetid: 31272cce-506a-4f18-bcbf-01ee45e36356
topic_type:
- apiref
ms.openlocfilehash: 17305f2c088dd6f479da4c823d3db0fd50c0b3d7
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74443225"
---
# <a name="imetadatatables-interface"></a>IMetaDataTables 인터페이스
테이블에서 메타데이터 정보를 스토리지 및 검색하는 메서드를 제공합니다.  
  
## <a name="methods"></a>메서드  
  
|메서드|설명|  
|------------|-----------------|  
|[GetBlob 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getblob-method.md)|지정 된 열 인덱스에 있는 BLOB (binary large object)에 대 한 포인터를 가져옵니다.|  
|[GetBlobHeapSize 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getblobheapsize-method.md)|BLOB 힙의 크기 (바이트)를 가져옵니다.|  
|[GetCodedTokenInfo 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getcodedtokeninfo-method.md)|지정 된 행 인덱스와 연결 된 토큰의 배열에 대 한 포인터를 가져옵니다.|  
|[GetColumn 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getcolumn-method.md)|지정 된 테이블 인덱스에 있는 테이블의 지정 된 열 인덱스에 있는 열에 포함 된 값에 대 한 포인터를 가져옵니다.|  
|[GetColumnInfo 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getcolumninfo-method.md)|지정 된 테이블의 지정 된 열에 대 한 데이터를 가져옵니다.|  
|[GetGuid 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getguid-method.md)|지정 된 인덱스의 행에서 GUID를 가져옵니다.|  
|[GetGuidHeapSize 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getguidheapsize-method.md)|GUID 힙의 크기 (바이트)를 가져옵니다.|  
|[GetNextBlob 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getnextblob-method.md)|테이블에서 다음 BLOB의 인덱스를 가져옵니다.|  
|[GetNextGuid 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getnextguid-method.md)|현재 테이블 열에서 다음 GUID 값의 인덱스를 가져옵니다.|  
|[GetNextString 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getnextstring-method.md)|현재 테이블 열에서 다음 문자열의 인덱스를 가져옵니다.|  
|[GetNextUserString 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getnextuserstring-method.md)|현재 테이블 열에 하드 코딩 된 다음 문자열이 포함 된 행의 인덱스를 가져옵니다.|  
|[GetNumTables 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getnumtables-method.md)|현재 `IMetaDataTables` 인스턴스의 범위에 있는 테이블 수를 가져옵니다.|  
|[GetRow 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getrow-method.md)|지정 된 테이블 인덱스에 있는 테이블의 지정 된 행 인덱스에 있는 행을 가져옵니다.|  
|[GetString 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getstring-method.md)|현재 참조 범위의 테이블 열에서 지정 된 인덱스에 있는 문자열을 가져옵니다.|  
|[GetStringHeapSize 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getstringheapsize-method.md)|문자열 힙의 크기 (바이트)를 가져옵니다.|  
|[GetTableIndex 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-gettableindex-method.md)|지정 된 토큰이 참조 하는 테이블의 인덱스를 가져옵니다.|  
|[GetTableInfo 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-gettableinfo-method.md)|지정 된 테이블 인덱스에 있는 테이블의 이름, 행 크기, 행 수, 열 수, 키 열 인덱스를 가져옵니다.|  
|[GetUserString 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getuserstring-method.md)|현재 범위에 있는 문자열 열의 지정 된 인덱스에서 하드 코드 된 문자열을 가져옵니다.|  
|[GetUserStringHeapSize 메서드](../../../../docs/framework/unmanaged-api/metadata/imetadatatables-getuserstringheapsize-method.md)|사용자 문자열 힙의 크기 (바이트)를 가져옵니다.|  
  
## <a name="requirements"></a>요구 사항  
 **플랫폼:** [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)을 참조하세요.  
  
 **헤더:** Cor  
  
 **라이브러리:** Mscoree.dll에서 리소스로 사용 됩니다.  
  
 **.NET Framework 버전:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>참고 항목

- [메타데이터 인터페이스](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md)
- [IMetaDataTables2 인터페이스](../../../../docs/framework/unmanaged-api/metadata/imetadatatables2-interface.md)
