---
title: "ALB 만들기"
date: 2018-08-07T08:30:11-07:00
weight: 11
---

## 매치메이커 로드밸런서 생성

### 기본 설정

1. 사이드 메뉴에서 **Load Balancers** 메뉴를 선택하고 **Create Load Balancer**를 선택합니다.
1. Application Load Balancer를 선택합니다.
1. 아래와 같이 내용을 입력합니다.

* **Name**: matchmaker-ALB
* **Availablility Zones** 의 **VPC**: tic-tac-toe-vpc
* **Availability Zones**의 모든 항목을 체크

{{% notice info %}}
가용영역 (Availability Zones)은 워크샵을 하는 사람에따라 다르게뜰 수 있습니다.<br>
us-west-2a, us-west-2b 가 아니라 us-west-2c, us-west-2d 가 나올수도 있습니다.<br>
us-west-2로 시작하면 문제 없으니 모두 선택해줍니다.
{{% /notice %}}

다른 항목은 기본값을 사용합니다.

![Example Service](/images/tic-tac-toe/alb-1.png)

**Next**을 선택하여 다음 화면으로 넘어갑니다.


### 보안 설정

HTTPS를 사용하지 않아서 나오는 경고는 무시합니다.<br>
**Next**을 선택하여 다음 화면으로 넘어갑니다.

### 보안 그룹 설정

1. Security Groups 에서 **Create a new security group** 선택합니다.
1. **Security group name**: WebServer-SG  입력합니다.
1. 포트 설정은 기본값을 그대로 둡니다.

![Example Service](/images/tic-tac-toe/alb-2.png)

**Next**을 선택하여 다음 화면으로 넘어갑니다.

### 라우팅 설정

Routing 설정에서 아래와 같이 입력합니다.

* **Target group**: Existing target group
* **Name**: matchmaker 선택

![Example Service](/images/tic-tac-toe/alb-3.png)

Review 화면까지이 나올때까지 **Next**선택을 선택합니다.

### 리뷰

**Create** 선택하고 ALB를 생성합니다.


로드밸런서가 생성되고 나면 리스트 화면에서 퍼블릭 주소를 복사해서 기록해 놓습니다.
나중에 필요합니다.

![Example Service](/images/tic-tac-toe/alb-4.png)


## 대시보드 로드밸런서

매치메이커 로드밸런서와 만드는 과정은 동일하며, 입력값만 아래처럼 넣습니다.

* **Name**: dashboard-ALB
* **Availablility Zones** 의 **VPC**: tic-tac-toe-vpc
* **Availability Zones**의 모든 항목을 체크
* **Security Group**: 앞서 만든 WebServer-SG을 선택
* **Target group**: Existing target group
* **Name**: dashboard 선택

로드밸런서가 생성되고 나면 리스트 화면에서 퍼블릭 주소를 복사해서 기록해 놓습니다.

## 클라이언트 로드밸런서

대시보드 로드밸런서와 만드는 방식은 동일하며, 파라메터만 다음과 같이 입력한다.

* **Name**: tic-tac-toe-client-ALB
* **Availablility Zones** 의 **VPC**: tic-tac-toe-vpc
* **Availability Zones**의 모든 항목을 체크
* **Security Group**: 앞서 만든 WebServer-SG을 선택
* **Target group**: Existing target group
* **Name**: tic-tac-toe-client 선택

로드밸런서가 생성되고 나면 리스트 화면에서 퍼블릭 주소를 복사해서 기록해 놓습니다.