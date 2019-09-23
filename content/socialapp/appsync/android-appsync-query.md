---
title: "게시물 리스트업하기 "
chapter: false
date: 2018-08-07T08:30:11-07:00
weight: 20
---

이제 메인 화면에서 사용자가 게시한 화면을 list up 해보도록 하겠습니다. 

MainActivity.java 의 onCreate 함수에서 위에서 ClientFactory 이용하여 AWSAppSyncClient를 생성합니다. 

아래와 같이 ClientFactory.appSyncInit(...)를 onCreate()함수내에 복사합니다. 

```java
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        checkPermission();
        //appsync
        ClientFactory.appSyncInit(getApplicationContext());
        ...
  }
```



onResume()에 아래와 같이 queryList()를 호출하도록 코드를 추가합니다.  

```java
    protected void onResume() {
        super.onResume();
        //appsync
        queryList();
    }
```

queryList() 함수와 필요한 쿼리 결과를 얻어오는 콜백함수를 아래와 같이 추가합니다.

```java
    private PostAdapter mAdapter;

public void queryList() {
        ClientFactory.getAppSyncClient().query(ListPostsQuery.builder()
                .id("DEV-DAY")
                .sortDirection(ModelSortDirection.DESC)
                .build())
                .responseFetcher(AppSyncResponseFetchers.CACHE_AND_NETWORK)
                .enqueue(queryCallback);
    }
    private ArrayList<ListPostsQuery.Item> mItems;

    private GraphQLCall.Callback<ListPostsQuery.Data> queryCallback = new GraphQLCall.Callback<ListPostsQuery.Data>() {

        @Override
        public void onResponse(@Nonnull Response<ListPostsQuery.Data> response) {
            Log.e(TAG, response.data().listPosts().items().toString());
            mItems = new ArrayList<>(response.data().listPosts().items());

            /* Log.i(TAG, "Retrieved list items: " + mItems.toString()); */

            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    mAdapter.setItems(mItems);
                    mAdapter.notifyDataSetChanged();
                }
            });
        }

        @Override
        public void onFailure(@Nonnull ApolloException e) {
						e.printStackTrace();
        }
    };
```





List는 RecyclerView를 통해 listup됩니다. RecyclerView에서 사용할 PostAdapter class를 생성합니다.  

PostAdapter.java 

```java
package com.example.socialandroidapp;


import android.content.Context;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.TextView;

import com.amazonaws.ClientConfiguration;
import com.amazonaws.Protocol;
import com.amazonaws.amplify.generated.graphql.ListPostsQuery;
import com.amazonaws.services.s3.AmazonS3Client;
import com.squareup.picasso.Picasso;

import java.net.URL;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;


public class PostAdapter extends RecyclerView.Adapter<PostAdapter.Holder> {

    private static String TAG = "dev-day-item";
    private LayoutInflater mInflater;
    private List<ListPostsQuery.Item> mData = new ArrayList<>();
    private Context ctx;

    PostAdapter(Context context) {
        ctx = context;
        this.mInflater = LayoutInflater.from(context);
    }

    @NonNull
    @Override
    public PostAdapter.Holder onCreateViewHolder(@NonNull ViewGroup viewGroup, int i) {
        View view = mInflater.inflate(R.layout.post_item, viewGroup, false);
        Holder holder = new Holder(view);
        return holder;
    }


    @Override
    public void onBindViewHolder(@NonNull final PostAdapter.Holder holder, int i) {
        holder.bindData(mData.get(i));

        ListPostsQuery.Photo so = mData.get(i).photo();

        String bucketName = so.fragments().s3Object().bucket();
        String pName = so.fragments().s3Object().key();

        ClientConfiguration clientConfig = new ClientConfiguration();
        clientConfig.setProtocol(Protocol.HTTP);

        AmazonS3Client s3 = new AmazonS3Client(ClientFactory.getAWSCredentials(), clientConfig);

        long d = System.currentTimeMillis() + (2 * 24 * 60 * 60 * 1000);

        URL url = s3.generatePresignedUrl(bucketName, pName, new Date(d));

        final String tmpStr = url.toString();
        Log.e(TAG, "URL = " + tmpStr);

        //new DownloadImageFromInternet(ctx, holder.iv).execute(tmpStr);
        Picasso.get().load(tmpStr).into(holder.iv);
    }

    @Override
    public int getItemCount() {
        return mData.size();
    }

    public void setItems(List<ListPostsQuery.Item> items) {
        mData = items;
    }



    public class Holder extends RecyclerView.ViewHolder {
        private TextView writerTxt, contentsTxt, titleTxt;

        private ImageView iv;
        private Button translateBtn;

        public Holder(View view) {
            super(view);

            iv = view.findViewById(R.id.contentImg);
            writerTxt = view.findViewById(R.id.writer);
            contentsTxt = view.findViewById(R.id.contents);
            titleTxt = view.findViewById(R.id.title);
            translateBtn = view.findViewById(R.id.translateBtn);
        }

        void bindData(final ListPostsQuery.Item item) {

            writerTxt.setText(item.author());
            contentsTxt.setText(item.content());
            titleTxt.setText(item.title());


        }
    }
}

```

RecyclerView에서 사용할 PostAdapter를 생성하여 연동합니다. 

MainActivity.java의 onCreate()함수 아랫부분에 아래와 같이 작성해주세요

```java
protected void onCreate(Bundle savedInstanceState) {
  ...
    mAdapter = new PostAdapter(getApplicationContext());

    recyclerView = findViewById(R.id.itemlist);
    recyclerView.setLayoutManager(new LinearLayoutManager(this));
    recyclerView.setHasFixedSize(true);
    recyclerView.setAdapter(mAdapter);
}
```

이제 앱을  실행하면 게시물작성하기에서 만들었던 첫번째 게시물을 확인하실 수 있습니다. 

<img src="/images/main-list.png" width="30%" hight="30%">