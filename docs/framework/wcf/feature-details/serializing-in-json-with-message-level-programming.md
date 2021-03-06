---
title: 메시지 수준의 프로그래밍으로 JSON으로 serialize
ms.date: 03/30/2017
ms.assetid: 5f940ba2-57ee-4c49-a779-957c5e7e71fa
ms.openlocfilehash: 1492ba138b5ae706e3ae70f416e95565d4b60d78
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976116"
---
# <a name="serializing-in-json-with-message-level-programming"></a>메시지 수준의 프로그래밍으로 JSON으로 serialize
WCF는 JSON 형식의 데이터 serialize를 지원합니다. 이 항목에서는 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>를 사용하여 WCF가 형식을 serialize하도록 하는 방법에 대해 설명합니다.  
  
## <a name="typed-message-programming"></a>형식화된 메시지 프로그래밍  
 서비스 작업에 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 또는 <xref:System.ServiceModel.Web.WebGetAttribute>가 적용되면 <xref:System.ServiceModel.Web.WebInvokeAttribute>를 사용합니다. 두 특성을 사용하여 `RequestFormat` 및 `ResponseFormat`을 지정할 수 있습니다. 요청 및 응답에 JSON을 사용 합니다. 둘 다 `WebMessageFormat.Json`로 설정 합니다.  JSON을 사용 하려면 <xref:System.ServiceModel.Description.WebHttpBehavior>를 자동으로 구성 하는 <xref:System.ServiceModel.WebHttpBinding>를 사용 해야 합니다. WCF 직렬화에 대 한 자세한 내용은 [serialization 및 Deserialization](../../../../docs/framework/wcf/feature-details/serialization-and-deserialization.md)을 참조 하세요. JSON 및 WCF에 대 한 자세한 내용은 [서비스 스테이션-wcf를 사용 하는 RESTful Services 소개](https://docs.microsoft.com/archive/msdn-magazine/2009/january/service-station-an-introduction-to-restful-services-with-wcf)를 참조 하세요.  
  
> [!IMPORTANT]
> JSON을 사용하려면 SOAP 통신을 지원하지 않는 <xref:System.ServiceModel.WebHttpBinding> 및 <xref:System.ServiceModel.Description.WebHttpBehavior>를 사용해야 합니다. <xref:System.ServiceModel.WebHttpBinding>와 통신 하는 서비스는 서비스 메타 데이터 노출을 지원 하지 않으므로 Visual Studio의 서비스 참조 추가 기능 또는 svcutil 명령줄 도구를 사용 하 여 클라이언트 쪽 프록시를 생성할 수 없습니다. <xref:System.ServiceModel.WebHttpBinding>를 사용 하는 서비스를 프로그래밍 방식으로 호출 하는 방법에 대 한 자세한 내용은 [WCF에서 REST 서비스](https://blogs.msdn.microsoft.com/pedram/2008/04/21/how-to-consume-rest-services-with-wcf/)를 사용 하는 방법을 참조 하세요.  
  
## <a name="untyped-message-programming"></a>형식화되지 않은 메시지 프로그래밍  
 형식화되지 않은 메시기 개체로 직접 작업할 때는 형식화되지 않은 메시지의 속성을 명시적으로 설정하여 JSON으로 serialize해야 합니다. 다음 코드 조각에서는 이를 수행하는 방법을 보여 줍니다.  
  
```csharp
 Message response = Message.CreateMessage(  
                  MessageVersion.None,    // No SOAP message version  
                             "*",                     // SOAP action, ignored since this is JSON  
                             "Response string: JSON format specified", // Message body  
                             new DataContractJsonSerializer(typeof(string))); // Specify DataContractJsonSerializer  
      response.Properties.Add( WebBodyFormatMessageProperty.Name,   
                    new WebBodyFormatMessageProperty(WebContentFormat.Json)); // Use JSON format  
```  
  
## <a name="see-also"></a>참조

- [AJAX 통합 및 JSON 지원](../../../../docs/framework/wcf/feature-details/ajax-integration-and-json-support.md)
- [독립 실행형 JSON Serialization](../../../../docs/framework/wcf/feature-details/stand-alone-json-serialization.md)
- [JSON Serialization](../../../../docs/framework/wcf/samples/json-serialization.md)
