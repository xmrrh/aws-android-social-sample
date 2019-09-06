---
title: "Amazon Cognito Service 만들기"
date: 2018-08-07T08:30:11-07:00
weight: 10
---

Amazon Cognito를 사용하면 모바일 앱에 빠르고 손쉽게 사용자 가입, 로그인 및 액세스 제어 기능을 추가할 수 있습니다. 아래 명령어를 통해 손쉽게 생성하실 수 있습니다. 

```shell
amplify add auth
```

진행 중 선택항목은 아래 그림을 참고하세요.

![Example Service](/images/addauth.png)



완료되면 다음 명령어를 통해 클라우드 리소스를 업데이트 합니다. 

```shell
amplify push
```

![Example Service](/images/pushauth.png)



참고로 status 명령어를 통해 amplify project의 상태를 확인하실 수 있습니다. 

```shell
amplify status
```

![Example Service](/images/authstatus.png)



Amazon Cognito 가 생성된 것을 확인하기 위해 콘솔에 접속합니다. 

<b>AWS console > service > Cognito > user pool > </b>

아래와 같이 user pool이 생성된 것을 확인하실 수 있습니다. 

![Example Service](/images/cognito.png)



또한 안드로이드 스튜디오에서 res/raw/awsconfiguration.json.file 파일이 생성된 것을 확인하실 수 있습니다. 

![Example Service](/images/configure-cognito.png)



