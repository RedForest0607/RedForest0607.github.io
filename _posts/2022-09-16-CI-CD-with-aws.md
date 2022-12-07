---
title: '[DevOps]아마존 AWS를 통한 CI/CD 구축 (with Github Actions)'
author: InGyuJang
image:
  path: /path/to/image/file
  width: 1000   # in pixels
  height: 400   # in pixels
  alt: image alternative text
date: 2022-09-16 17:51:35 +0900
categories: [Tech, DevOps]
tags: [Tech, TIL, DevOps, AWS, Github]
pin: false
---
flow
깃허브 업로드 -> actions에서 Gradle로 빌드 후 S3로 업로드, CodeDeploy에서 업로드 된 파일을 EC2에 배포 여기서 DB를 MySQL로 잡고 MySQL용 DB를 따로 생성하여서 연결

Github Actions yml파일
appspec yml 파일
시작과 끝에 shell 스크립트 작성 가능
