---
title: 회원관리 예제
author: InGyuJang
date: 2022-07-07 12:44:00 +0900
categories: [Tech, JAVA, Spring]
tags: [JAVA, TIL, Spring, JPA]
pin: false
---
### 회원 도메인과 리포지토리 만들기

Optional은 java8에 들어간 기능으로, NPE오류로 부터 자유롭게 만들어준다.

MVC 패턴을 통해서 회원 도메인과 리포지토리를 생성한다.

HelloController를 통해서 매핑을 받고 API 형태로 리턴하도록 만든다.

MemberRepository를 통해서 MemberRepository의 인터페이스를 생성한다.

MemoryMemberRepository를 통해서 위에서 작성한 인터페이스를 구현한다.

sequence는 키값을 생성해준다.

### 회원 리포지토리 테스트 케이스 작성

cmd - alt - V ⇒ 변수 추출하기

cmd - shift - T ⇒ 해당 클래스 테스트 클래스 생성

cmd -alt -M extract method

ctrl - T refactoring

### 회원 서비스 개발

repository를 가져오고

join이라는 메소드를 생성한다.

optional로 한번 감싸서 받게되면, if문을 따로 사용하여 활용할 필용 없이. ifPrsent 통해서 입력된 이름으로 존재하는 

그리고 validateDuplicateMember(), findMembers()와 findOne()메소드도 생성해준다.

### 회원 서비스 테스트

테스트 코드 작성시에 테스트 이름을 한글로 작성해도 상관없다.

클래스의 빈 곳 아무곳에나 오른쪽 마우스를 클릭해서 create test하면 손 쉽게 테스트를 생성할 수 있다.

```java
try{
  memberService.join(member2);
  fail();
} catch (IllegalStateException e) {
  assertThat(e.getMessage().isEqualTo("이미 존재하는 회원입니다."))
}
```

@어노테이션을 통해서 의존관계를 주입하는 것을 컴포넌트 스캔방식이라고 한다.

@Component 에노테이션이 있으면 스프링 빈에 등록됨, @Component를 포함하는 @Service @Repository @Contoroller도 스프링빈에 등록한다. 단순히 @Autowired 에노테이션으로는 스프링이 관리하지 않는다.

실행하는 어플리케이션의 같은 패키지만 스캔한다.

따로 @Configuration이라는 에노테이션을 통해서 config 클래스 내부에 @Bean 에노테이션을 통해서 연결할 수도 있다.

자바 설정파일을 직접 설정하는 경우, 한 파일 내에서 연결을 관리할 수 있다는 장점이 있다.

@Transcational 에노테이션으로 각 부분 테스트가 끝나고 DB를 롤백하는 방식으로 테스트 한다.

가급적으로는 순수한 단위테스트가 훨씬 좋은 테스트일 확률이 높다.

JDBC템플릿은 템플릿 디자인 패턴을 활용해서 순수 JDBC에서 반복 코드를 많이 제거한 버전이라고 생각하면 편하다.

# JPA

### ORM Object Relational Mapping

어플리케이션의 Class와 RDB의 테이블을 매핑한다는 뜻이며, 객체와 RDB 테이블을 자동으로 영속화 해준다고 생각하면 된다.

**pros**

- 메서드를 통해서 DB를 조작할 수 있음 (비즈니스 로직에 집중)
- 쿼리가 필요로 하는 선언문이 생략되어서 가독성이 높아짐
- 유지보수와 리팩토링에 유리하고, 객체지향적인 코드 작성이 가능함
- DB

특히나 상속관계가 존재하는 클래스와 상속관계라는 개념이 존재하지 않는 DB Query에서 이 사이의 패러다임 차이를 연결해 주는 것이 바로 JPA다

JPA + Spring JAP + Querydsl(동적 쿼리) 

복잡한 경우는 Native쿼리를 통해서 SQL을 작성한다.

# AOP Aspect Oriented Programming

핵심 비즈니스 로직 외의 핵심 관심 사항은 아니지만, 공통의 관심 사항인 경우에 필요로 한다.

모든 메소드의 실행 시간을 측정하는 경우, 모든 비즈니스 로직들에 다 섞어줘야 하고, 유지보수도 힘들고, 별도의 공통 로직으로 만들기 매우 어렵다.

기존의 의존관계는
![image](https://velog.velcdn.com/images/redforest/post/61142279-7258-4e29-b7e4-ff2bcc6396de/image.png)
컨트롤러 → 서비스 이지만
  
![image](https://velog.velcdn.com/images/redforest/post/96559935-08cf-4e4f-b467-dece125316b2/image.png)
AOP 적용 후에는
  
컨트롤러 → 스프링 컨테이너를 통한 프록시 → 실제 서비스  
  
![image](https://velog.velcdn.com/images/redforest/post/d25401d4-99d0-4b31-84bc-e0ca102246a9/image.png)
![image](https://velog.velcdn.com/images/redforest/post/a3ac0e4d-0fa3-4e32-87c1-d179e5ed2114/image.png)
  
DI를 통해서 helloController는 DI를 통해서 Injection을 받기 때문에 프록시로 바꿔치는 방식으로 사용할 수 있다.
