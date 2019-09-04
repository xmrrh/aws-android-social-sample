---
title: "저장소 생성 및 API 생성"
date: 2018-08-07T08:30:11-07:00
weight: 10
---



Amplify를 이용하여 api를 추가합니다.  

```bash
amplify add api 
```

입력값은 아래 그림을 참고하여 넣습니다. API인가를 위해 **Amazon Cognito User Pool**을 선택합니다. Amazon Cognito에 가입하고 로그인한 사용자의 경우 AWS 리소스접근에 대한 인가를 부여합니다. Api 접근시에도 인가가 필요한데, 이 인가 방법으로 cognito를 사용한다는 의미입니다.  

![Example Service](/images/addapi.png)

스키마는 아래 값을 사용합니다. 

```bash
type Mutation {
        putPostWithPhoto(
                id: ID!,
                author: String!,
                title: String,
                content: String,
                url: String,
                ups: Int,
                downs: Int,
                photo: S3ObjectInput,
                version: Int!
        ): Post
}

type Post @model{
        id: ID!
        author: String!
        title: String
        content: String
        url: String
        ups: Int
        downs: Int
        photo: S3Object
        version: Int!
}


type S3Object {
        bucket: String!
        key: String!
        region: String!
}

input S3ObjectInput {
        bucket: String!
        key: String!
        region: String!
        localUri: String
        mimeType: String
}
```

amplify add api 가 완료되면 클라우드리소스 생성을 위해 push합니다.

```
amplify push
```

필요한 입력값은 default 선택 후 **Enter** 하십시요

![Example Service](/images/apipush.png)

<b>AWS console > service > AppSync  </b>에 들어가보시면 다음과 같이 api가 생성된 것을 확인하실 수 있습니다. 

![Example Service](/images/console-api.png)

또한 <b>AWS console > service > AppSync >Data Sources</b> 에 들어가보시면 Dynamodb가 생성되고 Data Source로 연동된 모습을 확인하실 수 있습니다. 

![Example Service](/images/console-api-ds.png)



이제 Mutation 에 새로 추가한 api를 dynamodb와 연계할 수 있는 Resolver를 생성 해보겠습니다. <b>AWS console > service > AppSync >Schema</b>에서 Resolvers중 **putPostWithPhoto** 를 찾아 **Attach**를  누릅니다. 

![Example Service](/images/console-api-cr.png)



Data source name에 **PostTable** 를 선택합니다. Configure the request mapping template은 **Put item with S3 ObjectPut** 를 선택하시고 **Save Resolver **을 눌러 저장합니다. 

![Example Service](/images/console-cr-puts3.png)



또한 안드로이드 스튜디오에서 res/raw/awsconfiguration.json.file 파일이 생성된 것을 확인하실 수 있습니다. 

![Example Service](/images/json-appsync.png)

Amplify codegen을 통해 를 이용하여 api를 추가합니다.  api는 <b>AWS console > service > AppSync >Settings</b>에서 확인가능합니다. 

![Example Service](/images/console-codeapi.png)

```bash
amplify add codegen --apiId xxx
```

만약 "Codegen support only one GraphQL API per project" 에러가 나올경우 아래 처럼 remove하시고 다시 추가하십시요

```bash
amplify codegen remove
amplify add codegen --apiId xxx
```



진행중 나오는 질문은 모두 default로 처리하십시요

![Example Service](/images/codeapi.png)