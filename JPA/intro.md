
행동을 중심으로 놓는 객체지향과 데이터 중심에 놓는 관계형 데이터베이스 사이의 거리를 줄이기에는 한계가 있다. 유연하고 확장 가능한 객체지향 설계를 할수록 객체 구조와 데이터 모델 사이의 거리는 멀어지고, 복잡한 매핑에 점점 종속적인 애플리케이션으로 완성된다. JPA는 이러한 `객체 관계 임피던스 불일치 문제`를 해결하기 위해 자바 진영에서 발표한 ORM 표준이다.   

> 애플리케이션 객체 지향 언어: Java, Scala, C++ ..
> 데이터베이스 관계형 DB: Oracle, MySQL...


### 객체 vs 관계형 DB의 패러다임이 불일치
__객체 지향 프로그래밍__은 `추상화`, `캡슐화`, `정보은닉`, `상속`, `다형성` 등 시스템의 복잡성을 제어할 수 있는 장치를 제어한다. (하지만! 관계형 DB에서는 이러한 특징을 고려하지 않는다.)

__관계형 데이터베이스__는 데이터를 안전하게 잘 정리하는 것이 목표이다.

우리는 객체지향 프로그래밍을 하여 관계형 DB 저장하는데 이 과정에서 SQL 쿼리로 변환하여 RDB에 저장한다. (개발자가 하는 부분)

#### [차이]

##### 1. 상속

RDB에는 상속이란 개념이 존재하지 않으며, 비슷한 개념으로 table의 슈퍼타입, 서브타입 관계가 존재한다. 이를 이용하여 할 수는 있으나 객체 모델링을 할수록 바인딩하는 과정이 더 많고 복잡하여 DB에 저장할 객체에는 일반적으로 상속 관계를 쓰지 않는다

##### 2. 연관관계

- 객체 : `참조` 를 이용한다
- 테이블: `외래키` 사용 ( join )

##### 3. 데이터 타입

##### 4. 식별방법

- 객체:  `객체 그래프 탐색`이 가능하다.  (`객체 그래프 탐색`: 객체는 자유롭게 객체 그래프를 탐색 할 수 있다. )
- SQL 의존 :  처음 실행하는 SQL에 따라 탐색 범위를 결정이 된다.
  - 문제점
    - 엔티티 신뢰성 문제 발생
    - 모든 객체를 미리 로딩해야만 한다.
    - 계층형 아키텍처에서 __계층 분할__이 어려워 진다.



----

# JPA

- Java Persistence API

- 자바 진영의 ORM 기술 표준

>  ORM 이란?
>
> Object-relational mapping (객체 관계 매핑)
>
> 각자의 패러다임(객체 지향 프로그래밍, RDB)에 맞게 설계하도록 도와준다.
>
> ORM 프레임워크는 중간에서 매핑하는 역할을 하며 대중적인 언어에서는 대부분 ORM 기술이 존재한다.

ORM을 사용해 객체와 RDB 사이에 존재하는 개념과 접근을 객체지향적으로 다루기 위한 기술.

### JPA 표준 명세

- JPA 인터페이스의 모음
- JPA 2.1 표준 명세를 구현한 3가지 구현체 : 하이버네이트, EclipseLink, DataNucleus

![image-20190720172920845](/Users/juyoungyoo/Library/Application Support/typora-user-images/image-20190720172920845.png)

### 사용해야하는 이유?

- 도메인 데이터 중심 개발에서 객체 중심의 개발이 가능하도록 한다.
- 불필요한 CRUD를 자동으로 생성해 줌으로서 `생산성`에 좋다.

```
저장 : entityManager.persist(member)
조회 : entityManager.find(Member.class, memberId)
수정 : member.setName("변경할 이름")
삭제 : entityManager.remove(member)
```

- 유지보수 ex) 필드 추가
- 객체지향 프로그래밍과 RDB의 패러다임의 불일치를 해결해준다. (상속, 연관관계, 객체 그래프 탐색)
- 성능

``` 
1. 1차 캐시와 동일성(identity) 보장
 - 같은 트랜잭션 안에서는 같은 엔티티를 보장 (조회 시 성능 향상)
 - DB Isolation Level이 Read commit이어도 애플리케이션에서 Repeatable Read 보장

2. 트랜잭션을 지원하는 쓰기 지원 (transactional write-behind) : buffer writing ...
 - insert 인 경우: 트랜잭션 커밋 전까지 insert sql을 모아 JDBC Batch SQL 기능을 이용하여 한번에 SQL 전송한다.  
 - update 인 경우: update, delete로 인한 row lock 시간 최소화

3. 지연 로딩(Lazy Loading)
 - 지연 로딩 : 객체가 실제로 사용될 때 로딩
 - 즉시 로딩 : (egar) : Join SQL로 한번에 연관된 객체까지 미리 조회하는 방식
```

- 데이터 접근을 추상화하여 한 벤더에 의존하지 않고 벤더의 독립성을 가질 수 있다.


[자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
