---
title: "Amazon Pinpoint 서비스 생성"
chapter: false
weight: 19
---

이제 Amazon Pinpoint 서비스를 amplify 명령어를 이용하여 생성해보도록 하겠습니다.  

```bash
amplify add notifications
```

아래와 같이 notification channel로 **FCM** 을 선택합니다. 

ApkKey는 이전 페이지에서 생성한 Cloud Messaging의 **server key**를 복사하여 붙여 넣습니다. 

![Example Service](/images/addnoti.png)

생성된 서비스를 push합니다.  

```bash
amplify push
```

