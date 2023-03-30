---
title: '[BLOG] Git vs Perforce'
author: InGyuJang
date: 2022-08-16 12:00:00 +0900
categories: [Tech, Theory]
tags: [TIL, Tech]
pin: false
---

# :pushpin: Git vs Perforce
![img](https://media.giphy.com/media/BDUlTr7FR9wWJtoor4/giphy.gif)
> 상태관리 툴을 비교해보자

학교의 졸업 프로젝트를 진행할 때, 게임 개발이다 보니 Git이 아닌 다른 방식으로 상태관리를 하고 싶었다.
항상 개인 프로젝트를 할 때 Git을 이용해서 상태관리를 했지만, 과연 Perforce라는 다른 툴, 그것도 내가 직접 AWS에 서버를 파서 관리하는 상태는 어떤 느낌일지에 대해서 좀 적어보려고 한다.

## 📎 편의성의 Git

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Git-logo.svg/2560px-Git-logo.svg.png)
Git은 일단 `무료`라는 점이 엄청나게 매력적이다. 자그마한, 그것도 아직 수익이 따로 발생하지 않아서 추가적인 비용이 발생하는 것이 두려운 개인 개발자들에게 Git은 정말 매력적인 도구라고 생각한다. 커다란 규모의 인원이 아닌 소규모의 인원들로도, 충분히 버전의 최신화에 관해서 관리할 수 있기 때문에 Git - Github로 이어지는 조합이 참 매력적이라고 생각한다.
VScode등의 툴과 척척 붙는 것도 장점이고, CLI를 이용한 이용법을 찾는게 정말 쉽다는 장점이 있다.(터미널을 이용해서 정말 빠르게 컨트롤이 가능하다.)

## :paperclip: 규모의 Perforce
![img](https://www.perforce.com/sites/default/files/styles/social_preview_image/public/image/2021-03/image-blog-what-is-perforce.jpg?itok=oA5lThve)
Perforce는 규모있고 다양한 큰 파일을 관리하기 정말 편리하다. 이 장점만 봐도 게임개발에 적합하다는 것을 알 수 있다. 다양한 파일과 트렌젝션들, 파일들의 용량에 대해서 아무리 크고 잦은 변화라도 빠르게 반영한다. 또한, 각 파일들에 대해서 개발자들이 독점적으로 Lock을 걸고 사용하기 때문에 Step이 엉키는 경우가 없었다.
또한 언리얼에서 자체적으로 밀어주는 점이 있다 보니, 퍼포스가 언리얼과 잘 붙어 다니는 점도 매력적이었다. 3명이라는 적은 팀원이었지만, 다양한 방식의 상태관리를 체험하고 싶어서 사용하게 되었다. AWS를 통해서 매달 비용이 들어가는 부분이 있었지만, 그 부분을 감수하고서라도, 개발경험이(특히나 게임이) 많지 않았던 우리 팀에서 많은 소음 없이 게임을 개발할 수 있었다고 생각한다.
Perforce의  단점이라고 한다면 CLI가 상대적으로 매우 안좋다는 점이다. AWS에서 연동해서 AWS자체에서 문제를 처리한 경우가 훨씬 많다. 완전히 파일을 갈아 엎는 경우에도 AWS 콘솔 상에서 퍼포스 저장소를 지우고 새로 파는 경우가 훨씬 잦았다.

## 🤦 Perforce vs Git
가장 중요한점은 Perforces는 중앙서버에 커밋이 등록 된다는 것이고, Git은 Reop단계의 권한이 찢어지 경우 상호 의존성 문제가 발생할 수 있다는 점이다. 또한 짧은 시간동안 계속해서 커밋이 일어나는데, 그것에 대해서 실시간으로 빠르게 반영할 수 있는 (특히나 커다란 용량의 파일들) Perforce가 좋다고 생각했다. 
실제로 적용해보니, CheckOut이라는 시스템이 불편하지만, 또 이 CheckOut이라는 점이 안정적으로 게임을 개발할 수 있었다고 생각한다.
간단하게 Refresh만 해줘도 이미지부터 모델링까지 많은 파일들이 안정적으로 관리되어서 Unreal을 이용하는 개발자라면, 한번쯤은 이용해봐도 참 좋은 시스템이라고 생각한다.
단점은 Git에 비해서 문제점 해결이 쉽지 않았다. 이유없는 버전이슈나 접속이슈들(대게는 개개인의 컴퓨터 네트워크나 방화벽 문제였던 것으로 기억한다.) 혹은 무료티어인 헬릭스로 인해서 생기는 문제들이 좀 있었지만, 잘 핸들링 해서 결국 제출까지 이어질 수 있었다.
