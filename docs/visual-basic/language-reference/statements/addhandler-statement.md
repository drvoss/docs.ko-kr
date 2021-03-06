---
title: AddHandler 문
ms.date: 07/20/2015
f1_keywords:
- vb.AddHandlerMethod
- addhandler
- vb.addhandler
helpviewer_keywords:
- AddHandler statement [Visual Basic]
ms.assetid: cfe69799-2a0f-42c0-a99e-09fed954da01
ms.openlocfilehash: c110116af75d4fb39c016b8d6afcdb707fa6599b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350184"
---
# <a name="addhandler-statement"></a>AddHandler 문
런타임에 이벤트를 이벤트 처리기와 연결 합니다.  
  
## <a name="syntax"></a>구문  
  
```vb  
AddHandler event, AddressOf eventhandler  
```  
  
## <a name="parts"></a>요소  
|||
|---|---|
|이벤트|처리할 이벤트의 이름입니다.|  
|`eventhandler`|이벤트를 처리 하는 프로시저의 이름입니다.|
|||
  
## <a name="remarks"></a>설명  
 `AddHandler` 및 `RemoveHandler` 문을 사용 하면 프로그램을 실행 하는 동안 언제 든 지 이벤트 처리를 시작 하 고 중지할 수 있습니다.  
  
 `eventhandler` 프로시저의 서명은 이벤트 `event`의 시그니처와 일치 해야 합니다.  
  
 `Handles` 키워드와 `AddHandler` 문 모두 특정 프로시저에서 특정 이벤트를 처리하도록 지정하는 데 사용할 수 있지만 차이가 있습니다. `AddHandler` 문은 런타임에 프로시저를 이벤트에 연결합니다. 특정 이벤트를 처리하도록 지정하는 프로시저를 정의할 때 `Handles` 키워드를 사용합니다. 자세한 내용은 [핸들](../../../visual-basic/language-reference/statements/handles-clause.md)을 참조 하세요.  
  
> [!NOTE]
> 사용자 지정 이벤트의 경우 `AddHandler` 문은 이벤트의 `AddHandler` 접근자를 호출 합니다. 사용자 지정 이벤트에 대 한 자세한 내용은 [이벤트 문](../../../visual-basic/language-reference/statements/event-statement.md)을 참조 하십시오.  
  
## <a name="example"></a>예제  
 [!code-vb[VbVbalrEvents#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#17)]  
  
## <a name="see-also"></a>참고자료

- [RemoveHandler 문](../../../visual-basic/language-reference/statements/removehandler-statement.md)
- [!](../../../visual-basic/language-reference/statements/handles-clause.md)
- [Event 문](../../../visual-basic/language-reference/statements/event-statement.md)
- [이벤트](../../../visual-basic/programming-guide/language-features/events/index.md)
