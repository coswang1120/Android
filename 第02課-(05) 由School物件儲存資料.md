#第02課 加入ListView清單


## (5) 由School物件儲存資料


#####執行結果:
![GitHub Logo](/images/results02-5.jpg)



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
   |            |     |___School.java
   |            |
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



#####檔案名稱: School.java
```java
package com.abc.myapplication.data;

//------------------------
// 建立一個學校類別
//------------------------
public class School {
    private String name;
    private String address;

    public School(String name, String address) {
        this.name = name;
        this.address = address;
    }

    public String getName() {
        return this.name;
    }

    public String getAddress() {
        return this.address;
    }
}
```



#####檔案名稱: Common.java
```java
package com.abc.myapplication.data;

//----------------------------------------
// 宣告一個靜態陣列變數, 內容是3個School物件
//----------------------------------------
public class Common {
    public static School[] schools =
            new School[]{
                    new School("國立臺北商業大學", "台北市濟南路一段321號"),
                    new School("國立臺北科技大學",  "台北市忠孝東路三段1號"),
                    new School("國立臺灣科技大學", "台北市基隆路四段43號")};
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
import android.support.v7.widget.Toolbar;
import android.view.View;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;
import android.widget.SimpleAdapter;
import android.widget.Toast;
import com.abc.myapplication.data.*;
import java.util.ArrayList;
import java.util.HashMap;

public class MainActivity extends AppCompatActivity {
    //-------------------------------------------------
    // 一個Context物件, 存放目前應用程式的狀態內容
    // 一個存放成對資料的ArrayList物件
    //-------------------------------------------------
    Context context=this;
    ArrayList<HashMap<String, String>> list = new ArrayList<>();

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

        //----------------------------------------------------------------------------
        // 將schools陣列存放的School物件逐一取出, 生成HashMap物件, 再加入ListView物件中
        //----------------------------------------------------------------------------
        for(int i=0; i<Common.schools.length; i++){
            HashMap<String,String> item = new HashMap<>();
            item.put("name", Common.schools[i].getName());
            item.put("address", Common.schools[i].getAddress());
            list.add(item);
        }

        //-----------------------------------
        // 準備一個顯示資料的ListView物件
        //-----------------------------------
        ListView listView=(ListView)findViewById(R.id.myListView);

        //---------------------------------------
        // 準備一個橋接資料及示版型的Adapter物件
        //---------------------------------------
        SimpleAdapter simpleAdapter = new SimpleAdapter(
                this,
                list,
                android.R.layout.simple_list_item_2,
                new String[]{"name", "address"},
                new int[]{android.R.id.text1, android.R.id.text2}
        );

        //-----------------------------------
        // 將ListView物件連上Adapter物件
        //-----------------------------------
        listView.setAdapter(simpleAdapter);

        //-----------------------
        // 清單上的項目被點擊時
        //-----------------------
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                Toast.makeText(context, Common.schools[position].getName(), Toast.LENGTH_SHORT).show();
            }
        });
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
}
```
