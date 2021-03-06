---
title: 보호된 멤버
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- members [.NET Framework], protected
- protected members
- classes [.NET Framework], unsealed
- classes [.NET Framework], protected members
- unsealed classes
- customizing class behavior
ms.assetid: aa0b58ee-3956-494d-ab48-471ae5db8740
ms.openlocfilehash: 14ef02a760c9d4b77fe058334baffd63fcf29cfd
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709103"
---
# <a name="protected-members"></a>보호된 멤버
보호 된 멤버 자체는 확장성을 제공 하지 않지만 하위 클래스를 보다 강력 하 게 확장할 수 있습니다. 이러한 옵션을 사용 하 여 기본 public 인터페이스를 불필요 하 게 복잡 하 게 하지 않고 고급 사용자 지정 옵션을 노출할 수 있습니다.  
  
 "Protected" 라는 이름은 거짓 보안을 제공할 수 있기 때문에 프레임 워크 디자이너는 보호 된 멤버를 주의 해 서 사용 해야 합니다. 모든 사용자는 봉인 되지 않은 클래스를 서브클래싱하 고 보호 된 멤버에 액세스할 수 있으므로 public 멤버에 사용 되는 동일한 모든 방어 코딩 사례가 보호 된 멤버에 적용 됩니다.  
  
 **✓ CONSIDER** 를 사용 하 여 고급 사용자 지정에 대 한 멤버를 보호 합니다.  
  
 **✓ DO** 보안, 설명서 및 호환성 분석용으로 public으로 봉인 되지 않은 클래스의 보호 된 멤버를 처리 합니다.  
  
 누구나 클래스에서 상속 하 고 보호 된 멤버에 액세스할 수 있습니다.  
  
 *2005, 2009 Microsoft Corporation © 부분입니다. All rights reserved.*  
  
 *Pearson Education, Inc의 동의로 재인쇄. 출처: [Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) 작성자: Krzysztof Cwalina 및 Brad Abrams, 출판 정보: Oct 22, 2008 by Addison-Wesley Professional as part of the Microsoft Windows Development Series.*  
  
## <a name="see-also"></a>참조

- [프레임워크 디자인 지침](../../../docs/standard/design-guidelines/index.md)
- [확장성을 위한 디자인](../../../docs/standard/design-guidelines/designing-for-extensibility.md)
