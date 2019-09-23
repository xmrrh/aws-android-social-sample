---
title: "이메일 기반 회원 가입 기능 구현하기"
date: 2018-08-07T08:30:11-07:00
weight: 13
---

이메일 기반 로그인 기능은 회원 가입 메뉴와 이메일 기반 로그인 메뉴로 구성됩니다.

우선 회원 가입 기능을 구현해 보겠습니다. 
이전 실습에서 amplify add auth 명령 실행 시 Email을 로그인 수단으로 설정하였습니다. AWS 관리 콘솔의 Attributes 항목을 확인 해보면 아래 그림과 같이 이메일이 로그인 수단으로 설정된 것을 확인할 수 있습니다.
![Example Service](/images/auth-cognito-email-setting.png)

회원 가입을 위한 안드로이드 UI는 다음과 같이 구성되어 있습니다.

회원 가입은 다음과 같은 단계로 진행됩니다.

1. 이메일, 비밀번호를 입력하여 Signup을 진행합니다.
![Example Service](/images/app-signup.png)
아래 코드를 복사하여 회원 가입을 위한 **signUp** 메소드를 완성합니다. signUp 메소드에서는 입력받은 이메일과 비밀번호로 Cognito user pool에 새로운 사용자를 추가하고, 사용자 정보가 정상 등록된 경우에는 사용자 이메일 인증을 위한 SignUpConfirmFragment로 화면을 전환합니다.
```java
// SignUpActivity.java
public class SignUpActivity extends FragmentActivity
        implements SignUpFragment.OnFragmentInteractionListener,
        SignUpConfirmFragment.OnFragmentInteractionListener {
    
    ...

    @Override
    public void signUp(String email, String password) {
        userName = email;
        this.password = password;

        // Add code here
        final Map<String, String> attributes = new HashMap<>();
        attributes.put("email", email);
        AWSMobileClient.getInstance().signUp(userName, password, attributes, null, new Callback<SignUpResult>() {
            @Override
            public void onResult(final SignUpResult signUpResult) {
                runOnUiThread(() -> {
                    if (!signUpResult.getConfirmationState()) {
                        final UserCodeDeliveryDetails details = signUpResult.getUserCodeDeliveryDetails();
                        makeToast(context, "Confirm sign-up with: " + details.getDestination());
                        setSignUpConfirmFragment();
                    } else {
                        makeToast(context, "Sign-up done.");
                    }
                });
            }

            @Override
            public void onError(Exception e) {
                Log.e(TAG, "Sign-up error", e);
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

2. 등록한 이메일로 발송된 인증코드를 확인합니다.
![Example Service](/images/auth-email-verfication.png)

3. 인증코드 입력화면에서 이메일로 전달 받은 인증코드를 입력합니다.
![Example Service](/images/app-email-verification.png)
아래 코드를 복사하여 회원 가입을 위한 **confirmSignUp** 메소드를 완성합니다. confirmSignUp 메소드에서는 사용자명(이메일)과 인증코드를 이용해 이메일 인증 과정이 이뤄집니다. 
```java
// SignUpActivity.java
public class SignUpActivity extends FragmentActivity
        implements SignUpFragment.OnFragmentInteractionListener,
        SignUpConfirmFragment.OnFragmentInteractionListener {

    ...

    @Override
    public void confirmSignUp(String code) {
        // Add code here
        AWSMobileClient.getInstance().confirmSignUp(userName, code, new Callback<SignUpResult>() {
            @Override
            public void onResult(final SignUpResult signUpResult) {
                runOnUiThread(() -> {
                    Log.d(TAG, "Sign-up callback state: " + signUpResult.getConfirmationState());
                    if (!signUpResult.getConfirmationState()) {
                        final UserCodeDeliveryDetails details = signUpResult.getUserCodeDeliveryDetails();
                        makeToast(context,"Confirm sign-up with: " + details.getDestination());
                    } else {
                        makeToast(context, "Sign-up done.");
                        // SignIn and move to MainActivity
                        _signIn(userName, password);
                    }
                });
            }

            @Override
            public void onError(Exception e) {
                Log.e(TAG, "Confirm sign-up error", e);
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

4. 이메일과 인증코드를 기반으로 이메일 인증이 완료된 경우, Activity에 유지된 이메일과 비밀번호를 기반으로 로그인을 진행합니다. 정상적으로 로그인된 경우 앱 메인화면으로 이동하게 됩니다. <br>아래 코드를 복사하여 회원 가입을 위한 **_signIn** 메소드를 완성합니다.
```java
// SignUpActivity.java
public class SignUpActivity extends FragmentActivity
        implements SignUpFragment.OnFragmentInteractionListener,
        SignUpConfirmFragment.OnFragmentInteractionListener {

    ...

    private void _signIn(String username, String password) {
        // Add code here
        AWSMobileClient.getInstance().signIn(username, password, null, new Callback<SignInResult>() {
            @Override
            public void onResult(final SignInResult signInResult) {
                runOnUiThread(() -> {
                    Log.d(TAG, "Sign-in callback state: " + signInResult.getSignInState());
                    switch (signInResult.getSignInState()) {
                        case DONE:
                            makeToast(context, "Sign-in done.");
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

가입된 사용자 정보는 AWS 관리 콘솔의 Cognito 메뉴를 통해 확인 가능합니다.
![Example Service](/images/auth-cognito-email-user.png)

