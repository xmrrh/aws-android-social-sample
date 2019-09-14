---
title: "Facebook OAuth 설정하기"
date: 2018-08-07T08:30:11-07:00
weight: 7
---

Facebook을 통한 로그인을 지원하기 위해 Facebook 개발자 사이트에 OAuth 관련 설정을 진행해 보겠습니다. 

실습에서는 Amazon Cognito가 제공하는 Hosted UI 방식을 이용하여 OAuth2.0 연동을 구성하겠습니다. 이 방식을 이용할 경우 별도의 Facebook SDK 설치 없이 간단한 설정만으로 소셜 인증 기능을 구현하실 수 있습니다.

1. Facebook에 [개발자 계정을 생성](https://developers.facebook.com/docs/facebook-login)합니다.
2. 자신의 Facebook 계정으로 페이스북에 [로그인](https://developers.facebook.com/) 한다.
3. My App 메뉴에서 Add New App을 선택한다.
![Facebook](/images/facebook-create-app.png)

4. Facebook app 이름(AWSAndroidWorkshop)을 입력하고 Create App ID를 선택한다.
![Facebook](/images/facebook-new-app.png)

5. 좌측 네비게이션 바에서 Settings > Basic을 선택한다. 
![Facebook](/images/facebook-app-id.png)

6. 다음 실습에서 Facebook 인증 정보로 활용하기 위해 App ID와 App Secret을 기록해 둔다.


