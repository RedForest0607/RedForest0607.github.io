---
title: "Kotlin + Spring을 사용기"
author: InGyuJang
date: 2022-09-07 18:20:20 +0900
categories: [Blogging, Greetings]
tags: [세미나, PeachTri]
pin: false
---

![image](https://media.giphy.com/media/3orieM6nWrbIISDDy0/giphy.gif)

## 코프링 탐험기 🚵

코틀린이라는 언어는, 스프링도 익숙하지 않은 나에게 굉장히 큰 도전이었다. 함수가 1급 시민이라는 점, 명시적인 Type과 Null-Safety도 어색했다. 겪어보면 코딩하면서 뜨던 많은 빨간줄들은 아마 자바를 사용하면서는 런타임에서 터질 오류였을지도 모른다.

하지만 아직도 어색한 코틀린이기 때문에, 코틀린(정확히는 코프링)을 사용하면서 내가 기록하고 싶은 주의점들에 대해서 적어놓으려고 한다.

#### Ko-Pring에서 생성자 주입 방법

Java와 달리 Kotlin-Spring에서는 좀 다른 형태로 생성자를 주입하게 된다.

#### Ko-Pring에서 Kotest를 사용해야 하는 이유

Kotlin은 DSL의 적극적인 사용이 가능한 언어로, 일반적인 JUnit과 같은 JAVA기반의 테스트들은 반복적인 코드 사용으로 인해서 Kotlin의 강점이 많이 빛바랜다.  
Junit에서 자주 사용되는

#### Kotest 작성시 zero - args

Kotest는 기본적으로 `argument`를 따로 받지 않는다, 하지만 스프링에서 생성자를 주입한다던지 다른 주입을 위해서 생성자단에 입력해줘야 할 때, zero - args 오류가 뜨게 된다.

### 그 외의 오류들

#### spring.jpa.hibernate.ddl-auto

#### JPA 와 SQL 구문 충돌

![HEX코드 등장](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b571671b-a291-4e58-9ad1-869f71ba786b/Untitled.png)

| HEX코드로 저장되어서 Padding값이 지정되어 있는 모습

https://dblog94.tistory.com/entry/JPA-UUID%EB%A1%9C-findBy-%EC%A1%B0%ED%9A%8C%EA%B0%80-%EC%95%88%EB%90%98%EB%8A%94-%EC%9D%B4%EC%9C%A0
