---
title: 관계형 데이터베이스를 azure로 마이그레이션
description: Azure 클라우드와 Windows 컨테이너를 사용하여 기존 .NET 응용 프로그램 최신화 | 관계형 데이터베이스를 azure로 마이그레이션
ms.date: 04/28/2018
ms.openlocfilehash: 3d4f03e61144bb6a442a50916d7fd024d38ec611
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "69578376"
---
# <a name="migrate-your-relational-databases-to-azure"></a><span data-ttu-id="fecf9-103">관계형 데이터베이스를 azure로 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="fecf9-103">Migrate your relational databases to azure</span></span>

<span data-ttu-id="fecf9-104">목표: Azure에서는 가장 포괄적인 데이터베이스 마이그레이션을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-104">Vision: Azure offers the most comprehensive database migration.</span></span>

<span data-ttu-id="fecf9-105">Azure에서 IaaS VM(순수한 리프트 앤 시프트)에 직접 데이터베이스 서버를 마이그레이션하거나, 추가적인 이득을 얻기 위해 Azure SQL Database로 마이그레이션할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-105">In Azure, you can migrate your database servers directly to IaaS VMs (pure lift and shift), or you can migrate to Azure SQL Database, for additional benefits.</span></span> <span data-ttu-id="fecf9-106">Azure SQL Database에서는 관리되는 인스턴스와 서비스 형태의 전체 데이터베이스(DBaaS) 옵션을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-106">Azure SQL Database offers managed instance and full database-as-a-service (DBaaS) options.</span></span> <span data-ttu-id="fecf9-107">그림 3-1에서는 Azure에서 사용할 수 있는 다양한 관계형 데이터베이스 마이그레이션 경로를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-107">Figure 3-1 shows the multiple relational database migration paths available in Azure.</span></span>

![Azure의 데이터베이스 마이그레이션 경로](./media/image3-1.png)

> <span data-ttu-id="fecf9-109">**그림 3-1.**</span><span class="sxs-lookup"><span data-stu-id="fecf9-109">**Figure 3-1.**</span></span> <span data-ttu-id="fecf9-110">Azure의 데이터베이스 마이그레이션 경로</span><span class="sxs-lookup"><span data-stu-id="fecf9-110">Database migration paths in Azure</span></span>

## <a name="when-to-migrate-to-azure-sql-database-managed-instance"></a><span data-ttu-id="fecf9-111">Azure SQL Database Managed Instance로 마이그레이션해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="fecf9-111">When to migrate to Azure SQL Database Managed Instance</span></span>

<span data-ttu-id="fecf9-112">대부분의 경우 Azure로 데이터를 마이그레이션할 때 Azure SQL Database Managed Instance는 생각할 수 있는 가장 좋은 선택입니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-112">In most cases, Azure SQL Database Managed Instance will be your best option to consider when you migrate your data to Azure.</span></span> <span data-ttu-id="fecf9-113">SQL Server 데이터베이스를 마이그레이션할 때 응용 프로그램을 다시 작성하거나 데이터 또는 데이터에 액세스하는 코드를 거의 변경하지 않아도 된다는 것을 100% 보장해야 하는 경우 Azure SQL Database의 Managed Instance 기능을 선택하십시오.</span><span class="sxs-lookup"><span data-stu-id="fecf9-113">If you are migrating SQL Server databases and need nearly 100% assurance that you won't need to rearchitect your application or make changes to your data or data access code, choose the Managed Instance feature of Azure SQL Database.</span></span>

<span data-ttu-id="fecf9-114">Azure SQL Database Managed Instance는 표준 Azure SQL 데이터베이스(단일 데이터베이스 모델)에서 제공되는 기능을 넘어선 격리 요구 사항이나 SQL Server 인스턴스 수준의 기능에 대한 추가적인 요구 사항이 있는 경우 최상의 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-114">Azure SQL Database Managed Instance is the best option if you have additional requirements for SQL Server instance-level functionality, or isolation requirements beyond the features provided in a standard Azure SQL Database (single database model).</span></span> <span data-ttu-id="fecf9-115">마지막은 PaaS 중심의 선택이지만 기존 SQL 서버와 동일한 기능을 제공하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-115">This last one is the most PaaS-oriented choice, but it doesn't offer the same features as that of a traditional SQL server.</span></span> <span data-ttu-id="fecf9-116">마이그레이션은 마찰을 일으킬 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-116">Migration might surface frictions.</span></span>

