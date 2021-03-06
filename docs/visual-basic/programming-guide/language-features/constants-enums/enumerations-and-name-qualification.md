---
title: 열거형 및 이름 한정
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], enumerations
- Imports statement [Visual Basic], namespace declarations
- declaring namespaces [Visual Basic], enumerations
- name collisions
- ambiguous names [Visual Basic], enumerations
- enumerations [Visual Basic], name qualification
- names [Visual Basic], avoiding conflicts
- namespaces [Visual Basic], declaring
- naming conflicts, enumerations
- naming conflicts, qualifying names
- declaring enumerations
- references [Visual Basic], enumeration members
- naming conventions [Visual Basic], naming conflicts
- declarations [Visual Basic], namespaces
ms.assetid: 08ba2738-df52-4140-bc55-f57c871c9b73
ms.openlocfilehash: 4121266447b771ba954ad52a46e0d8b88de3f9cc
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347490"
---
# <a name="enumerations-and-name-qualification-visual-basic"></a>열거형 및 이름 한정(Visual Basic)
일반적으로 열거형의 멤버를 참조할 때는 열거형 이름으로 멤버 이름을 한 정해야 합니다. 예를 들어 `Days` 열거형의 `Sunday` 멤버를 참조 하려면 다음 구문을 사용 합니다.  
  
 [!code-vb[VbEnumsTask#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#18)]  
  
## <a name="using-the-imports-statement"></a>Imports 문 사용  
 다음 예제와 같이 코드의 네임 스페이스 선언 섹션에 `Imports` 문을 추가 하 여 정규화 된 이름을 사용 하지 않을 수 있습니다.  
  
 [!code-vb[VbEnumsTask#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#22)]  
  
 `Imports` 문은 참조 된 프로젝트 및 어셈블리에서 그리고 문이 표시 되는 모듈과 동일한 프로젝트 내에서 네임 스페이스 이름을 가져옵니다. 이 문이 추가 되 면 다음 예제와 같이 한정자를 사용 하지 않고 열거형 멤버를 참조할 수 있습니다.  
  
 [!code-vb[VbEnumsTask#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#24)]  
  
 관련 상수 집합을 열거형으로 구성 하면 서로 다른 컨텍스트에서 동일한 상수 이름을 사용할 수 있습니다. 예를 들어 `Days`의 weekday 상수에 동일한 이름을 사용 하 고 열거를 `WorkDays` 수 있습니다. 열거형과 함께 `Imports` 문을 사용 하는 경우 모호한 참조가 발생 하지 않도록 주의 해야 합니다. 다음 예제를 참조하세요.  
  
 [!code-vb[VbEnumsTask#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#22)]  
  
 [!code-vb[VbEnumsTask#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#25)]  
  
 `Monday`이 `Days` 열거와 `Workdays` 열거 모두의 멤버인 것으로 가정 하면이 코드는 컴파일러 오류를 생성 합니다. 개별 상수를 참조할 때 모호한 참조를 방지 하려면 해당 열거형을 사용 하 여 상수 이름을 한정 합니다. 다음 코드는 `Days` 및 `WorkDays` 열거형의 `Saturday` 상수를 참조 합니다.  
  
 [!code-vb[VbEnumsTask#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#32)]  
  
## <a name="see-also"></a>참고 항목

- [상수 및 열거형](../../../../visual-basic/language-reference/constants-and-enumerations.md)
- [방법: 열거형 선언](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-declare-enumerations.md)
- [방법: 열거형 멤버 참조](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-refer-to-an-enumeration-member.md)
- [방법: Visual Basic에서 열거형 반복](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-iterate-through-an-enumeration.md)
- [방법: 열거형 값과 연결된 문자열 확인](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [열거형을 사용하는 경우](../../../../visual-basic/programming-guide/language-features/constants-enums/when-to-use-an-enumeration.md)
- [상수 및 리터럴 데이터 형식](../../../../visual-basic/programming-guide/language-features/constants-enums/constant-and-literal-data-types.md)
- [Enum 문](../../../../visual-basic/language-reference/statements/enum-statement.md)
- [Imports 문(.NET 네임스페이스 및 형식)](../../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)
- [데이터 형식](../../../../visual-basic/language-reference/data-types/index.md)
