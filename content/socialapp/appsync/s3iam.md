---
title: "IAM 권한 주기"
chapter: false
weight: 13
---

Write 화면에서 작성된 게시물은 <b>AWS console > service > Dynamodb  </b> 에 생성된 table에 추가가 되어 있지 않습니다.  아래 로그로 인해 업로드 되지 않았는데요, 바로 S3에 대한 **AccessDenied**이 원인입니다.

```verilog
2019-09-04 11:13:55.305 8048-8081/com.example.socialandroidapp D/EGL_emulation: eglMakeCurrent: 0xe1005300: ver 3 0 (tinfo 0xe10036b0)
2019-09-04 11:13:55.321 8048-8057/com.example.socialandroidapp W/System: A resource failed to call close. 
2019-09-04 11:13:55.330 8048-8081/com.example.socialandroidapp D/EGL_emulation: eglMakeCurrent: 0xe1005300: ver 3 0 (tinfo 0xe10036b0)
2019-09-04 11:13:55.334 8048-8081/com.example.socialandroidapp I/chatty: uid=10085(com.example.socialandroidapp) RenderThread identical 1 line
2019-09-04 11:13:55.366 8048-8081/com.example.socialandroidapp D/EGL_emulation: eglMakeCurrent: 0xe1005300: ver 3 0 (tinfo 0xe10036b0)
2019-09-04 11:13:57.057 8048-8153/com.example.socialandroidapp D/com.amazonaws.request: Received error response: com.amazonaws.services.s3.model.AmazonS3Exception: Access Denied (Service: null; Status Code: 403; Error Code: AccessDenied; Request ID: 80D7D91725581D4C), S3 Extended Request ID: dY+iw43vR8nE8DDB4L3F4/ijgC/Ydts/KKgwpwTcplUrtWTuN9GBfAxWcQNIgLRgqW0uR1OEYmA=
2019-09-04 11:13:57.059 8048-8153/com.example.socialandroidapp V/InterceptorCallback: Thread:[1086]: onFailure() S3 upload failed.

```

Amplify init으로 만들어진 IAM role이 있었습니다. 이 role에 S3 권한을 주어 해결할 수 있습니다. 

<b>AWS console > service > IAM > Role > 검색창에 aws-android-workshop-dev </b>를 눌러 **auth role**을 선택합니다. 

![Example Service](/images/iamauthrole.png)

권한탭에 **정책 연결**을 누르세요

![Example Service](/images/permission1.png)

필터 부분에 **S3**를 입력합니다. 검색된 내용중 **AmazonS3FullAccess**를 선택하신후 **정책 연결** 버튼을 누릅니다.

![Example Service](/images/permission2.png)

최종모습은 다음과 같습니다. 

![Example Service](/images/permission3.png)

이제 다시 어플리케이션으로 돌아가서 글을 작성해 보시기 바랍니다. 

작성 후 다이나모 디비에 들어가 보시면 새로운 item이 추가 된것을 확인 하실 수 있습니다. 

![Example Service](/images/dynamodb.png)

S3에도 마찬가지로 사진이 업로드 된것을 확인 하실 수 있습니다.  

![Example Service](/images/s3-upload.png)