---
title: "소셜 로그인 기능 구현하기"
date: 2018-08-07T08:30:11-07:00
weight: 12
---

이번 실습에는 Cognito의 Hosted UI 기능을 사용해 페이스북 기반 소셜 로그인 기능을 구현해 보겠습니다. 

Hosted UI를 이용하면 OAuth2 인증이 Server to Server로 에 진행되기 때문에 페이스북에서 제공하는 별도의 모바일 SDK를 설치하실 필요는 없습니다. 현재 Hosted UI를 이용해 인증 가능한 Identity Provider는 Facebook, Google, Amazon입니다.


아래 코드를 복사하여 Facebook 로그인을 위한 Hosted UI기능 추가를 위한 **_openFacebookLogin** 메소드를 완성해 봅니다.
```java
// AuthMainActivity.java

public class AuthMainActivity extends AppCompatActivity {

    ...

    private void _openFacebookLogin() {
        // Add code here
        HostedUIOptions hostedUIOptions = HostedUIOptions.builder()
                .scopes("openid", "email")
                .identityProvider("Facebook")
                .build();

        SignInUIOptions signInUIOptions = SignInUIOptions.builder()
                .hostedUIOptions(hostedUIOptions)
                .build();

        AWSMobileClient.getInstance().showSignIn((Activity) context, signInUIOptions, new Callback<UserStateDetails>() {
            @Override
            public void onResult(UserStateDetails details) {
                Log.d(TAG, "onResult: " + details.getUserState());
            }

            @Override
            public void onError(Exception e) {
                Log.e(TAG, "onError: ", e);
            }
        });
    }

    ...
}
```

다음으로, 소셜 인증 완료 후 호출된 callback 정보(**socialdemoapp://callback/**)을 감지하여 해당 이벤트를 처리하는 코드를 추가합니다. 실습에서는 해당 scheme 호출 시 앱 메인으로 이동하는 기능을 구현합니다.
```xml
<!-- AndroidManifest.xml -->
        <activity android:name=".AuthMainActivity"
            android:screenOrientation="portrait"
            android:theme="@style/AppTheme.NoActionBar">
            <!-- Add code here-->
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="socialdemoapp" />
            </intent-filter>
        </activity>
```

```java
// AuthMainActivity.java
public class AuthMainActivity extends AppCompatActivity {
    
    ...

    @Override
    protected void onResume() {
        super.onResume();

        // Add code here
        Intent activityIntent = getIntent();
        if (activityIntent.getData() != null &&
                "socialdemoapp".equals(activityIntent.getData().getScheme())) {
            if (AWSMobileClient.getInstance().handleAuthResponse(activityIntent))
                CommonAction.checkSession(this, true);
        }
    }

    ...

}
```

안드로이드 스튜디오 프로젝트 상단에 **실행버튼** 을 눌러 이미 생성하신 에뮬레이터로 어플리케이션을 실행합니다.
![c9after](/images/run.png)

아래와 같은 화면이 나오면 **페이스북으로 시작** 버튼을 눌러서 페이스북 로그인을 진행해 봅니다.
![Example Service](/images/app-authmain.png)

에뮬레이터에서 최초 실행할 경우 아래와 같은 페이스북 로그인 화면이 나오는 경우, 반드시 **이전 실습에서 만든 페이스북 개발자 계정** 정보를 입력합니다. **페이스북 앱이 개발모드로 설정되어 있어 다른 계정을 사용한 경우 '앱 설정 안 됨'이라는 오류 메시지가 나오고 로그인에 실패하게 됩니다.**
![Example Service](/images/facebook-oauth2.png)

로그인이 정상적으로 이루어진 경우 아래와 같은 앱 메인화면으로 이동하게 됩니다.
![Example Service](/images/app-main-init.png)

워크샵 시작시에 설치한 데모 앱과 callback 주소의 URI scheme이 동일한 경우 아래와 같은 메시지 창을 확인하 실 수 있습니다. 
![Example Service](/images/app-select.png)

Cognito Hosted UI를 통해 서버간 소셜 인증이 정상적으로 이루어진 경우, 다음과 같이 Cognito User Pool에 사용자의 정보가 저장되게 됩니다.
저장된 사용자 정보 확인은 AWS Console > Cognito > General Settings > Users and groups에 Users 탭의 내용을 통해 확인하실 수 있습니다.
![Example Service](/images/app-facebook-login-result.png)

실습을 마무리 한 경우, 다음 실습을 위해 로그아웃을 진행합니다.

1. 앱 메인 화면의 SETTINGS 버튼을 선택
![Example Service](/images/app-main-empty.png)
2. 앱 설정 화면의 LOGOUT 버튼을 선택
![Example Service](/images/app-settings.png)
