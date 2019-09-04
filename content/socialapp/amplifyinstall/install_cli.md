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



본 워크샵은  **us-east-1** 에서 진행됩니다. Default Region을 **us-east-1** 을 넣으세요. 

User name에는 이미 생성하신 IAM계정을 넣으세요. 

마찬가지로 이미 생성하신 Access Key와 Secret Access Key를 넣으세요. 

Profile Name은 default로 사용하시거나 별도의 이름을 주어 --profile옵션으로 활용하실 수 있습니다. **us-east-1-profile** 이라고 넣습니다. (원하는 이름을 넣으셔도 됩니다.) 



![Create User](/images/awsconfigure.png)

