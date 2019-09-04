---
title: "IAM 권한 주기"
chapter: false
weight: 15
---

번역이 안되는 원인은 logcat에서 다음과 같은 에러, 즉  **AccessDenied**이 원인입니다.

```
2019-09-04 18:00:40.159 10974-11061/com.example.socialandroidapp E/dev-day-item: Error occurred in translating the text: User: arn:aws:sts::539063931014:assumed-role/aws-android-workshop-dev-20190903125208-authRole/CognitoIdentityCredentials is not authorized to perform: translate:TranslateText (Service: AmazonTranslate; Status Code: 400; Error Code: AccessDeniedException; Request ID: af77f01a-f368-4987-96f0-780c7c6ca26d)



```

Amplify init으로 만들어진 IAM role이 있었습니다. 이 role에 Translate 권한을 주어 해결할 수 있습니다. 

<b>AWS console > service > IAM > Role > 검색창에 aws-android-workshop-dev </b>를 눌러 **auth role**을 선택합니다. 

![Example Service](/images/iamauthrole.png)

권한탭에 **정책 연결**을 누르세요

![Example Service](/images/permission1.png)

필터 부분에 **Translate**를 입력합니다. 검색된 내용중 **TranslateFullAccess**를 선택하신후 **정책 연결** 버튼을 누릅니다.

<img src="/images/search-translate.png">

최종모습은 다음과 같습니다. 

<img src="/images/iam-translate.png" >

이제 다시 어플리케이션으로 돌아가서 **TRANSLATE** 버튼을 클릭합니다. 지정된 언어로 변경되는 것을 확인할 수 있습니다.  

<img src="/images/result.png" width="30%" hight="30%">

