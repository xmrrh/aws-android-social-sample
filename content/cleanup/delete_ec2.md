---
title: "EC2 삭제"
chapter: false
weight: 2
---

ECS 클러스터를 삭제할때 사용한 EC2는 자동삭제 되지 않습니다.

아래 명령어를 Cloud9 터미널에 입력하여 ECS 클러스터로 사용했던 EC2를 종료합니다.
```
aws ec2 describe-instances --filter Name=tag:Name,Values="ECS Instance - EC2ContainerService-tic-tac-toe-cluster" | jq -r '.Reservations[] | .Instances[] | .InstanceId' > /tmp/ids
aws ec2 terminate-instances --instance-ids $(cat /tmp/ids | paste -sd " " -)
rm /tmp/ids
```

**EC2** 서비스로 가서 인스터스가 모두 종료되었는지 (종료 중인지) 확인합니다.