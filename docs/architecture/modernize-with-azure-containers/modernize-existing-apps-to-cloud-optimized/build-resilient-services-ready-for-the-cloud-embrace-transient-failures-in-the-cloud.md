---
title: '클라우드에 대한 복원력 있는 서비스 구축: 클라우드에서 일시적 오류 포용'
description: Azure 클라우드와 Windows 컨테이너를 사용하여 기존 .NET 응용 프로그램 최신화 | 클라우드에 대한 복원력 있는 서비스를 구축하십시오. 클라우드에서 일시적 오류 포용
ms.date: 04/30/2018
ms.openlocfilehash: 5f44029a214cf1f366fc787e27a9ac34599c4dca
ms.sourcegitcommit: c70542d02736e082e8dac67dad922c19249a8893
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70373965"
---
# <a name="build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud"></a><span data-ttu-id="59ec7-105">클라우드에 대한 복원력 있는 서비스 구축: 클라우드에서 일시적 오류 포용</span><span class="sxs-lookup"><span data-stu-id="59ec7-105">Build resilient services ready for the cloud: Embrace transient failures in the cloud</span></span>

<span data-ttu-id="59ec7-106">복원력은 오류를 복구하고 기능을 계속 동작시키는 능력입니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-106">Resiliency is the ability to recover from failures and continue to function.</span></span> <span data-ttu-id="59ec7-107">복원력은 오류를 방지하는 것이 아니라 오류가 발생할 수 있다는 사실을 받아들이고 이러한 오류에 대해 가동 중지 시간이나 데이터 손실을 피하는 방식으로 대응하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-107">Resiliency is not about avoiding failures, but accepting the fact that failures will occur, and then responding to them in a way that avoids downtime or data loss.</span></span> <span data-ttu-id="59ec7-108">복원력의 목표는 오류가 발생한 애플리케이션을 완벽하게 동작하는 상태로 되돌리는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-108">The goal of resiliency is to return the application to a fully functioning state after a failure.</span></span>

<span data-ttu-id="59ec7-109">최소한 하드웨어 기반 모델이 아닌 소프트웨어 기반의 복원력 모델을 구현한 경우, 응용 프로그램을 클라우드에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-109">Your application is ready for the cloud when, at a minimum, it implements a software-based model of resiliency, rather than a hardware-based model.</span></span> <span data-ttu-id="59ec7-110">클라우드 응용 프로그램에서는 반드시 발생하는 부분적인 오류를 수용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-110">Your cloud application must embrace the partial failures that will certainly occur.</span></span> <span data-ttu-id="59ec7-111">예상되는 부분적인 오류를 복원할 수 있도록 응용 프로그램을 부분적으로 리팩터링하거나 설계합니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-111">Design or partially refactor your application to achieve resiliency with expected partial failures.</span></span> <span data-ttu-id="59ec7-112">일시적인 네트워크 중단과 클라우드에서 노드나 VM간의 충돌과 같은 부분 오류에 대처할 수 있도록 설계되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-112">It should be designed to cope with partial failures, like transient network outages and nodes, or VMs crashing in the cloud.</span></span> <span data-ttu-id="59ec7-113">또한 오케스트레이터 클러스터 내의 다른 노드로 컨테이너를 옮겨도 응용 프로그램에서 간헐적인 단기 오류가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-113">Even containers being moved to a different node within an orchestrator cluster can cause intermittent short failures within the application.</span></span>

## <a name="handling-partial-failure"></a><span data-ttu-id="59ec7-114">부분적인 오류 처리</span><span class="sxs-lookup"><span data-stu-id="59ec7-114">Handling partial failure</span></span>

<span data-ttu-id="59ec7-115">클라우드 기반 응용 프로그램에서는 부분적인 오류가 발생할 위험성이 항상 존재합니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-115">In a cloud-based application, there's an ever-present risk of partial failure.</span></span> <span data-ttu-id="59ec7-116">예를 들어 단일 웹 사이트 인스턴스나 컨테이너에서 오류가 발생하거나, 사용할 수 없게 되거나, 짧은 시간 동안 응답하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-116">For instance, a single website instance or a container might fail, or it might be unavailable or unresponsive for a short time.</span></span> <span data-ttu-id="59ec7-117">또는 단일 VM이나 서버가 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-117">Or, a single VM or server might crash.</span></span>

