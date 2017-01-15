#第08課 CardView設計


## (2) 使用 Adapter



#####執行結果:
![GitHub Logo](/images/results08-2.jpg)



#####模擬器
```
Nexus 5, API 23
```



#####icon 資源
```
Google Material icons: https://material.io/icons/
```



#####建立專案設定
```
(1) Company Domain: abc.com 
(2) Minimum SDK: API 21: Android 5.0(Lollipop)
(3) 選用 Basic Activity
```



#####檔案放置方式:
```
 app 
   |___<java>
   |     |___<com.abc.myapplication>
   |            |___<adapter>
   |            |      |___MyAdapter.java
   |            |
   |            |___<data>
   |            |      |___Life.java   
   |            |
   |            |___MainActivity.java    
   | 
   |___<res>
   |     |___<drawable>
   |     |      |___logo48.png      (尺寸: 48px*48px)	
   |     |      |___picture_1.jpg   (尺寸: 600px*400px)	
   |     |      |___picture_2.jpg   (尺寸: 600px*400px)
   |     |      |___picture_3.jpg   (尺寸: 600px*400px)   
   |     |    
   |     |___<layout>
   |     |      |___activity_main.xml		 
   |     |      |___content_main.xml
   |     |      |___mylayout.xml   
   |     | 	 
   |     |___<values>
   |     |      |___colors.xml  
   |            |
   |            |___<dimens.xml(2)> 
   |            |        |___dimens.xml  
   |            |
   |            |___strings.xml
   |            |
   |            |___<styles.xml(2)> 
   |                     |___styles.xml  
   |
   |___<Gradle Scripts>
                |___build.gradle(Module: app)		 
```


#####檔案名稱: colors.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#550000</color>
    <color name="colorPrimaryDark">#000</color>
    <color name="colorAccent">#550000</color>
    <color name="windowBackground">#000</color>
    <color name="textColor">#000</color>
    <color name="iconTextFront">#ff0000</color>
    <color name="iconTextEnd">#ffaaaa</color>
    <color name="cardBack">#fff</color>
</resources>
```



#####檔案名稱: dimens.xml
```xml
<resources>
    <!-- Default screen margins, per the Android Design guidelines. -->
    <dimen name="activity_horizontal_margin">5dp</dimen>
    <dimen name="activity_vertical_margin">10dp</dimen>
    
    <!-- fab按鈕位置設定 -->
    <dimen name="fab_margin">6dp</dimen>
    <dimen name="fab_marginBottom">40dp</dimen>

    <!-- 卡片尺寸設定 -->
    <dimen name="textSize">18dp</dimen>
    <dimen name="spaceVertical">6dp</dimen>
    <dimen name="textMargin">10dp</dimen>
    <dimen name="cardHeight">350dp</dimen>
    <dimen name="imageHeight">200dp</dimen>
    <dimen name="cardBottomMargin">20dp</dimen>
    <dimen name="cardCornerRadius">5dp</dimen>
</resources>
```



#####檔案名稱: strings.xml
```xml
<resources>
    <string name="app_name"></string>
    <string name="action_settings">Settings</string>
    <string name="logo">商標圖示</string>
</resources>
```



#####檔案名稱: styles.xml
```xml
<resources>
    <!-- 基本應用程式樣式 -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <item name="android:windowBackground">@color/windowBackground</item>
    </style>

    <style name="AppTheme.NoActionBar">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
    </style>

    <style name="AppTheme.AppBarOverlay" parent="ThemeOverlay.AppCompat.Dark.ActionBar" >
        <item name="android:textColorPrimary">@color/textColor</item>
    </style>

    <style name="AppTheme.PopupOverlay" parent="ThemeOverlay.AppCompat.Light" />

