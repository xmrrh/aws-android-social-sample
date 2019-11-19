---
title: "앱 시작 화면 만들기"
date: 2018-08-07T08:30:11-07:00
weight: 11
---

이번 실습에서는 앱의 시작화면을 구성해 봅니다. 시작화면에서는 Cognito의 로그인 상태를 확인하는 코드가 추가가 됩니다. 기존 로그인 정보가 없는 경우는 로그인 메인 화면으로 이동하고, 로그인 정보가 있는 경우는 앱 메인 화면으로 이동합니다.

아래 코드 부분을 복사하여 **_initCognito** 메소드를 완성해 봅니다.


```java
// SplashActivity.java

public class SplashActivity extends AppCompatActivity {
    ...

    private void _initCognito() {
        // Add code here
        if (AWSMobileClient.getInstance().getConfiguration() == null){
            // Initialize user
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

        } else if (AWSMobileClient.getInstance().isSignedIn()){
            // Logined user
            CommonAction.openMain(context);
        } else {
            // Logouted user
            CommonAction.openAuthMain(context);
        }

    }

    ...
}
```

안드로이드 스튜디오 프로젝트 상단에 **실행버튼** 을 눌러 이미 생성하신 에뮬레이터로 어플리케이션을 실행합니다.
![c9after](/images/run.png)

앱이 정상 실행되면 로그인 여부를 확인하는 로직이 포함된 시작화면이 표시됩니다.
![Example Service](/images/app-splash.png)

로그인 상태 확인이 정상적으로 완료된 경우에는 아래와 같은 인증 메인화면으로 이동하게 됩니다.
![Example Service](/images/app-authmain.png)
