---
title: "첫번째 리포지토리 만들기"
date: 2018-08-07T08:30:11-07:00
weight: 10
---

이 페이지에서는 matchmaker 리포지토리를 만듭니다. 순서를 잘 기억하시고 다음페이지에서 나머지 3개의 리포지토리를 만들때 참고하시기 바랍니다.

1. ECS 메뉴에서 사이드의 **Repositories**를 선택합니다.
1. **Create repository**를 선택합니다.
1. 첫번째 리포지토리는 이름을 matchmaker로 합니다.

![Example Service](/images/tic-tac-toe/ecr-1.png)

방금 만든 matchmaker를 선택하고 메뉴에서 **View push commands**를 선택합니다.
![Example Service](/images/tic-tac-toe/ecr-2.png)

이 화면에는 4개의 커맨드를 포함하고 있습니다. 이는 ECR에 컨테이너 이미지를 푸시하기 위해 필요한 명령어 순서 입니다.
이 명령어들을 순서에 맞춰 복사해 놓습니다.

Cloud9 워크샵으로 돌아가서 아래 명령어를 입력합니다.
```
c9 open ~/environment/tic-tac-toe-workshop/server/matchmaker/build.sh
```


이 파일엔 ECR 푸시명령어들이 주석처리되어 있는데 이 주석들을 지우고 아까 복사한 명령어를 넣습니다.

결과 화면은 다음 스크린샷처럼 5줄의 명령어로 이뤄어져있어야 합니다.
![Example Service](/images/tic-tac-toe/ecr-3.png)

파일 수정이 끝났으면 저장을 하고 화면을 닫습니다.