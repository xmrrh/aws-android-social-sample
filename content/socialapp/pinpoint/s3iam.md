---
title: "Push 메시지 보내기"
chapter: false
weight: 30
---

Push 메시지는 pinpoint의 campaign을 통해 보내실 수 있습니다.  shell에서 아래와 같이 실행하신 후 (콘솔상에서 직접 이동하셔도 됩니다.)

```bash
amplify notifications console
```

<b>AWS console > service > Pinpoint > campaigns > create a campaign </b> 까지 이동합니다. 

Campaign name에 **first-push**라고 입력하신후 **Next**  단계로 이동합니다. 



![Example Service](/images/cam1.png)

**Create a segment** 를 선택하신후 **myGroup** 라고 이름을 넣습니다. 나머지 설정은 그대로 두신 후  **Next**  단계로 이동합니다. 우측에 **3 endpoints** 는 현재 여러분의 앱이 활성화 되어있는 모바일 디바이스의 수를 의미합니다. segment group의 대상이 되는 모바일 디바이스 수이며, filter를 변경하면 endpoint 수가 변경됩니다. 

![Example Service](/images/cam2.png)

**Push notification details** 의 **title** 과 **Body** 를 작성하신 후 **Next**  단계로 이동합니다.

![Example Service](/images/cam3.png)

default로 두신 후 **Next**  단계로 이동합니다.

![Example Service](/images/cam4.png)

맨 아래 **Launch campaign** 을 누르면 작성된 push 메시지가 각 디바이스로 전송됩니다. 

![Example Service](/images/cam5.png)

다음은 Push 메시지를 수신 받은 모습입니다.  

<img src="/images/cam6.png" width="30%" hight="30%">