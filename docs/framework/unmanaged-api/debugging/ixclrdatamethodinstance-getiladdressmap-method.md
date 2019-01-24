---
title: IXCLRDataMethodInstance::GetILAddressMap 메서드
ms.date: 01/16/2019
api.name:
- IXCLRDataMethodInstance::GetILAddressMap Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataMethodInstance::GetILAddressMap Method
helpviewer.keywords:
- IXCLRDataMethodInstance::GetILAddressMap Method [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 8442d1373ede241d262ab41928fd5d9924ec9c80
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54567193"
---
# <a name="ixclrdatamethodinstancegetiladdressmap-method"></a><span data-ttu-id="75525-102">IXCLRDataMethodInstance::GetILAddressMap 메서드</span><span class="sxs-lookup"><span data-stu-id="75525-102">IXCLRDataMethodInstance::GetILAddressMap Method</span></span>

<span data-ttu-id="75525-103">IL을 주소 매핑 정보를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="75525-103">Gets the IL to address mapping information.</span></span>

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a><span data-ttu-id="75525-104">구문</span><span class="sxs-lookup"><span data-stu-id="75525-104">Syntax</span></span>

```
HRESULT GetILAddressMap(
    [in] ULONG32                                   mapLen,
    [out] ULONG32                                 *mapNeeded,
    [out, size_is(mapLen)] CLRDATA_IL_ADDRESS_MAP  maps[]
);
```

### <a name="parameters"></a><span data-ttu-id="75525-105">매개 변수</span><span class="sxs-lookup"><span data-stu-id="75525-105">Parameters</span></span>

<span data-ttu-id="75525-106">`mapLen` [in] 제공 된 맵 배열의 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="75525-106">`mapLen` [in] The length of the provided maps array.</span></span>

<span data-ttu-id="75525-107">`mapNeeded` [out] 메서드는 맵 항목의 수입니다.</span><span class="sxs-lookup"><span data-stu-id="75525-107">`mapNeeded` [out] The number of map entries that the method needs.</span></span>

<span data-ttu-id="75525-108">`maps` [out, size_is(mapLen)] 맵 항목을 저장 하기 위한 배열입니다.</span><span class="sxs-lookup"><span data-stu-id="75525-108">`maps` [out, size_is(mapLen)] The array for storing the map entries.</span></span>

## <a name="remarks"></a><span data-ttu-id="75525-109">설명</span><span class="sxs-lookup"><span data-stu-id="75525-109">Remarks</span></span>

<span data-ttu-id="75525-110">제공 된 메서드는의 일부는 `IXCLRDataMethodInstance` 인터페이스 및 가상 메서드 테이블의 14 슬롯에 해당 합니다.</span><span class="sxs-lookup"><span data-stu-id="75525-110">The provided method is part of the `IXCLRDataMethodInstance` interface and corresponds to the 14th slot of the virtual method table.</span></span>

## <a name="requirements"></a><span data-ttu-id="75525-111">요구 사항</span><span class="sxs-lookup"><span data-stu-id="75525-111">Requirements</span></span>

<span data-ttu-id="75525-112">**플랫폼:** [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="75525-112">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
<span data-ttu-id="75525-113">**헤더:** 없음</span><span class="sxs-lookup"><span data-stu-id="75525-113">**Header:** None</span></span>  
<span data-ttu-id="75525-114">**라이브러리:** 없음</span><span class="sxs-lookup"><span data-stu-id="75525-114">**Library:** None</span></span>  
<span data-ttu-id="75525-115">**.NET Framework 버전:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span><span class="sxs-lookup"><span data-stu-id="75525-115">**.NET Framework Versions:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span></span>  

## <a name="see-also"></a><span data-ttu-id="75525-116">참고자료</span><span class="sxs-lookup"><span data-stu-id="75525-116">See also</span></span>

- [<span data-ttu-id="75525-117">디버깅</span><span class="sxs-lookup"><span data-stu-id="75525-117">Debugging</span></span>](../../../../docs/framework/unmanaged-api/debugging/index.md)
- [<span data-ttu-id="75525-118">IXCLRDataMethodInstance 인터페이스</span><span class="sxs-lookup"><span data-stu-id="75525-118">IXCLRDataMethodInstance Interface</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdatamethodinstance-interface.md)