---
title: "클라우드 맵 삭제"
chapter: false
weight: 3
---

아래 명령어를 Cloud9 터미널에 입력하여 Cloud Map 리소스를 삭제합니다.
```
aws servicediscovery delete-service --id $(aws servicediscovery list-services | jq -r '.Services[0].Id')
aws servicediscovery delete-namespace --id $(aws servicediscovery list-namespaces | jq -r '.Namespaces[0].Id')
```

네임스페이스를 지우는데는 시간이 몇분정도 걸립니다. 5분정도 기다린 후 확인을 해주세요.