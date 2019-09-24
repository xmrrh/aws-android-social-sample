---
title: "이메일 기반 로그인 기능 구현하기"
date: 2018-08-07T08:30:11-07:00
weight: 14
---

이번 실습에서는 이메일 기반 로그인 기능을 구현해 보겠습니다. 

아래 코드를 복사하여 이메일 기반 로그인을 수행하는 **_signIn** 메소드를 완성합니다.
```java
// LoginActivity.java
public class LoginActivity extends AppCompatActivity implements Validator.ValidationListener {

    ...

    private void _signIn(String userName, String password) {
        AWSMobileClient.getInstance().signIn(userName, password, null, new Callback<SignInResult>() {
            @Override
            public void onResult(final SignInResult signInResult) {
                runOnUiThread(() -> {
                    Log.d(TAG, "Sign-in callback state: " + signInResult.getSignInState());
                    switch (signInResult.getSignInState()) {
                        case DONE:
                            makeToast(context,"Sign-in done.");
                            CommonAction.openMain(context);
                            break;
                        case SMS_MFA:
                            makeToast(context, "Please confirm sign-in with SMS.");
                            break;
                        case NEW_PASSWORD_REQUIRED:
                            makeToast(context, "Please confirm sign-in with new password.");
                            break;
                        default:
                            makeToast(context, "Unsupported sign-in confirmation: " + signInResult.getSignInState());
                            break;
                    }
                });
            }

            @Override
            public void onError(Exception e) {
                Log.e(TAG, "Sign-in error", e);
                runOnUiThread(() -> {
                    if (e instanceof AmazonServiceException)
                        makeToast(context, ((AmazonServiceException) e).getErrorMessage());
                });
            }
        });
    }

    ...

}

```

이제 작성한 코드를 실행해 보도록 하겠습니다.

안드로이드 스튜디오 프로젝트 상단에 **실행버튼**을 눌러 에뮬레이터로 어플리케이션을 실행합니다.
![c9after](/images/run.png)

아래와 같은 화면이 나오면 **이메일로 로그인** 텍스트를 눌러서 이메일 기반 로그인 메뉴로 이동합니다.
![Example Service](/images/app-authmain.png)


이전 단계에서 가입한 사용자 정보를 활용하여 로그인을 진행해 봅니다. 
![Example Service](/images/app-login.png)

정상적으로 로그인이 완료된 경우 아래와 같은 앱 메인화면으로 이동하게 됩니다.
![Example Service](/images/app-main-empty.png)
