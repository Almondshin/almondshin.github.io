<hr>
title: Introduction to JPA 02
permalink: /posts/2024/06/2024-06-23-Introduction-to-JPA-02/
date: 2024-06-23
tags:
  - Spring Data JPA
  - JPA
  - EntityManager
  - Annotation
<hr>

JPA의 세 가지 핵심 측면인 객체-테이블 매핑, 필드-컬럼 매핑, 기본 키 매핑에 대해 알아보려고 합니다.

## JPA 매핑 소개

Java Persistence API(JPA)는 Java 애플리케이션에서 관계형 데이터를 관리하기 위한 중요한 프레임워크입니다. 객체 지향 프로그래밍과 관계형 데이터베이스 간의 간극을 메우기 위해 표준화된 접근 방식을 제공하여 Java 객체를 데이터베이스 테이블에 매핑할 수 있도록 도와줍니다.

<hr>-

## 1. 객체-테이블 매핑

### 객체-테이블 매핑이란 무엇인가요?


객체-테이블 매핑은 Java 클래스(엔티티)를 관계형 데이터베이스의 테이블과 연결하는 과정입니다. 이 매핑을 통해 JPA 프로바이더가 엔티티 인스턴스를 관리하고 데이터베이스 작업을 원활하게 수행할 수 있습니다.

### 객체-테이블 매핑을 왜 사용하나요?

- **상호작용의 용이성**: 개발자가 Java 객체를 직접 사용하여 데이터베이스 작업을 단순화합니다.
- **데이터 일관성**: Java 객체의 변경 사항이 데이터베이스에 반영되도록 보장합니다.
- **데이터베이스 독립성**: JPA 프로바이더가 데이터베이스별 세부 사항을 처리하므로 애플리케이션이 데이터베이스에 독립적일 수 있습니다.

### 애노테이션과 옵션

- **@Entity**: 이 애노테이션은 클래스를 엔티티로 표시합니다.
- **@Table**: 엔티티가 매핑되는 데이터베이스의 테이블을 지정합니다. 지정하지 않을경우 클래스 네임으로 됩니다.

```java
import javax.persistence.Entity;
import javax.persistence.Table;

@Entity
@Table(name = "users")
public class User {
    // 필드, 게터, 세터
}
```

- **@Entity**: 이 클래스가 엔티티임을 나타냅니다.
- **@Table(name = "users")**: 이 엔티티가 "users" 테이블에 매핑됨을 지정합니다.

**@Table**의 옵션:

- **name**: 테이블 이름을 지정합니다. 예: `@Table(name = "users")`.
- **catalog**: 데이터베이스의 카탈로그를 지정합니다. 예: `@Table(catalog = "myCatalog")`.
- **schema**: 데이터베이스의 스키마를 지정합니다. 예: `@Table(schema = "mySchema")`.
- **uniqueConstraints**: 하나 이상의 컬럼에 대한 고유 제약 조건을 정의합니다. 예: `@Table(uniqueConstraints = @UniqueConstraint(columnNames = {"username", "email"}))`.

## 카탈로그란?

카탈로그(Catalog)는 데이터베이스 시스템 내에서 객체들을 논리적으로 그룹화하는 컨테이너입니다. 이를 통해 여러 데이터베이스 객체(테이블, 뷰 등)를 하나의 카탈로그 안에 포함시켜 관리할 수 있습니다.

### 카탈로그의 역할

1. **논리적 그룹화**: 카탈로그는 데이터베이스 객체들을 논리적으로 그룹화하여 관리합니다. 이를 통해 객체들의 관리 및 접근이 용이해집니다.
2. **독립성 보장**: 각 카탈로그는 독립적인 데이터베이스 객체들을 포함할 수 있습니다. 따라서, 동일한 데이터베이스 서버 내에서도 카탈로그 간에 객체들이 독립적으로 운영될 수 있습니다.
3. **스키마와의 관계**: 카탈로그는 여러 개의 스키마를 포함할 수 있습니다. 스키마는 카탈로그 내에서 더 세분화된 그룹을 의미하며, 각 스키마는 자체적인 테이블, 뷰 등을 가질 수 있습니다.

### 카탈로그 사용 예

