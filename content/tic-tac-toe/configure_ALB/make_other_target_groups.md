---
title: "대시보드 타겟 그룹 만들기"
date: 2018-08-07T08:30:11-07:00
weight: 4
---

타겟그룹을 2개 더 만듭니다.

## 대시보드 타겟 그룹 생성

앞서 만든 매치메이커 타겟그룹과 과정은 동일합니다.

* **Create target group**을 선택하고 아래와 같이 입력합니다.
* **Target group name**: dashboard
* **Target type**: ip
* **VPC**: tic-tac-toe-vpc
* **Port**: 80
* **Health check settings - Path**: /

나머지 항목은 기본값으로 둡니다.

![Example Service](/images/tic-tac-toe/target_group-dashboard-1.png)

헬스체크 시간을 단축하기 위해 **Advanced health check settings**의 파라메터를 아래와 같이 수정합니다.

* **Healthy threshold**: 2
* **Interval**: 10

![Example Service](/images/tic-tac-toe/target_group-2.png)

**Create**를 선택하여 타겟그룹을 생성합니다.


## 클라이언트 타겟 그룹 생성
 
**Create target group**을 선택하고 아래와 같이 입력합니다. 위에서 만든 dashboard 타겟 그룹과 이름만 다릅니다.

* **Target group name**: tic-tac-toe-client
* **Target type**: ip
* **VPC**: tic-tac-toe-vpc
* **Port**: 80
* **Health check settings - Path**: /

**Create**를 선택하여 타겟그룹을 생성합니다.

## 결과 화면

3개의 타겟그룹을 다 만들고 나면 다음과 같은 결과화면을 보게됩니다. Port와 Target type을 확인하시기 바랍니다.

![Example Service](/images/tic-tac-toe/target_group-3.png)

