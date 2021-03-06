---
title: 컨테이너 기반 애플리케이션의 Azure 컴퓨팅 플랫폼 선택
description: Azure Cloud 및 Windows 컨테이너를 사용하여 기존 .NET 애플리케이션 현대화 | 컨테이너 기반 애플리케이션용 Azure 컴퓨팅 플랫폼 선택
ms.date: 05/04/2018
ms.openlocfilehash: 079c9c5ca02b6dc75214d63cb59afdead03d3190
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73737007"
---
# <a name="choosing-azure-compute-platforms-for-container-based-applications"></a>컨테이너 기반 애플리케이션의 Azure 컴퓨팅 플랫폼 선택

이전 섹션을 읽으면서 알았듯이 Azure는 여러 선택 항목을 제공하는 개방 클라우드입니다. 사용자 요구에 가장 적합한 기능을 사용할 수 있지만, 컨테이너화된 애플리케이션에 어떤 제품/기술을 사용해야 하는지 결정하는 문제도 대두됩니다.

*기본* 권장 사항으로 이 지침에서 권장하는 기본 기준은 다음과 같습니다.

- **단일 모놀리식 앱:** Azure App Service 선택
- **N 계층 앱:** 하나 이상의 백 엔드 서비스가 있는 경우 Azure Kubernetes Service(AKS) 또는 App Service와 같은 오케스트레이터를 선택
- **마이크로 서비스:** AKS 또는 Azure Web Apps for Containers를 선택
- **서버리스 함수 및 이벤트 처리기:** Azure Functions를 선택
- **대규모 일괄 처리:** Azure Batch를 선택

그러나 제품 선택은 특정 애플리케이션의 요구 사항 및 특성에 따라 달라지므로 이러한 권장 사항은 참고만 해야 합니다. 처음에는 비슷한 유형으로 보일 수 있어도 애플리케이션은 다 다릅니다.

애플리케이션의 요구 사항을 심층적으로 분석한 후에는 제품 선택이 달라질 수 있습니다. 하지만 시작점으로, 특정 우선 순위에 따라 평가 및 테스트를 시작할 수 있는 초기 지침을 마련하는 것이 좋습니다.

그림 1에서 다양한 종류의 앱과 이상적인 Azure 호스팅 시나리오에 대한 분석을 볼 수 있습니다.

![다양한 앱에 가장 적합한 Azure 호스팅 시나리오의 표](./media/choosing-azure-compute-options-for-container-based-applications/azure-hosting-scenarios-for-apps.png)

> [!div class="step-by-step"]
> [이전](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
> [다음](build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud.md)