<span data-ttu-id="fecf9-117">예를 들어 인스턴스 수준의 SQL Server 기능에 많은 투자를 한 조직은 SQL Managed Instance로 마이그레이션할 때 이점을 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-117">For example, an organization that has made deep investments in instance-level SQL Server capabilities would benefit from migrating to SQL Managed Instance.</span></span> <span data-ttu-id="fecf9-118">인스턴스 수준 SQL Server 기능의 예로는 SQL 공용 언어 런타임(CLR) 통합과 SQL Server 에이전트, 크로스 데이터베이스 쿼리가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-118">Examples of instance-level SQL Server capabilities include SQL common language runtime (CLR) integration, SQL Server Agent, and cross-database querying.</span></span> <span data-ttu-id="fecf9-119">이러한 기능에 대한 지원은 표준 Azure SQL 데이터베이스(단일 데이터베이스 모델)에서 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-119">Support for these features is not available in standard Azure SQL Database (a single-database model).</span></span>

<span data-ttu-id="fecf9-120">규제가 엄격한 산업에서 운영되고 보안 목적으로 격리를 유지해야하는 조직은 SQL Managed Instance 모델을 선택하면 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-120">An organization that operates in a highly regulated industry, and which needs to maintain isolation for security purposes, also might benefit from choosing the SQL Managed Instance model.</span></span>

<span data-ttu-id="fecf9-121">Azure SQL Database에서 관리되는 인스턴스는 다음과 같은 특징이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-121">Managed Instance in Azure SQL Database has the following characteristics:</span></span>

- <span data-ttu-id="fecf9-122">Azure Virtual Network를 통한 보안 격리</span><span class="sxs-lookup"><span data-stu-id="fecf9-122">Security isolation through Azure Virtual Network</span></span>

- <span data-ttu-id="fecf9-123">다음 기능을 사용한 응용 프로그램 노출 호환성</span><span class="sxs-lookup"><span data-stu-id="fecf9-123">Application surface compatibility, with these features:</span></span>

  - <span data-ttu-id="fecf9-124">SQL Server 에이전트 및 SQL Server Profiler</span><span class="sxs-lookup"><span data-stu-id="fecf9-124">SQL Server Agent and SQL Server Profiler</span></span>

  - <span data-ttu-id="fecf9-125">크로스 데이터베이스 참조와 쿼리, SQL CLR, 복제, 변경 데이터 캡처(CDC) 및 Service Broker</span><span class="sxs-lookup"><span data-stu-id="fecf9-125">Cross-database references and queries, SQL CLR, replication, change data capture (CDC), and Service Broker</span></span>

- <span data-ttu-id="fecf9-126">최대 35TB 데이터베이스 크기</span><span class="sxs-lookup"><span data-stu-id="fecf9-126">Database sizes up to 35 TB</span></span>

- <span data-ttu-id="fecf9-127">다음 기능을 사용한 최소 가동 중지 시간 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="fecf9-127">Minimum-downtime migration, with these features:</span></span>

  - <span data-ttu-id="fecf9-128">Azure Database Migration Service</span><span class="sxs-lookup"><span data-stu-id="fecf9-128">Azure Database Migration Service</span></span>

  - <span data-ttu-id="fecf9-129">네이티브 백업 및 복원 및 로그 전달</span><span class="sxs-lookup"><span data-stu-id="fecf9-129">Native backup and restore, and log shipping</span></span>

<span data-ttu-id="fecf9-130">이러한 기능을 사용하여, Azure SQL Database에 기존 응용 프로그램 데이터베이스를 마이그레이션하는 경우 Managed Instance 모델은 SQL Server에 대한 PaaS의 이점의 거의 100%를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-130">With these capabilities, when you migrate existing application databases to Azure SQL Database, the Managed Instance model offers nearly 100% of the benefits of PaaS for SQL Server.</span></span> <span data-ttu-id="fecf9-131">Managed Instance는 응용 프로그램 디자인을 변경하지 않고 인스턴스 수준의 기능을 계속해서 사용할 수 있는 SQL Server 환경입니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-131">Managed Instance is a SQL Server environment where you continue using instance-level capabilities without changing your application design.</span></span>

