---
title: "ECS 클러스터 삭제"
chapter: false
weight: 1
---

## ECS 클러스터 삭제

### 서비스 종료

아래 명령어를 Cloud9 터미널에 입력하여 서비스의 작업을 중단합니다.
```
aws ecs update-service --cluster tic-tac-toe-cluster --service matchmaker --desired-count 0
aws ecs update-service --cluster tic-tac-toe-cluster --service tic-tac-toe-client --desired-count 0
aws ecs update-service --cluster tic-tac-toe-cluster --service dashboard --desired-count 0
```

모든 서비스의 작업들이 종료될때까지 1분가량 기다려줍니다.

### 작업 종료

1. **ECS** 서비스로 갑니다.
1. 사이드의 **Clusters** 메뉴를 선택합니다.
1. tic-tac-toe-cluster 를 선택합니다.
1. 탭 메뉴에서 **Tasks**를 선택합니다.
1. **Stop All** 을 선택합니다.

![Example Service](/images/tic-tac-toe/delete-cluster-1.png)

1. 다이얼로그 창의 텍스트 박스에 STOP ALL 을 입력합니다.
1. **Stop all**을 선택하여 모든 작업을 정지시킵니다.

![Example Service](/images/tic-tac-toe/delete-cluster-2.png)



{{% notice info %}}
새로고침 아이콘을 눌러 모든 작업(Task)이 종료된것을 확인하면 다음으로 넘어가세요.
{{% /notice %}}

![Example Service](/images/tic-tac-toe/delete-cluster-3.png)

아래 명령어를 Cloud9 터미널에 입력하여 등록된 서비스를 해지합니다.

```
aws ecs delete-service --cluster tic-tac-toe-cluster --service matchmaker
aws ecs delete-service --cluster tic-tac-toe-cluster --service dashboard
aws ecs delete-service --cluster tic-tac-toe-cluster --service tic-tac-toe-client
```

모든 서비스가 삭제될때까지 약간의 시간이 걸릴 수 있습니다. 1분가량 기다려줍니다.
아래 명령어를 Cloud9 터미널에 입력하여 클러스터를 삭제합니다.
```
aws ecs delete-cluster --cluster tic-tac-toe-cluster
```

{{% notice info %}}
클러스터 삭제 명령이 에러가 날 경우, 잠시 기다렸다가 다시 시도하시기 바랍니다.
{{% /notice %}}

ECS 클러스터를 모두 삭제 했습니다.