---
title: "Configuring Amazon Cognito Service"
date: 2018-08-07T08:30:11-07:00
weight: 10
---

Amazon Cognito allows you to quickly and easily add user sign-up, sign-in, and access control to your mobile app.

Run the following command from the top level of your Android project.

```shell
amplify add auth
```

Please refer to the picture below for the selection in progress.

![Example Service](/images/amplify-auth-config.png)

**When setting the domain name, names including aws, amazon, cognito, and _ cannot be used. You should enter the name in the form of 'android-workshop + random text'.**

Enter the redirect URI as shown below.

**redirect signin URI :**

```shell
socialdemoapp://callback/
```

**redirect signout URI :**

```shell
socialdemoapp://signout/
```



When selecting a facebook from **social providers**, select it using the **Space bar**.

**App ID** and **App Secret** information uses the App ID and App Secret from the Facebook credentials you created in the previous tutorial.

Select No for **Do you want to configure advanced settings for the GraphQL API?**.


When finished, update the relevant AWS resources with the following command:

```shell
amplify push
```

![Example Service](/images/amplify-auth-push.png)



If the necessary resources have been created normally, you can check the Hosted UI Endpoint as shown below.
![Example Service](/images/amplify-auth-cli-result.png)


To verify that the Cognito User Pool has been created, access the AWS Management Console(AWS console> Services> Cognito> User Pools).
![Example Service](/images/auth-cognito-userpool.png)

You can also see that each configuration value is created in res/raw/awsconfiguration.json file in Android Studio.
![Example Service](/images/auth-android-config.png)


Finally, update the Cognito domain information you created on the Facebook developer site.

1. On the Facebook developer site, select the app you created in the previous tutorial (AWSAndroidWorkshop).

2. From the left navigation bar, select Settings > Basic. Save Hosted UI Endpoint value output from Amplify CLI execution in App Domains. The same value as Cognito's User Pool Domain Name can also be found in the AWS Management Console's menu [AWS console> Services> Cognito> User Pools> Domain Name]. <br>
**https://&lt;your-user-pool-domain&gt;**
![Facebook](/images/facebook-app-domain.png)

3. Go to the Dashboard menu on the Facebook developer site. <br>
Under 'Add a Product', choose the 'Set Up' button to enable Facebook Login. 
![Facebook](/images/facebook-add-product.png)

4. Move to 'Products > Facebook Login > Settings' on the Facebook developer site. <br>
Enable 'Embedded Browser OAuth Login', enter the following URI in Valid OAuth Redirects URIs, and click Save Changes.<br>
**https://&lt;your-user-pool-domain&gt;/oauth2/idpresponse**
![Facebook](/images/facebook-oauth-redirect.png)


