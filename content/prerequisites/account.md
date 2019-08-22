---
title: "AWS 계정 생성"
chapter: false
weight: 1
---

{{% notice warning %}}
여러분의 계정은 새로운 IAM 역할(role)을 만들거나 다른 퍼미션(permission)을 만들 수 있는 권한이 있어야 합니다.
{{% /notice %}}

1. 관리자 권한이 있는 AWS 계정이 없다면: [여기를 눌러 새로 만드세요](https://aws.amazon.com/getting-started/)
1. AWS 계정을 가지고 있다면, 다음 링크를 눌러 관리자 권한이 있는 새로운 IAM 유저를 만듭니다.
[Create a new IAM user to use for the workshop](https://console.aws.amazon.com/iam/home?#/users$new)

1. 사용자 내용을 적습니다:
![Create User](/images/iam-1-create-user.png)

1. AdministratorAccess IAM 정책을 추가합니다.:
![Attach Policy](/images/iam-2-attach-policy.png)

1. **Create user**를 선택합니다.:
![Confirm User](/images/iam-3-create-user.png)

1. 로그인 URL을 적어놓고 저장합니다.:
![Login URL](/images/iam-4-save-url.png)
