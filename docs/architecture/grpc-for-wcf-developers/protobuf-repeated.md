---
title: 목록 및 배열에 대 한 반복 되는 필드-WCF 개발자를 위한 gRPC
description: Protobuf에서 수집을 처리 하는 방법 및 이러한 컬렉션이 .NET 컬렉션과 어떻게 관련 되는지를 이해 합니다.
ms.date: 09/09/2019
ms.openlocfilehash: 17c579bc98ba62ea74b9452bdb28efd96fc51406
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73967360"
---
# <a name="repeated-fields-for-lists-and-arrays"></a>목록 및 배열을 위한 반복 필드

목록은 `repeated` prefix 키워드를 사용 하 여 Protobuf에 지정 됩니다. 다음 예에서는 목록을 만드는 방법을 보여 줍니다.

```protobuf
message Person {
    // Other fields elided
    repeated string aliases = 8;
}
```

생성 된 코드에서 `repeated` 필드는 기본 제공 .NET 컬렉션 형식이 아닌 `Google.Protobuf.Collections.RepeatedField<T>` 제네릭 형식으로 표시 됩니다. `RepeatedField<T>` 형식에는 목록을 이진 통신 형식으로 serialize 및 deserialize 하는 데 필요한 코드가 포함 되어 있습니다. <xref:System.Collections.Generic.IList%601> 및 <xref:System.Collections.Generic.IEnumerable%601>와 같은 모든 표준 .NET 컬렉션 인터페이스를 구현 하므로 LINQ 쿼리를 사용 하거나 배열 또는 목록으로 쉽게 변환할 수 있습니다.

>[!div class="step-by-step"]
>[이전](protobuf-nested-types.md)
>[다음](protobuf-reserved.md)
