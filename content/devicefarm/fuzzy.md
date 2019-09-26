---
title: "Fuzzy App Test"
chapter: false
weight: 22
---



Fuzzy 테스트를 통해 간단하게 다양한 디바이스에서 테스트를 수행해 봅니다.

⦁	테스트 프로젝트를 생성하기 위하여 AWS콘솔로 돌아가 서비스 목록에서 Device Farm을 선택합니다.
⦁	화면에 +Create a New project를 선택합니다.
⦁	Create project 팝업에서 적절한 Project name을 입력합니다.
⦁	Success문구와 함께 테스트 프로젝트 화면으로 진입하게됩니다. 앱 테스트를 위한 Device Farm project 생성이 완료되었습니다.

⦁	Create a new run을 선택합니다.

![Create User](/images/fuzzy1.png)

⦁	안드로이드와 사과모양 아이콘을 클릭하고, 아래에 Spending되는 항목에서 Upload를 눌러서 이전에 생성해둔 apk파일을 선택합니다. 혹은 파일을 업로드 영역으로 드래그 앤 드랍하여도 됩니다.

![Create User](/images/fuzzy2.png)

⦁	업로드가 완료되면 해당 앱에 대한 기본적인 정보가 표출되게 됩니다. 패키지의 도메인네임과 메인 액티비티명, 최소 지원 SDK버전, 지원하는 스크린 사이즈과 같은 정보들이 나열됩니다. Next step을 선택합니다.

![Create User](/images/fuzzy3.png)


⦁	Configure항목이 나타나며, Test항목에 default로 Built-in: Fuzz로 설정되어있습니다. Event Count는 지 테스트가 수행할 사용자 인터페이스 이벤트 수를 나타내는 1에서 10,000 사이의 숫자를 지정합니다. Event throttle은 퍼지 테스트가 다음 사용자 인터페이스 이벤트를 수행하기 전에 대기할 시간(밀리초)을 나타내는 1에서 1,000 사이의 숫자를 지정합니다. Randomizer seed는 퍼지 테스트가 사용자 인터페이스 이벤트를 임의로 지정할 때 사용할 숫자를 지정합니다. 후속 퍼지 테스트에 동일한 숫자를 지정하면 동일한 이벤트 시퀀스가 보장됩니다.

![Create User](/images/fuzzy4.png)


⦁	Fuzz테스트 에서는 기본적인 값으로 두고 진행합니다. Next step을 클릭합니다.
⦁	Select devices항목입니다. 기본적으로 Top Devices로 대개 많이 사용되는 기종에 대해 미리 구성된 리스트를 사용합니다. 업로드한 앱이 기종에 호환되는지 여부가 미리 표시되게 됩니다. 케이스에 따라서 Create a new device pool에서 원하는 기종들 OS버전별, Form factor에 따른 테스트 리스트 프리셋을 구성하실 수 있습니다.  Next step으로 다음으로 이동합니다.

![Create User](/images/fuzzy5.png)
![Create User](/images/fuzzy6.png)


⦁	Specify device state 페이지에서는 앱 구동 혹은 테스트에 필요한 추가 파일과 추가 앱을 설정 할 수 있습니다. 또한 WIFI, Bluetooth와 같은 옵션들을 켜거나 끌 수 있습니다. Device location옵션으로 Latitude and Longitude를 임의로 지정 할 수 있습니다. 나머지 값들은 전부 Default로 두고 Next step으로 이동합니다.
⦁	Review and start run항목에서는 실행에 할애 할 최대 시간을 설정 할 수 있습니다. 이는 테스트 할 디바이스들이 예외나 예상치 못한 동작들로 긴 시간동안 떠있는 경우를 방지할 수 있습니다. 본 데모에서는 앱의 기능들이 많지 않고, 짧은 시간에 테스트가 완료되므로 동작 최소시간인 5분으로 설정하겠습니다. 총 5대의 디바이스에 5분씩 동작하여 25분이 소요됩니다. Confirm and start run을 통해 Fuzz테스트를 시작합니다.

![Create User](/images/fuzzy7.png)


⦁	Fuzz Test 측정 및 결과

⦁	위의 단계를 완료하면 project test페이지로 돌아오게 되며, 해당 앱이 테스트 동작중임을 나타내게 됩니다. 동작이 완료되는데에는 아까 설정한 최대 시간 만큼이 소요되게 됩니다.

![Create User](/images/fuzzy8.png)


⦁	Run 리스트를 눌러 상세에 들어가게 되면 각 디바이스 별 실행 결과와 테스트 단계에서 생성된 스크린 샷을 보여주게 됩니다. Summery형식의 결과표로써 각 디바이스 별 데이터 포인트를 손쉽게 확인 할 수 있습니다.
⦁	디바이스 리스트 중 하나를 클릭해서 상세로 가게 되면 테스팅 과정에 대한 영상을 열람 할 수 있으며, 각 테스트 단계의 상세한 내역과 로그들을 보거나 다운로드 받을 수 있습니다.
⦁	특히 Performance내역에는 CPU, Memory 사용량과 Threads지표를 상세히 보여줌으로써 발생할 수 있는 퍼포먼스 이슈에 대한 확인이 가능합니다.

![Create User](/images/fuzzy9.png)
