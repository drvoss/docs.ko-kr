---
title: '방법: 문자열 구문 분석(C#)'
ms.date: 07/20/2015
ms.assetid: 81e5686c-9658-42d8-a7e3-b11be0a2c98b
ms.openlocfilehash: 086a4baecee9ee927b08d6da53d16324ef32e8a8
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2019
ms.locfileid: "74140984"
---
# <a name="how-to-parse-a-string-c"></a>방법: 문자열 구문 분석(C#)

이 항목에서는 C#에서 문자열의 구문을 분석하여 XML 트리를 만드는 방법을 보여 줍니다.

## <a name="example"></a>예

다음 C# 코드에서는 XML 문자열의 구문을 분석하는 방법을 보여줍니다.

```csharp
XElement contacts = XElement.Parse(
    @"<Contacts>
        <Contact>
            <Name>Patrick Hines</Name>
            <Phone Type=""home"">206-555-0144</Phone>
            <Phone Type=""work"">425-555-0145</Phone>
            <Address>
            <Street1>123 Main St</Street1>
            <City>Mercer Island</City>
            <State>WA</State>
            <Postal>68042</Postal>
            </Address>
            <NetWorth>10</NetWorth>
        </Contact>
        <Contact>
            <Name>Gretchen Rivas</Name>
            <Phone Type=""mobile"">206-555-0163</Phone>
            <Address>
            <Street1>123 Main St</Street1>
            <City>Mercer Island</City>
            <State>WA</State>
            <Postal>68042</Postal>
            </Address>
            <NetWorth>11</NetWorth>
        </Contact>
    </Contacts>");
Console.WriteLine(contacts);
```

루트 `Contacts` 노드에는 두 개의 `Contact` 노드가 있습니다. 구문 분석된 XML에서 특정 데이터에 액세스하려면 [XElement.Elements()](xref:System.Xml.Linq.XContainer.Elements) 메서드를 사용합니다. 이 경우에는 루트 `Contacts` 노드의 자식 요소를 반환합니다. 다음 예제에서는 첫 번째 `Contact` 노드를 콘솔에 출력합니다.

```csharp
List<XElement> contactNodes = contacts.Elements("Contact").ToList();
Console.WriteLine(contactNodes[0]);
```

## <a name="see-also"></a>참고 항목

- [특정 특성으로 요소를 찾는 방법(C#)](how-to-find-an-element-with-a-specific-attribute.md)