<span data-ttu-id="fecf9-132">Managed Instance는 현재 SQL Server를 사용 중이고 클라우드에서 네트워크 보안에 유연성이 필요한 기업에 가장 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-132">Managed Instance is probably the best fit for enterprises that currently are using SQL Server, and which require flexibility in their network security in the cloud.</span></span> <span data-ttu-id="fecf9-133">이는 SQL database용 전용 가상 네트워크와 같습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-133">It's like having a private virtual network for your SQL databases.</span></span>

## <a name="when-to-migrate-to-azure-sql-database"></a><span data-ttu-id="fecf9-134">Azure SQL Database로 마이그레이션해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="fecf9-134">When to migrate to Azure SQL Database</span></span>

<span data-ttu-id="fecf9-135">앞서 언급했듯이 표준 Azure SQL Database는 완전히 관리되는 관계형 DBaaS입니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-135">As mentioned, the standard Azure SQL Database is a fully managed, relational DBaaS.</span></span> <span data-ttu-id="fecf9-136">SQL Database는 현재 전세계 38개의 데이터 센터에서 수백만 개의 상용 데이터베이스를 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-136">SQL Database currently manages millions of production databases, across 38 datacenters, around the world.</span></span> <span data-ttu-id="fecf9-137">간단한 트랜잭션 데이터 관리에서부터 글로벌 규모의 고급 데이터 처리를 필요로 하며 데이터를 가장 많이 사용하고 중요한 업무용 응용 프로그램에 이르기까지 광범위한 응용 프로그램과 워크로드를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-137">It supports a broad range of applications and workloads, from managing straightforward transactional data, to driving the most data-intensive, mission-critical applications that require advanced data processing at a global scale.</span></span>

<span data-ttu-id="fecf9-138">PaaS의 완벽한 기능, 더 나은 가격 책정(궁극적으로 저렴한 비용)으로 인해, 기본 표준 SQL 데이터베이스를 사용하는 응용 프로그램이 있고 추가 인스턴스 기능이 없는 경우 "기본적인 선택"으로 표준 Azure SQL 데이터베이스로 이동해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-138">Because of its full PaaS features, better pricing-and ultimately lower cost-you should move to the standard Azure SQL Database as your "by-default choice" if you have an application that uses basic, standard SQL databases, and no additional instance features.</span></span> <span data-ttu-id="fecf9-139">표준 Azure SQL Database에서는 SQL CLR 통합과 SQL Server 에이전트, 크로스 데이터베이스 쿼리와 같은 SQL Server 기능은 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-139">SQL Server features like SQL CLR integration, SQL Server Agent, and cross-database querying are not supported in the standard Azure SQL Database.</span></span> <span data-ttu-id="fecf9-140">이러한 기능은 Azure SQL Database Managed Instance 모델에서만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-140">Those features are available only in the Azure SQL Database Managed Instance model.</span></span>

<span data-ttu-id="fecf9-141">Azure SQL Database는 앱 개발자를 위해 구축된 지능형 클라우드 데이터베이스 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-141">Azure SQL Database is the only intelligent cloud database service that's built for app developers.</span></span> <span data-ttu-id="fecf9-142">멀티 테넌트 앱을 효율적으로 제공하기 위해 가동 중단 시간 없이 즉석에서 크기를 조정할 수 있는 유일한 클라우드 데이터베이스 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-142">It's also the only cloud database service that scales on-the-fly, without downtime, to help you efficiently deliver multitenant apps.</span></span> <span data-ttu-id="fecf9-143">궁극적으로 Azure SQL Database를 사용하면 혁신에 더 많은 시간을 사용할 수 있고 출시를 앞당겨 줍니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-143">Ultimately, Azure SQL Database leaves you more time to innovate, and it accelerates your time to market.</span></span> <span data-ttu-id="fecf9-144">원하는 플랫폼과 언어를 사용하여 보안 앱을 만들고 SQL database에 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-144">You can build secure apps and connect to your SQL database by using the languages and platforms that you prefer.</span></span>

<span data-ttu-id="fecf9-145">Azure SQL Database에서는 다음과 같은 이점을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-145">Azure SQL Database offers the following benefits:</span></span>

- <span data-ttu-id="fecf9-146">앱을 학습하고 적응하는 지능 내장(기계 학습)</span><span class="sxs-lookup"><span data-stu-id="fecf9-146">Built-in intelligence (machine learning) that learns and adapts to your app</span></span>

- <span data-ttu-id="fecf9-147">온 디멘드 데이터베이스 프로비저닝</span><span class="sxs-lookup"><span data-stu-id="fecf9-147">On-demand database provisioning</span></span>

