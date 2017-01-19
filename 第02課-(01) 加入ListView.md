#第02課 加入ListView清單


## (1) 加入ListView


#####執行結果:
![GitHub Logo](/images/results02-1.jpg)



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
   |            |___MainActivity.java		 
   |___<res>
         |___<layout>
                |___content_main.xml                				
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


public class MainActivity extends AppCompatActivity {
    //--------------------
    // 字串陣列資料
    //--------------------
    private String[] city = {"基隆","台北","新北","桃園","新竹","苗栗", "台南", "高雄"};

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
    
    //-----------------------------------------------------------------
    // 當 MainActivity 首次執行及由其他Activity返回時執行onResume()
    //-----------------------------------------------------------------
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
                city
        );

        //-----------------------------------
        // 將ListView物件連上Adapter物件
        //-----------------------------------
        listView.setAdapter(arrayAdapter);
    }
    
}
```
