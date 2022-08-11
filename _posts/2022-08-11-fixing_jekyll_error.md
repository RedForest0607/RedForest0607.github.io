---
title: Jekylㅣ테마 오류 수정
author: InGyuJang
date: 2022-08-11 14:54:10 +0900
categories: [Blogging, Chat]
tags: [Blog, Github, Issues]
pin: false
---
![image](https://velog.velcdn.com/images/a87380/post/cc31493a-1290-4870-b332-b00152291854/image.png)
깃허브 블로그 테마를 jekyll-theme-chirpy로 바꾸면서 다음과 같은 에러가 계속해서 발생했다.
내가 변경하려는 테마는 기본적으로 CI/CD가 설정되어 있어서 블로그 설정이 제대로 되어 있지 않다면 동작하지 않는다는 것을 알았다.
![image](https://velog.velcdn.com/images/redforest/post/d456169b-e2d9-4151-9ea6-75f9fecd8550/image.png)
다음과 같이 커밋되는 코드를 검토하여서, 해당하는 코드가 문제가 없을때만 gh-pages라는 브랜치로 푸쉬되어서 실제 페이지에 반영된다.
이러한 과정이 반영되지 않아서 테마가 적용되지 않고 index.html의 내용이 날것 그대로 보여지는 것이었다.

### 원인이 무엇일까? 🔨
나의 경우는 Gemfiles.lock을 gitignore에 반영하지 않아서 생기는 문제였다. 해당하는 파일을 gitignore에 올려주니 제대로 커밋되었다.
또한, 프로필 사진으로 올려둔 PNG이미지가, config파일에서 호출했지만, 디렉토리상에 실제로 넣어두지 않아서 오류가 발생한 부분도 있었다.

커밋되었을때 ✅ 가 잘 뜨는지 까지 확인하고 문제가 생긴다면 배포 어느 부분에서 문제가 생겼는지 확인해야 한다.

![image](https://velog.velcdn.com/images/redforest/post/ebb3f39e-ac13-45bd-a680-a0f6a17f1392/image.png)
> 편안~

깃허브 블로그는 항상 손 댈때마다 문제가 조금씩이라도 발생한다. 🤦