<span data-ttu-id="59ec7-118">클라이언트나 서비스는 별도의 프로세스이기 때문에 서비스 클라이언트의 요청에 시기적절하게 응답하지 못할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-118">Because clients and services are separate processes, a service might not be able to respond in a timely manner to a client's request.</span></span> <span data-ttu-id="59ec7-119">서비스가 너무 많은 요청으로 느리게 응답하거나 네트워크 문제로 인해 짧은 시간 동안 응답하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-119">The service might be overloaded and respond slowly to requests, or it might not be accessible for a short time because of network issues.</span></span>

<span data-ttu-id="59ec7-120">Azure SQL Database에서 데이터베이스에 액세스하는 모놀리식 .NET 응용 프로그램을 예로 들어 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-120">For example, consider a monolithic .NET application that accesses a database in Azure SQL Database.</span></span> <span data-ttu-id="59ec7-121">Azure SQL database나 그 밖의 타사 서비스가 응답하지 않는 경우(Azure SQL database에서는 다른 노드나 서버로 이동하는 경우 수 초 동안 응답하지 않을 수 있습니다), 사용자가 어떤 동작을 수행하려고 한다면 해당 응용 프로그램은 작동이 중지되고 동시에 예외를 발생시킵니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-121">If the Azure SQL database or any other third-party service is unresponsive for a brief time (an Azure SQL database might be moved to a different node or server, and be unresponsive for a few seconds), when the user tries to do any action, the application might crash and show an exception at the same moment.</span></span>

<span data-ttu-id="59ec7-122">HTTP 서비스를 사용 하는 앱에서 유사한 시나리오가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-122">A similar scenario might occur in an app that consumes HTTP services.</span></span> <span data-ttu-id="59ec7-123">단기 일시적 오류 중에는 클라우드에서 네트워크 또는 서비스 자체를 사용 하지 못할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-123">The network or the service itself might not be available in the cloud during a short, transient failure.</span></span>

<span data-ttu-id="59ec7-124">그림 4-9에 표시 된 것과 같은 탄력적 응용 프로그램은 응용 프로그램에 리소스의 일시적인 오류를 처리할 수 있는 기회를 제공 하기 위해 "지 수 백오프를 통해 다시 시도"와 같은 기술을 구현 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-124">A resilient application like the one shown in Figure 4-9 should implement techniques like "retries with exponential backoff" to give the application an opportunity to handle transient failures in resources.</span></span> <span data-ttu-id="59ec7-125">또한 응용 프로그램에서 "회로 차단기"를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-125">You also should use "circuit breakers" in your applications.</span></span> <span data-ttu-id="59ec7-126">회로 차단기는 실제로 장기 오류일 때 응용 프로그램이 리소스에 액세스 하는 것을 중지 합니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-126">A circuit breaker stops an application from trying to access a resource when it's actually a long-term failure.</span></span> <span data-ttu-id="59ec7-127">응용 프로그램은 회로 차단기를 사용 하 여 서비스 거부 흥미로운 방지 합니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-127">By using a circuit breaker, the application avoids provoking a denial of service to itself.</span></span>

![지 수 백오프를 사용 하 여 재시도로 처리 되는 부분 오류](./media/image9.png)

<span data-ttu-id="59ec7-129">**그림 4-9.**</span><span class="sxs-lookup"><span data-stu-id="59ec7-129">**Figure 4-9.**</span></span> <span data-ttu-id="59ec7-130">지 수 백오프를 사용 하 여 재시도로 처리 되는 부분 오류</span><span class="sxs-lookup"><span data-stu-id="59ec7-130">Partial failures handled by retries with exponential backoff</span></span>

<span data-ttu-id="59ec7-131">HTTP 리소스와 데이터베이스 리소스에서 이러한 기술을 모두 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-131">You can use these techniques both in HTTP resources and in database resources.</span></span> <span data-ttu-id="59ec7-132">그림 4-9에서 응용 프로그램은 3 계층 아키텍처를 기반으로 하므로 서비스 수준 (HTTP) 및 데이터 계층 수준 (TCP)에서 이러한 기술이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-132">In Figure 4-9, the application is based on a 3-tier architecture, so you need these techniques at the services level (HTTP) and at the data tier level (TCP).</span></span> <span data-ttu-id="59ec7-133">데이터베이스 외에도 단일 앱 계층만 사용 하는 모놀리식 응용 프로그램 (추가 서비스 또는 마이크로 서비스 없음)에서 데이터베이스 연결 수준에서 일시적인 오류를 처리 하는 것으로 충분할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-133">In a monolithic application that uses only a single app tier in addition to the database (no additional services or microservices), handling transient failures at the database connection level might be enough.</span></span> <span data-ttu-id="59ec7-134">이 시나리오에서는 데이터베이스 연결의 특정 구성만 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-134">In that scenario, just a particular configuration of the database connection is required.</span></span>

