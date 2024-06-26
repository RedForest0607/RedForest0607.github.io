---
title: '[PeachTri] PeachTri 어떤 DB를 사용해야 할까? - 2'
date: 2022-08-10 22:02:00 +0900
categories: [Blogging, PeachTri, Iride-Scent]
img : Seminar_banner_small.png
tags: [세미나, PeachTri, DB, Iride-Scent]
pin: false
---
# 사용해야할 DB
![image](https://media.giphy.com/media/xT9C25UNTwfZuk85WP/giphy-downsized-large.gif)
## SQL
 우리는 기본적으로 향수에 대하여 정리하고, 정리된 데이터를 기반으로 향수를 추천하는 어플리케이션을 제작할 예정이다. 이에 따라서 다양한 방식을 살펴볼 수 있는데, 내가 1차적으로 생각하는 것은 SQL형식의 RDB이다.
## 왜 RDB인가?
 기본적으로 noSQL에 대해서 팀원들의 전체적인 이해도가 없다. 이에 가장 큰 걱정인 것은, noSQL의 `러닝커브` 대비해서, 어플의 성능문제나 noSQL에 대해 들어가는 공부의 비용이 어플리케이션을 만들면서 들어오는 이점 대비 너무 크다는 점이었다. 대한민국 대부분의 서비스들이 RDB형식으로도 충분히 만들 수 있고, 각각의 향수에 대한 데이터가 RDB로도 충분하기 때문에 RDB로 하는 것을 고려해 볼 생각이다.
## 어떤 RDB를 사용할 것인가?
 RDB는 다양한 종류가 존재한다. 다음은 대표적으로 사용하는 RDB들이다.
 1. ### Oracle
   - `오라클`에서 개발한 RDB로 가장 많이 사용한다.
   - 중앙 집중 방식이고, 다양한 기능을 재공하며, 장애에 대처가 유연하다.
   - 기본적으로 큰 비용이 발생한다.
 2. ### MySQL
   - 오픈소스에서 파생되어서 현재는 `오라클`로 넘어갔다.
   - 경량의 장점이 있으나, 기능확장이 되면서 다른 RDB들과 비슷해졌으며, 오라클로 넘어가서 라이센스 문제가 생겼따.
 3. ### Microsoft SQL SERVER
   - 엔터프라이즈급 관리
   - 비싼 가격, Windows기반 서버 전용
 4. ### MariaDB
   - MySQL의 라이선스 상태에 반발하여 만들어진 `오픈소스프로젝트`
   - MySQL에 비해서도 빠르며, 다양한 기능을 제공하고, MySQL보다 잘 관리되고 업데이트되며, 오픈소스 특성상 많은 이용자들이 존재한다.
 5. ### PostgreSQL
   - MariaDB와 같은 `오픈소스 프로젝트`
   - 멀티 프로세스 방식으로 복잡한 쿼리나 join처리에서 좀 더 뛰어난 성능을 보여준다.
   - 상당히 높은 점유율로 인해서 정보를 찾기 편하다.
![image](https://velog.velcdn.com/images/redforest/post/04241207-1bcc-4e1f-91db-f5c59f267c9e/image.png)
다음과 같은 RDB들 중에서 PostgreSQL를 추천해볼까 한다. 오픈소스 프로젝트라는 점, 그래서 이용자도 많다는 점, 정보나 업데이트도 빠르기 때문에 우리의 토이프로젝트 적용에도 매우 좋을 것으로 생각이 든다.