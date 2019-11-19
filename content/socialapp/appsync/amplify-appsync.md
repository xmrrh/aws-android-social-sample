---
title: "Create Repository and Create API"
date: 2018-08-07T08:30:11-07:00
weight: 10
---



Add api using Amplify.  

```bash
amplify add api 
```

Input the value by referring to the picture below. Choose **Amazon Cognito User Pool** for the API. For users who signed up and signed in to Amazon Cognito, grant permission for accessing AWS resources. Authorization is also required for the API approach, which means using cognito as the authorization method.

![Example Service](/images/addapi.png)



You will be prompted to enter the schema in progress. The **editor** you set during amplify init will automatically appear during the schema entry process.

**Windows** users often do not have an editor of their choice. If there is no editor, amplify add api is terminated without the schema input window. In this case, go to the path shown in the console message and open the file with the editor.

Example :  amplify\backend/api/awsandroidworkshop/schema.graphql



The editor that enters the schema comes pre-filled with the **ToDo** schema. This is a sample schema. **Delete** all of them and replace with the schema below.

The schema uses the following values: 

Use @model to automatically create DynamoDB tables, AppSync DataSources, IAM roles, AppSync Resolvers, and more. Generate a partition and sort key for DynamoDB by @key. In other words, we will sort Post with ID ("DEV-DAY") by upload time.

```bash

type Mutation {
      putPostWithPhoto(
          id: String!,
          author: String!,
          title: String,
          content: String,
          url: String,
          uptime: String!,
          photo: S3ObjectInput
          version: Int!
      ): Post
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
  type Post @model @key(fields:["id","uptime"]){
      id: String!
      author: String!
      title: String
      content: String
      url: String
      uptime: String!
      photo: S3Object
      version: Int!
  }
```

When 'amplify add api' is completed, push to create cloud resource.

```bash
amplify push
```

Select the default value and press **Enter**.

![Example Service](/images/apipush.png)

If you go to <b> AWS console> service> AppSync </b>, you can see that api is created as follows.

![Example Service](/images/console-api.png)

Also, if you go to <b> AWS console> service> AppSync> Data Sources </b>, you can see that Dynamodb is created and linked with Data Source.

![Example Service](/images/console-api-ds.png)



Now let's change the Resolver so that we can associate the newly added api with dynamodb. In <b> AWS console> service> AppSync> Schema </b>, find **putPostWithPhoto (..)** in Resolvers and click **Attach**.

![Example Service](/images/console-api-cr.png)



Select **PostTable** for Data source name. Select **Put item with S3 ObjectPut** for Configure the request mapping template.

![Example Service](/images/put_1.png)

Modify the key part of the loaded template as shown below. It means to use id received from source.

```bash
"key" : {
        "id" : { "S" : "$ctx.args.id" }
    },
```

Delete the existing code from 'Configure the response mapping template' and write as follows.

Then press **Save Resolver** to save.

```bash
$util.toJson($util.dynamodb.fromS3ObjectJson($context.source.file))

```



![Example Service](/images/put_2.png)

You can see that res /raw/awsconfiguration.json file is created in Android Studio.

![Example Service](/images/json-appsync.png)

Download the generated code using Amplify codegen to your Android project folder.

```bash
amplify codegen
```



Create a bucket in S3, which is the actual storage space.

Go to **AWS console> service> S3**.

Select the blue button <b> Create Bucket </b>.

![Example Service](/images/bucket1_eng.png)

Enter a bucket name and press **Create**. The bucket name must be globally unique.

![Example Service](/images/bucket2_eng.png)