- <span data-ttu-id="fecf9-148">모든 워크로드에 대한 다양한 제품</span><span class="sxs-lookup"><span data-stu-id="fecf9-148">A range of offers, for all workloads</span></span>

- <span data-ttu-id="fecf9-149">가용성 99.99% SLA, 유지관리 없음</span><span class="sxs-lookup"><span data-stu-id="fecf9-149">99.99% availability SLA, zero maintenance</span></span>

- <span data-ttu-id="fecf9-150">데이터 보호를 위한 지리적 복제 및 복원 서비스</span><span class="sxs-lookup"><span data-stu-id="fecf9-150">Geo-replication and restore services for data protection</span></span>

- <span data-ttu-id="fecf9-151">Azure SQL Database 시점 복원 기능</span><span class="sxs-lookup"><span data-stu-id="fecf9-151">Azure SQL Database Point in Time Restore feature</span></span>

- <span data-ttu-id="fecf9-152">하이브리드 및 마이그레이션을 포함한 SQL Server 2016과의 호환성</span><span class="sxs-lookup"><span data-stu-id="fecf9-152">Compatibility with SQL Server 2016, including hybrid and migration</span></span>

<span data-ttu-id="fecf9-153">표준 Azure SQL 데이터베이스는 Azure SQL Database Managed Instance보다 PaaS에 더 가깝습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-153">The standard Azure SQL Database is closer to PaaS than Azure SQL Database Managed Instance.</span></span> <span data-ttu-id="fecf9-154">표준 Azure SQL 데이터베이스를 선호하는 이유는 관리되는 클라우드에서 더 많은 이점을 얻을 수 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-154">Prefer the standard Azure SQL Database because you'll get more benefits from a managed cloud.</span></span> <span data-ttu-id="fecf9-155">그러나 Azure SQL 데이터베이스는 일반적인 온-프레미스 SQL Server 인스턴스와 구별되는 몇 가지 주요 차이점이 존재합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-155">However, Azure SQL Database has some key differences from regular and on-premises SQL Server instances.</span></span> <span data-ttu-id="fecf9-156">Azure SQL 데이터베이스는 기존 애플리케이션의 데이터베이스 요구사항과 엔터프라이즈 요구사항 및 정책에 따라 클라우드로의 마이그레이션을 계획할 때 최선의 선택이 아닐 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-156">Depending on your existing application's database requirements, and your enterprise requirements and policies, it might not be the best choice when you are planning your migration to the cloud.</span></span>

## <a name="when-to-move-your-original-rdbms-to-a-vm-iaas"></a><span data-ttu-id="fecf9-157">원래 RDBMS를 VM(IaaS)으로 이동하는 시점</span><span class="sxs-lookup"><span data-stu-id="fecf9-157">When to move your original RDBMS to a VM (IaaS)</span></span>

<span data-ttu-id="fecf9-158">마이그레이션 옵션 중 하나는 Oracle, IBM DB2, MySQL, PostgreSQL 또는 SQL Server를 포함한 원래의 관계형 데이터베이스 관리 시스템(RDBMS)을 Azure VM에서 실행 중인 서버와 유사한 서버로 이동하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-158">One of your migration options is to move your original relational database management system (RDBMS), including Oracle, IBM DB2, MySQL, PostgreSQL, or SQL Server, to a similar server that's running on an Azure VM.</span></span> <span data-ttu-id="fecf9-159">전혀 변경하지 않거나 최소한의 변경만으로 가장 빠르게 클라우드로 마이그레이션을 수행해야 하는 기존 애플리케이션이 있으면 클라우드의 IaaS로 직접 마이그레이션하는 것이 적절한 옵션일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-159">If you have existing applications that require the fastest migration to the cloud with minimal changes, or no changes at all, a direct migration to IaaS in the cloud might be a fair option.</span></span> <span data-ttu-id="fecf9-160">이 방법이 클라우드의 모든 이점을 활용하는 최선의 방법은 아니지만 아마도 가장 빠르게 시작하는 방법일 것입니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-160">It might not be the best way to take advantage of all the cloud's benefits, but it's probably the fastest initial path.</span></span>

