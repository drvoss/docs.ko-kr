---
title: Entity SQL 개요
ms.date: 03/30/2017
ms.assetid: f0bb8120-e709-40a3-ac1e-5520dc47477d
ms.openlocfilehash: e0f154ab2d9db1a1fdbaba8c72bc7e43ad71ee0b
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73738495"
---
# <a name="entity-sql-overview"></a>Entity SQL 개요
[!INCLUDE[esql](../../../../../../includes/esql-md.md)]은 Entity Framework에서 개념적 모델을 쿼리 하는 데 사용할 수 있는 SQL 유사 언어입니다. 개념적 모델은 데이터를 엔터티 및 관계로 표현 하며, [!INCLUDE[esql](../../../../../../includes/esql-md.md)]을 사용 하면 SQL을 사용한 사용자에 게 친숙 한 형식으로 엔터티 및 관계를 쿼리할 수 있습니다.  
      
 Entity Framework는 저장소 관련 데이터 공급자와 함께 작동 하 여 일반 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]를 저장소 관련 쿼리로 변환 합니다. EntityClient 공급자는 엔터티 모델에 대해 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 명령을 실행하고 스칼라 결과, 결과 집합, 개체 그래프를 포함한 다양한 데이터 형식을 반환하는 방법을 제공합니다. <xref:System.Data.EntityClient.EntityCommand> 개체를 생성할 때 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 쿼리 문자열을 해당 <xref:System.Data.EntityClient.EntityCommand.CommandText%2A?displayProperty=nameWithType> 속성에 할당하여 저장 프로시저 이름 또는 쿼리 텍스트를 지정할 수 있습니다. <xref:System.Data.EntityClient.EntityDataReader>는 EDM에 대한 <xref:System.Data.EntityClient.EntityCommand> 실행 결과를 노출합니다. <xref:System.Data.EntityClient.EntityDataReader>를 반환하는 명령을 실행하려면 <xref:System.Data.EntityClient.EntityCommand.ExecuteReader%2A>를 호출합니다.  
  
 EntityClient 공급자 외에도 Entity Framework을 사용 하면 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]를 사용 하 여 개념적 모델에 대해 쿼리를 실행 하 고 엔터티 형식의 인스턴스인 강력한 형식의 CLR 개체로 데이터를 반환할 수 있습니다. 자세한 내용은 [개체 작업](../working-with-objects.md)을 참조 하세요.  
  
 이 단원에서는 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]에 대한 개념 정보를 제공합니다.  
  
## <a name="in-this-section"></a>단원 내용  
 [Entity SQL과 Transact-SQL의 차이점](how-entity-sql-differs-from-transact-sql.md)  
  
 [Entity SQL 빠른 참조](entity-sql-quick-reference.md)  
  
 [형식 시스템](type-system-entity-sql.md)  
  
 [형식 정의](type-definitions-entity-sql.md)  
  
 [형식 생성](constructing-types-entity-sql.md)  
  
 [쿼리 계획 캐싱](query-plan-caching-entity-sql.md)  
  
 [네임스페이스](namespaces-entity-sql.md)  
  
 [식별자](identifiers-entity-sql.md)  
  
 [매개 변수](parameters-entity-sql.md)  
  
 [변수](variables-entity-sql.md)  
  
 [지원되지 않는 식](unsupported-expressions-entity-sql.md)  
  
 [리터럴](literals-entity-sql.md)  
  
 [Null 리터럴 및 형식 유추](null-literals-and-type-inference-entity-sql.md)  
  
 [입력 문자 집합](input-character-set-entity-sql.md)  
  
 [쿼리 식](query-expressions-entity-sql.md)  
  
 [함수](functions-entity-sql.md)  
  
 [연산자 우선 순위](operator-precedence-entity-sql.md)  
  
 [페이징](paging-entity-sql.md)  
  
 [비교 의미 체계](comparison-semantics-entity-sql.md)  
  
 [중첩 Entity SQL 쿼리 작성](composing-nested-entity-sql-queries.md)  
  
 [null 허용 구조적 형식](nullable-structured-types-entity-sql.md)  
  
## <a name="see-also"></a>참조

- [엔터티 SQL 참조](entity-sql-reference.md)
- [Entity SQL 언어](entity-sql-language.md)
- [CSDL, SSDL 및 MSL 사양](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)
