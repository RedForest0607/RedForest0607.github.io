---
title: "[NodeJS] NodeJS는 어떻게 동작할까? - 작성중"
author: InGyuJang
date: 2022-03-30 11:42:00 +0900
categories: [ProgrammingLanguage, NodeJS]
tags: [JavaScript, ProgrammingLanguage, NodeJS]
pin: false
---
![싱글스레드에 꾸겨넣는 이벤트들](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNDJlNWYyYzkxMmI1ZWY1NGZiOThmODc0MDI5NzIyNjYwOTYwNTJlYSZjdD1n/3o6Mbfd5fQszubehmE/giphy.gif)
# 📌 NodeJS는 어떻게 돌아가는 걸까?
최근 `NestJS`를 공부해보고 싶어서, 자바스크립트를 한번 다시 돌이켜보고, 타입스크립트도 공부해보려는 요량으로 있었다. 스프링과 상당히 유사하게 돌아가는(것 처럼 보이는) 프레임워크로 꼽히는 `NestJS`가 과연 어떤식으로 돌아갈까 보려고 하다가 결국 NodeJS가 어떤식으로 돌아가는지 찾아보고 (사실상 JVM공부 마냥 들어가는 느낌이다.) 정리하려고 이 포스트를 작성한다.

> 해당포스트는 여러 정보를 기반으로 작성하며, 앞으로 계속해서 수정하려고 한다. 


`Java + Spring` 스택의 코드 예시이다.

```java
package com.example.springboot;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

	@GetMapping("/")
	public String index() {
		return "Greetings from Spring Boot!";
	}

}
```
그리고 `NestJS` 스택의 코드 예시이다.

```javascript
import { Controller, Get } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
}
```
컨트롤러 샘플만 보면 정말 비슷한 것 같다.  
NestJS를 써본 분들이라면, 저런 컨트롤러쪽에 provider같은게 더 추가되어야 한다는 점을 알겠지만 일단은 생략한다.
> 와 뭐야 스프링이네

근데 문제는 이녀석들의 언어가 문제이다. `JS(TS)`와 `JAVA` 더 나아가서 `Node`와 `JVM`의 차이는 생각보다 크고, 이 다른점들이 각각의 프레임워크에도 어느정도 녹아있는 느낌이다. 

## 📎 그럼 노드는 어떤게 다른걸까?
`Spring`이라는 프레임워크는 `JVM`이라는 가상머신에서 돌아가는 `JAVA`라는 언어에 뿌리를 두고 있다. 동기적, 멀티스레드의 성향이 나타나고, 최근에는 `WebFlux`를 통해서 비동기 처리에도 박차를 가하고 있다. 전체적으로 웹개발을 위해 필요한 모든 기능이 전부 들어가 있는 패키지에 가깝다. `Security`와 같은 보안까지 빠짐없이 전부 챙겨주는 든든한 구성이며, 엔터프라이즈급 프로젝트나, 보안이 정말 중요한 프로젝트에서는 이런 구성이 엄청나게 큰 도움이 된다.  
  
`NodeJS`는 `"싱글스레드, 이벤트기반, 비동기"`를 바탕으로 두고 있다. 태생이 스크립트 언어이며, 브라우저 `V8`엔진을 통해 돌아가도록 디자인된 자바스크립틀 바탕으로 두고 있기 때문에, 노드도 이러한 JS의 기반을 무시할 수 없었을 것이다. 그래서 `Nest`의 코드 이곳 저곳에도 이러한 특징이 묻어나게 된다.  
  
`자바스크립트`라는 언어의 구동방식 특징이 바로 브라우저상에서 돌아가는 언어라는 점이다. 다른 컴퓨터의 많은 리소스들을 활용할 수 있었던 다른 언어들과는 다르게, `자바스크립트` 언어자체가 싱글 스레드로 돌아가고, 중간의 이벤트들은 API로 보내놓고 이벤트 루프를 통해서 다시 받아오는 방식의 비동기 처리를 통해 진행되었다. 이런 웹 브라우저의 환경 (`V8`엔진이 동작하는 환경)이 아닌, 컴퓨터의 리소스와 직접적으로 맞닿아 있는 NodeJS는 과연 어떤식으로 동작할까?
여기서 핵심적으로 이용되는 라이브러리가 바로 `libuv`이다. 


## 🤦 Trouble Shooting
