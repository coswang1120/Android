# 第15課 使用 SQL Lite



## (1) 使用 SQL Lite



##### 執行結果:
![GitHub Logo](/images/results15-1.jpg)



##### 模擬器
```
Nexus 5, API 23
```



##### 建立專案設定
```
(1) Company Domain: abc.com 
(2) Minimum SDK: API 21: Android 5.0(Lollipop)
(3) 選用 Basic Activity
```



##### 檔案放置方式:
```
 <app> 
   |___<java>
   |     |___<com.abc.myapplication>
   |            |___<db>
   |            |     |___MyDBHelper.java
   |            |
   |            |___MainActivity.java    
   | 
   |___<res>
         |___<layout>
         |      |___content_main.xml   
         | 	 
         |___<values>
                |___colors.xml  

```


##### 檔案名稱: colors.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#4f0010</color>
    <color name="colorPrimaryDark">#000</color>
    <color name="colorAccent">#4f0010</color>
</resources>
```



##### 檔案名稱: content_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto" android:layout_width="match_parent"
    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:orientation="vertical"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"
    tools:showIn="@layout/activity_main" tools:context=".MainActivity">

    <android.support.design.widget.TextInputLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/stuNo"
            android:hint="學號" />
    </android.support.design.widget.TextInputLayout>

    <android.support.design.widget.TextInputLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"> 
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/stuName"
            android:hint="姓名" />
    </android.support.design.widget.TextInputLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="新增"
            android:id="@+id/buttonInsert" />

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="更改"
            android:id="@+id/buttonUpdate" />

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="刪除"
            android:id="@+id/buttonDelete" />
    </LinearLayout>

    <ListView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/listView" />
</LinearLayout>
```



##### 檔案名稱: MyDBHelper.java
```java
package com.abc.myapplication.db;

import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;


public class MyDBHelper extends SQLiteOpenHelper{
    //==================
    // 資料庫名稱
    //==================
    public static final String DATABASE_NAME = "studb.db";
    public static final int DATABASE_VERSION = 1;

    //==================
    // 建立資料表
    //==================
    public static final String CREATE_TABLE_SQL =
            "CREATE TABLE " +
                    "student (_id INTEGER PRIMARY KEY, stuNo TEXT, stuName TEXT, " +
                    "created_time TIMESTAMP default CURRENT_TIMESTAMP);";

    //=================
    // 刪除資料表
    //=================
    public static final String DROP_TABLE_SQL =
            "DROP TABLE IF EXISTS student";

    public MyDBHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL(CREATE_TABLE_SQL);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        onDropTable(db);
        onCreate(db);
    }

    public void onDropTable(SQLiteDatabase db) {
        db.execSQL(DROP_TABLE_SQL);
    }
}
```



##### 檔案名稱: MainActivity.java
```java
package com.abc.myapplication;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.os.Bundle;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.view.View;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;

import com.abc.myapplication.db.MyDBHelper;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    //---------------------------
    Context context;
    MyDBHelper myDBHelper;
    //---------------------------

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

        context=this;

        //======================
        // 顯示現有資料庫內容
        //======================
        myDBHelper = new MyDBHelper(context);

        SQLiteDatabase db = myDBHelper.getWritableDatabase();
        showList(db);
        db.close();

        //===============
        // 新增記錄
        //===============
        Button buttonInsert=(Button)findViewById(R.id.buttonInsert);

        buttonInsert.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView stuNoView=(TextView)findViewById(R.id.stuNo);
                TextView stuNameView=(TextView)findViewById(R.id.stuName);

                String stuNo=stuNoView.getText().toString();
                String stuName=stuNameView.getText().toString();

                SQLiteDatabase db = myDBHelper.getWritableDatabase();

                ContentValues contentValues = new ContentValues();
                contentValues.put("stuNo", stuNo);
                contentValues.put("stuName", stuName);
                long id = db.insert("student", null, contentValues);

                //顯示清單
                showList(db);

                stuNoView.setText("");
                stuNameView.setText("");

                contentValues.clear();
                db.close();
            }
        });


        //===============
        // 更改記錄
        //===============
        Button buttonUpdate=(Button)findViewById(R.id.buttonUpdate);

        buttonUpdate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView stuNoView=(TextView)findViewById(R.id.stuNo);
                TextView stuNameView=(TextView)findViewById(R.id.stuName);

                String stuNo=stuNoView.getText().toString();
                String stuName=stuNameView.getText().toString();

                SQLiteDatabase db = myDBHelper.getWritableDatabase();

                ContentValues contentValues = new ContentValues();
                contentValues.put("stuName", stuName);
                db.update("student", contentValues, "stuNo=?", new String[]{stuNo});

                //顯示清單
                showList(db);

                stuNoView.setText("");
                stuNameView.setText("");

                contentValues.clear();
                db.close();
            }
        });


        //===============
        // 刪除記錄
        //===============
        Button buttonDelete=(Button)findViewById(R.id.buttonDelete);

        buttonDelete.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView stuNoView=(TextView)findViewById(R.id.stuNo);
                String stuNo=stuNoView.getText().toString();

                SQLiteDatabase db = myDBHelper.getWritableDatabase();

                db.delete("student", "stuNo=?", new String[]{stuNo});

                //顯示清單
                showList(db);

                stuNoView.setText("");

                db.close();
            }
        });
    }


    //===========================
    // 顯示SQL Lite內容
    //===========================
    private void showList(SQLiteDatabase db){
        Cursor myCursor= db.rawQuery("SELECT stuNo, stuName FROM student", null);

        List<String> arrayList = new ArrayList<String>();
        myCursor.moveToFirst();

        for(int i=0; i<myCursor.getCount(); i++){
            String s = myCursor.getString(0) + "," + myCursor.getString(1);
            arrayList.add(s);
            myCursor.moveToNext();
        }

        ListView listView=(ListView)findViewById(R.id.listView);
        String[] list=arrayList.toArray(new String[0]);

        ArrayAdapter<String> arrayAdapter = new ArrayAdapter(
                context,
                android.R.layout.simple_list_item_activated_1,
                list
        );

        listView.setAdapter(arrayAdapter);
    }
}
```
