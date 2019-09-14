---
title: "Amazon Cognito Service 만들기"
date: 2018-08-07T08:30:11-07:00
weight: 10
---

Amazon Cognito를 사용하면 모바일 앱에 빠르고 손쉽게 사용자 가입, 로그인 및 액세스 제어 기능을 추가할 수 있습니다. 

안드로이드 프로젝트 최상위 위치에서 다음 명령을 실행합니다.

```shell
amplify add auth
```

진행 중 선택항목은 아래 그림을 참고하세요.

![Example Service](/images/amplify-auth-config.png)



완료되면 다음 명령어를 통해 클라우드 리소스를 업데이트 합니다. 

```shell
amplify push
```

![Example Service](/images/amplify-auth-push.png)



정상적으로 필요한 자원이 생성된 경우 아래 그림과 같이 Hosted UI Endpoint를 확인할 수 있습니다. 
![Example Service](/images/amplify-auth-cli-result.png)


Cognito User Pool이 생성된 것을 확인하기 위해 AWS 관리 콘솔(AWS console > Services > Cognito > User Pools)에 접속합니다. 
![Example Service](/images/auth-cognito-userpool.png)

또한 안드로이드 스튜디오에서 res/raw/awsconfiguration.json 파일에 각 환경설정 값들이 생성된 것을 확인하실 수 있습니다. 
![Example Service](/images/auth-android-config.png)


마지막으로, Facebook 개발자 사이트에 접속하여 생성한 Cognito 도메인 정보를 업데이트 합니다.

1. Facebook 개발자 사이트에서 이전 장에서 생성한 앱(AWSAndroidWorkshop)을 선택합니다.

2. 좌측 네비게이션 바에서 Settings > Basic을 선택한다. App Domains에 Amplify CLI 실행 결과 출력된 Hosted UI Endpoint (Cognito의 User Pool Domain Name과 동일한 값) 값을 저장한다. <br>
**https://&lt;your-user-pool-domain&gt;**
![Facebook](/images/facebook-app-domain.png)

3. Facebook 개발자 사이트의 Dashboard 메뉴로 이동한다. <br>
Add a Product에서 Facebook Login 항목의 Set Up 버튼을 선택한다. 
![Facebook](/images/facebook-add-product.png)

4. Facebook 개발자 사이트의 Products > Facebook Login > Settings 항목으로 이동한다. <br>
Embedded Browser OAuth Login을 활성화 하고, Valid OAuth Redirects URIs에 아래 형식의 URI를 입력한다. <br>
**https://&lt;your-user-pool-domain&gt;/oauth2/idpresponse**
![Facebook](/images/facebook-oauth-redirect.png)


