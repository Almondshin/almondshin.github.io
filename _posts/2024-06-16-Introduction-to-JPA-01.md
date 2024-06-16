---
title: Introduction to JPA 01
permalink: /posts/2024/06/2024-06-16-Introduction-to-JPA-01/
date: 2024-06-16
tags:
  - Spring Data JPA
  - JPA
  - EntityManager
  - Annotation
---

JPA에 대해 소개하고 사용 이유에 대해 알아보려고 합니다.

# JPA(Java Persistence API) 소개 및 사용 이유

JPA는 자바 애플리케이션에서 데이터베이스와의 상호 작용을 간단하게 하기 위해 사용되는 프레임워크로, 자바의 표준 명세입니다. 이 글에서는 JPA의 주요 개념, 사용 이유, 그리고 실무에서의 활용 방법에 대해 설명하겠습니다.

## 표준 명세란?

표준 명세는 인터페이스들의 모음으로, 다양한 구현체들이 이를 기반으로 동작합니다. 대표적인 JPA의 구현체는 다음과 같습니다:

- **Hibernate**: 가장 널리 사용되는 JPA 구현체입니다.
- **EclipseLink**: Eclipse 재단에서 제공하는 구현체입니다.
- **DataNucleus**: 다양한 데이터 저장소를 지원하는 구현체입니다.

## JPA를 사용하는 이유

### 1. 객체 중심 개발

JPA는 SQL 중심적인 개발에서 객체 중심으로 개발을 가능하게 합니다. 이를 통해 개발자가 데이터베이스의 테이블 구조보다는 비즈니스 로직에 더 집중할 수 있습니다.

### 2. 생산성

JPA는 간단한 CRUD (Create, Read, Update, Delete) 작업을 쉽게 처리할 수 있는 API를 제공합니다.

```java
// 저장
entityManager.persist(member);

// 조회
Member member = entityManager.find(Member.class, memberId);

// 수정
member.setName("변경할 이름");

// 삭제
entityManager.remove(member);
```

### 3. 유지보수

기존 필드를 변경할 때 모든 SQL을 수정할 필요 없이 엔티티 클래스만 수정하면 됩니다.

```java
public class Member {
    private String memberId;
    private String name; // < 추가
}
```

### 4. 패러다임의 불일치 해결

객체와 관계형 데이터베이스 간의 패러다임 불일치를 해결하여, 객체 지향적인 코드를 작성할 수 있습니다.

### 5. 성능

JPA는 다양한 성능 최적화 기능을 제공합니다. 예를 들어, 지연 로딩(Lazy Loading)은 객체가 실제로 사용될 때 프록시 객체를 초기화하여 사용합니다.

### 6. 데이터 접근 추상화와 벤더 독립성

JPA는 데이터베이스 벤더에 종속되지 않고, 코드 변경 없이 다양한 데이터베이스를 지원할 수 있습니다.

### 7. 표준화

JPA는 표준 명세를 따르므로, 다양한 구현체 간의 호환성이 보장됩니다.

---

## EntityManager란?

EntityManager는 JPA의 핵심 인터페이스로, 엔티티를 관리하고 데이터베이스와의 상호 작용을 담당합니다. EntityManager를 통해 엔티티를 데이터베이스에 저장, 수정, 삭제, 조회할 수 있습니다.

### EntityManager의 주요 기능

1. **엔티티의 생명 주기 관리**: 엔티티를 영속성 컨텍스트에 포함시키거나 제외시킬 수 있습니다.
2. **트랜잭션 관리**: 엔티티의 상태를 변경하고 데이터베이스에 반영하는 트랜잭션을 시작하고 종료할 수 있습니다.
3. **쿼리 실행**: JPQL(Java Persistence Query Language)을 사용하여 데이터베이스에 질의를 수행할 수 있습니다.


```java
    // EntityManager
    EntityManager entityManager = entityManagerFactory.createEntityManager();
    entityManager.getTransaction().begin();
    
    // 엔티티 저장
    entityManager.persist(newEntity);
    
    // 엔티티 조회
    MyEntity entity = entityManager.find(MyEntity.class, entityId);
    
    // 엔티티 수정
    entity.setName("newName");
    
    // 엔티티 삭제
    entityManager.remove(entity);
    
    entityManager.getTransaction().commit();
    entityManager.close();
```

---

## 요즘 JPA는 어떻게 활용될까?

JPA는 객체 지향적인 개발을 가능하게 하고 생산성을 높이기 때문에 많은 프로젝트에서 널리 사용되고 있습니다. 특히 Spring Framework와 함께 사용되면서 그 활용도가 더욱 높아졌습니다. 그러나 JPA의 기본 제공 기능만으로는 실무에서 발생하는 모든 요구사항을 처리하기 어렵기 때문에 다양한 확장과 최적화 기법이 개발되었습니다.

### Spring Data JPA

Spring Data JPA는 JPA를 더욱 편리하게 사용하기 위한 라이브러리로, 복잡한 데이터 접근 로직을 간소화하고 표준화된 방법으로 처리할 수 있게 해줍니다. Spring Data JPA를 사용하면 기본적인 CRUD 작업은 물론, 복잡한 쿼리 작성도 간단해집니다.


```java
public interface MemberRepository extends JpaRepository<Member, Long> {
    List<Member> findByLastName(String lastName);
}
```

이처럼 JpaRepository 인터페이스를 상속받아 간단한 메소드 선언만으로 다양한 쿼리를 작성할 수 있습니다. 또한, Spring Data JPA는 자동으로 트랜잭션 관리를 해주기 때문에 개발자가 직접 트랜잭션을 처리할 필요가 없습니다.

---

## 직접 EntityManager를 사용한 개발의 단점