<span data-ttu-id="59ec7-135">사용 중인 .NET 버전에 따라 데이터베이스에 액세스 하는 복원 력 있는 통신을 구현 하는 경우, 예를 들어 [Entity Framework 6 이상인](/ef/ef6/fundamentals/connection-resiliency/retry-logic)경우에는 간단 하 게 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-135">When implementing resilient communications that access the database, depending on the version of .NET you are using, it can be straightforward (for example, [with Entity Framework 6 or later](/ef/ef6/fundamentals/connection-resiliency/retry-logic).</span></span> <span data-ttu-id="59ec7-136">데이터베이스 연결을 구성 하는 것이 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-136">It's just a matter of configuring the database connection).</span></span> <span data-ttu-id="59ec7-137">또는 [임시 오류 처리 응용 프로그램 블록](https://docs.microsoft.com/previous-versions/msp-n-p/hh680934(v=pandp.50)) (이전 버전의 .net의 경우)과 같은 추가 라이브러리를 사용 하거나 사용자 고유의 라이브러리를 구현 해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-137">Or, you might need to use additional libraries like the [Transient Fault Handling Application Block](https://docs.microsoft.com/previous-versions/msp-n-p/hh680934(v=pandp.50)) (for earlier versions of .NET), or even implement your own library.</span></span>

<span data-ttu-id="59ec7-138">HTTP 재시도 및 회로 차단기를 구현할 때 .NET의 권장 사항은 .NET Core 지원을 포함 하는 .NET Framework 4.0, .NET Framework 4.5 및 .NET Standard 1.1를 대상으로 하는 [기능 라이브러리를](https://github.com/App-vNext/Polly) 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="59ec7-138">When implementing HTTP retries and circuit breakers, the recommendation for .NET is to use the [Polly](https://github.com/App-vNext/Polly) library, which targets .NET Framework 4.0, .NET Framework 4.5, and .NET Standard 1.1, which includes .NET Core support.</span></span>

<span data-ttu-id="59ec7-139">클라우드의 부분 실패를 처리 하기 위한 전략을 구현 하는 방법을 알아보려면 다음 참조를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="59ec7-139">To learn how to implement strategies for handling partial failures in the cloud, see the following references.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="59ec7-140">추가 자료</span><span class="sxs-lookup"><span data-stu-id="59ec7-140">Additional resources</span></span>

- <span data-ttu-id="59ec7-141">**부분 오류를 처리 하기 위한 복원 력 통신 구현**</span><span class="sxs-lookup"><span data-stu-id="59ec7-141">**Implementing resilient communication to handle partial failure**</span></span>

    [https://docs.microsoft.com/dotnet/architecture/microservices/implement-resilient-applications/partial-failure-strategies](../../microservices/implement-resilient-applications/partial-failure-strategies.md)

- <span data-ttu-id="59ec7-142">**Entity Framework 연결 복원 력 및 재시도 논리 (버전 6 이상)**</span><span class="sxs-lookup"><span data-stu-id="59ec7-142">**Entity Framework connection resiliency and retry logic (version 6 and later)**</span></span>

    [https://docs.microsoft.com/ef/ef6/fundamentals/connection-resiliency/retry-logic](/ef/ef6/fundamentals/connection-resiliency/retry-logic)

- <span data-ttu-id="59ec7-143">**일시적인 오류 처리 응용 프로그램 블록**</span><span class="sxs-lookup"><span data-stu-id="59ec7-143">**The Transient Fault Handling Application Block**</span></span>

- <https://docs.microsoft.com/previous-versions/msp-n-p/hh680934(v=pandp.50)>

- <span data-ttu-id="59ec7-144">**복원 력 있는 HTTP 통신용 정책**</span><span class="sxs-lookup"><span data-stu-id="59ec7-144">**Polly library for resilient HTTP communication**</span></span>

    https://github.com/App-vNext/Polly

>[!div class="step-by-step"]
><span data-ttu-id="59ec7-145">[이전](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
>[다음](modernize-your-apps-with-monitoring-and-telemetry.md)</span><span class="sxs-lookup"><span data-stu-id="59ec7-145">[Previous](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
[Next](modernize-your-apps-with-monitoring-and-telemetry.md)</span></span>