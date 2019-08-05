---
title: 도메인 이벤트. 디자인 및 구현
description: 컨테이너화된 .NET 애플리케이션용 .NET 마이크로 서비스 아키텍처 | 집계 간에 통신을 설정하는 주요 개념인 도메인 이벤트의 세부적인 보기를 가져옵니다.
ms.date: 10/08/2018
ms.openlocfilehash: 5f7084ef638a1d04e0050eab447cb8903c973f45
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68674260"
---
# <a name="domain-events-design-and-implementation"></a><span data-ttu-id="51004-104">도메인 이벤트: 디자인 및 구현</span><span class="sxs-lookup"><span data-stu-id="51004-104">Domain events: design and implementation</span></span>

<span data-ttu-id="51004-105">도메인 이벤트를 사용하여 도메인 내 변경의 파생 작업을 명시적으로 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-105">Use domain events to explicitly implement side effects of changes within your domain.</span></span> <span data-ttu-id="51004-106">다시 말해, DDD 용어를 사용하여, 도메인 이벤트를 사용하여 여러 집합체 전반에 파생 작업을 명시적으로 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-106">In other words, and using DDD terminology, use domain events to explicitly implement side effects across multiple aggregates.</span></span> <span data-ttu-id="51004-107">선택적으로, 데이터베이스 잠금의 확장성을 높이고 영향을 줄이려면 동일한 도메인 내의 집합체 간에 최종 일관성을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-107">Optionally, for better scalability and less impact in database locks, use eventual consistency between aggregates within the same domain.</span></span>

## <a name="what-is-a-domain-event"></a><span data-ttu-id="51004-108">도메인 이벤트란?</span><span class="sxs-lookup"><span data-stu-id="51004-108">What is a domain event?</span></span>

<span data-ttu-id="51004-109">이벤트는 과거에 발생한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-109">An event is something that has happened in the past.</span></span> <span data-ttu-id="51004-110">도메인 이벤트란 동일한 도메인의 다른 부분(프로세스 중)을 인식하고자 하는 도메인에서 발생한 이벤트입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-110">A domain event is, something that happened in the domain that you want other parts of the same domain (in-process) to be aware of.</span></span> <span data-ttu-id="51004-111">알림된 부분은 일반적으로 어떤 식으로든 이벤트에 대응합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-111">The notified parts usually react somehow to the events.</span></span>

<span data-ttu-id="51004-112">도메인 이벤트의 중요한 이점은 부작용이 명시적으로 표현될 수 있다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-112">An important benefit of domain events is that side effects can be expressed explicitly.</span></span>

<span data-ttu-id="51004-113">예를 들어, Entity Framework를 사용하고 있고 일부 이벤트에 대한 반응이 있어야만 하는 경우에 이벤트를 트리거하는 것과 가까운 곳에 코딩할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-113">For example, if you're just using Entity Framework and there has to be a reaction to some event, you would probably code whatever you need close to what triggers the event.</span></span> <span data-ttu-id="51004-114">따라서 규칙은 암시적으로 코드에 결합되고, 다행히 코드를 조사해서 규칙을 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-114">So the rule gets coupled, implicitly, to the code, and you have to look into the code to, hopefully, realize the rule is implemented there.</span></span>

<span data-ttu-id="51004-115">반면에 `DomainEvent`와 적어도 하나 이상의 `DomainEventHandler`가 포함되어 있기 때문에 도메인 이벤트를 사용하면 개념을 명시적으로 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="51004-115">On the other hand, using domain events makes the concept explicit, because there is a `DomainEvent` and at least one `DomainEventHandler` involved.</span></span>

<span data-ttu-id="51004-116">예를 들어 eShopOnContainers 애플리케이션에서 주문이 생성되면 사용자가 구매자가 되므로 `ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler`에서 `OrderStartedDomainEvent`가 발생하고 처리되므로 기본 개념이 명확해집니다.</span><span class="sxs-lookup"><span data-stu-id="51004-116">For example, in the eShopOnContainers application, when an order is created, the user becomes a buyer, so an `OrderStartedDomainEvent` is raised and handled in the `ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler`, so the underlying concept is evident.</span></span>

<span data-ttu-id="51004-117">즉, 도메인 이벤트를 통해 도메인 전문가가 제공하는 유비쿼터스 언어에 기반한 도메인 규칙을 명시적으로 표현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-117">In short, domain events help you to express, explicitly, the domain rules, based in the ubiquitous language provided by the domain experts.</span></span> <span data-ttu-id="51004-118">또한 도메인 이벤트를 사용하면 동일한 도메인 내의 클래스 간에 문제를 더 효과적으로 분리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-118">Domain events also enable a better separation of concerns among classes within the same domain.</span></span>

<span data-ttu-id="51004-119">데이터베이스 트랜잭션과 마찬가지로 도메인 이벤트와 관련된 모든 작업이 성공적으로 완료되거나 수행되지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-119">It's important to ensure that, just like a database transaction, either all the operations related to a domain event finish successfully or none of them do.</span></span>

<span data-ttu-id="51004-120">도메인 이벤트는 메시징 스타일 이벤트와 유사하지만 한 가지 중요한 차이점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-120">Domain events are similar to messaging-style events, with one important difference.</span></span> <span data-ttu-id="51004-121">실제 메시지, 메시지 큐, 메시지 broker 또는 AMQP를 사용하는 서비스 버스에서, 메시지는 항상 비동기식으로 전송되고 프로세스와 머신에서 통신됩니다.</span><span class="sxs-lookup"><span data-stu-id="51004-121">With real messaging, message queuing, message brokers, or a service bus using AMQP, a message is always sent asynchronously and communicated across processes and machines.</span></span> <span data-ttu-id="51004-122">이것은 다수의 바인딩된 컨텍스트, 마이크로 서비스 또는 다른 애플리케이션을 통합하는 데 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-122">This is useful for integrating multiple Bounded Contexts, microservices, or even different applications.</span></span> <span data-ttu-id="51004-123">하지만 도메인 이벤트의 경우 현재 실행 중인 도메인 작업에서 이벤트를 발생시키려고 하지만 파생 작업은 동일한 도메인 내에서 발생하기를 바랍니다.</span><span class="sxs-lookup"><span data-stu-id="51004-123">However, with domain events, you want to raise an event from the domain operation you are currently running, but you want any side effects to occur within the same domain.</span></span>

<span data-ttu-id="51004-124">도메인 이벤트와 그 파생 작업(이벤트 처리기가 관리하는 이후에 트리거되는 동작)은 거의 즉시, 일반적으로 프로세스 내에서 그리고 동일한 도메인 내에서 발생해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-124">The domain events and their side effects (the actions triggered afterwards that are managed by event handlers) should occur almost immediately, usually in-process, and within the same domain.</span></span> <span data-ttu-id="51004-125">따라서 도메인 이벤트는 동기 또는 비동기일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-125">Thus, domain events could be synchronous or asynchronous.</span></span> <span data-ttu-id="51004-126">단, 통합 이벤트는 항상 비동기여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-126">Integration events, however, should always be asynchronous.</span></span>

## <a name="domain-events-versus-integration-events"></a><span data-ttu-id="51004-127">도메인 이벤트와 통합 이벤트</span><span class="sxs-lookup"><span data-stu-id="51004-127">Domain events versus integration events</span></span>

<span data-ttu-id="51004-128">도메인 및 통합 이벤트는 의미상으로 동일합니다. 즉, 방금 발생한 일에 대한 알림입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-128">Semantically, domain and integration events are the same thing: notifications about something that just happened.</span></span> <span data-ttu-id="51004-129">하지만 구현은 달라야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-129">However, their implementation must be different.</span></span> <span data-ttu-id="51004-130">도메인 이벤트는 IoC 컨테이너 또는 다른 메서드를 기반으로 메모리 내 중재자로 구현될 수 있는 도메인 이벤트 디스패처에 푸시되는 메시지입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-130">Domain events are just messages pushed to a domain event dispatcher, which could be implemented as an in-memory mediator based on an IoC container or any other method.</span></span>

<span data-ttu-id="51004-131">반면에 통합 이벤트의 목적은 커밋된 트랜잭션 및 업데이트를 다른 마이크로 서비스, 바인딩된 컨텍스트 또는 외부 애플리케이션과 같은 추가적인 하위 시스템에 전파하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-131">On the other hand, the purpose of integration events is to propagate committed transactions and updates to additional subsystems, whether they are other microservices, Bounded Contexts or even external applications.</span></span> <span data-ttu-id="51004-132">따라서 엔터티가 성공적으로 지속되고 있는 경우에만 발생해야 하며, 그렇지 않으면 전체 작업이 발생하지 않은 것처럼 처리됩니다.</span><span class="sxs-lookup"><span data-stu-id="51004-132">Hence, they should occur only if the entity is successfully persisted, otherwise it's as if the entire operation never happened.</span></span>