예를 들어, 한 데이터베이스 서버에서 여러 애플리케이션이 서로 다른 데이터베이스를 사용할 경우, 각 애플리케이션은 자체 카탈로그를 가질 수 있습니다. 이렇게 하면 데이터가 논리적으로 분리되어 관리됩니다.

- **회사 데이터베이스 서버**: 회사 데이터베이스 서버에서 HR, Sales, Finance와 같은 서로 다른 부서를 위한 카탈로그를 운영할 수 있습니다.
    - `HR` 카탈로그: 직원 정보, 급여 정보 등.
    - `Sales` 카탈로그: 고객 정보, 주문 정보 등.
    - `Finance` 카탈로그: 회계 정보, 지출 정보 등.

각 카탈로그는 독립적으로 운영되며, 각기 다른 스키마를 포함할 수 있습니다.

### JPA에서의 카탈로그 사용

JPA에서는 `@Table` 애너테이션의 `catalog` 속성을 통해 엔티티가 특정 카탈로그에 속하도록 지정할 수 있습니다. 이를 통해 데이터베이스 내에서 엔티티가 속할 카탈로그를 명확하게 지정할 수 있습니다.

> 데이터베이스 카탈로그는 메타데이터들로 구성된 데이터베이스 내의 인스턴스이다.  
> 즉, 카탈로그에 저장된 데이터 = 메타데이터인 것이다.

한마디로 정리하자면 카탈로그는 스키마 정보들을 포함하고 있으며, 카탈로그는 모든 종류의 스키마를 가진 공간입니다.

### MySQL에서의 카탈로그

일반적으로 사용하는 MySQL은 3계층 구조 데이터베이스로 데이터베이스와 스키마를 동의어로 사용하고 있어 카탈로그 계층이 없습니다. 그래서 3계층 구조인 MySQL에서는 카탈로그가 사용되지 않습니다.

## 계층 구조 예시

- **4계층 구조**: 인스턴스 - 데이터베이스 (카탈로그)- 스키마 - 테이블
- **3계층 구조**: 인스턴스 - 스키마 - 테이블

MySQL에서는 3계층 구조를 사용하므로 카탈로그 계층 없이 인스턴스, 스키마, 테이블로 구성됩니다. 이 때문에 MySQL에서는 `@Table` 애너테이션의 `catalog` 속성을 사용하지 않습니다.

프로젝트에서는 각 Java 클래스를 데이터베이스의 해당 테이블에 명확히 매핑하기 위해 `@Entity` 및 `@Table` 애노테이션을 사용하여 구조화되고 일관된 데이터 레이어를 보장합니다.

<hr>

## 2. 필드-컬럼 매핑

### 필드-컬럼 매핑이란 무엇인가요?

필드-컬럼 매핑은 Java 엔티티의 각 필드를 데이터베이스 테이블의 특정 컬럼에 연결하는 과정입니다. 이 매핑을 통해 JPA 프로바이더는 Java 객체에 저장된 데이터를 데이터베이스에 정확히 저장하고, 데이터베이스로부터 필요한 데이터를 올바르게 검색할 수 있게 됩니다. 필드-컬럼 매핑은 JPA의 핵심 기능 중 하나로, 객체와 관계형 데이터베이스 간의 원활한 데이터 교환을 가능하게 합니다.

### 필드-컬럼 매핑을 왜 사용하나요?

- **정확성**: 엔티티 필드가 데이터베이스 컬럼에 정확하게 매핑되도록 보장합니다.
- **커스터마이징**: 컬럼의 속성(이름, 타입, 제약 조건 등)을 지정할 수 있습니다.
- **데이터 무결성**: 제약 조건을 통해 데이터 무결성을 유지합니다.

### 애노테이션과 옵션

- **@Column**: 필드를 데이터베이스의 컬럼에 매핑합니다.

```java
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Table;

@Entity
@Table(name = "users")
public class User {
    @Column(name = "username", nullable = false, unique = true)
    private String username;

    @Column(name = "password")
    private String password;

    // 게터와 세터
}

```

