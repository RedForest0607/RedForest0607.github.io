---
title: "[DevOps]아마존 AWS를 통한 CI/CD 구축 (with Github Actions)"
author: InGyuJang
date: 2022-09-16 17:51:35 +0900
categories: [Tech, DevOps]
tags: [Tech, TIL, DevOps, AWS, Github]
pin: false
---

![image](https://media.giphy.com/media/3o6MbrpSX5lxAzjGXS/giphy.gif)

# 📌 AWS의 CI/CD

최대의 `SaaS`서비스인 아마존 역시 CI/CD 서비스를 제공한다. 젠킨스와 같이 다양한 커스터마이징이나, 개인 서버에 파이프라인 전체를 저장할 수 있지는 않지만, AWS의 다양한 Scalable한 서비스들과 함꼐 사용하면 커다란 시너지 효과를 낼 수 있다.

## 📎 AWS를 이용한 CI/CD 파이프 라인
![image](https://user-images.githubusercontent.com/74250270/207194885-d7ef7df6-8f09-44ad-966b-700babf0af31.png)  
초기에 생각했던 파이프라인은 다음과 같다. Github Actions를 통해서 빌드를 하고 빌드한 항목을 그대로 Deploy를 하는 방식이었는데, 이러한 방식을 선택하게 된 이유는
1. 코드가 올라가는 깃허브 내부에서 Handle하는게 더 유용하다고 느껴졌다.
2. AWS의 서비스들에 대한 지식이 부족했다.
정도의 이유들로 정해놨다. 2편에서 작성하겠지만, AWS 측의 CI/CD 파이프라인 관리도 매우 훌륭해서, 충분히 활용한 여지가 있다는 것을 알고, 아예 전체 파이프라인을 AWS를 이용한 방식으로 잡아버렸다.  

사실 Github Actions를 많이 활용을 못해보았다. 거의 설계단계에서, 회사내에서 AWS사용에 훨씬 큰 이점이 있기 때문에, AWS로 거의 바로 넘어갔고, Github Actions는 스크립트 작성 시작 도중에 넘어가는 바람에 거의 활용하지 못해서, Github Actions는 개인적으로 공부를 할 예정이다.