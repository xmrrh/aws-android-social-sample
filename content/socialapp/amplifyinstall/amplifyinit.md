---
title: "Amplify 초기화"
chapter: false
weight: 2
---

다운받은 sample app의 root directory로 이동 후 Amplify 프로젝트를 초기화 합니다

```shell
cd aws-android-workshop/
amplify init
```

프로젝트 이름은 default를 사용하기 위해 **Enter** 를 입력합니다. 

environment는 **dev**,

editor는 편하신 것을 선택합니다. 여기서는 **vim** 을 사용하도록 하겠습니다. 

os type은 **Android**

res directory는 default를 사용하기 위해 **Enter**를 입력합니다. 

AWS Profile을 사용하겠다고 선택(<b>Y</b>) 하시고, 이미 생성하신 프로파일을 선택합니다. **us-east-1-profile**

아래와 그림을 참고하셔서 입력하십시요.  

![Example Service](/images/amplifyinit.png)



Amplify init이 수행 완료 되면 인증된 사용자와 비인증사용자를 위한 IAM role이 자동생성됩니다. 아래와 같이 2개의 IAM role이 생성된 것을 확인할 수 있습니다. 

<b>AWS console > service > IAM > Role > 검색창에 aws-android-workshop-dev </b>

![Example Service](/images/iamauthrole.png)



또한 안드로이드 스튜디오에서 res/raw/awsconfiguration.json.file 파일이 생성된 것을 확인하실 수 있습니다. 

![Example Service](/images/jsonfile.png)



이제 Amplify CLI command를 통해 다양한 AWS Service를 생성 하실 수 있습니다. 

command는 다음을 같이 사용하실 수 있습니다.  

- `amplify add <category>`
- `amplify remove <category>`
- `amplify push <category>`

