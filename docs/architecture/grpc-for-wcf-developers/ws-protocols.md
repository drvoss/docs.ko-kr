---
title: WS-* 프로토콜-WCF 개발자를 위한 gRPC
description: WCF에서 지원 되는 WS-* 프로토콜 및 gRPC에서 사용할 수 있는 대체 항목의 검토
author: markrendle
ms.date: 09/02/2019
ms.openlocfilehash: 4e7b80df182fb69cc51e14738e59ad87efaf5dd2
ms.sourcegitcommit: 337bdc5a463875daf2cc6883e5a2da97d56f5000
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2019
ms.locfileid: "73841314"
---
# <a name="ws--protocols"></a>WS\* 프로토콜

WCF (Windows Communication Foundation)를 사용 하는 경우의 실질적인 이점 중 하나는 기존 _WS-\*_ 표준 프로토콜을 많이 지원 하기 위한 것입니다. 이 섹션에서는 gRPC가 동일한 WS\* 프로토콜을 관리 하는 방법에 대해 간략하게 설명 하 고 대안이 없을 때 사용할 수 있는 옵션을 설명 합니다.

## <a name="metadata-exchange---ws-policy-ws-discovery-and-so-on"></a>메타 데이터 교환-정책, WS-검색 등

SOAP 서비스는 데이터 형식, 작업 또는 통신 옵션과 같은 정보를 사용 하 여 WSDL (웹 서비스 기술 언어) 스키마 문서를 노출 합니다. 이 스키마는 클라이언트 코드를 생성 하는 데 사용할 수 있습니다.

gRPC는 동일한 `.proto` 파일에서 서버 및 클라이언트를 생성할 때 가장 잘 작동 하지만 서버 리플렉션 선택적 확장 프로그램은 실행 중인 서버에서 동적 정보를 노출 하는 방법을 제공 합니다. 자세한 내용은 [grpc. 리플렉션](https://nuget.org/packages/Grpc.Reflection) NuGet 패키지 및 [Grpc C# 서버 리플렉션](https://github.com/grpc/grpc/blob/master/doc/csharp/server_reflection.md) 문서를 참조 하세요.

WS 검색 프로토콜은 로컬 네트워크에서 서비스를 찾는 데 사용 됩니다. gRPC 서비스는 일반적으로 DNS 또는 Consul 또는 사육 사와 같은 서비스 레지스트리를 사용 하 여 배치 됩니다.

## <a name="security--ws-security-ws-federation-xml-encryption-and-so-on"></a>보안 – WS-SECURITY, WS-FEDERATION, XML 암호화 등

보안, 인증 및 권한 부여에 대 한 자세한 내용은 [6 장](security.md)에서 설명 합니다. 그러나 WCF와는 달리 gRPC는 WS-SECURITY, WS 페더레이션 또는 XML 암호화를 지원 하지 않는다는 점을 기억해 야 합니다. 그러나 gRPC는 뛰어난 보안 기능을 제공 합니다. TLS를 통한 HTTP/2를 사용 하는 경우 모든 gRPC 네트워크 트래픽이 자동으로 암호화 되며, 상호 클라이언트/서버 인증에 X509 인증서를 사용할 수 있습니다.

## <a name="ws-reliablemessaging"></a>WS-ReliableMessaging

gRPC는 WS-RELIABLEMESSAGING에 해당 하는 기능을 제공 하지 않습니다. 재시도 의미 체계가 같은 라이브러리를 사용 하 여 코드에서 처리 되어야 [합니다.](https://github.com/App-vNext/Polly) Kubernetes 또는 유사한 오케스트레이션 환경에서 실행 하는 경우 [서비스 메시](service-mesh.md) 는 서비스 간에 신뢰할 수 있는 메시징을 제공 하는 데도 도움이 됩니다.

## <a name="ws-transaction-ws-coordination"></a>WS 트랜잭션, WS 조정

WCF의 분산 트랜잭션 구현에서는 Windows의 Microsoft DTC(Distributed Transaction Coordinator) 또는 MSDTC를 사용 합니다. SQL Server, MSMQ 또는 Windows 파일 시스템과 같이 구체적으로 지 원하는 리소스 관리자와 함께 작동 합니다. 최신 마이크로 서비스 세계에서 사용 되는 광범위 한 기술 덕분에 아직 동일 하지 않습니다. 트랜잭션에 대 한 설명은 [부록 a](appendix.md)를 참조 하세요.

>[!div class="step-by-step"]
>[이전](error-handling.md)
>[다음](migrate-wcf-to-grpc.md)
