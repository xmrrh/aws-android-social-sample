---
title: "Push Notification을 위한 안드로이드 코드 작성하기  "
chapter: false
date: 2018-08-07T08:30:11-07:00
weight: 20
---

이제 안드로이드 프로젝트에 Push Notification 을 받기 위해 안드로이드 프로젝트와 연동해보도록 하겠습니다.  

우선 build.gradle (Module: Project) 에 아래와 같이 dependencies를 추가 합니다. 

```java
dependencies {
        ...
        classpath 'com.google.gms:google-services:4.0.1'
        ...
    }
```



build.gradle (Module: app) 에 plugin을 적용시킵니다. 

```java
apply plugin: 'com.google.gms.google-services'
```



또한 같은 파일에 -build.gradle (Module: app)-  에 아래와 같이 dependency를  추가 합니다. 

```java
dependencies {
...
// Overrides an auth dependency to ensure correct behavior
     implementation 'com.google.android.gms:play-services-auth:15.0.1'

     implementation 'com.google.firebase:firebase-core:16.0.1'
     implementation 'com.google.firebase:firebase-messaging:17.3.0'

     implementation 'com.amazonaws:aws-android-sdk-pinpoint:2.15.+'
...
}
```



AndroidManifest.xml로 이동하여 Push Listener Service를 정의합니다. 

```xml
<application>
        ...
        <service
            android:name=".PushListenerService">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT"/>
            </intent-filter>
        </service>
</application>
```

Push 메시지를 누르면 App을 open 시키기 위해 아래와 같이 Receiver를 AndroidManifest.xml에 추가합니다. 

```xml
<application>
        ...
        <receiver android:name="com.amazonaws.mobileconnectors.pinpoint.targeting.notification.PinpointNotificationReceiver" android:exported="false" >
        <intent-filter>
            <action android:name="com.amazonaws.intent.fcm.NOTIFICATION_OPEN" />
        </intent-filter>
    </receiver>
</application>
```





MainActivity로 이동하여 Amazon Pinpoint client 생성을 위한 코드를 작성합니다. 

```java
public class MainActivity extends AppCompatActivity {
  ...
private static PinpointManager pinpointManager;

    public static PinpointManager getPinpointManager(final Context applicationContext) {
        if (pinpointManager == null) {
            final AWSConfiguration awsConfig = new AWSConfiguration(applicationContext);
            AWSMobileClient.getInstance().initialize(applicationContext, awsConfig, new Callback<UserStateDetails>() {
                @Override
                public void onResult(UserStateDetails userStateDetails) {
                    Log.i("INIT", userStateDetails.getUserState().toString());
                }

                @Override
                public void onError(Exception e) {
                    Log.e("INIT", "Initialization error.", e);
                }
            });

            PinpointConfiguration pinpointConfig = new PinpointConfiguration(
                    applicationContext,
                    AWSMobileClient.getInstance(),
                    awsConfig);

            pinpointManager = new PinpointManager(pinpointConfig);

            FirebaseInstanceId.getInstance().getInstanceId()
                    .addOnCompleteListener(new OnCompleteListener<InstanceIdResult>() {
                        @Override
                        public void onComplete(@NonNull Task<InstanceIdResult> task) {
                            if (!task.isSuccessful()) {
                                Log.w(TAG, "getInstanceId failed", task.getException());
                                return;
                            }
                            final String token = task.getResult().getToken();
                            Log.d(TAG, "Registering push notifications token: " + token);
                            pinpointManager.getNotificationClient().registerDeviceToken(token);
                        }
                    });
        }
        return pinpointManager;
    }
  ...
}
```



MainActivity의 onCreate함수에서 방금생성한 getPinpointManager함수를 통해 초기화 합니다.  

MainActivity.java 

```java
 @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ...
        // Initialize PinpointManager
        getPinpointManager(getApplicationContext());
    	  ...
    }

```



PushListenerService.java 를 생성하여 Push메시지를 받을 수 있는 서비스를 생성합니다. 

```java
package com.example.socialandroidapp;
import android.content.Intent;
 import android.os.Bundle;
 import android.support.v4.content.LocalBroadcastManager;
 import android.util.Log;

 import com.amazonaws.mobileconnectors.pinpoint.targeting.notification.NotificationClient;
 import com.amazonaws.mobileconnectors.pinpoint.targeting.notification.NotificationDetails;
 import com.google.firebase.messaging.FirebaseMessagingService;
 import com.google.firebase.messaging.RemoteMessage;

 import java.util.HashMap;

 public class PushListenerService extends FirebaseMessagingService {
     public static final String TAG = PushListenerService.class.getSimpleName();

     // Intent action used in local broadcast
     public static final String ACTION_PUSH_NOTIFICATION = "push-notification";
     // Intent keys
     public static final String INTENT_SNS_NOTIFICATION_FROM = "from";
     public static final String INTENT_SNS_NOTIFICATION_DATA = "data";

     @Override
     public void onNewToken(String token) {
         super.onNewToken(token);

         Log.d(TAG, "Registering push notifications token: " + token);
         MainActivity.getPinpointManager(getApplicationContext()).getNotificationClient().registerDeviceToken(token);
     }

     @Override
     public void onMessageReceived(RemoteMessage remoteMessage) {
         super.onMessageReceived(remoteMessage);
         Log.d(TAG, "Message: " + remoteMessage.getData());

         final NotificationClient notificationClient = MainActivity.getPinpointManager(getApplicationContext()).getNotificationClient();

         final NotificationDetails notificationDetails = NotificationDetails.builder()
                 .from(remoteMessage.getFrom())
                 .mapData(remoteMessage.getData())
                 .intentAction(NotificationClient.FCM_INTENT_ACTION)
                 .build();

         NotificationClient.CampaignPushResult pushResult = notificationClient.handleCampaignPush(notificationDetails);

         if (!NotificationClient.CampaignPushResult.NOT_HANDLED.equals(pushResult)) {
             /**
                The push message was due to a Pinpoint campaign.
                If the app was in the background, a local notification was added
                in the notification center. If the app was in the foreground, an
                event was recorded indicating the app was in the foreground,
                for the demo, we will broadcast the notification to let the main
                activity display it in a dialog.
             */
             if (NotificationClient.CampaignPushResult.APP_IN_FOREGROUND.equals(pushResult)) {
                 /* Create a message that will display the raw data of the campaign push in a dialog. */
                 final HashMap<String, String> dataMap = new HashMap<>(remoteMessage.getData());
                 broadcast(remoteMessage.getFrom(), dataMap);
             }
             return;
         }
     }

     private void broadcast(final String from, final HashMap<String, String> dataMap) {
         Intent intent = new Intent(ACTION_PUSH_NOTIFICATION);
         intent.putExtra(INTENT_SNS_NOTIFICATION_FROM, from);
         intent.putExtra(INTENT_SNS_NOTIFICATION_DATA, dataMap);
         LocalBroadcastManager.getInstance(this).sendBroadcast(intent);
     }

     /**
      * Helper method to extract push message from bundle.
      *
      * @param data bundle
      * @return message string from push notification
      */
     public static String getMessage(Bundle data) {
         return ((HashMap) data.get("data")).toString();
     }
 }
```

이제 코드는 모두 완성이 되었습니다. 안드로이드 스튜디오 프로젝트 상단에 **실행버튼** 을 눌러 이미 생성하신 에뮬레이터로 실행시키세요. 실행 후 MainActivity화면까지 이동합니다. 
![c9after](/images/run.png)

