---
title: "안드로이드 툴 설치하기"
chapter: false
weight: 22
---



Java SDK, Android SDK, and Android Studio가 필요합니다. 설치가 필요하면 아래 링크를 통해 각각 설치하십시요

**참고로 Android SDK, AVD의 버젼은 크게 중요하지 않습니다.**

#### Java SDK Install

Java SDK8.0 이상 설치 되어있지 않다면 [Download](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)



#### Android Studio 및 SDK Install

Android Studio 설치되어있지 않다면  [Download](https://developer.android.com/studio/)

설치과정중 다음과 같이 **Android SDK**를 함께 인스톨 하세요. 

![Create User](/images/sdkinstall.png)





#### Android AVD (Emulator) Install

Android Studio를 설치하신 후 실행합니다. 실행후 다음과 같은 화면에서 **AVD Manager** 를 선택하세요.

![Create User](/images/selectavd.png)

<b>Create Virtual Device.</b>을 선택합니다. 

![Create User](/images/androidstudio-avd-1.png)

<b>Pixel 2</b> 를 선택하시고  <b> Next </b> 를 누르세요.  

![Create User](/images/androidstudio-avd-2.png)

<b>설치된 AVD가 하나도 없다면 Pie 나 Q 를 선택하세요. Next </b> 를 누르세요. 

<b>!! 설치시간이 매우 오래 걸립니다.  만약 Oreo, Nougat정도 설치되어 있으면 AVD는 추가 설치 없이 진행해도 됩니다. cancel을 눌러 취소하세요.</b>

![Create User](/images/emul-q.png)

AVD name 을 넣으시고 <b> Finish</b> 를 누르세요.

![Create User](/images/androidstudio-avd-4.png)

![Create User](/images/androidstudio-avd-5.png)