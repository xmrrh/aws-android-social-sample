---
title: "Implementing social login"
date: 2018-08-07T08:30:11-07:00
weight: 12
---

In this tutorial, we will implement Facebook-based social login using Cognito's Hosted UI feature.

With Hosted UI feature, OAuth2 authentication is done between servers, so we do not need to install a mobile SDK provided by Facebook. Currently, supporting Identity Providers are Facebook, Google, and Amazon.

Copy the code below to complete the **_ openFacebookLogin** method for adding Hosted UI functionality for Facebook login.
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

Next, add code to detect the callback(**socialdemoapp://callback/**) called after social authentication completes and handle the event. In the lab, the page will be move to main menu when the **scheme** is called.
implement the function to move to the main activity when the **scheme** is called.
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

Press **Run** button at the top of your Android Studio IDE to run the application with the emulator you have already created.
![c9after](/images/run.png)

When the following screen appears, press the **Start with Facebook** button to proceed with Facebook login.
![Example Service](/images/app-authmain.png)

When you first run it in the emulator, if you see the following Facebook login screen, be sure to enter the ** Facebook developer account ** information you created in the previous tutorial.
**Since the Facebook app is in development mode, using a different account for login will cause the login to fail.**
![Example Service](/images/facebook-oauth2.png)

If login is successful, the screen will be moved to the main page as shown below.
![Example Service](/images/app-main-init.png)

If social authentication between servers is successful through the Cognito Hosted UI, the user's information will be registered in the Cognito User Pool. The newly registered user information will be provided in the **AWS Console> Cognito> General Settings> Users and groups**. In some cases, around 1 minute may be required to see the update in the web console.
![Example Service](/images/app-facebook-login-result.png)

After completing the lab, log out for the next tutorial.

1. Select the **SETTINGS** button on the app main screen
![Example Service](/images/app-main-empty.png)
2. Select the **LOGOUT** button on the app settings screen
![Example Service](/images/app-settings.png)
