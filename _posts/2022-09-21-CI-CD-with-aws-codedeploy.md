---
title: "[DevOps]아마존 AWS를 통한 CI/CD 구축 -2 (with CodeDeploy)"
author: InGyuJang
date: 2022-09-21 17:51:35 +0900
categories: [Tech, DevOps]
tags: [Tech, TIL, DevOps, AWS, Github]
pin: false
---

![image](https://media.giphy.com/media/3o6MbrpSX5lxAzjGXS/giphy.gif)  
오픈 소스들을 통한 (ex - 젠킨스) CI/CD는 커스텀 가능한 파이프라인을 통한 장점과 각각의 CI/CD툴을 선택해서 사용할 수 있다는 장점이 있다. 그렇다면, 최대의 서버리스 서비스를 지원하는 AWS는 과연 어떤 식으로 파이프라인을 구축할 수 있을까?

## 📎 AWS의 CodePipeline

![image](/assets/img/aws-server-form.png)
aws는 CodePipeline이라는 서비스를 통해서 이용자에게 CI/CD 파이프라인을 매니지먼트 할 수 있도록 한다. 각 파이프 라인에서, 어떤 빌드, 어떤 배포를 선택할지 정할 수 있으며, 유동적으로 변경과 재실행이 가능하게 관리 가능하다는 점이 정말로 편리하다.

![스크린샷 2022-12-13 오전 9 48 48](https://user-images.githubusercontent.com/74250270/207199128-f16522fa-119c-4005-b8de-476b226c4a36.png)

전체 파이프라인의 진행 상태도 확인할 수 있으며, 각각 세부적인 파트의 상태또한 확인할 수 있어서 직관적이다.

CodePipeline에서는 코드를 가져올 소스, 코드를 어떤 방식으로 빌드 할지, 코드를 어떤 방식으로 Deploy할지에 관해서 한눈에 파악이 가능하고, 그 정보들을 바탕으로 파이프라인의 흐름이 잘 유지되고 있는지 확인할 수 있다.

🏗 AWS CodeBuild

AWS의 CodeBuild서비스는 말 그대로 코드를 빌드하는 서비스다(이름부터 직관적이다.) 코드를 어떤 환경에서, jar파일을 어떤 방식으로, 어떤 환경변수를 넣어서 사용할지에 대해서 구체적으로 정해줄 수 있다. 만약 application.yml파일을 사용한다면, 어떤 프로파일을, EC2내부에 저장했다면 어떤 위치에 있는 파일을 활용할지에 대해서도 정해줄 수 있다.

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      java: corretto17
  build:
    commands:
      - echo Build Starting on `date`
      - gradle wrap
      - chmod +x ./gradlew
      - ./gradlew build
  post_build:
    commands:
      - echo $(basename ./build/libs/*.jar)
      - pwd

artifacts:
  files:
    - appspec.yml
    - build/libs/*.jar
    - scripts/**
  discard-paths: yes

cache:
  paths:
    - "/root/.gradle/caches/**/*"
```

> buildspec.yml

다음과 같이 `yaml`파일을 작성해서 런타임 버전이라던지, 명령어라던지, 빌드가 끝나고 마무리를 정할 수 있다.

## 🤦 Trouble Shooting

#### AWS 연결 권한 이슈 [해결]

github와 AWS간의 커뮤니케이션 딜레이

#### S3 권한 설정 이슈 [해결]

Github에서 AWS S3버킷으로 보내거나 CodeDeploy를 사용하기 위한 권한, Key가 존재하지 않아서 생김.

AWS를 사용함에 있어서 중요한 것은 IAM을 통한 권한 설정입니다. Deploy를 위한 권한, S3 버킷을 이용하기 위한 권한 등등이 Deploy에 필요로 하기 때문에 이러한 권한이 적절히 부여되어 있는지 확인해야 합니다.

https://goodgid.github.io/Github-Action-CI-CD-AWS-S3/

#### S3 CLIENT_ERROR: repository not found for primary source [해결]

buildspec.yml이 존재하지 않아서 생기는 문제

#### COMMAND_EXECUTION_ERROR: Error while executing command: ./gradlew build. Reason: exit status 1[해결]

Test Code문제 Kotest는 생성자를 받을 수 없으나, Spring은 필요로 한다 ➝ Spring 호환 관련 의존성 추가
testImplementation("io.kotest.extensions:kotest-extensions-spring:1.1.1")
https://jaehhh.tistory.com/118

#### java.lang.ClassNotFoundException: org.gradle.wrapper.GradleWrapperMain [해결]

- gradle wrap 명령어를 buildspec.yml에 써 둘것!

#### AWS Codedeploy agent -ruby 버전 문제 [해결]

최신 ubuntu의 루비에서 호환성 문제가 생기며, 2.7번대의 버전은 설치가 불가능한 문제가 존재한다.
ubuntu 20.04로 EC2 구축 요망

#### Unable to access the artifact with Amazon S3 object key 'iVOLVE-US-Server/BuildArtif/ZrALXLv' located in the Amazon S3 artifact bucket 'ivolve-us-server-deployment-bucket'. The provided role does not have sufficient permissions. [해결]

EC2 - IAM 권한 부여를 통해서 codedeploy agent에게 정확한 IAM 권한을 부여해야한다.
https://jojoldu.tistory.com/283
https://gunbin91.github.io/aws/2020/01/10/aws_2_codedeploy.html

#### war jar 명령어 오타..., .war .jar 배포에 따른 쉘스크립트 차이

이 두가지는 그냥 순수하게 오타의 문제이다. jar로 말고, 설정되어 있는걸 war명령어로 실행해서 계속해서 문제가 발생했다. 오타 주의
