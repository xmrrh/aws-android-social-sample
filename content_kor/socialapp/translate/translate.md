---
title: "Translate서비스 연동하기"
chapter: false
weight: 13

---

build.gradle (Module: app) 에 아래와 같이 dependency를  추가 합니다. 

```xml
dependencies {
...
    implementation 'com.amazonaws:aws-android-sdk-translate:2.13.+'

}
```

TRANSLATE 버튼은 각 게시물에 위치합니다. 버튼 클릭이벤트를 이용하여 번역을 하기 위해 PostAdapter의 bindData()에 버튼 클릭이벤트와 리스너를 등록합니다. 

```java
void bindData(final ListPostsQuery.Item item) {

            titleTxt.setText(item.title());
  ...
  //add 
            translateBtn.setOnClickListener(new Button.OnClickListener() {
                public void onClick(View v) {
                    doTranslate(contentsTxt);
                    doTranslate(titleTxt);
                }
            });
        }

```

실제 번역을 담당하는 함수를 **bindData() 함수** 밑에 작성합니다. AmazonTranslateAsyncClient는 Cognito로부터 얻은AWSCredential을 이용하여 인증합니다. 

```java
 private void doTranslate(final TextView tv) {
        AmazonTranslateAsyncClient translateAsyncClient = new AmazonTranslateAsyncClient(ClientFactory.getAWSCredentials());
        TranslateTextRequest translateTextRequest = new TranslateTextRequest()
                .withText(tv.getText().toString()).withTargetLanguageCode(Util.getLanguageCode(ctx))
                .withSourceLanguageCode("auto");

        translateAsyncClient.translateTextAsync(translateTextRequest, new AsyncHandler<TranslateTextRequest, TranslateTextResult>() {
            @Override
            public void onError(Exception e) {
                Log.e(TAG, "Error occurred in translating the text: " + e.getLocalizedMessage());
            }

            @Override
            public void onSuccess(TranslateTextRequest request, TranslateTextResult translateTextResult) {
                tv.setText(translateTextResult.getTranslatedText());

            }
        });

    }
```

권한탭에 **정책 연결**을 누르세요

안드로이드 스튜디오 프로젝트 상단에 **실행버튼** 을 눌러 이미 생성하신 에뮬레이터로 어플리케이션을 실행합니다.
![c9after](/images/run.png)

메인화면에서 **SETTINGS** 버튼을 통해 화면에 진입합니다. 

<img src="/images/main-list.png" width="30%" hight="30%">

Preferred language setting의 list box에서 번역하고자 하는 언어를 선택합니다. (Ex,korean) 

<img src="/images/language.png" width="30%" hight="30%">

**save** 하고 메인화면으로 돌아옵니다. **Translate**버튼을 눌러 번역이 되는지 확인합니다. 번역이 안되는 것이 맞습니다. 

<img src="/images/main-list.png" width="30%" hight="30%">





