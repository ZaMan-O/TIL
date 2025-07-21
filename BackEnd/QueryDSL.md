# JPQL이란?
JPQL(Java Persistence Query Language)는 테이블을 대상으로 쿼리하는 것이 아닌 엔티티 객체를 대상으로 쿼리합니다.

SQL과 비슷한 문법을 가지며 JPQL은 결국 SQL로 변환됩니다.

 

JPA에서 제공하는 CRUD와 같은 메서드 호출만으로 섬세한 쿼리 작성이 어려운 이유로 나오게 되었습니다.

## JPQL 특징
- 테이블이 아닌 객체를 검색하는 객체지향 쿼리

- JPA는 JPQL을 분석하여 SQL을 생성한 뒤 DB에서 조회

기본 문법
- 전체 회원 조회

        SELECT m FROM Member m
- 조건 조회

        SELECT m FROM Member m WHERE m.age > 17
- 이름만 조회

        SELECT m.name FROM Member m

        String jpql = "SELECT m FROM Member m WHERE m.name = :name";
        List<Member> members = em.createQuery(jpql, Member.class)
            .setParameter("name", "배재")
            .getResultList();
## SQL과 다른 점
### 대소문자 구분
엔티티와 속성은 대소문자를 구분한다.

엔티티 이름인 Member, 그리고 Member의 속성 name은 대소문자를 구분해줘야 한다.

반면에 SELECT, FROM, AS 같은 JPQL 키워드는 대소문자를 구분하지 않아도 된다.
### 엔티티 이름
JPQL에서 사용한 Member는 클래스 이름이 아닌 엔티티 이름이다. 엔티티 이름은 @Entity(name="task")로 설정 가능하다.

name 속성을 생략하면 기본 값으로 클래스 이름을 사용한다.
### 별칭
JPQL에서 엔티티의 별칭은 필수적으로 명시해야 한다.

별칭을 명시하는 AS 키워드는 생략할 수 있다.

# QueryDSL이란?
Spring Data JPA가 기본적으로 제공해주는 CRUD 메서드 및 쿼리 메서드 기능을 사용하더라도, 원하는 조건의 데이터를 수집하기 위해서는 필연적으로 JPQL을 작성하게 됩니다.

간단한 로직을 작성하는데 큰 문제는 없으나, 복잡한 로직의 경우 쿼리 문자열이 상당히 길어집니다.

JPQL 문자열에 오타 혹은 문법적인 오류가 존재하는 경우, 만약 정적 쿼리라면 어플리케이션 로딩 시점에 이를 발견할 수 있으나 그 외는 런타임 시점에서 에러가 발생합니다.

이러한 문제를 어느 정도 해소하는데 기여하는 프레임워크가 바로 QueryDSL입니다. QueryDSL은 JPQL을 자바 코드로 안전하게 작성할 수 있게 해주는 라이브러리입니다.

        String jpql = "SELECT m FROM Member m WHERE m.age > 20";
        List<Member> members = em.createQuery(jpql, Member.class).getResultList();
        QMember m = QMember.member;
___
        List<Member> members = queryFactory
            .selectFrom(m)
            .where(m.age.gt(17)) // age가 없거나 타입이 다르면 컴파일 오류 발생
            .fetch();
- IDE 자동 완성 됨
- 안전하고 가독성도 좋아요