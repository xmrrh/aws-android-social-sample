---
title: "Amplify CLI 설치"
chapter: false
weight: 1
---

Amplify를 사용하기 위해 Amplify CLI를 설치합니다. 

```bash
npm install -g @aws-amplify/cli
or 
sudo npm install -g --unsafe-perm --verbose @aws-amplify/cli
```



Amplify에서 사용할 configure를 설정합니다. 

```bash
amplify configure 
```

amplify configure는 다음의 명령 순서에 따라 진행합니다. 

먼저 aws console 로그인 화면이 브라우저로 연결됩니다. **로그인** 하시고 **Enter** 를 입력하세요.

본 워크샵은  us-east-1 에서 진행됩니다. Default Region을 **us-east-1** 을 넣으세요. 

User name에는 새롭게 생성하실 IAM 사용자를 입력합니다. (만약 Access Key와 Secret Access Key 를 알고 있는 기존의 사용자가 있으면 이용하셔도 됩니다)

![Create User](/images/amplify-configure-1.png)

로그인된 브라우저를 통해 aws console의 사용자 추가 화면으로 이동합니다. 

![Example Service](/images/adduser-1.png)

마지막 **create user** 까지 default로 설정을 그대로 놔두면서 아래 화면까지 이동합니다. 

**Download .csv** 를 로컬에 저장합니다. 

![Example Service](/images/adduser-2.png)



**Enter** 를 입력하시고 Access Key와 Secret Access Key 를 차례대로 입력합니다. Access Key와 Secret Access Key 는 download받으신 scv파일이나 사용자 추가 화면에서 가져오실 수 있습니다. 







Profile Name은 default로 사용하시거나 별도의 이름을 주어 --profile옵션으로 활용하실 수 있습니다. **us-east-1-profile** 이라고 넣습니다. (원하는 이름을 넣으셔도 됩니다.) 



![Create User](/images/awsconfigure.png)