JPA의 핵심 인터페이스인 EntityManager를 직접 사용하는 경우, 여러 단점이 존재합니다. EntityManager를 직접 사용하면 트랜잭션 관리, 엔티티 상태 관리, 쿼리 실행 등을 개발자가 직접 처리해야 하므로 코드의 복잡성이 증가합니다. 반복적으로 사용되는 코드 패턴(예: 트랜잭션 시작 및 종료, 예외 처리 등)이 많아져 코드 중복이 발생할 수 있으며, EntityManager를 직접 사용한 코드는 단위 테스트 작성이 어렵습니다. 데이터베이스와의 상호 작용을 모킹(Mock)하기 어렵기 때문입니다. 또한, 수동으로 트랜잭션을 관리하다 보면 트랜잭션 경계를 명확히 구분하기 어렵고, 실수할 가능성이 높아집니다. EntityManager를 직접 사용하면 비즈니스 로직과 데이터 접근 로직이 혼재되어 코드의 가독성이 떨어지고 유지 보수가 어렵습니다.

## 왜 EntityManager를 직접 사용하지 않는가?

### Spring Data JPA의 등장

이러한 단점들을 극복하기 위해, Spring Data JPA가 등장했습니다. Spring Data JPA는 JPA의 추상화를 제공하여 복잡한 EntityManager 작업을 간소화합니다. 기본적인 CRUD 작업은 JpaRepository 인터페이스를 상속받기만 하면 됩니다.

Spring Data JPA는 데이터 접근 로직과 비즈니스 로직을 명확하게 분리하여 코드의 가독성과 유지 보수성을 향상시킵니다. 또한, 트랜잭션 관리를 자동으로 처리하여 @Transactional 애노테이션을 통해 트랜잭션 경계를 지정할 수 있으며, 롤백이나 커밋을 신경 쓸 필요가 없습니다. Spring Data JPA는 테스트 작성이 용이하도록 설계되었으며, Mock 프레임워크와 함께 사용하면 데이터베이스와의 상호 작용을 쉽게 모킹할 수 있어 단위 테스트 작성이 수월합니다. 또한, Spring Data JPA는 활발한 커뮤니티와 광범위한 문서, 다양한 예제 코드 등을 통해 개발 지원을 받기 쉽습니다.

주요 개념들로는 Entity, EntityManager, Repository, @Transactional 등이 있습니다. Entity는 데이터베이스 테이블과 매핑되는 자바 클래스이며, EntityManager는 엔티티를 관리하고 데이터베이스와의 상호 작용을 담당하는 JPA의 핵심 인터페이스입니다. Repository는 데이터 접근 로직을 추상화한 인터페이스이며, Spring Data JPA는 JpaRepository, CrudRepository 등 다양한 리포지토리 인터페이스를 제공합니다. @Transactional은 메소드나 클래스에 적용하여 트랜잭션 경계를 지정하는 애노테이션입니다. Spring Data JPA는 이 애노테이션을 통해 트랜잭션을 자동으로 관리합니다.

## JPA 영속성 관리

### 영속성 컨텍스트

영속성 컨텍스트는 "엔티티를 영구 저장하는 환경"을 의미합니다. 엔티티는 영속성 컨텍스트에 올라가면 자동으로 관리됩니다. 예를 들어, `findById`, `findAll`, `findBy...` 메소드를 통해 조회된 엔티티는 자동으로 영속성 컨텍스트에 올라가 관리됩니다. `save` 메소드는 새로운 엔티티의 경우 `EntityManager.persist()`를, 이미 존재하는 엔티티의 경우 `EntityManager.merge()`를 호출합니다.

### 영속성 컨텍스트의 이점

- **1차 캐시**: 데이터 조회 시 1차 캐시를 활용하여 성능을 최적화합니다.
- **동일성 보장**: 동일한 트랜잭션 내에서 동일한 엔티티 인스턴스를 보장합니다.
- **트랜잭션을 지원하는 쓰기 지연**: 트랜잭션이 커밋될 때까지 SQL 실행을 지연시킵니다.
- **변경 감지 (Dirty Checking)**: 엔티티의 변경 사항을 자동으로 감지하여 데이터베이스에 반영합니다.
- **지연 로딩**: 실제로 필요한 시점에 데이터를 로딩합니다.

## 엔티티 수정과 변경 감지

JPA는 엔티티의 변경 사항을 자동으로 감지하고, 트랜잭션이 커밋되는 시점에 해당 변경 사항을 데이터베이스에 반영하는 기능을 제공합니다. 이를 변경 감지(Dirty Checking)라고 합니다.


```java
@Transactional
public void updateEntity(Long entityId) {
    MyEntity entity = entityManager.find(MyEntity.class, entityId);
    entity.setSomeField("newValue");
    // 트랜잭션이 커밋되는 시점에 변경 사항이 반영됩니다.
}

```

## JPQL(Java Persistence Query Language)

JPQL은 JPA에서 제공하는 객체 지향 쿼리 언어로, 데이터베이스에 독립적인 방식으로 데이터 조회, 업데이트, 삭제 작업을 수행할 수 있도록 해줍니다. 하지만 최근에는 Querydsl과 같은 더 강력한 동적 쿼리 작성을 지원하는 라이브러리가 등장하면서 JPQL의 사용이 줄어들고 있습니다.

EntityManager를 직접 사용하는 대신 Spring Data JPA를 사용하면 코드의 간결성, 유지 보수성, 테스트 용이성 등이 크게 향상되며, 이는 개발 생산성을 높이는 데 기여합니다. JPA와 Spring Data JPA를 활용하여 객체 지향적인 개발을 하고, 유지 보수성과 성능을 동시에 향상시킬 수 있습니다.