</resources>
```



#####檔案名稱: activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context="com.abc.myapplication.MainActivity">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/AppTheme.AppBarOverlay">

        <!-- 修改 Toolbar 內容 -->
        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            app:popupTheme="@style/AppTheme.PopupOverlay">

            <!-- 加入一個icon -->
            <ImageView
                android:layout_width="wrap_content"
                android:contentDescription="@string/logo"
                android:layout_height="wrap_content"
                android:layout_gravity="left"
                android:src="@drawable/logo48"/>

            <!-- 加入第1個文字 -->
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="20dp"
                android:text="Lin "
                android:layout_gravity="left"
                android:textSize="30dp"
                android:textStyle="bold"
                android:textColor="@color/iconTextFront" />

            <!-- 加入第2個文字 -->
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="D"
                android:layout_gravity="left"
                android:textSize="30dp"
                android:textStyle="bold"
                android:textColor="@color/iconTextEnd" />

            <!-- 加入第3個文字 -->
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="esign"
                android:layout_gravity="left"
                android:textSize="22dp"
                android:textStyle="bold"
                android:textColor="@color/iconTextEnd" />
        </android.support.v7.widget.Toolbar>
    </android.support.design.widget.AppBarLayout>

    <include layout="@layout/content_main" />

    <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:layout_marginRight="@dimen/fab_margin"
        android:layout_marginBottom="@dimen/fab_marginBottom"
        android:src="@android:drawable/ic_dialog_email" />

</android.support.design.widget.CoordinatorLayout>
```



#####檔案名稱: content_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_marginTop="55dp"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:orientation="vertical"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    tools:context=".MainActivity">

    <!-- 加入RecycleView, 用來放置多張CardView -->
    <!-- ............................................... -->
    <android.support.v7.widget.RecyclerView
        android:id="@+id/myRecycleView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
    <!-- ............................................... -->

</LinearLayout>
```



#####檔案名稱: mylayout.xml
```xml
<?xml version="1.0" encoding="utf-8"?>

<!-- 設計一張卡片版型 -->
<android.support.v7.widget.CardView
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:card_view="http://schemas.android.com/apk/res-auto"
    android:id="@+id/card_view"
    android:layout_gravity="center"
    android:layout_width="match_parent"
    android:layout_height="@dimen/cardHeight"
    android:layout_marginBottom="@dimen/cardBottomMargin"
    card_view:cardCornerRadius="@dimen/cardCornerRadius">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/cardBack"
        android:orientation="vertical">

        <ImageView
            android:id="@+id/imageView"
            android:layout_width="match_parent"
            android:layout_height="@dimen/imageHeight"
            android:scaleType="centerCrop"/>

        <TextView
            android:id="@+id/info_text"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_margin="@dimen/textMargin"
            android:lineSpacingExtra="@dimen/spaceVertical"
            android:background="@color/cardBack"
            android:textSize="@dimen/textSize"/>

    </LinearLayout>
</android.support.v7.widget.CardView>
```



#####檔案名稱: Life.java
```java
package com.abc.myapplication.data;

public class Life {
    private int picture;  //圖片id
    private String info;  //文字

    public Life(int picture, String info){
        this.picture=picture;
        this.info=info;
    }

    public int getPicture(){return this.picture;}
    public String getInfo(){return this.info;}
}
```



#####檔案名稱: MyAdapter.java
```java
package com.abc.myapplication.adapter;

import android.support.v7.widget.CardView;
import android.support.v7.widget.RecyclerView;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;
import android.widget.TextView;
import com.abc.myapplication.*;
import com.abc.myapplication.data.*;

import java.util.ArrayList;


// 用來連結 <CardView> 和 <資料>的調適器
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.LifeViewHolder>{
    ArrayList<Life> lifes;  //資料

    // 連結資料的CardView及顯示物件
    public static class LifeViewHolder extends RecyclerView.ViewHolder {
        CardView cardView;
        TextView info_TextView;
        ImageView picture_ImageView;

        LifeViewHolder(View itemView) {
            super(itemView);

            cardView = (CardView)itemView.findViewById(R.id.card_view);
            info_TextView = (TextView)itemView.findViewById(R.id.info_text);
            picture_ImageView = (ImageView)itemView.findViewById(R.id.imageView);
        }
    }

    // 將連結的資料
    public MyAdapter(ArrayList<Life> lifes){
        this.lifes = lifes;
    }

    @Override
    public int getItemCount() {
        return lifes.size();
    }

    @Override
    public LifeViewHolder onCreateViewHolder(ViewGroup viewGroup, int i) {
        // 顯示資料物件來自 R.layout.mylayout 中
        View view = LayoutInflater.from(viewGroup.getContext()).inflate(R.layout.mylayout, viewGroup, false);
        LifeViewHolder lifeViewHolder = new LifeViewHolder(view);
        return lifeViewHolder;
    }

    @Override
    public void onBindViewHolder(LifeViewHolder bookViewHolder, int i) {
        // 顯示資料物件 及 資料項目 的對應
        bookViewHolder.info_TextView.setText(lifes.get(i).getInfo());
        bookViewHolder.picture_ImageView.setImageResource(lifes.get(i).getPicture());
    }

    @Override
    public void onAttachedToRecyclerView(RecyclerView recyclerView) {
        super.onAttachedToRecyclerView(recyclerView);
    }
}
```



#####檔案名稱: MainActivity.java
```java
package com.abc.myapplication;

