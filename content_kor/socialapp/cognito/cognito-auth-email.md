---
title: "이메일 기반 회원 가입 기능 구현하기"
date: 2018-08-07T08:30:11-07:00
weight: 13
---

이번 실습에서는 이메일 기반 회원 가입 기능을 구현해 보겠습니다. 

실습 시작 전, AWS Conginto 관리 콘솔에 접근해 이메일이 로그인 방법으로 지정되어 있는지 확인해보도록 하겠습니다. AWS 관리 콘솔의 Attributes 항목의 내용이 아래 그림과 같이 설정되어 있는지 확인합니다. 
![Example Service](/images/auth-cognito-email-setting.png)

이제 단계별로 이메일 기반 회원 가입을 위한 코드를 완성해 보겠습니다.

1. 입력받은 이메일, 비밀번호를 이용해 회원 가입을 진행. <br>아래 코드를 복사하여 회원 가입을 위한 **signUp** 메소드를 완성합니다. signUp 메소드에서는 입력받은 이메일과 비밀번호로 Cognito user pool에 새로운 사용자를 추가합니다. 사용자 정보가 정상 등록된 경우에는 사용자 이메일 인증을 위한 SignUpConfirmFragment로 화면을 전환합니다.
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

2. 이메일 인증. <br>1 단계에서 회원가입에 성공한 경우 이메일로 인증 코드가 발송됩니다. 아래 코드를 복사하여 이메일과 인증코드를 입력받아 회원 가입 단계를 마무리하는 **confirmSignUp** 메소드를 완성합니다.
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

3. 자동 로그인. <br>이메일 인증이 완료된 경우, 가입 단계에서 사용한 이메일과 비밀번호를 이용하여 자동 로그인 기능을 수행하는 **_signIn** 함수를 완성합니다. 
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

이제 작성한 코드를 실행해 보도록 하겠습니다.

안드로이드 스튜디오 프로젝트 상단에 **실행버튼**을 눌러 에뮬레이터로 어플리케이션을 실행합니다.
![c9after](/images/run.png)

아래와 같은 화면이 나오면 **회원가입** 텍스트를 눌러서 이메일 기반 회원 가입 메뉴로 이동합니다.
![Example Service](/images/app-authmain.png)

아래와 같은 화면이 나오면 이메일과 비밀번호를 입력하여 회원 가입을 진행합니다. 이메일 인증 과정이 필수로 적용되어 있기 때문에 수신 가능한 이메일 계정을 사용해주시기 바랍니다.
![Example Service](/images/app-signup.png)

이메일 수신함을 확인하여 Cognito 서비스에서 발송된 인증 코드를 확인합니다.
![Example Service](/images/auth-email-verfication.png)

인증코드 입력화면에서 이메일로 전달 받은 인증코드를 입력합니다.
![Example Service](/images/app-email-verification.png)

회원가입이 정상적으로 이루어진 경우 아래와 같은 앱 메인화면으로 이동하게 됩니다.
![Example Service](/images/app-main-empty.png)

가입된 사용자 정보는 AWS 관리 콘솔의 Cognito 메뉴를 통해 확인 가능합니다.
![Example Service](/images/auth-cognito-email-user.png)


실습을 마무리 한 경우, 다음 실습을 위해 로그아웃을 진행합니다.

1. 앱 메인 화면의 SETTINGS 버튼을 선택
![Example Service](/images/app-main-empty.png)
2. 앱 설정 화면의 LOGOUT 버튼을 선택
![Example Service](/images/app-settings.png)
