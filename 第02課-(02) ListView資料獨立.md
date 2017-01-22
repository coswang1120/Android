#第02課 加入ListView清單


## (2) ListView資料獨立



#####執行結果:
![GitHub Logo](/images/results02-2.jpg)



#####模擬器
```
Nexus 5, API 23
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
   | 
   |___<java>
   |     |___<com.abc.myapplication>
   |            |___<data>	
   |            |     |___Common.java
   |            |
   |            |___MainActivity.java	   
   |___<res>
         |___<layout>
         |      |___content_main.xml                                
         |
         |___<values>
                |___colors.xml           
                |
                |___<dimens.xml(2)>
                        |___dimens.xml                  				
```


#####檔案名稱: colors.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#4f0010</color>
    <color name="colorPrimaryDark">#000</color>
    <color name="colorAccent">#4f0010</color>
</resources>
```



#####檔案名稱: dimens.xml
```xml
<resources>
    <!-- Default screen margins, per the Android Design guidelines. -->
    <dimen name="activity_horizontal_margin">0dp</dimen>
    <dimen name="activity_vertical_margin">0dp</dimen>
    <dimen name="fab_margin">16dp</dimen>
</resources>
```



#####檔案名稱: content_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto" android:layout_width="match_parent"
    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"
    tools:showIn="@layout/activity_main" tools:context=".MainActivity">

    <!-- 加入一個ListView物件 -->
    <ListView
        android:id="@+id/myListView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
</RelativeLayout>
```


#####檔案名稱: Common.java
```java
package com.abc.myapplication.data;

public class Common {
    public static String[] cultureGroup = {
            "板橋區原住民族發展協進會-新北市板橋區中山路1段1號4樓之1",
            "臺北山舞藝術團-新北市板橋區民生路一段9號20樓",
            "新莊區原住民族發展協進會-新北市新莊區裕民街128號6樓",
            "巴里豊文化藝術團-新北市樹林區東興街一四號一樓",
            "原緣圈-新北市泰山區民權街45號7樓",
            "比西里岸文化藝術團-新北市樹林區樹新路219巷26號4樓",
            "新北市原住民族文教推展協會-新北市汐止區茄興街20巷1弄17號",
            "新北市原住民族多元事務競爭力發展協會-新北市汐止區樟樹一路133巷1號2樓",
            "新北市三峽區原住民族婦女會-新北市三峽區龍埔里隆恩街241-9號",
            "旅北三地門鄉同鄉會-新北市三重區大仁街13號4F",
            "烏來區原住民族發展協進會-新北市烏來區信賢里信福路169號之1",
            "瑪伐琉•王野-新北市樹林區柑園街一段133巷6號B棟",
            "旅北都蘭同鄉會-\"新北市新莊區民安西路49巷7號2樓",
            "東河傳承文化藝術團-新北市蘆洲區信義路266巷24弄7號4樓",
            "新北市原住民文化藝術協會-新北市永和區中和路三四三號8樓之3"};
}
```



#####檔案名稱: MainActivity.java
```java
package com.abc.myapplication;

import android.os.Bundle;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.view.View;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.ArrayAdapter;
import android.widget.ListView;

import com.abc.myapplication.data.Common;


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
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int id = item.getItemId();
        if (id == R.id.action_settings) {
            return true;
        }
        return super.onOptionsItemSelected(item);
    }


    //=============================================================================
    // 首次載入App時會執行onResume(), 下次Activity由背景回到前景時也會執行onResume()
    //=============================================================================
    @Override
    protected void onResume() {
        super.onResume();

        //-----------------------------------
        // 準備一個顯示資料的ListView物件
        //-----------------------------------
        ListView listView=(ListView)findViewById(R.id.myListView);

        //-------------------------------------
        // 準備一個橋接資料及示版型的Adapter物件
        //-------------------------------------
        ArrayAdapter<String> arrayAdapter = new ArrayAdapter(
                this,
                android.R.layout.simple_list_item_activated_1,
                Common.cultureGroup
        );

        //-----------------------------------
        // 將ListView物件連上Adapter物件
        //-----------------------------------
        listView.setAdapter(arrayAdapter);
    }
}
```
