---
title: IXCLRDataModule::Request 메서드
ms.date: 01/16/2019
api.name:
- IXCLRDataModule::Request Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataModule::Request Method
helpviewer.keywords:
- IXCLRDataModule::Request Method [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 0226a11b142515296d976e3d645cb2475d634420
ms.sourcegitcommit: b56d59ad42140d277f2acbd003b74d655fdbc9f1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2019
ms.locfileid: "54416588"
---
# <a name="ixclrdatamodulerequest-method"></a><span data-ttu-id="2ecdb-102">IXCLRDataModule::Request 메서드</span><span class="sxs-lookup"><span data-stu-id="2ecdb-102">IXCLRDataModule::Request Method</span></span>

<span data-ttu-id="2ecdb-103">모듈의 데이터를 사용 하 여 지정 된 버퍼를 채우는 데 요청 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ecdb-103">Requests to populate the buffer given with the module's data.</span></span>

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a><span data-ttu-id="2ecdb-104">구문</span><span class="sxs-lookup"><span data-stu-id="2ecdb-104">Syntax</span></span>
```
HRESULT Request([in] ULONG32 reqCode,
    [in] ULONG32 inBufferSize,
    [in, size_is(inBufferSize)] BYTE* inBuffer,
    [in] ULONG32 outBufferSize,
    [out, size_is(outBufferSize)] BYTE* outBuffer);
```

### <a name="parameters"></a><span data-ttu-id="2ecdb-105">매개 변수</span><span class="sxs-lookup"><span data-stu-id="2ecdb-105">Parameters</span></span>

<span data-ttu-id="2ecdb-106">`reqCode` [in] 요청 전송 하는 유형입니다.</span><span class="sxs-lookup"><span data-stu-id="2ecdb-106">`reqCode` [in] Request type to be sent.</span></span>

<span data-ttu-id="2ecdb-107">`inBufferSize` [in]에 전달 되어야 하는 입력된 버퍼의 크기입니다.</span><span class="sxs-lookup"><span data-stu-id="2ecdb-107">`inBufferSize` [in] size of the input buffer to be passed in.</span></span>

<span data-ttu-id="2ecdb-108">`inBuffer` [in, size_is(inBufferSize)] 요청에서 보낼 원시 데이터에 대 한 버퍼 포인터입니다.</span><span class="sxs-lookup"><span data-stu-id="2ecdb-108">`inBuffer` [in, size_is(inBufferSize)] Buffer pointer for the raw data to be sent in the request.</span></span>

<span data-ttu-id="2ecdb-109">`outBufferSize` [in] 출력 버퍼의 크기입니다.</span><span class="sxs-lookup"><span data-stu-id="2ecdb-109">`outBufferSize` [in] Size of the output buffer.</span></span>

<span data-ttu-id="2ecdb-110">`outBuffer` [out, size_is(outBufferSize)] 요청 응답을 저장 하는 데 버퍼 포인터입니다.</span><span class="sxs-lookup"><span data-stu-id="2ecdb-110">`outBuffer` [out, size_is(outBufferSize)] Buffer pointer to used to store the request response.</span></span>

## <a name="remarks"></a><span data-ttu-id="2ecdb-111">설명</span><span class="sxs-lookup"><span data-stu-id="2ecdb-111">Remarks</span></span>

<span data-ttu-id="2ecdb-112">제공 된 메서드는의 일부는 `IXCLRDataModule` 인터페이스 및 가상 메서드 테이블의 36th 슬롯에 해당 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ecdb-112">The provided method is part of the `IXCLRDataModule` interface and corresponds to the 36th slot of the virtual method table.</span></span>

## <a name="requirements"></a><span data-ttu-id="2ecdb-113">요구 사항</span><span class="sxs-lookup"><span data-stu-id="2ecdb-113">Requirements</span></span>

<span data-ttu-id="2ecdb-114">**플랫폼:** [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="2ecdb-114">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
<span data-ttu-id="2ecdb-115">**헤더:** 없음</span><span class="sxs-lookup"><span data-stu-id="2ecdb-115">**Header:** None</span></span>   
<span data-ttu-id="2ecdb-116">**라이브러리:** 없음</span><span class="sxs-lookup"><span data-stu-id="2ecdb-116">**Library:** None</span></span>  
<span data-ttu-id="2ecdb-117">**.NET Framework 버전:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span><span class="sxs-lookup"><span data-stu-id="2ecdb-117">**.NET Framework Versions:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span></span>  

## <a name="see-also"></a><span data-ttu-id="2ecdb-118">참고 항목</span><span class="sxs-lookup"><span data-stu-id="2ecdb-118">See Also</span></span>
- [<span data-ttu-id="2ecdb-119">디버깅</span><span class="sxs-lookup"><span data-stu-id="2ecdb-119">Debugging</span></span>](../../../../docs/framework/unmanaged-api/debugging/index.md)
- [<span data-ttu-id="2ecdb-120">IXCLRDataModule 인터페이스</span><span class="sxs-lookup"><span data-stu-id="2ecdb-120">IXCLRDataModule Interface</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdatamodule-interface.md)