- **@Column(name = "username", nullable = false, unique = true)**: `username` 필드를 `username` 컬럼에 매핑하며, 이 컬럼이 null 값을 가질 수 없고 고유해야 함을 지정합니다.
- **@Column(name = "password")**: `password` 필드를 `password` 컬럼에 매핑합니다.

**@Column**의 옵션:

- **name**: 컬럼 이름을 지정합니다. 예: `@Column(name = "username")`.
- **nullable**: 컬럼이 null 값을 가질 수 있는지 여부를 지정합니다. 예: `@Column(nullable = false)`.
- **unique**: 컬럼 값이 고유해야 하는지 여부를 지정합니다. 예: `@Column(unique = true)`.
- **length**: 컬럼의 길이를 지정합니다(문자열 값의 경우). 예: `@Column(length = 50)`.
- **columnDefinition**: 컬럼에 대한 DDL 생성 시 사용되는 SQL 조각을 정의합니다. 예: `@Column(columnDefinition = "TEXT")`.
- **insertable**: 컬럼이 INSERT 작업에 포함될지 여부를 지정합니다. 예: `@Column(insertable = false)`.
- **updatable**: 컬럼이 UPDATE 작업에 포함될지 여부를 지정합니다. 예: `@Column(updatable = false)`.


데이터베이스 스키마가 애플리케이션 요구 사항과 일치하도록 `@Column` 애노테이션을 사용하여 엔티티 필드와 데이터베이스 컬럼 간의 정확한 매핑을 정의합니다.


<hr>


## 3. 기본 키 매핑

### 기본 키 매핑이란 무엇인가요?

기본 키 매핑은 엔티티의 기본 키가 데이터베이스 테이블의 해당 기본 키 컬럼에 어떻게 매핑되는지를 정의합니다. 기본 키는 테이블의 각 레코드를 고유하게 식별합니다.

### 기본 키 매핑을 왜 사용하나요?

- **고유성**: 각 엔티티 인스턴스가 고유한 식별자를 가지도록 보장합니다.
- **정체성**: 엔티티 인스턴스를 쉽게 검색하고 관리할 수 있습니다.
- **참조 무결성**: 기본 키 연관을 통해 엔티티 간의 관계를 유지합니다.

### 애노테이션과 옵션

- **@Id**: 엔티티의 기본 키를 지정합니다.
- **@GeneratedValue**: 기본 키 값이 생성되는 방식을 지정합니다.

**@GeneratedValue**의 전략 옵션:

- **GenerationType.AUTO**: JPA 프로바이더가 데이터베이스에 적합한 기본 키 생성 전략을 자동으로 선택합니다.
- **GenerationType.IDENTITY**: 데이터베이스의 IDENTITY 컬럼을 사용하여 기본 키 값을 자동으로 생성합니다.
- **GenerationType.SEQUENCE**: 데이터베이스 시퀀스를 사용하여 기본 키 값을 생성합니다. 이 경우 시퀀스를 정의해야 합니다.
- **GenerationType.TABLE**: 키 값을 생성하기 위해 별도의 키 생성 테이블을 사용합니다.

```java 
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "username", nullable = false, unique = true)
    private String username;

    @Column(name = "password")
    private String password;

    // 게터와 세터
}

```

- **@Id**: `id` 필드를 기본 키로 지정합니다.
- **@GeneratedValue(strategy = GenerationType.IDENTITY)**: 기본 키 값이 데이터베이스에서 자동으로 생성됨을 지정합니다. 주로 MySQL 같은 데이터베이스에서 사용됩니다.


프로젝트에서는 `@Id`와 `@GeneratedValue` 애노테이션을 사용하여 기본 키를 효율적으로 관리하고 각 엔티티 인스턴스가 고유하게 식별되도록 보장합니다.

JPA 매핑은 Java 애플리케이션이 관계형 데이터베이스와 상호작용하는 데 있어 중요한 역할을 합니다. 객체-테이블, 필드-컬럼, 기본 키 매핑을 정확히 구현하면, 데이터 일관성과 무결성을 유지하면서도 유지보수가 용이한 데이터 액세스 레이어를 만들 수 있습니다. 이러한 매핑을 이해하고 올바르게 활용함으로써 개발자는 원활하고 신뢰할 수 있는 데이터베이스 작업을 수행할 수 있습니다.