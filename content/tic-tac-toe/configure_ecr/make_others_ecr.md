---
title: "나머지 리포지토리 만들기"
chapter: false
date: 2018-08-07T08:30:11-07:00
weight: 11
---

앞서 만든 방법과 동일하게 3개의 리파지토리를 더 만듭니다.

## Dashboard 리포지토리 만들기

1. matchmaker 리포지토리를 만든 방식대로 dashboard 리포지토리를 만듭니다.
1. 아래 명령어를 Cloud9의 터미널에 입력하여 dashboard 의 빌드 스크립트 파일을 엽니다.

```
c9 open ~/environment/tic-tac-toe-workshop/client/dashboard/build.sh
```
dashboard 에서 얻은 푸시 명령어를 반영하고, 저장 후 파일을 닫습니다.


## tic-tac-toe-server 리포지토리 만들기

1. matchmaker 리포지토리를 만든 방식대로 tic-tac-toe-server 리포지토리를 만듭니다.
1. 아래 명령어를 Cloud9의 터미널에 입력하여 tic-tac-toe 서버의 빌드 스크립트 파일을 엽니다.

```
c9 open ~/environment/tic-tac-toe-workshop/server/tic-tac-toe/build.sh
```
tic-tac-toe-server 에서 얻은 푸시 명령어를 반영하고 저장후 파일을 닫습니다.


## tic-tac-toe-client 리포지토리 만들기

1. matchmaker 리포지토리를 만든 방식대로 tic-tac-toe-client 리포지토리를 만듭니다.
1. 아래 명령어를 Cloud9의 터미널에 입력하여 tic-tac-toe 클라이언트의 빌드 스크립트 파일을 엽니다.

```
c9 open ~/environment/tic-tac-toe-workshop/client/tic-tac-toe/build.sh
```
tic-tac-toe-client 에서 얻은 푸시 명령어를 반영하고 저장후 파일을 닫습니다.


간단한 반복 작업이니 천천히 확인하면서 하시면 어렵지 않습니다.<br>
4개의 리포지토리를 다 만들고 나면 다음 화면과 같이 보입니다.
![Example Service](/images/tic-tac-toe/ecr-4.png)