<span data-ttu-id="51004-133">앞서 언급했듯이 통합 이벤트는 여러 마이크로 서비스(기타 바인딩된 컨텍스트) 또는 외부 시스템/애플리케이션 간에도 비동기 통신을 기반으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-133">As mentioned before, integration events must be based on asynchronous communication between multiple microservices (other Bounded Contexts) or even external systems/applications.</span></span>

<span data-ttu-id="51004-134">따라서 이벤트 버스 인터페이스에는 잠재적인 원격 서비스 간에 분산된 프로세스 간 통신을 허용하는 인프라가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-134">Thus, the event bus interface needs some infrastructure that allows inter-process and distributed communication between potentially remote services.</span></span> <span data-ttu-id="51004-135">상용 서비스 버스, 큐, 사서함으로 사용되는 공유된 데이터베이스 또는 그 밖의 분산된 푸시 기반 메시지 시스템을 기반으로 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-135">It can be based on a commercial service bus, queues, a shared database used as a mailbox, or any other distributed and ideally push based messaging system.</span></span>

## <a name="domain-events-as-a-preferred-way-to-trigger-side-effects-across-multiple-aggregates-within-the-same-domain"></a><span data-ttu-id="51004-136">동일한 도메인 내의 여러 집합체 전반에서 파생 작업을 트리거하는 기본 방법인 도메인 이벤트</span><span class="sxs-lookup"><span data-stu-id="51004-136">Domain events as a preferred way to trigger side effects across multiple aggregates within the same domain</span></span>

<span data-ttu-id="51004-137">하나의 집계 인스턴스와 관련된 명령을 실행하기 위해 하나 이상의 추가 집합체에서 추가 도메인 규칙을 실행해야 하는 경우에는 해당 파생 작업이 도메인 이벤트에 의해 트리거되도록 설계하고 구현해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-137">If executing a command related to one aggregate instance requires additional domain rules to be run on one or more additional aggregates, you should design and implement those side effects to be triggered by domain events.</span></span> <span data-ttu-id="51004-138">그림 7-14와 같이 가장 중요한 사용 사례 중 하나인 도메인 이벤트를 사용하여 동일한 도메인 모델 내의 여러 집계에 상태 변경 사항을 전파해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-138">As shown in Figure 7-14, and as one of the most important use cases, a domain event should be used to propagate state changes across multiple aggregates within the same domain model.</span></span>

