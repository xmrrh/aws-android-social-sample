---
title: "ALB 삭제"
chapter: false
weight: 4
---

아래 명령어를 Cloud9 터미널에 입력하여 ALB를 삭제합니다.
```
aws elbv2 delete-load-balancer --load-balancer-arn $(aws elbv2 describe-load-balancers --names tic-tac-toe-client-ALB | jq -r '.LoadBalancers[0].LoadBalancerArn')
aws elbv2 delete-load-balancer --load-balancer-arn $(aws elbv2 describe-load-balancers --names matchmaker-ALB | jq -r '.LoadBalancers[0].LoadBalancerArn')
aws elbv2 delete-load-balancer --load-balancer-arn $(aws elbv2 describe-load-balancers --names dashboard-ALB | jq -r '.LoadBalancers[0].LoadBalancerArn')
```

아래 명령어를 Cloud9 터미널에 입력하여 타겟 그룹을 삭제합니다.
```
aws elbv2 delete-target-group --target-group-arn $(aws elbv2 describe-target-groups --names dashboard | jq -r '.TargetGroups[] | .TargetGroupArn')
aws elbv2 delete-target-group --target-group-arn $(aws elbv2 describe-target-groups --names matchmaker | jq -r '.TargetGroups[] | .TargetGroupArn')
aws elbv2 delete-target-group --target-group-arn $(aws elbv2 describe-target-groups --names tic-tac-toe-client | jq -r '.TargetGroups[] | .TargetGroupArn')
```

1. **EC2** 서비스로 넘어갑니다.
1. 화면의 좌측 사이드 메뉴에서 **Load Balancers**를 선택하고 워크샵에서 생성한 ALB가 모두 삭제됐는지 확인합니다.
1. 화면의 좌측 사이드 메뉴에서 **Target Groups**를 선택하고 워크샵에서 생성한 타겟그룹이 모두 삭제됐는지 확인합니다.