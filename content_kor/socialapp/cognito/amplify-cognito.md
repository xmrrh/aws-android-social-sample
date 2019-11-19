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

진행 중 선택항목은 아래 그림의 내용을 참고하세요.

![Example Service](/images/amplify-auth-config.png)

**domain name 설정 시 aws, amazon, cognito 및 _를 포함한 이름은 사용하실 수 없습니다. 'android-workshop+랜덤문자' 형태로 이름을 입력해주셔야 합니다.**

redirect URI는 아래와 같이 입력합니다. 

**redirect signin URI :**

```shell
socialdemoapp://callback/
```

**redirect signout URI :**

```shell
socialdemoapp://signout/
```



**social providers**  에서  facebook 선택시 **Space bar** 를 이용해 선택할 수 있습니다. 

**App ID**와 **App Secret** 정보는 앞단계에서 생성한 Facebook 인증 정보의 App ID와 App Secret를 사용합니다.

**Do you want to configure advanced settings for the GraphQL API?**에서는 No를 선택합니다.





완료되면 다음 명령어를 통해 관련된 AWS 리소스를 업데이트 합니다. 

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

2. 좌측 네비게이션 바에서 Settings > Basic을 선택합니다. App Domains에 Amplify CLI 실행 결과 출력된 Hosted UI Endpoint (Cognito의 User Pool Domain Name과 동일한 값으로 AWS 관리 콘솔의 메뉴[AWS console > Services > Cognito > User Pools > Domain Name]에서도 확인 가능) 값을 저장합니다. <br>
**https://&lt;your-user-pool-domain&gt;**
![Facebook](/images/facebook-app-domain.png)

3. Facebook 개발자 사이트의 Dashboard 메뉴로 이동합니다. <br>
Add a Product에서 Facebook Login 항목의 Set Up 버튼을 선택합니다. 
![Facebook](/images/facebook-add-product.png)

4. Facebook 개발자 사이트의 Products > Facebook Login > Settings 항목으로 이동합니다. <br>
Embedded Browser OAuth Login을 활성화 하고, Valid OAuth Redirects URIs에 아래 형식의 URI를 입력 후, Save Changes를 클릭합니다. <br>
**https://&lt;your-user-pool-domain&gt;/oauth2/idpresponse**
![Facebook](/images/facebook-oauth-redirect.png)


