---
title: Spring 프로젝트 환경설정
author: InGyuJang
date: 2022-07-03 01:02:00 +0900
categories: [Tech, JAVA, Spring]
tags: [JAVA, TIL, Spring]
pin: false
---
start.spring.io를 통해서 스프링 부트로 스프링 프로젝트 생성

프로젝트 호환성 관리는 메이븐이나 그래들 사용하는 툴을 통해서 생성.

Spring boot 버전은 SNAPSHOT과 M$버전은 테스트중인 경우가 많으므로, 다른 꼬리말이 없는 stable 버전으로 생성

group은 보통 기업이름

Artifact는 빌드될 때 나올 결과물

다운 받고 나서, 인텔리 제이에서 open or import를 통해서 gradle 파일을 열어서 연다.

기본적으로 gradle로 자동으로 Depedencies를 설정해주면서. main매서드를 실행하면 Tomcat이라는 웹서버를 띄우면서 스프링 부트도 올라오며 포트 8080에 사이트를 연다.

### 라이브러리 살펴보기

maven이나 gradle은 의존관계를 설정해준다. 단순한 의존관계가 아니라, Spring web이 필요로 하는 의존 그 의존을 물고 물어서 스프링 코어까지 땡겨온다.

예전에는 서버에 웹서버를 설치하지만, 요즘은 소스에서 웹서버 실행 자체를 가지고 있다.

핵심 라이브러리

- 스프링 부트 스타터 웹
    - 톰캣
    - MVC
- 타임리프
- 스프링 부트 + 스프링 코어 + 로깅
    - spring boot
        - spring core
    - spring boot starter logging
        - logback slf4j

테스트 라이브러리

- spring - boot starter test
    - junit 테스트 프레임 워크
    - mockito 목 라이브러리
    - assertj 테스트 코드 작성 도와주는 라이브러리
    - spring test 스프링 통합 테스트 지원

### View 환경설정

src - java - resources - static
해당 경로에는 정적 페이지가 들어간다.

웹 브라우저에서 경로/hello 가 넘어오면, 스프링부트에 내장된 톰캣 서버가 스프링에게 hello를 물어본다.

그러면 스프링은 코드들중에서 GetMapping(”hello”)를 찾고, 모델로 넘어와서 addAttribute로 키와 벨류를 만들고 hello 라는 이름으로 return한다. hello라는 이름을 들고 hello.html에게 넘긴다.

스프링 부트 템플릿 엔진 기본 view mapping

resources:templates/ {name}.html

해당 폴더의 ./gradlew build로 빌드

build/libs/에서 java -jar 빌드된파일.jar를 실행해서 띄운다.

### 정적 컨텐츠

간단한 설명

웹 브라우저가 톰캣 서버에게 html을 요청하면, 톰캣 서버는 스프링 컨테이너에 해당하는 컨트롤러가 없는 것을 확인하고, 리소스에서 static에 존재하는 것을 톰캣이 확인하고 톰캣이 리턴한다.

*컨트롤러가 우선순위를 먼저 가지고 정적 컨텐츠가 가진다*

### MVC와 템플릿 엔진

@RequestPara을 통해서 파라미터를 요청하고 리턴한다.

타임 리프는 th태그의 내용을 작동시키고, 값이 없으면 정적인 내용을 보여준다.

intelilj에서 cmd+p로 파라미터 정보를 볼 수 있다.

uri에 ?name=spring 이라고 하면 파라미터가 name의 키에 값을 spring으로 바꾼다.

웹브라우저가 톰캣에게 요청을 보내면, 톰캣은 요청을 컨트롤러에 있는것을 보고 넘긴다. 그러면 컨트롤러는 모델을 변환해서 viewResolver가 타임리프를 통해 전환을 하고, 변환하고 웹브라우저에게 톰캣을 통해 보낸다.

### API

@ResponseBody를 통해서 html body부에 데이터를 직접 넘겨준다는 의미를 가진다. → HTML의 마크업 없이 return에 데이터를 그대로 보낸다.

Getter와 Setter를 통한 자바 bean 규약(property 접근방식)을 통해서 생성한다.

만약 @ResposeBody를 보면 viewResolver를 사용하지 않고 바로 던지는데, 객체라면 Json Converter나 StringConverter를 통해서 컨버팅을 하고 던진다.

클라이언트의 Accept헤더를 조합하여서 xml과 같은 JSON이외의 형태로 전달할 수 있다.
