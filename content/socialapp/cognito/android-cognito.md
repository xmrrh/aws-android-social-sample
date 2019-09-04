---
title: "안드로이드 로그인 액티비티 생성하기 - 코드 설명하지 않음 :customUI로 변경예정(by성진님)"
chapter: false
date: 2018-08-07T08:30:11-07:00
weight: 11
---

이제 안드로이드 프로젝트에 로그인에 사용할 액티비티를 생성해 보도록 하겠습니다. 

우선 build.gradle (Module: app) 에 아래와 같이 추가 합니다.

AWSMobileClient를 이용하면 애플리케이션에서 사용자의 "로그인 상태"를 쿼리 할 수 있습니다. 또한 애플리케이션에 간단한 "드롭 인"UI를 위해 드롭 인 인증 UI를 다음과 같이 추가합니다. 

```
    //For AWSMobileClient only:
    implementation('com.amazonaws:aws-android-sdk-mobile-client:2.14.+@aar') { transitive = true }
    //For the drop-in UI also:
    implementation('com.amazonaws:aws-android-sdk-auth-userpools:2.14.+@aar') { transitive = true }
    implementation 'com.amazonaws:aws-android-sdk-auth-ui:2.14.+'
```


다음으로 인증을 위한 액티비티를 생성합니다. 

AuthActivity.java 

```
package com.example.socialandroidapp;

import android.content.Intent;
import android.graphics.Color;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.widget.EditText;

import com.amazonaws.mobile.client.AWSMobileClient;
import com.amazonaws.mobile.client.Callback;
import com.amazonaws.mobile.client.SignInUIOptions;
import com.amazonaws.mobile.client.UserStateDetails;

public class AuthActivity extends AppCompatActivity {

    private static final String TAG = "aws-dev-auth";
    private EditText userid, userpw;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.authentication_layout);
        
    }

    @Override
    protected void onResume() {
        super.onResume();
        AWSMobileClient.getInstance().initialize(getApplicationContext(), initListenerCallback);

    }

    Callback<UserStateDetails> initListenerCallback = new Callback<UserStateDetails>() {

        @Override
        public void onResult(UserStateDetails userStateDetails) {
            Log.i(TAG, "onResult: " + userStateDetails.getUserState());
            switch (userStateDetails.getUserState()) {
                case SIGNED_IN:
                    //AWSMobileClient.getInstance().signOut();
                    goMainActivity();
                    break;
                case SIGNED_OUT:
                    //break;
                default:
                    //AWSMobileClient.getInstance().signOut();
                    showSignIn();
                    break;
            }
        }

        @Override
        public void onError(Exception e) {
            Log.e(TAG, "Initialization error.", e);
            AWSMobileClient.getInstance().initialize(getApplicationContext(), initListenerCallback);
        }
    };

    public void goMainActivity() {


        try {
            Log.e(TAG, " AWSMobileClient.getInstance().getAWSCredentials()=" +
                    AWSMobileClient.getInstance().getAWSCredentials());
            Log.e(TAG, " AWSMobileClient.getInstance().getConfiguration()=" +
                    AWSMobileClient.getInstance().getConfiguration());

        } catch (Exception e) {
            e.printStackTrace();
        }

        Intent intent = new Intent(AuthActivity.this, MainActivity.class);
        intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK | Intent.FLAG_ACTIVITY_CLEAR_TOP);
        startActivity(intent);
    }

    private void showSignIn() {
        try {
            finish();
            AWSMobileClient.getInstance().showSignIn(this,
                    SignInUIOptions.builder().logo(R.drawable.devday).backgroundColor(Color.BLUE).//hostedUIOptions()
                            nextActivity(MainActivity.class).build());//, lisenercallback);
        } catch (Exception e) {
            Log.e(TAG, "Show singIN" + e.toString());
        }
    }
 

}

```



AndroidManifest.xml 파일을 수정합니다. 기존의 MAIN action과 LAUNCHER category를 가지고 있던  MainActivity의 속성을 제거하시고  AuthActivity 에 제거된 속성을 부여합니다. 아래 코드를 참고하세요.

```
<activity android:name=".AuthActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
<activity android:name=".MainActivity" />
```



안드로이드 스튜디오 프로젝트 상단에 **실행버튼** 을 눌러 이미 생성하신 에뮬레이터로 어플리케이션을 실행합니다.
![c9after](/images/run.png)

실행된 화면은 다음과 같습니다.
![c9after](/images/signin.png)

가입을 합니다. 이때 비밀번호의 경우 대문자와 특수문자가 포함된 8자리 이상으로 구성되어야 합니다.

![c9after](/images/signup.png)

가입시 적은 이메일을 통해 컨펌 번호를 입력하시면 가입이 완료됩니다. 

![c9after](/images/confirm.png)

가입시 적은 이메일을 통해 컨펌 번호를 입력하시면 가입이 완료됩니다. 

![c9after](/images/confirm.png)

<b>AWS console > service > Cognito > user pool > </b>를 통해 새로운 사용자가 등록된 것을 확인 하실 수 있습니다. 

![c9after](/images/cognitouser.png)




