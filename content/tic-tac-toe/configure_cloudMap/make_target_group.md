---
title: "타겟 그룹 만들기"
date: 2018-08-07T08:30:11-07:00
weight: 3
---

## 매치메이커 타겟 그룹 만들기 

1. EC2 서비스로 넘어갑니다.
1. 좌측 서이드 메뉴에서 **Target Groups**를 선택하고 **Create target group**을 선택하고 아래와 같이 입력합니다.

* **Target group name**: matchmaker
* **Target type**: IP
* **VPC**: tic-tac-toe-vpc
* **Health check settings/Path**: /api/health
![Example Service](/images/tic-tac-toe/target_group-1.png)

헬스체크 시간을 단축하기 위해 **Advanced health check settings**의 파라메터를 아래와 같이 수정합니다.

* **Healthy threshold**: 3
* **Interval**: 10
![Example Service](/images/tic-tac-toe/target_group-2.png)


위와 같은 방법으로 2개의 타겟 그룹을 더 만듭니다.
달라지는것은 **Target group name**와 **Health check settings**/**Path** 입니다.

## 대시보드 타겟 그룹 만들기 

* **Target group name**: dashboard
* **Health check settings/Path**: /

## 클라이언트 타겟 그룹 만들기 

* **Target group name**: tic-tc-toe-client
* **Health check settings/Path**: /
![Example Service](/images/tic-tac-toe/target_group-3.png)

{{% notice info %}}
matchmaker 만 Port와 Target type이 다른것을 주의하세요.
{{% /notice %}}