<span data-ttu-id="fecf9-161">현재 Microsoft Azure에서는 IaaS Vm으로 배포되는 최대 [331개의 다양한 데이터베이스 서버](https://azuremarketplace.microsoft.com/marketplace/apps/category/databases?page=1&subcategories=databases-all)를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-161">Currently, Microsoft Azure supports up to [331 different database servers](https://azuremarketplace.microsoft.com/marketplace/apps/category/databases?page=1&subcategories=databases-all) deployed as IaaS VMs.</span></span> <span data-ttu-id="fecf9-162">여기에는 SQL Server와 Oracle, MySQL, PostgreSQL, IBM DB2와 같은 인기있는 RDBMS와 MongoDB, Cassandra, DataStax, MariaDB, 및 Cloudera와 같은 다양한 NoSQL 데이터베이스가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-162">These include popular RDBMS like SQL Server, Oracle, MySQL, PostgreSQL, and IBM DB2, and many other NoSQL databases like MongoDB, Cassandra, DataStax, MariaDB, and Cloudera.</span></span>

> [!NOTE]
> <span data-ttu-id="fecf9-163">Azure VM으로 사용자 RDBMS 데이터를 이전하는 것이 IaaS인 클라우드로 마이그레이션하는 가장 빠른 방법일 수 있지만, 이 방법을 사용하려면 IT 팀(데이터베이스 관리자와 IT 전문가)에 많은 투자를 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-163">Although moving your RDBMS to an Azure VM might be the fastest way to migrate your data to the cloud (because it is IaaS), this approach requires a significant investment in your IT teams (database administrators and IT pros).</span></span> <span data-ttu-id="fecf9-164">엔터프라이즈 팀은 SQL Server의 고가용성과 재해 복구, 패치를 설정하고 관리할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-164">Enterprise teams need to be able to set up and manage high availability, disaster recovery, and patching for SQL Server.</span></span> <span data-ttu-id="fecf9-165">또한 이 컨텍스트에는 전체 관리 권한이 있는 사용자 지정된 환경이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-165">This context also needs a customized environment, with full administrative rights.</span></span>

## <a name="when-to-migrate-to-sql-server-as-a-vm-iaas"></a><span data-ttu-id="fecf9-166">VM (IaaS)으로 SQL Server로 마이그레이션해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="fecf9-166">When to migrate to SQL Server as a VM (IaaS)</span></span>

<span data-ttu-id="fecf9-167">일반 VM의 SQL Server로 마이그레이션해야 하는 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-167">There might be a few cases where you still need to migrate to SQL Server as a regular VM.</span></span> <span data-ttu-id="fecf9-168">예를 들면 SQL Server Reporting Services를 사용해야하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-168">An example scenario is if you need to use SQL Server Reporting Services.</span></span> <span data-ttu-id="fecf9-169">하지만 대부분의 경우 Azure SQL Database Managed Instance는 온-프레미스 SQL 서버에서 마이그레이션하는 데 필요한 모든 것을 제공할 수 있으므로 SQL Server VM으로의 마이그레이션은 마지막 수단으로 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-169">In most cases, though, Azure SQL Database Managed Instance can provide everything you need to migrate from on-premises SQL servers, so migration to a SQL Server VM should be your last resort to try.</span></span>

## <a name="use-azure-database-migration-service-to-migrate-your-relational-databases-to-azure"></a><span data-ttu-id="fecf9-170">Azure Database Migration Service를 사용하여 Azure에 관계형 데이터베이스 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="fecf9-170">Use Azure Database Migration Service to migrate your relational databases to Azure</span></span> 

<span data-ttu-id="fecf9-171">대상 데이터베이스가 Azure SQL Database나 Azure SQL Database Managed Instance, Azure VM의 SQL Server 중 어느 것이든, Azure Database Migration Service를 사용하여 SQL Server와 Oracle, MySQL과 같은 관계형 데이터베이스를 Azure로 마이그레이션할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-171">You can use Azure Database Migration Service to migrate relational databases like SQL Server, Oracle, and MySQL to Azure, whether your target database is Azure SQL Database, Azure SQL Database Managed Instance, or SQL Server on an Azure VM.</span></span>

<span data-ttu-id="fecf9-172">평가 보고 기능이 있는 자동화된 워크플로는 데이터베이스를 마이그레이션하기 전에 변경해야 할 사항을 안내합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-172">The automated workflow, with assessment reporting, guides you through the changes you need to make before you migrate the database.</span></span> <span data-ttu-id="fecf9-173">준비가 되면 서비스는 원본 데이터베이스를 Azure로 마이그레이션합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-173">When you are ready, the service migrates the source database to Azure.</span></span>

<span data-ttu-id="fecf9-174">원래의 RDBMS를 변경할 때마다 다시 테스트해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-174">Whenever you change an original RDBMS, you might need to retest.</span></span> <span data-ttu-id="fecf9-175">테스트 결과에 따라 응용 프로그램에서 SQL 문장이나 ORM(Object-Relational Mapping) 코드를 변경해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-175">You also might need to change the SQL sentences or Object-Relational Mapping (ORM) code in your application, depending on testing results.</span></span>

<span data-ttu-id="fecf9-176">다른 데이터베이스(예: IBM DB2)가 있고 리프트 앤 시프트 방식을 선택하는 경우, 보다 복잡한 데이터 마이그레이션을 수행하려는 경우가 아니면 Azure에서 이러한 데이터베이스를 IaaS VM으로 계속 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-176">If you have any other database (for example, IBM DB2) and you opt for a lift and shift approach, you might want to continue using those databases as IaaS VMs in Azure, unless you are willing to perform a more complex data migration.</span></span> <span data-ttu-id="fecf9-177">더 복잡한 데이터 마이그레이션은 새로운 스키마와 다양한 프로그래밍 라이브러리를 사용하여 다양한 데이터베이스 유형으로 마이그레이션하기 때문에 추가적인 노력이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-177">A more complex data migration will require additional effort because you'd be migrating to a different database type with new schema and different programming libraries.</span></span>

<span data-ttu-id="fecf9-178">Azure Database Migration Service를 사용하여 데이터베이스를 마이그레이션하는 방법에 알아보려면 [Azure SQL Database Managed Instance와 Azure Database Migration Service를 사용하여 클라우드로 신속하게 이동하기](https://channel9.msdn.com/Events/Build/2017/P4008)를 참조합니다.</span><span class="sxs-lookup"><span data-stu-id="fecf9-178">To learn how to migrate databases by using Azure Database Migration Service, see [Get to the cloud faster with Azure SQL Database Managed Instance and Azure Database Migration Service](https://channel9.msdn.com/Events/Build/2017/P4008).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fecf9-179">추가 자료</span><span class="sxs-lookup"><span data-stu-id="fecf9-179">Additional resources</span></span>

- <span data-ttu-id="fecf9-180">**클라우드 SQL Server 옵션 선택: Azure SQL Database (PaaS) 또는 Azure VM (IaaS)의 SQL Server**</span><span class="sxs-lookup"><span data-stu-id="fecf9-180">**Choose a cloud SQL Server option: Azure SQL Database (PaaS) or SQL Server on Azure VM (IaaS)**</span></span>

    <https://docs.microsoft.com/azure/sql-database/sql-database-paas-vs-sql-server-iaas>

- <span data-ttu-id="fecf9-181">**Azure SQL Database Managed Instance와 Database Migration Service를 사용하여 신속하게 클라우드 이동하기**</span><span class="sxs-lookup"><span data-stu-id="fecf9-181">**Get to the cloud faster with Azure SQL DB Managed Instance and Database Migration Service**</span></span>

    <https://channel9.msdn.com/Events/Build/2017/P4008>

- <span data-ttu-id="fecf9-182">**클라우드 SQL Database로 SQL Server 데이터베이스 마이그레이션**</span><span class="sxs-lookup"><span data-stu-id="fecf9-182">**SQL Server database migration to SQL Database in the cloud**</span></span>

    <https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate>

- <span data-ttu-id="fecf9-183">**Azure SQL Database**</span><span class="sxs-lookup"><span data-stu-id="fecf9-183">**Azure SQL Database**</span></span>

    <https://azure.microsoft.com/services/sql-database/?v=16.50>

- <span data-ttu-id="fecf9-184">**가상 컴퓨터의 SQL Server**</span><span class="sxs-lookup"><span data-stu-id="fecf9-184">**SQL Server on virtual machines**</span></span>

    <https://azure.microsoft.com/services/virtual-machines/sql-server/>

> [!div class="step-by-step"]
> <span data-ttu-id="fecf9-185">[이전](lift-and-shift-existing-apps-azure-iaas.md)
> [다음](modernize-existing-apps-to-cloud-optimized/index.md)</span><span class="sxs-lookup"><span data-stu-id="fecf9-185">[Previous](lift-and-shift-existing-apps-azure-iaas.md)
[Next](modernize-existing-apps-to-cloud-optimized/index.md)</span></span>