![<span data-ttu-id="51004-139">집계 간의 일관성은 도메인 이벤트에 의해 달성되고, 주문 집계에서는 구매자 집계 업데이트로 처리되는 OrderStarted 도메인 이벤트를 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="51004-139">Consistency between aggregates is achieved by domain events, the Order Aggregate sends an OrderStarted domain event that's handled to update the Buyer Aggregate.</span></span> ](./media/image15.png)

<span data-ttu-id="51004-140">**그림 7-14**.</span><span class="sxs-lookup"><span data-stu-id="51004-140">**Figure 7-14**.</span></span> <span data-ttu-id="51004-141">동일한 도메인 내의 여러 집합체 간에 일관성을 유지하기 위한 도메인 이벤트</span><span class="sxs-lookup"><span data-stu-id="51004-141">Domain events to enforce consistency between multiple aggregates within the same domain</span></span>

<span data-ttu-id="51004-142">그림에서 사용자가 주문을 시작하면 OrderStarted 도메인 이벤트는 ID 마이크로 서비스의 원래 사용자 정보를 기반으로(CreateOrder 명령에 제공된 정보를 사용하여) Ordering(주문) 마이크로 서비스에서 Buyer 개체 생성을 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-142">In the figure, when the user initiates an order, the OrderStarted domain event triggers creation of a Buyer object in the ordering microservice, based on the original user info from the identity microservice (with information provided in the CreateOrder command).</span></span> <span data-ttu-id="51004-143">도메인 이벤트는 주문 집계가 처음 생성될 때 주문 집계에 의해 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="51004-143">The domain event is generated by the order aggregate when it is created in the first place.</span></span>

<span data-ttu-id="51004-144">또는 집계 루트를 집합체의 멤버(자식 엔터티)가 생성한 이벤트에 가입시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-144">Alternately, you can have the aggregate root subscribed for events raised by members of its aggregates (child entities).</span></span> <span data-ttu-id="51004-145">예를 들어 각 OrderItem 자식 엔터티는 항목 가격이 특정 금액보다 높거나 제품 항목 가격이 너무 많으면 이벤트를 발생시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-145">For instance, each OrderItem child entity can raise an event when the item price is higher than a specific amount, or when the product item amount is too high.</span></span> <span data-ttu-id="51004-146">그런 다음, 집계 루트가 해당 이벤트를 수신하여 전역 계산 또는 집계를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-146">The aggregate root can then receive those events and perform a global calculation or aggregation.</span></span>

<span data-ttu-id="51004-147">이런 이벤트 기반 통신은 집합체 내에서 직접 수행되지 않는 다는 점을 이해하는 것이 중요합니다. 도메인 이벤트 처리기를 구현해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-147">It is important to understand that this event-based communication is not implemented directly within the aggregates; you need to implement domain event handlers.</span></span>

<span data-ttu-id="51004-148">도메인 이벤트 처리는 애플리케이션 관련 문제입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-148">Handling the domain events is an application concern.</span></span> <span data-ttu-id="51004-149">도메인 모델 계층은 도메인 논리(도메인 전문가가 이해하는 논리)에만 집중해야 하며 리포지토리를 이용한 파생 작업 지속 동작 및 처리기와 같은 애플리케이션 인프라는 신경 쓸 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-149">The domain model layer should only focus on the domain logic—things that a domain expert would understand, not application infrastructure like handlers and side-effect persistence actions using repositories.</span></span> <span data-ttu-id="51004-150">따라서 애플리케이션 계층 수준은 도메인 이벤트가 발생할 때 동작을 트리거하는 도메인 이벤트 처리기가 있어야 하는 곳입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-150">Therefore, the application layer level is where you should have domain event handlers triggering actions when a domain event is raised.</span></span>

<span data-ttu-id="51004-151">도메인 이벤트는 다수의 애플리케이션 동작을 트리거하는 데에도 사용할 수 있으며 이보다 중요한 점은 향우 분리된 방식으로 이 숫자를 늘리는 것이 가능해야 한다는 점입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-151">Domain events can also be used to trigger any number of application actions, and what is more important, must be open to increase that number in the future in a decoupled way.</span></span> <span data-ttu-id="51004-152">예를 들어 주문이 시작되면 도메인 이벤트를 게시하여 해당 정보를 다른 집합체에 전파하거나 알림과 같은 애플리케이션 동작을 발생시킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-152">For instance, when the order is started, you might want to publish a domain event to propagate that info to other aggregates or even to raise application actions like notifications.</span></span>

<span data-ttu-id="51004-153">여기서 요점은 도메인 이벤트가 발생할 때 실행되는 동작의 수가 변화할 수 있다는 점입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-153">The key point is the open number of actions to be executed when a domain event occurs.</span></span> <span data-ttu-id="51004-154">궁극적으로 도메인과 애플리케이션의 작업 및 규칙 수는 증가합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-154">Eventually, the actions and rules in the domain and application will grow.</span></span> <span data-ttu-id="51004-155">어떤 일이 일어날 때 발생하는 파생 작업의 복잡성과 수는 증가하지만, 코드가 "접착제"(즉, `new`를 사용하여 특정 개체를 만드는 것)와 결합된 경우 새 작업을 추가해야 할 때마다 작업 및 테스트 코드도 변경해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-155">The complexity or number of side-effect actions when something happens will grow, but if your code were coupled with “glue” (that is, creating specific objects with `new`), then every time you needed to add a new action you would also need to change working and tested code.</span></span>

<span data-ttu-id="51004-156">이 변경으로 인해 새 버그가 발생할 수 있으며, 이 방식은 [SOLID](https://en.wikipedia.org/wiki/SOLID)의 [개방/폐쇄 원칙](https://en.wikipedia.org/wiki/Open/closed_principle)에도 위배됩니다.</span><span class="sxs-lookup"><span data-stu-id="51004-156">This change could result in new bugs and this approach also goes against the [Open/Closed principle](https://en.wikipedia.org/wiki/Open/closed_principle) from [SOLID](https://en.wikipedia.org/wiki/SOLID).</span></span> <span data-ttu-id="51004-157">뿐만 아니라, 작업을 오케스트레이션하는 원래 클래스가 계속 증가하게 되는데, 이것은 SRP([단일 책임 원칙](https://en.wikipedia.org/wiki/Single_responsibility_principle))에 위배됩니다.</span><span class="sxs-lookup"><span data-stu-id="51004-157">Not only that, the original class that was orchestrating the operations would grow and grow, which goes against the [Single Responsibility Principle (SRP)](https://en.wikipedia.org/wiki/Single_responsibility_principle).</span></span>

<span data-ttu-id="51004-158">반면에 도메인 이벤트를 사용하면 다음 방법을 사용하여 책임을 분리하여 세밀하고 분리된 구현을 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-158">On the other hand, if you use domain events, you can create a fine-grained and decoupled implementation by segregating responsibilities using this approach:</span></span>

1. <span data-ttu-id="51004-159">명령 보내기(예: CreateOrder).</span><span class="sxs-lookup"><span data-stu-id="51004-159">Send a command (for example, CreateOrder).</span></span>
2. <span data-ttu-id="51004-160">명령 처리기에서 명령 수신하기</span><span class="sxs-lookup"><span data-stu-id="51004-160">Receive the command in a command handler.</span></span>
   - <span data-ttu-id="51004-161">단일 집계의 트랜잭션 실행</span><span class="sxs-lookup"><span data-stu-id="51004-161">Execute a single aggregate’s transaction.</span></span>
   - <span data-ttu-id="51004-162">(선택 사항) 파생 작업에 대해 도메인 이벤트 발생시키기(예: OrderStartedDomainEvent)</span><span class="sxs-lookup"><span data-stu-id="51004-162">(Optional) Raise domain events for side effects (for example, OrderStartedDomainEvent).</span></span>
3. <span data-ttu-id="51004-163">여러 집합체나 애플리케이션 동작에서 다수의 파생 작업을 실행하는 도메인 이벤트(현재 프로세스 내) 처리</span><span class="sxs-lookup"><span data-stu-id="51004-163">Handle domain events (within the current process) that will execute an open number of side effects in multiple aggregates or application actions.</span></span> <span data-ttu-id="51004-164">예:</span><span class="sxs-lookup"><span data-stu-id="51004-164">For example:</span></span>
   - <span data-ttu-id="51004-165">buyer(구매자) 및 payment(지불) 메서드를 검증 또는 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-165">Verify or create buyer and payment method.</span></span>
   - <span data-ttu-id="51004-166">관련 통합 이벤트를 생성하고 이벤트 버스에 보내서 마이크로 서비스에 상태를 전파하거나 외부 동작(예: 구매자에게 이메일 보내기)을 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-166">Create and send a related integration event to the event bus to propagate states across microservices or trigger external actions like sending an email to the buyer.</span></span>
   - <span data-ttu-id="51004-167">다른 파생 작업을 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-167">Handle other side effects.</span></span>

<span data-ttu-id="51004-168">그림 7-15와 같이 동일한 도메인 이벤트에서 시작하여, 도메인의 다른 집계와 관련된 여러 작업 또는 통합 이벤트 및 이벤트 버스를 연결하는 마이크로 서비스 전반에 걸쳐 수행해야 하는 추가 애플리케이션 작업을 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-168">As shown in Figure 7-15, starting from the same domain event, you can handle multiple actions related to other aggregates in the domain or additional application actions you need to perform across microservices connecting with integration events and the event bus.</span></span>

![애플리케이션 계층에서 동일한 도메인 이벤트에 대한 여러 처리기가 있을 수 있으며, 하나의 처리기는 집계 간의 일관성을 해결할 수 있으며, 다른 처리기는 통합 이벤트를 게시할 수 있으므로 다른 마이크로 서비스와 함께 처리할 수 있습니다.](./media/image16.png)

<span data-ttu-id="51004-170">**그림 7-15**.</span><span class="sxs-lookup"><span data-stu-id="51004-170">**Figure 7-15**.</span></span> <span data-ttu-id="51004-171">도메인당 여러 동작 처리</span><span class="sxs-lookup"><span data-stu-id="51004-171">Handling multiple actions per domain</span></span>

<span data-ttu-id="51004-172">이벤트 처리기는 일반적으로 애플리케이션 계층에 있는데, 리포지토리나 애플리케이션 API와 같은 인프라 개체를 마이크로 서비스의 동작에 사용하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-172">The event handlers are typically in the application layer, because you will use infrastructure objects like repositories or an application API for the microservice’s behavior.</span></span> <span data-ttu-id="51004-173">이런 의미에서 이벤트 처리기는 명령 처리기와 유사하기 때문에 두 가지 모두 애플리케이션 계층의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-173">In that sense, event handlers are similar to command handlers, so both are part of the application layer.</span></span> <span data-ttu-id="51004-174">중요한 차이점은 명령을 한 번만 처리해야 한다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-174">The important difference is that a command should be processed only once.</span></span> <span data-ttu-id="51004-175">도메인 이벤트는 0번 또는 *n*번 처리될 수 있습니다. 각 처리기마다 다른 목적으로 여러 수신자 또는 이벤트 처리기에서 수신될 수 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-175">A domain event could be processed zero or *n* times, because it can be received by multiple receivers or event handlers with a different purpose for each handler.</span></span>

<span data-ttu-id="51004-176">도메인 이벤트당 처리기의 개수가 공개되어 있으면, 현재 코드에 영향을 주지 않고 필요에 따라 도메인 규칙을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-176">Having an open number of handlers per domain event allows you to add as many domain rules without as needed, without affecting  current code.</span></span> <span data-ttu-id="51004-177">예를 들어 다음과 같은 비즈니스 규칙을 구현하는 것은 몇 가지 이벤트 처리기를 추가(또는 하나만 추가)하는 것만큼 쉬울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-177">For instance, implementing the following business rule might be as easy as adding a few event handlers (or even just one):</span></span>

> <span data-ttu-id="51004-178">상점에서 고객이 구매한 총 금액(주문 수와 상관없이)이 6,000달러를 초과하는 경우, 모든 신규 주문에 10% 할인을 적용하고 향후 주문에 대한 할인을 고객에게 이메일로 알립니다.</span><span class="sxs-lookup"><span data-stu-id="51004-178">When the total amount purchased by a customer in the store, across any number of orders, exceeds $6,000, apply a 10% off discount to every new order and notify the customer with an email about that discount for future orders.</span></span>

## <a name="implement-domain-events"></a><span data-ttu-id="51004-179">도메인 이벤트 구현</span><span class="sxs-lookup"><span data-stu-id="51004-179">Implement domain events</span></span>

<span data-ttu-id="51004-180">C#에서 도메인 이벤트는 DTO와 같이 단지 데이터를 보유하는 구조체 또는 클래스이며, 도메인에서 방금 발생한 내용과 관련된 모든 정보가 다음 예제와 같이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="51004-180">In C#, a domain event is simply a data-holding structure or class, like a DTO, with all the information related to what just happened in the domain, as shown in the following example:</span></span>

```csharp
public class OrderStartedDomainEvent : INotification
{
    public string UserId { get; }
    public int CardTypeId { get; }
    public string CardNumber { get; }
    public string CardSecurityNumber { get; }
    public string CardHolderName { get; }
    public DateTime CardExpiration { get; }
    public Order Order { get; }

    public OrderStartedDomainEvent(Order order,
                                   int cardTypeId, string cardNumber,
                                   string cardSecurityNumber, string cardHolderName,
                                   DateTime cardExpiration)
    {
        Order = order;
        CardTypeId = cardTypeId;
        CardNumber = cardNumber;
        CardSecurityNumber = cardSecurityNumber;
        CardHolderName = cardHolderName;
        CardExpiration = cardExpiration;
    }
}
```

<span data-ttu-id="51004-181">이것은 본질적으로 OrderStarted 이벤트와 관련된 모든 데이터를 보유하는 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-181">This is essentially a class that holds all the data related to the OrderStarted event.</span></span>

<span data-ttu-id="51004-182">도메인의 유비쿼터스 언어 측면에서 볼 때, 이벤트는 과거에 발생한 것이기 때문에 이벤트의 클래스 이름은 OrderStartedDomainEvent 또는 OrderShippedDomainEvent와 같이 과거 시제 동사로 나타내야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-182">In terms of the ubiquitous language of the domain, since an event is something that happened in the past, the class name of the event should be represented as a past-tense verb, like OrderStartedDomainEvent or OrderShippedDomainEvent.</span></span> <span data-ttu-id="51004-183">이는 eShopOnContainers의 주문 마이크로 서비스에서 도메인 이벤트가 구현되는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-183">That's how the domain event is implemented in the ordering microservice in eShopOnContainers.</span></span>

<span data-ttu-id="51004-184">이전에 언급했듯이 이벤트의 중요한 특징은 이벤트는 과거에 발생한 것이므로 변경되지 않는다는 점입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-184">As noted earlier, an important characteristic of events is that since an event is something that happened in the past, it should not change.</span></span> <span data-ttu-id="51004-185">따라서 변경할 수 없는 클래스여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-185">Therefore, it must be an immutable class.</span></span> <span data-ttu-id="51004-186">이전 코드에서 속성이 읽기 전용임을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-186">You can see in the previous code that the properties are read-only.</span></span> <span data-ttu-id="51004-187">개체를 업데이트할 방법이 없으며, 개체를 만들 때만 값을 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-187">There's no way to update the object, you can only set values when you create it.</span></span>

<span data-ttu-id="51004-188">여기서 중요한 것은 이벤트 개체를 직렬화 및 역 직렬화해야 하는 큐를 사용하여 도메인 이벤트를 비동기적으로 처리해야 하는 경우, 속성은 읽기 전용 대신 "개인 설정" 이어야 하므로 deserializer는 큐에서 제거할 때 값을 할당할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-188">It’s important to highlight here that if domain events were to be handled asynchronously, using a queue that required serializing and deserializing the event objects, the properties would have to be “private set” instead of read-only, so the deserializer would be able to assign the values upon dequeuing.</span></span> <span data-ttu-id="51004-189">이는 도메인 이벤트 pub/sub가 MediatR을 사용하여 동기적으로 구현되므로 주문 마이크로 서비스에서는 문제가 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-189">This is not an issue in the Ordering microservice, as the domain event pub/sub is implemented synchronously using MediatR.</span></span>

### <a name="raise-domain-events"></a><span data-ttu-id="51004-190">도메인 이벤트 발생</span><span class="sxs-lookup"><span data-stu-id="51004-190">Raise domain events</span></span>

<span data-ttu-id="51004-191">다음 질문은 관련 이벤트 처리기에 도달하도록 도메인 이벤트를 발생시키는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-191">The next question is how to raise a domain event so it reaches its related event handlers.</span></span> <span data-ttu-id="51004-192">여러 가지 방법을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-192">You can use multiple approaches.</span></span>

<span data-ttu-id="51004-193">Udi Dahan은 이벤트를 관리하고 발생시키기 위해 정적 클래스를 사용하도록 원래 제안했습니다(예: [Domain Events – Take 2](http://udidahan.com/2008/08/25/domain-events-take-2/)(도메인 이벤트 – 테이크 2)와 같은 몇 가지 관련 게시물).</span><span class="sxs-lookup"><span data-stu-id="51004-193">Udi Dahan originally proposed (for example, in several related posts, such as [Domain Events – Take 2](http://udidahan.com/2008/08/25/domain-events-take-2/)) using a static class for managing and raising the events.</span></span> <span data-ttu-id="51004-194">여기에는 `DomainEvents.Raise(Event myEvent)`와 같은 구문을 사용하여, 도메인 이벤트가 호출될 때 즉시 발생시키는 DomainEvents라는 정적 클래스가 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-194">This might include a static class named DomainEvents that would raise domain events immediately when it is called, using syntax like `DomainEvents.Raise(Event myEvent)`.</span></span> <span data-ttu-id="51004-195">Jimmy Bogard는 유사한 방식을 권장하는 블로그 게시물인 [Strengthening your domain: Domain Events](https://lostechies.com/jimmybogard/2010/04/08/strengthening-your-domain-domain-events/)(도메인: 강화: 도메인 이벤트)를 썼습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-195">Jimmy Bogard wrote a blog post ([Strengthening your domain: Domain Events](https://lostechies.com/jimmybogard/2010/04/08/strengthening-your-domain-domain-events/)) that recommends a similar approach.</span></span>

<span data-ttu-id="51004-196">하지만 도메인 이벤트 클래스가 정적인 경우 처리기에 즉시 디스패치됩니다.</span><span class="sxs-lookup"><span data-stu-id="51004-196">However, when the domain events class is static, it also dispatches to handlers immediately.</span></span> <span data-ttu-id="51004-197">이로 인해 테스트와 디버깅이 더 어려워지며, 이것은 파생 작업 논리가 있는 이벤트 처리기가 이벤트가 발생한 직후에 실행되기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-197">This makes testing and debugging more difficult, because the event handlers with side-effects logic are executed immediately after the event is raised.</span></span> <span data-ttu-id="51004-198">테스트 및 디버깅을 수행하는 경우에는 현재 집계 클래스 및 여기서 벌어지는 일에만 집중하는 것이 좋습니다. 다른 집합체나 애플리케이션 논리와 관련된 파생 작업에 대한 다른 이벤트 처리기로 갑자기 리디렉션되는 것은 좋지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-198">When you are testing and debugging, you want to focus on and just what is happening in the current aggregate classes; you do not want to suddenly be redirected to other event handlers for side effects related to other aggregates or application logic.</span></span> <span data-ttu-id="51004-199">이런 이유 때문에 다른 방식이 진화하였으며 이 내용은 다음 섹션에 설명되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-199">This is why other approaches have evolved, as explained in the next section.</span></span>

#### <a name="the-deferred-approach-to-raise-and-dispatch-events"></a><span data-ttu-id="51004-200">발생 및 디스패치 이벤트에 대한 지연된 접근 방법</span><span class="sxs-lookup"><span data-stu-id="51004-200">The deferred approach to raise and dispatch events</span></span>

<span data-ttu-id="51004-201">도메인 이벤트 처리기에 즉시 디스패치하는 것보다 좋은 방법은 도메인 이벤트를 컬렉션에 추가한 다음, 트랜잭션을 커밋하기 *직전*이나 *바로* *후*에 도메인 이벤트를 디스패치하는 것입니다(EF의 SaveChanges와 같이).</span><span class="sxs-lookup"><span data-stu-id="51004-201">Instead of dispatching to a domain event handler immediately, a better approach is to add the domain events to a collection and then to dispatch those domain events *right before* or *right* *after* committing the transaction (as with SaveChanges in EF).</span></span> <span data-ttu-id="51004-202">(이 방식은 Jimmy Bogard의 게시물인 [A better domain events pattern](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/)(더 나은 도메인 이벤트 패턴)에 설명되어 있습니다.)</span><span class="sxs-lookup"><span data-stu-id="51004-202">(This approach was described by Jimmy Bogard in this post [A better domain events pattern](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/).)</span></span>

<span data-ttu-id="51004-203">도메인 이벤트를 트랜잭션을 커밋하기 직전에 보낼지 또는 직후에 보낼지를 결정하는 것은 중요합니다. 그에 따라 파생 작업을 동일한 이벤트의 일부로 포함시킬 지 또는 다른 트랙잭션에 포함시킬 지가 결정되기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-203">Deciding if you send the domain events right before or right after committing the transaction is important, since it determines whether you will include the side effects as part of the same transaction or in different transactions.</span></span> <span data-ttu-id="51004-204">후자의 경우 여러 집합체 전반에서 최종 일관성을 처리해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-204">In the latter case, you need to deal with eventual consistency across multiple aggregates.</span></span> <span data-ttu-id="51004-205">이 토픽은 다음 섹션에 설명되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-205">This topic is discussed in the next section.</span></span>

<span data-ttu-id="51004-206">지연된 방식은 eShopOnContainers에 사용되는 방식입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-206">The deferred approach is what eShopOnContainers uses.</span></span> <span data-ttu-id="51004-207">우선 엔터티에서 발생하는 이벤트를 엔터티당 이벤트 목록 또는 컬렉션에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-207">First, you add the events happening in your entities into a collection or list of events per entity.</span></span> <span data-ttu-id="51004-208">이 목록은 다음 Entity 기준 클래스 예제와 같이 엔터티 개체의 일부이거나 더 좋게는 기준 엔터티 클래스의 일부여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-208">That list should be part of the entity object, or even better, part of your base entity class, as shown in the following example of the Entity base class:</span></span>

```csharp
public abstract class Entity
{
     //... 
     private List<INotification> _domainEvents;
     public List<INotification> DomainEvents => _domainEvents; 

     public void AddDomainEvent(INotification eventItem)
     {
         _domainEvents = _domainEvents ?? new List<INotification>();
         _domainEvents.Add(eventItem);
     }

     public void RemoveDomainEvent(INotification eventItem)
     {
         _domainEvents?.Remove(eventItem);
     }
     //... Additional code
}
```

<span data-ttu-id="51004-209">이벤트를 발생시키려면 aggregate-root 엔터티의 메서드에 있는 코드에서 이벤트 컬렉션에 이벤트를 추가하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="51004-209">When you want to raise an event, you just add it to the event collection from code at any method of the aggregate-root entity.</span></span>

<span data-ttu-id="51004-210">다음 코드는 [eShopOnContainers의 Order agregate-root](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/AggregatesModel/OrderAggregate/Order.cs)의 일부이며 예제를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="51004-210">The following code, part of the [Order aggregate-root at eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/AggregatesModel/OrderAggregate/Order.cs), shows an example:</span></span>

```csharp
var orderStartedDomainEvent = new OrderStartedDomainEvent(this, //Order object
                                                          cardTypeId, cardNumber,
                                                          cardSecurityNumber,
                                                          cardHolderName,
                                                          cardExpiration);
this.AddDomainEvent(orderStartedDomainEvent);
```

<span data-ttu-id="51004-211">AddDomainEvent 메서드가 수행하는 유일한 작업은 목록에 이벤트를 추가하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-211">Notice that the only thing that the AddDomainEvent method is doing is adding an event to the list.</span></span> <span data-ttu-id="51004-212">아직 디스패치된 이벤트가 없고 호출된 이벤트 처리기도 아직 없습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-212">No event is dispatched yet, and no event handler is invoked yet.</span></span>

<span data-ttu-id="51004-213">나중에 데이터베이스에 트랜잭션을 커밋할 때 이벤트를 디스패치하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-213">You actually want to dispatch the events later on, when you commit the transaction to the database.</span></span> <span data-ttu-id="51004-214">Entity Framework Core를 사용하는 경우, 다음 코드와 같이 EF DbContext의 SaveChanges 메서드를 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-214">If you are using Entity Framework Core, that means in the SaveChanges method of your EF DbContext, as in the following code:</span></span>

```csharp
// EF Core DbContext
public class OrderingContext : DbContext, IUnitOfWork
{
    // ...
    public async Task<bool> SaveEntitiesAsync(CancellationToken cancellationToken = default(CancellationToken))
    {
        // Dispatch Domain Events collection.
        // Choices:
        // A) Right BEFORE committing data (EF SaveChanges) into the DB. This makes
        // a single transaction including side effects from the domain event
        // handlers that are using the same DbContext with Scope lifetime
        // B) Right AFTER committing data (EF SaveChanges) into the DB. This makes
        // multiple transactions. You will need to handle eventual consistency and
        // compensatory actions in case of failures.        
        await _mediator.DispatchDomainEventsAsync(this);

        // After this line runs, all the changes (from the Command Handler and Domain
        // event handlers) performed through the DbContext will be committed
        var result = await base.SaveChangesAsync();
    }
}
```

<span data-ttu-id="51004-215">이 코드를 사용하여 엔터티 이벤트를 해당 이벤트 처리기에 디스패치합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-215">With this code, you dispatch the entity events to their respective event handlers.</span></span>

<span data-ttu-id="51004-216">도메인 이벤트 발생시키기를(메모리 목록에 간단히 추가하기) 이벤트 처리기에 도메인 이벤트를 디스패치하기와 분리시킨 것이 전반적인 결과입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-216">The overall result is that you have decoupled the raising of a domain event (a simple add into a list in memory) from dispatching it to an event handler.</span></span> <span data-ttu-id="51004-217">또한 사용 중인 디스패처의 종류에 따라 이벤트를 동기적으로 또는 비동기적으로 디스패치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-217">In addition, depending on what kind of dispatcher you are using, you could dispatch the events synchronously or asynchronously.</span></span>

<span data-ttu-id="51004-218">여기서 트랜잭션 경계가 중요한 역할을 한다는 점에 유의해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-218">Be aware that transactional boundaries come into significant play here.</span></span> <span data-ttu-id="51004-219">작업 단위와 트랜잭션이 둘 이상의 집합체에 있을 수 있으면(EF Core와 관계형 데이터베이스를 사용할 때처럼), 잘 작동할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-219">If your unit of work and transaction can span more than one aggregate (as when using EF Core and a relational database), this can work well.</span></span> <span data-ttu-id="51004-220">하지만 트랜잭션이 여러 집합체에 있을 수 없으면(예: Azure CosmosDB와 같은 NoSQL 데이터베이스를 사용하는 경우) 일관성을 달성하기 위해 추가 단계를 구현해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-220">But if the transaction cannot span aggregates, such as when you are using a NoSQL database like Azure CosmosDB, you have to implement additional steps to achieve consistency.</span></span> <span data-ttu-id="51004-221">이것은 지속성 무시가 보편적이지 않은 또 다른 이유이며, 사용하는 스토리지 시스템에 따라 달라집니다.</span><span class="sxs-lookup"><span data-stu-id="51004-221">This is another reason why persistence ignorance is not universal; it depends on the storage system you use.</span></span> 

### <a name="single-transaction-across-aggregates-versus-eventual-consistency-across-aggregates"></a><span data-ttu-id="51004-222">집합체 전반의 단일 트랜잭션 및 집합체 전반의 최종 일관성</span><span class="sxs-lookup"><span data-stu-id="51004-222">Single transaction across aggregates versus eventual consistency across aggregates</span></span>

<span data-ttu-id="51004-223">집합체 전반에서 단일 트랜잭션을 수행할지 또는 집합체 전반에서 최종 일관성에 의존할지에 대한 질문에는 논쟁의 여지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-223">The question of whether to perform a single transaction across aggregates versus relying on eventual consistency across those aggregates is a controversial one.</span></span> <span data-ttu-id="51004-224">Eric Evans 및 Vaughn Vernon과 같은 많은 DDD 작성자는 하나의 트랜잭션이 하나의 집계라는 규칙을 옹호하기 때문에 집합체 전반의 최종 일관성을 주장합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-224">Many DDD authors like Eric Evans and Vaughn Vernon advocate the rule that one transaction = one aggregate and therefore argue for eventual consistency across aggregates.</span></span> <span data-ttu-id="51004-225">예를 들어 Eric Evans의 저서인 *Domain-Driven Design*(도메인 기반 디자인)에는 다음과 같은 내용이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-225">For example, in his book *Domain-Driven Design*, Eric Evans says this:</span></span>

> <span data-ttu-id="51004-226">집합체에 걸쳐있는 규칙은 항상 최신 상태가 유지될 것으로 예상되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-226">Any rule that spans Aggregates will not be expected to be up-to-date at all times.</span></span> <span data-ttu-id="51004-227">이벤트 처리, 일괄 처리 또는 기타 업데이트 메커니즘을 통해 다른 종속성이 특정 시간 내에 확인될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-227">Through event processing, batch processing, or other update mechanisms, other dependencies can be resolved within some specific time.</span></span> <span data-ttu-id="51004-228">(128페이지)</span><span class="sxs-lookup"><span data-stu-id="51004-228">(page 128)</span></span>

<span data-ttu-id="51004-229">Vaughn Vernon은 [Effective Aggregate Design. Part II: Making Aggregates Work Together](https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf)(효과적인 집계 설계 2부: 집합체가 함께 작동하도록 만들기)에서 다음과 같이 언급하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-229">Vaughn Vernon says the following in [Effective Aggregate Design. Part II: Making Aggregates Work Together](https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf):</span></span>

> <span data-ttu-id="51004-230">따라서 하나의 집계 인스턴스에서 명령을 실행하는 경우 하나 이상의 집합체에서 추가적인 비즈니스 규칙을 실행해야 하며, 최종 일관성을 사용하여 \[...\] DDD 모델에서 최종 일관성을 지원하는 실용적인 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-230">Thus, if executing a command on one aggregate instance requires that additional business rules execute on one or more aggregates, use eventual consistency \[...\] There is a practical way to support eventual consistency in a DDD model.</span></span> <span data-ttu-id="51004-231">집계 메서드는 하나 이상의 비동기 구독자에게 제 시간에 배달되는 도메인 이벤트를 게시합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-231">An aggregate method publishes a domain event that is in time delivered to one or more asynchronous subscribers.</span></span>

<span data-ttu-id="51004-232">이러한 근거는 많은 집합체나 엔터티에 걸쳐 있는 트랜잭션 대신 세분화된 트랜잭션을 수용하는 데 기반합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-232">This rationale is based on embracing fine-grained transactions instead of transactions spanning many aggregates or entities.</span></span> <span data-ttu-id="51004-233">두 번째의 경우, 높은 확장성이 필요한 대규모 애플리케이션에는 데이터베이스 잠금 수가 상당할 것이라는 생각 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-233">The idea is that in the second case, the number of database locks will be substantial in large-scale applications with high scalability needs.</span></span> <span data-ttu-id="51004-234">확장성이 높은 애플리케이션은 여러 집합체 간에 즉각적인 트랜잭션 일관성이 필요하지 않다는 팩트를 수용하면 최종 일관성 개념을 받아들이는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="51004-234">Embracing the fact that highly scalable applications need not have instant transactional consistency between multiple aggregates helps with accepting the concept of eventual consistency.</span></span> <span data-ttu-id="51004-235">비즈니스에서 원자성 변경은 필요하지 않은 경우가 많으며 특정 작업에 원자성 트랜잭션이 필요한지 여부를 결정하는 것은 어떤 경우든 도메인 전문가의 책임입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-235">Atomic changes are often not needed by the business, and it is in any case the responsibility of the domain experts to say whether particular operations need atomic transactions or not.</span></span> <span data-ttu-id="51004-236">작업에 여러 집합체 사이의 원자성 트랜잭션이 항상 필요한 경우에는 집계가 더 커야 하는지 또는 제대로 설계되지 않은 것인지에 의문을 가져야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-236">If an operation always needs an atomic transaction between multiple aggregates, you might ask whether your aggregate should be larger or was not correctly designed.</span></span>

<span data-ttu-id="51004-237">하지만 Jimmy Bogard와 같은 설계자나 다른 개발자는 단일 트랜잭션이 여러 집합체에 걸쳐 있어도 괜찮다고 합니다. 단, 추가적인 집합체가 동일한 원래 명령의 파생 작업과 관련이 있는 경우에 한합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-237">However, other developers and architects like Jimmy Bogard are okay with spanning a single transaction across several aggregates—but only when those additional aggregates are related to side effects for the same original command.</span></span> <span data-ttu-id="51004-238">예를 들어 Bogard는 [A better domain events pattern](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/)(더 나은 도메인 이벤트 패턴)에서 다음과 같이 언급하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-238">For instance, in [A better domain events pattern](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/), Bogard says this:</span></span>

> <span data-ttu-id="51004-239">일반적으로 도메인 이벤트의 파생 작업이 논리 트랜잭션 내에서 발생하기를 바라지만 도메인 이벤트를 발생시키는 범위와 동일해야 하는 것은 아닙니다. \[...\] 트랜잭션을 커밋하기 직전에 이벤트를 해당 처리기에 디스패치합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-239">Typically, I want the side effects of a domain event to occur within the same logical transaction, but not necessarily in the same scope of raising the domain event \[...\] Just before we commit our transaction, we dispatch our events to their respective handlers.</span></span>

<span data-ttu-id="51004-240">원래 트랜잭션을 커밋하기 직*전*에 도메인 이벤트를 디스패치하는 것은 이러한 이벤트의 파생 작업을 동일한 트랜잭션에 포함시키기를 바라기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-240">If you dispatch the domain events right *before* committing the original transaction, it is because you want the side effects of those events to be included in the same transaction.</span></span> <span data-ttu-id="51004-241">예를 들어 EF DbContext SaveChanges 메서드가 실패하면 트랜잭션은 관련 도메인 이벤트 처리기가 수행한 파생 작업의 결과를 비롯한 모든 변경 내용을 롤백합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-241">For example, if the EF DbContext SaveChanges method fails, the transaction will roll back all changes, including the result of any side effect operations implemented by the related domain event handlers.</span></span> <span data-ttu-id="51004-242">이것은 DbContext 수명 범위가 기본적으로 "범위 지정됨(scoped)"으로 정의되어 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-242">This is because the DbContext life scope is by default defined as "scoped."</span></span> <span data-ttu-id="51004-243">따라서 DbContext 개체는 동일한 범위나 개체 그래프 내에서 인스턴스화되는 다수의 리포지터리 개체 간에 공유됩니다.</span><span class="sxs-lookup"><span data-stu-id="51004-243">Therefore, the DbContext object is shared across multiple repository objects being instantiated within the same scope or object graph.</span></span> <span data-ttu-id="51004-244">이것은 Web API 또는 MVC 앱을 개발할 때 HttpRequest 범위와 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-244">This coincides with the HttpRequest scope when developing Web API or MVC apps.</span></span>

<span data-ttu-id="51004-245">실제로 두 가지 방식(단일 원자성 트랜잭션 및 최종 일관성)이 모두 맞을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-245">Actually, both approaches (single atomic transaction and eventual consistency) can be right.</span></span> <span data-ttu-id="51004-246">도메인이나 비즈니스 요구 사항 및 도메인 전문가의 의견에 따라 달라질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-246">It really depends on your domain or business requirements and what the domain experts tell you.</span></span> <span data-ttu-id="51004-247">또한 서비스의 확장성이 얼마나 필요한지에 따라서도 달라집니다. (보다 세분화된 트랜잭션은 데이터베이스 잠금과 관련된 영향이 적습니다.)</span><span class="sxs-lookup"><span data-stu-id="51004-247">It also depends on how scalable you need the service to be (more granular transactions have less impact with regard to database locks).</span></span> <span data-ttu-id="51004-248">코드에 투자할 의향이 얼마나 되는지에 따라서도 달라집니다. 최종 일관성을 위해서는 집합체 전반에서 잠재적인 비일관성을 감지하고 보정 작업을 구현하기 위해 더 복잡한 코드가 필요하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-248">And it depends on how much investment you are willing to make in your code, since eventual consistency requires more complex code in order to detect possible inconsistencies across aggregates and the need to implement compensatory actions.</span></span> <span data-ttu-id="51004-249">원래 집합체에 변경 내용을 커밋한 후 이벤트가 디스패치될 때 문제가 발생하고 이벤트 처리기가 파생 작업을 커밋할 수 없는 경우 집합체 사이에 불일치가 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-249">Consider that if you commit changes to the original aggregate and afterwards, when the events are being dispatched, if there is an issue and the event handlers cannot commit their side effects, you will have inconsistencies between aggregates.</span></span>

<span data-ttu-id="51004-250">보정 작업을 허용하는 방법은 추가 데이터베이스 테이블에 도메인 이벤트를 저장하여 원래 트랜잭션의 일부가 될 수 있도록 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-250">A way to allow compensatory actions would be to store the domain events in additional database tables so they can be part of the original transaction.</span></span> <span data-ttu-id="51004-251">이후에 현재 집합체 상태와 이벤트 목록을 비교하여 불일치를 감지하고 보정 작업을 실행하는 일괄 처리 프로세스를 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-251">Afterwards, you could have a batch process that detects inconsistencies and runs compensatory actions by comparing the list of events with the current state of the aggregates.</span></span> <span data-ttu-id="51004-252">보정 작업은 사용자 쪽에서도 비즈니스 사용자 및 도메인 전문가와의 토론을 비롯하여 심층적인 분석이 필요한 복잡한 토픽에 속합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-252">The compensatory actions are part of a complex topic that will require deep analysis from your side, which includes discussing it with the business user and domain experts.</span></span>

<span data-ttu-id="51004-253">어떤 경우든 필요한 방식을 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-253">In any case, you can choose the approach you need.</span></span> <span data-ttu-id="51004-254">하지만 EF Core와 관계형 데이터베이스를 사용하는 경우에는 초기 지연 방식(커밋하기 전에 이벤트를 발생시키기 때문에 단일 트랜잭션을 사용)이 가장 간단한 방식입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-254">But the initial deferred approach—raising the events before committing, so you use a single transaction—is the simplest approach when using EF Core and a relational database.</span></span> <span data-ttu-id="51004-255">구현하기 쉽고 많은 비즈니스 사례에 유효합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-255">It is easier to implement and valid in many business cases.</span></span> <span data-ttu-id="51004-256">eShopOnContainers의 Ordering(주문) 마이크로 서비스에 사용된 방식이기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-256">It is also the approach used in the ordering microservice in eShopOnContainers.</span></span>

<span data-ttu-id="51004-257">하지만 이러한 이벤트를 해당 이벤트 처리기에 실제로 어떻게 디스패치합니까?</span><span class="sxs-lookup"><span data-stu-id="51004-257">But how do you actually dispatch those events to their respective event handlers?</span></span> <span data-ttu-id="51004-258">이전 예제에서 볼 수 있는 `_mediator` 개체는 무엇인가요?</span><span class="sxs-lookup"><span data-stu-id="51004-258">What's the `_mediator` object you see in the previous example?</span></span> <span data-ttu-id="51004-259">이벤트와 이벤트 처리기 간의 매핑에 사용하는 기술 및 아티팩트와 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-259">It has to do with the techniques and artifacts you use to map between events and their event handlers.</span></span>

### <a name="the-domain-event-dispatcher-mapping-from-events-to-event-handlers"></a><span data-ttu-id="51004-260">도메인 이벤트 디스패처: 이벤트에서 이벤트 처리기로 매핑</span><span class="sxs-lookup"><span data-stu-id="51004-260">The domain event dispatcher: mapping from events to event handlers</span></span>

<span data-ttu-id="51004-261">이벤트를 디스패치하거나 게시하는 것이 가능해지면 이벤트를 푸시할 아티팩트가 필요합니다. 그래야 모든 관련 처리기가 이벤트를 받아서 이를 기반으로 파생 작업을 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-261">Once you're able to dispatch or publish the events, you need some kind of artifact that will publish the event, so that every related handler can get it and process side effects based on that event.</span></span>

<span data-ttu-id="51004-262">한 가지 방법은 실제 메시지 시스템 또는 메모리 내 이벤트와 대조적으로 서비스 버스를 기반으로 할 수 있는 이벤트 버스입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-262">One approach is a real messaging system or even an event bus, possibly based on a service bus as opposed to in-memory events.</span></span> <span data-ttu-id="51004-263">하지만 첫 번째의 경우, 동일한 프로세스 내(즉, 동일한 도메인 및 애플리케이션 계층 내)에 있는 이벤트만 처리하면 되기 때문에 실제 메시지는 도메인 이벤트를 처리하기에 과도합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-263">However, for the first case, real messaging would be overkill for processing domain events, since you just need to process those events within the same process (that is, within the same domain and application layer).</span></span>

<span data-ttu-id="51004-264">이벤트를 여러 이벤트 처리기에 매핑하는 또 다른 방법은 IoC 컨테이너의 형식 등록을 사용하여 이벤트를 디스패치할 곳을 동적으로 추론하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-264">Another way to map events to multiple event handlers is by using types registration in an IoC container so you can dynamically infer where to dispatch the events.</span></span> <span data-ttu-id="51004-265">다시 말해, 특정 이벤트를 얻기 위해 이벤트 처리기에 무엇이 필요한지 알아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-265">In other words, you need to know what event handlers need to get a specific event.</span></span> <span data-ttu-id="51004-266">그림 7-16은 이 방식에 대한 간단한 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="51004-266">Figure 7-16 shows a simplified approach for this approach.</span></span>

![MediatR에서 사용하는 방법인 종속성 주입을 사용하여 이벤트를 이벤트 처리기와 연관시킬 수 있습니다.](./media/image17.png)

<span data-ttu-id="51004-268">**그림 7-16**.</span><span class="sxs-lookup"><span data-stu-id="51004-268">**Figure 7-16**.</span></span> <span data-ttu-id="51004-269">IoC를 사용한 도메인 이벤트 디스패처</span><span class="sxs-lookup"><span data-stu-id="51004-269">Domain event dispatcher using IoC</span></span>

<span data-ttu-id="51004-270">배관 및 아티팩트를 모두 구축하여 이런 방식을 직접 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-270">You can build all the plumbing and artifacts to implement that approach by yourself.</span></span> <span data-ttu-id="51004-271">하지만 커버에서 IoC 컨테이너를 사용하는 [MediatR](https://github.com/jbogard/MediatR)와 같은 사용 가능한 라이브러리를 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-271">However, you can also use available libraries like [MediatR](https://github.com/jbogard/MediatR) that uses your IoC container under the covers.</span></span> <span data-ttu-id="51004-272">그러면 미리 정의된 인터페이스 및 중재자 개체의 게시/디스패치 메서드를 직접 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-272">You can therefore directly use the predefined interfaces and the mediator object’s publish/dispatch methods.</span></span>

<span data-ttu-id="51004-273">코드에서 먼저 IoC 컨테이너에 이벤트 처리기 형식을 등록해야 합니다. 다음은 [eShopOnContainers Ordering(주문) 마이크로 서비스](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Infrastructure/AutofacModules/MediatorModule.cs)의 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="51004-273">In code, you first need to register the event handler types in your IoC container, as shown in the following example at [eShopOnContainers Ordering microservice](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Infrastructure/AutofacModules/MediatorModule.cs):</span></span>

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        // Other registrations ...
        // Register the DomainEventHandler classes (they implement IAsyncNotificationHandler<>)
        // in assembly holding the Domain Events
        builder.RegisterAssemblyTypes(typeof(ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler)
                                       .GetTypeInfo().Assembly)
                                         .AsClosedTypesOf(typeof(IAsyncNotificationHandler<>));
        // Other registrations ...
    }
}
```

<span data-ttu-id="51004-274">먼저 코드는 처리기가 있는 어셈블리를 찾아서 도메인 이벤트 처리기가 포함된 어셈블리를 식별하지만(typeof(ValidateOrAddBuyerAggregateWhenXxxx) 사용) 다른 이벤트 처리기를 선택하여 어셈블리를 찾을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-274">The code first identifies the assembly that contains the domain event handlers by locating the assembly that holds any of the handlers (using typeof(ValidateOrAddBuyerAggregateWhenXxxx), but you could have chosen any other event handler to locate the assembly).</span></span> <span data-ttu-id="51004-275">모든 이벤트 처리기가 IAsyncNotificationHandler 인터페이스를 구현하기 때문에 코드는 해당 형식을 검색하여 모든 이벤트 처리기를 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-275">Since all the event handlers implement the IAsyncNotificationHandler interface, the code then just searches for those types and registers all the event handlers.</span></span>

### <a name="how-to-subscribe-to-domain-events"></a><span data-ttu-id="51004-276">도메인 이벤트 구독 방법</span><span class="sxs-lookup"><span data-stu-id="51004-276">How to subscribe to domain events</span></span>

<span data-ttu-id="51004-277">MediatR을 사용하는 경우 각 이벤트 처리기는 INotificationHandler 인터페이스의 제네릭 매개 변수에 제공된 이벤트 형식을 사용해야 합니다. 다음 코드를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="51004-277">When you use MediatR, each event handler must use an event type that is provided on the generic parameter of the INotificationHandler interface, as you can see in the following code:</span></span>

```csharp
public class ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler
  : IAsyncNotificationHandler<OrderStartedDomainEvent>
```

<span data-ttu-id="51004-278">구독이라고 간주할 수 있는 이벤트와 이벤트 처리기 사이의 관계를 기반으로 MediatR 아티팩트는 각 이벤트에 대한 모든 이벤트 처리기를 검색하고 해당되는 각각의 이벤트 처리기를 트리거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-278">Based on the relationship between event and event handler, which can be considered the subscription, the MediatR artifact can discover all the event handlers for each event and trigger each one of those event handlers.</span></span>

### <a name="how-to-handle-domain-events"></a><span data-ttu-id="51004-279">도메인 이벤트 처리 방법</span><span class="sxs-lookup"><span data-stu-id="51004-279">How to handle domain events</span></span>

<span data-ttu-id="51004-280">마지막으로 이벤트 처리기는 대개 인프라 리포지토리를 사용하여 필요한 추가 집합체를 확보하고 파생 작업 도메인 논리를 실행하는 애플리케이션 계층 코드를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-280">Finally, the event handler usually implements application layer code that uses infrastructure repositories to obtain the required additional aggregates and to execute side-effect domain logic.</span></span> <span data-ttu-id="51004-281">다음 [eShopOnContainers의 도메인 이벤트 처리기 코드](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/DomainEventHandlers/OrderStartedEvent/ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler.cs)에서 구현 예제를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="51004-281">The following [domain event handler code at eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/DomainEventHandlers/OrderStartedEvent/ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler.cs), shows an implementation example.</span></span>

```csharp
public class ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler
                   : INotificationHandler<OrderStartedDomainEvent>
{
    private readonly ILoggerFactory _logger;
    private readonly IBuyerRepository<Buyer> _buyerRepository;
    private readonly IIdentityService _identityService;

    public ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler(
        ILoggerFactory logger,
        IBuyerRepository<Buyer> buyerRepository,
        IIdentityService identityService)
    {
        // ...Parameter validations...
    }

    public async Task Handle(OrderStartedDomainEvent orderStartedEvent)
    {
        var cardTypeId = (orderStartedEvent.CardTypeId != 0) ? orderStartedEvent.CardTypeId : 1;        
        var userGuid = _identityService.GetUserIdentity();
        var buyer = await _buyerRepository.FindAsync(userGuid);
        bool buyerOriginallyExisted = (buyer == null) ? false : true;

        if (!buyerOriginallyExisted)
        {
            buyer = new Buyer(userGuid);
        }

        buyer.VerifyOrAddPaymentMethod(cardTypeId,
                                       $"Payment Method on {DateTime.UtcNow}",
                                       orderStartedEvent.CardNumber,
                                       orderStartedEvent.CardSecurityNumber,
                                       orderStartedEvent.CardHolderName,
                                       orderStartedEvent.CardExpiration,
                                       orderStartedEvent.Order.Id);

        var buyerUpdated = buyerOriginallyExisted ? _buyerRepository.Update(buyer) 
                                                                      : _buyerRepository.Add(buyer);

        await _buyerRepository.UnitOfWork
                .SaveEntitiesAsync();

        // Logging code using buyerUpdated info, etc.
    }
}
```

<span data-ttu-id="51004-282">이전 도메인 이벤트 처리기 코드는 인프라 리포지토리를 사용하기 때문에 애플리케이션 계층 코드로 간주되었습니다. 이 내용은 인프라 지속성 계층에 관한 다음 섹션에 설명되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-282">The previous domain event handler code is considered application layer code because it uses infrastructure repositories, as explained in the next section on the infrastructure-persistence layer.</span></span> <span data-ttu-id="51004-283">이벤트 처리기는 다른 인프라 구성 요소도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-283">Event handlers could also use other infrastructure components.</span></span>

#### <a name="domain-events-can-generate-integration-events-to-be-published-outside-of-the-microservice-boundaries"></a><span data-ttu-id="51004-284">도메인 이벤트는 마이크로 서비스 경계 밖에 게시할 통합 이벤트를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-284">Domain events can generate integration events to be published outside of the microservice boundaries</span></span>

<span data-ttu-id="51004-285">마지막으로 여러 마이크로 서비스에 걸쳐 이벤트를 전파해야 하는 경우가 있을 수 있다는 것을 언급하는 것이 중요합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-285">Finally, it's important to mention that you might sometimes want to propagate events across multiple microservices.</span></span> <span data-ttu-id="51004-286">이 전파는 통합 이벤트로서, 특정 도메인 이벤트 처리기의 이벤트 버스를 통해 게시될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-286">That propagation is an integration event, and it could be published through an event bus from any specific domain event handler.</span></span>

## <a name="conclusions-on-domain-events"></a><span data-ttu-id="51004-287">도메인 이벤트에 대한 결론</span><span class="sxs-lookup"><span data-stu-id="51004-287">Conclusions on domain events</span></span>

<span data-ttu-id="51004-288">언급했듯이, 도메인 이벤트를 사용하여 도메인 내 변경의 파생 작업을 명시적으로 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-288">As stated, use domain events to explicitly implement side effects of changes within your domain.</span></span> <span data-ttu-id="51004-289">DDD 용어를 사용하려면, 도메인 이벤트를 사용하여 하나 또는 여러 집합체 전반에 파생 작업을 명시적으로 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-289">To use DDD terminology, use domain events to explicitly implement side effects across one or multiple aggregates.</span></span> <span data-ttu-id="51004-290">추가적으로, 데이터베이스 잠금의 확장성을 높이고 영향을 줄이려면 동일한 도메인 내의 집합체 간에 최종 일관성을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="51004-290">Additionally, and for better scalability and less impact on database locks, use eventual consistency between aggregates within the same domain.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="51004-291">추가 자료</span><span class="sxs-lookup"><span data-stu-id="51004-291">Additional resources</span></span>

- <span data-ttu-id="51004-292">**Greg Young. 도메인 이벤트란?**</span><span class="sxs-lookup"><span data-stu-id="51004-292">**Greg Young. What is a Domain Event?**</span></span> \
  <https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf#page=25>

- <span data-ttu-id="51004-293">**Jan Stenberg. 도메인 이벤트 및 최종 일관성** </span><span class="sxs-lookup"><span data-stu-id="51004-293">**Jan Stenberg. Domain Events and Eventual Consistency** </span></span>\
  <https://www.infoq.com/news/2015/09/domain-events-consistency>

- <span data-ttu-id="51004-294">**Jimmy Bogard. 더 나은 도메인 이벤트 패턴** </span><span class="sxs-lookup"><span data-stu-id="51004-294">**Jimmy Bogard. A better domain events pattern** </span></span>\
  <https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/>

- <span data-ttu-id="51004-295">**Vaughn Vernon. Effective Aggregate Design Part II: Making Aggregates Work Together** \(효과적인 집계 설계 2부: 집합체가 함께 작동하도록 만들기)에서 다음과 같이 언급하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="51004-295">**Vaughn Vernon. Effective Aggregate Design Part II: Making Aggregates Work Together** \</span></span>
  [https://dddcommunity.org/wp-content/uploads/files/pdf\_articles/Vernon\_2011\_2.pdf](https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf)

- <span data-ttu-id="51004-296">**Jimmy Bogard. Strengthening your domain: Domain Events** \(도메인 강화: 도메인 이벤트)</span><span class="sxs-lookup"><span data-stu-id="51004-296">**Jimmy Bogard. Strengthening your domain: Domain Events** \</span></span>
  <https://lostechies.com/jimmybogard/2010/04/08/strengthening-your-domain-domain-events/>

- <span data-ttu-id="51004-297">**Tony Truong. 도메인 이벤트 패턴 예제** </span><span class="sxs-lookup"><span data-stu-id="51004-297">**Tony Truong. Domain Events Pattern Example** </span></span>\
  <https://www.tonytruong.net/domain-events-pattern-example/>

- <span data-ttu-id="51004-298">**Udi Dahan. 완벽하게 캡슐화된 도메인 모델을 만드는 방법** </span><span class="sxs-lookup"><span data-stu-id="51004-298">**Udi Dahan. How to create fully encapsulated Domain Models** </span></span>\
  <http://udidahan.com/2008/02/29/how-to-create-fully-encapsulated-domain-models/>

- <span data-ttu-id="51004-299">**Udi Dahan. 도메인 이벤트 - 테이크 2** </span><span class="sxs-lookup"><span data-stu-id="51004-299">**Udi Dahan. Domain Events – Take 2** </span></span>\
  <http://udidahan.com/2008/08/25/domain-events-take-2/>

- <span data-ttu-id="51004-300">**Udi Dahan. 도메인 이벤트 - 구원** </span><span class="sxs-lookup"><span data-stu-id="51004-300">**Udi Dahan. Domain Events – Salvation** </span></span>\
  <http://udidahan.com/2009/06/14/domain-events-salvation/>

- <span data-ttu-id="51004-301">**Jan Kronquist. 도메인 이벤트를 게시하지 말고 반환하라!**</span><span class="sxs-lookup"><span data-stu-id="51004-301">**Jan Kronquist. Don't publish Domain Events, return them!**</span></span> \
  <https://blog.jayway.com/2013/06/20/dont-publish-domain-events-return-them/>

- <span data-ttu-id="51004-302">**Cesar de la Torre. Domain Events vs. DDD 및 마이크로 서비스 아키텍처의 통합 이벤트** </span><span class="sxs-lookup"><span data-stu-id="51004-302">**Cesar de la Torre. Domain Events vs. Integration Events in DDD and microservices architectures** </span></span>\
  <https://blogs.msdn.microsoft.com/cesardelatorre/2017/02/07/domain-events-vs-integration-events-in-domain-driven-design-and-microservices-architectures/>

>[!div class="step-by-step"]
><span data-ttu-id="51004-303">[이전](client-side-validation.md)
>[다음](infrastructure-persistence-layer-design.md)</span><span class="sxs-lookup"><span data-stu-id="51004-303">[Previous](client-side-validation.md)
[Next](infrastructure-persistence-layer-design.md)</span></span>