---
title: '[PeachTri] PeachTri GCP와 DB 설정'
date: 2022-08-11 23:26:00 +0900
categories: [Blogging, PeachTri, Iride-Scent]
img : Seminar_banner_small.png
tags: [세미나, PeachTri, DB, Iride-Scent]
pin: false
---
# GCP Google Cloud Platform

![image](https://media.giphy.com/media/y47oj4ptjPm5W/giphy.gif)
> 우리는 파이어베이스라는 쉬운 길을 버리고 어려운 길을 선택했다.

먼저 데이터베이스를 올릴 클라우드 서비스를 선택했다. 프로젝트에서 AWS를 선택한 것과는 다르게 이번에는 `Google Cloud Platform` 을 통해서 진행하기로 결정했다. ~~~무료 티어가 있다는 이야기를 시작으로 GCP에 더 이끌린 느낌도 있지만~~~, aws에서 이미 퍼포스등을 통한 환경 설정을 해본 나는 GCP라는 새로운 플랫폼을 통해서 설정해보고 싶다는 생각이 들었다.

#### GCP 설정

GCP에 EC2 VM을 하나 설정하여서 서버로 사용할 계획이다.  

![image](https://user-images.githubusercontent.com/74250270/184180124-490dc56a-b649-415c-b410-38351ce78e37.png)

  

키를 통해서 접속도 가능하지만, 비밀번호를 통한 손쉬운 SSH접속을 위해서 팀원 3명의 게정을 생성하여서 SSH 접속을 시도 하였다.  

`sudo passwd`와 `adduser`명령어를 통해서 인원을 추가했다.  

#### GCP 방화벽 설정

GCP는 다양한 가상머신을 병렬적으로 다뤄야할 상황을 상정하여서 규칙을 생성하고 그곳에 태그를 정하여, 해당하는 태그를 가진 VM들이 규칙을 따르도록 설정한다.  

![image](https://user-images.githubusercontent.com/74250270/184181792-45c8f0db-d7cd-472b-baa7-f156bb2ed66a.png)

인그레스와 이그레스에 대한 SQL포트 `3306`에 대해서 모든 ip들(`0.0.0.0/0`)들의 접속을 허용하도록 설정을 잡았다. 해당하는 설정을 통해서 DBeaver와 SQL 클라이언트가 SSH키 없이 접속할 수 있도록 설정을 잡아주었다.
  
#### 리눅스 포트 접근 허용하기
GCP의 방화벽 설정을 했으면, GCP안에 리눅스에서도 포트접근을 허용해야 한다.
`sudo iptables -I INPUT -p tcp -m tcp --dport 22 -j ACCEPT`와 `sudo iptables -I OUTPUT -p tcp -m tcp --dport 22 -j ACCEPT`명령어를 통해서 mysql이 사용하는 22번포트의 인풋 아웃풋을 허용하는 규칙을 설정해준다.
  
#### Connection refused:connect

`sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf`

명령어로 MySQL설정 파일을 연다.

![img](https://user-images.githubusercontent.com/74250270/184182774-f924f507-5f6b-4974-9d12-98c162b248d4.png)

다음과 같은 두 항복이 루프백 주소로 되어 있어서 외부의 접속을 막고 있는데, 이를 주석처리 하거나 완전히 오픈하여서 외부의 다른 IP도 접속할 수 있도록 설정을 잡아줘야한다.

### MYSQL

DB는 MySQL을 사용하기로 했다. PostgreSQL의 조금이라도 존재하는 러닝커브를 피하기 위함이다.(현재 생각중인 Flutter, Spring 모두 공부를 필요로 한다.). 

#### SQL에서도 비밀번호를 통해서 접속하기

#### Connection refused : connect, Public Key Retrieval is not allowed

8.0이상의 DBeaver에서는 주소값과 드라이버이름, 유저아이디, 패스워드만 가지고는 에러가 발생한다.  

![img](https://user-images.githubusercontent.com/74250270/184183509-842266aa-cd11-4c2f-a069-a000f8180f91.png)

드라이버 설정의 AllowPublicKeyRetrival 옵션을 true로 바꾸면 해결할 수 있다.
다만 이러한 키 방식의 번거로움을 줄이고자 비밀번호를 통하여 유저를 인증하는 `Authentication Plugin`을 사용하기로 했다.

MySQL 8.0부터는 기본적인 authentication plugin이 `caching_sha2_password`이 되면서 RSA key를 필요로 한다. 단, 이러한 key를 이용한 방식이 아닌 단순 암호를 사용하는 방식으로 바꾸기 위해서는 다음과 같은 코드를 사용해야한다.

```sql
CREATE USER 'nativeuser'@'localhost'IDENTIFIED WITH mysql_native_password BY 'password';

혹은

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'test'

flush privilages;
```

다음과 같은 query를 통해서 `mysql_native_password`를 사용할 수 있다.


### 역시 환경설정이 제일 어려운법

![img](https://media.giphy.com/media/NV4cSrRYXXwfUcYnua/giphy.gif)

  

환경설정은 항상 느끼지만 어렵다. 나의 발목을 잡는다는 느낌이 드는 것도 당연하다고 생각한다.Firebase를 사용한다면 이런 오류를 만나지 않았겠지만, 만나지 않았기 때문에 조금은 더 성장했다는 느낌을 받았다.  

앞으로 DB의 세부 테이블이나 내용은 다음주에 또 다뤄보도록 하겠다.
