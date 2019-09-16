---
title: "소셜 로그인 기능 구현하기"
date: 2018-08-07T08:30:11-07:00
weight: 12
---

인증 메인화면에는 소셜 로그인(페이스북, Google)을 위한 버튼과 이메일 기반의 로그인 및 회원 가입을 위한 링크가 제공 됩니다.
![Example Service](/images/app-authmain.png)

소셜 로그인의 경우 Cognito의 Hosted UI 기능을 사용해 서버간 OAuth2 인증이 이루어지고 User Pool에 등록도 자동으로 처리됩니다. 별도의 Identity Provider의 SDK를 설치하실 필요 없습니다.

```java
// AuthMainActivity.java

public class AuthMainActivity extends AppCompatActivity {

    private static final String TAG = AuthMainActivity.class.getSimpleName();

    private Context context;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_auth_main);

        context = this;
    }

    public void openLogin(View view) {
        Intent intent = new Intent(context, LoginActivity.class);
        startActivity(intent);
    }

    public void openRegistration(View view) {
        Intent intent = new Intent(context, SignUpActivity.class);
        startActivity(intent);
    }

    public void openFacebookLogin(View view) {
        _openFacebookLogin();
    }

    private void _openFacebookLogin() {
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
}
```

Cognito Hosted UI를 통해 서버간 소셜 인증이 정상적으로 이루어진 경우, 다음과 같이 Cognito User Pool에 사용자의 정보가 저장되게 됩니다.
추가된 사용자 정보 확인은 AWS Console > Cognito > General Settings > Users and groups에 Users 탭의 내용을 통해 확인하실 수 있습니다.
![Example Service](/images/app-facebook-login-result.png)

마지막으로, 소셜 인증 완료 후 호출된 callback 정보(**socialdemoapp://callback/**)을 감지하여 해당 이벤트를 처리하는 코드를 추가한다. 실습에서는 해당 scheme 호출 시 앱 메인으로 이동하는 기능을 구현한다.
```xml
<!-- AndroidManifest.xml -->
        <activity android:name=".AuthMainActivity"
            android:screenOrientation="portrait"
            android:theme="@style/AppTheme.NoActionBar">
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
        Intent activityIntent = getIntent();
        if (activityIntent.getData() != null &&
                "socialdemoapp".equals(activityIntent.getData().getScheme())) {
            if (AWSMobileClient.getInstance().handleAuthResponse(activityIntent))
                CommonAction.openMain(context);
            else
                CommonAction.openAuthMain(context);
        }
    }
    ...

}
```

이메일 기반 인증 기능은 개별 기능을 구현하는 Activity에서 처리 됩니다. 