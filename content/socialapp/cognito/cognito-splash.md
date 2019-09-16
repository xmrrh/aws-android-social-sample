---
title: "앱 시작 화면 만들기"
date: 2018-08-07T08:30:11-07:00
weight: 11
---

다음과 같은 형태의 앱 시작화면을 구성해보도록 하겠습니다.
![Example Service](/images/app-splash.png)


시작화면에서는 Cognito의 로그인 상태를 확인하는 코드가 추가가 됩니다. 로그인 정보가 있는 경우 앱 메인 화면으로 이동하게 됩니다. 로그인 정보가 없는 경우는 로그인 메인 화면으로 이동하게 됩니다. 


```java
// SplashActivity.java

public class SplashActivity extends AppCompatActivity {
    ...

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_splash);

        context = this;
        _initCognito();
    }

    private void _initCognito() {
        AWSMobileClient.getInstance().initialize(getApplicationContext(), new Callback<UserStateDetails>() {
            @Override
            public void onResult(UserStateDetails userStateDetails) {

                switch (userStateDetails.getUserState()){

                    case SIGNED_IN:
                        // Open Main Activity
                        CommonAction.openMain(context);
                        break;
                    case SIGNED_OUT:
                        Log.d(TAG, "Do nothing yet");
                        CommonAction.openAuthMain(context);
                        break;
                    default:
                        AWSMobileClient.getInstance().signOut();
                        break;
                }
            }

            @Override
            public void onError(Exception e) {
                Log.e("INIT", e.toString());
            }
        });
    }

    ...
}
```


