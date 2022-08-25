---
title: '[PeachTri] DB 생성'
author: InGyuJang
date: 2022-08-22 12:00:00 +0900
categories: [Blogging, PeachTri, Iride-Scent]
tags: [세미나, PeachTri, DB, Iride-Scent]
pin: false
---
![image](https://media.giphy.com/media/xT5LMqvvGAWVZJ1iq4/giphy.gif)
## 💻 DB를 생성해보자!
저번주에 전체적인 테이블의 RDB 구조를 작성했다. 이번주 세미나는 같이 직접 `MySQL`테이블을 생성하는 `스크립트`를 작성해보았고, 그 과정에서 우리가 겪었던 어려움에 대해서 기록해보려고 한다.

#### 📌 잡다한 오탈자
사실 잡다한 오탈자는 너무나도 자주 발생한다. 다른 언어의 스니펫같이 자동완성을 어마어마하게 지원하지도 않고, 오탈자가 자주 발생할 만한 단어들이 산재해 있기 때문에 끊임없이 마주치게 되는 오탈자가 첫번째 문제였다. 또, `CREATE 'TABLE'`을 뺴먹는 경우도 존재하여서 시간을 크게 소모했다.

#### :pushpin:  TIMESTAMP
`sql`에서 삽입,수정을 한 주체와 시간을 로그로 기록하기 위해서 타임스탬프값을 DB내에서 지정했다. 이 과정에서 `TIMESTAMP`의 몇가지 특징을 메모하려고 한다.
- `TIMESTAMP`는 디폴트 값이 존재해야한다.
```
MODIFIED_TIMESTAMP TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
```
따라서 다음과 같이 수정한 날짜에 대해서 현재 값을 DEFAULT로 저장하는 방식으로 스크립트를 작성했다.  

- `TIMESTAMP`는 기본적으로 `NOT NULL`이다.  
그래서 `TIMESTAMP`에서 따로 `NOT NULL`을 지정해줄 필요는 없고, 그냥 `NULL`인 경우만 옆에 조건으로 적어주면 된다.

#### 📌  복합기본키
![image](https://user-images.githubusercontent.com/74250270/186566366-9ba6000b-b742-4a81-855a-5e30f18788af.png)  
다음과 같이 다른 테이블들의 기본키들을 외래키로 가져와 `unique`한 기본키들로 사용해야 하는 경우, 둘 다 `Primary Key`로 지정하게 되면 오류가 발생한다. 이런 경우 단순하게 둘 다 `Primary Key`로 지정하는 것이 아니라, 제약조건을 통해서 지정해줘야 한다.
```
CONSTRAINT BLEND_PK PRIMARY KEY (PERFUME_ID, PERFUMER_ID)
```
다음과 같은 쿼리 문으로, 사용할 제약조건의 이름`BLEND_PK`와 함께 이 제약조건에 들어갈 기본키 두개를 지정해주면, 두 키의 조합 자체가 `unique`하게 된다.


#### :pushpin: auto_increment 문제
굉장히 편리한 기능인 auto_increment는 사용하지 않기로 했다.
메모리에서 동작하는 innoDB에서 초기화 되는 문제가 발생하기도 하거니와 여러가지 테이블 락 등의 문제가 존재하여서, 직접 인덱싱을 하는 방향으로 결정했다.  
또한 향수 이름이나 향수 회사이름이 정말정말정말 긴 경우가 많아서 자료형에서 `VARCHAR`의 길이를 정하는데 고민이 좀 있었던 것 같다. fragrantica사이트에서 충분한 검토후 선정하였다.  

### 🤦 아직 공부중인 문제
![image](https://user-images.githubusercontent.com/74250270/186464117-b69f094c-772d-42f5-bd63-f18f3a2347cf.png)  
다음과 같이 MySql내부의 파일에 접속시 `function>`만 덩그러니 놓여있는 오류도 존재했는데 원인을 찾을 수 없었다. 구글링에 정상적인 MySQL실행 방법이라고 하는데, 나의 경우 적용되지 않았다.  

#  
    
📖 참고자료
---
https://blog.naver.com/PostView.naver?blogId=sory1008&logNo=222346919445&parentCategoryNo=&categoryNo=79&viewDate=&isShowPopularPosts=false&from=postView

https://yun5o.tistory.com/entry/MySQL-AUTOINCREMENT-%EC%9E%90%EB%8F%99%EC%A6%9D%EA%B0%80-%EA%B0%92-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0

https://abbo.tistory.com/208