import android.content.Context;
import android.os.Bundle;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.support.v7.widget.Toolbar;
import android.view.View;
import android.view.Menu;
import android.view.MenuItem;

import com.abc.myapplication.adapter.MyAdapter;
import com.abc.myapplication.data.Life;

import java.util.ArrayList;


public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
                        .setAction("Action", null).show();
            }
        });

        //--------------------------
        initializeData(this);
        //--------------------------
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }



    //----------------------
    // 產生資料
    //----------------------
    private void initializeData(Context context) {
        // 產生將顯示的資料
        ArrayList<Life> lifes = new ArrayList<>();
        lifes.add(new Life(R.drawable.picture_1, "蔣勳曾說自己是用佈道的心情傳播對美的感動，現在他已經可說是美的宗教家。近幾年來，蔣勳走遍竹科與各地演講，到場的聽眾們彷彿都期待受到「開釋」，除了在席間對於充滿抑揚頓挫的感性分析頻頻點頭，發問的許多也都是已超乎美學的人生之問。事實上，這幾年來，蔣勳談的美，也有很大的變化。他過去比較偏向幫助大家賞析藝術之美，但他在與聽眾愈來愈多的互動中發現，許多上班族的餘暇時間已極度有限了，刻意附庸風雅地去欣賞音樂會、畫展已經沒有必要，重拾與周遭人的感情，重新找回「像個人樣」的生活方式，才能對美真正有所體會。本期美學學院專訪蔣勳，分享上班族也可以找回的生活之美。"));
        lifes.add(new Life(R.drawable.picture_2, "幾年來，幾乎所有的竹科企業我都去過了，和企業的人有所接觸後，我才知道我過去有「知識偏執」的狀況，但我並沒有真正認識30歲上下的職場工作人員。竹科有一家上市公司的員工平均年齡是31.8歲，他們都是最優秀大學畢業的菁英。 在開始工作的前10年，是人生很重要的階段，但他們卻通常是11點以後才下班。要戀愛，可能沒有時間戀愛；要買房子，就用世俗的固定模式買房子，找一個大家認為有名的設計師；要結婚，但用很草率的方式結婚。我知道很多工程師經由輔導去娶烏克蘭新娘，他們可能連戀愛的時間、耐心都沒有。"));
        lifes.add(new Life(R.drawable.picture_3, "我現在不問工程師有沒有去聽音樂、看展覽，反而是問他們：「你們在這裡工作5年了，有沒有人可以告訴我公司門口那一排是什麼樹？」但很少人能夠回答的出來。事實上，他們公司門口那排小葉欖仁的葉子漂亮得不得了，綠色會在陽光裡發亮。後來我再去，就有一個員工和我說，「謝謝你告訴我這件事，我現在下班時會先看看小葉欖仁再回家，所以比較不會和太太吵架了。」他也問我現在5歲的女兒將來該學鋼琴、還是小提琴，但我建議11點下班的他多抱抱女兒，比較重要。"));

        // 產生 RecyclerView
        RecyclerView recyclerView = (RecyclerView) findViewById(R.id.myRecycleView);
        recyclerView.setHasFixedSize(true);

        // 設定 RecycleView的版型
        LinearLayoutManager linearLayoutManager = new LinearLayoutManager(context);
        recyclerView.setLayoutManager(linearLayoutManager);

        // 產生一個 MyAdapter物件, 連結將加入的資料
        MyAdapter myAdapter = new MyAdapter(lifes);

        // 將結合資料後的 myAdapter 加入 RecyclerView物件中
        recyclerView.setAdapter(myAdapter);
    }
}
```
