---
title: "이메일 기반 로그인 기능 구현하기"
date: 2018-08-07T08:30:11-07:00
weight: 14
---

이번 실습에서는 이메일 기반 로그인 기능을 구현해 보겠습니다. 

이메일 기반 로그인을 위한 안드로이드 UI는 다음과 같이 구성되어 있습니다. 
![Example Service](/images/app-login.png)

아래 코드를 복사하여 회원 가입을 위한 **_signIn** 메소드를 완성합니다.
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

정상적으로 로그인이 완료된 경우 앱 메인화면으로 이동하게 됩니다.