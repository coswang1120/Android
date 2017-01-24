#第16課 Facebook登入



## (1) Facebook登入



#####執行結果:
![GitHub Logo](/images/results16-1.jpg)



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



#####在Facebook建立應用程式畫面:
![GitHub Logo](/images/fig16-1.jpg)



#####在Facebook建立應用程式說明
```
(1)
在 https://developers.facebook.com/ 網址中建立一個應用程式, 此應用程式將有一個應用程式編號.

(2)
在 https://sourceforge.net/projects/openssl/?source=typ_redirect
下載openssl程式, 內有一<bin>資料夾, 複製到c碟, 資料夾名稱改為<openssl>

(3)
在命令題示字元中, 進入c碟, 再進入openssl資料夾:
c:
cd\openssl

執行:
keytool -exportcert -alias androiddebugkey -keystore %HOMEPATH%\.android\debug.keystore | openssl sha1 -binary | openssl base64  命令,
輸入密碼後會產生一個28個字元的離湊金鑰(hash code).
(*如果找不到 keytool 命令, 它在 <java>中的<bin> 內)

(4)
步驟(3)產生的金鑰會存在 Android studio 中, 
同時也要手動加入 步驟(1) 建好的應用程式內的金鑰雜湊項目中(在網頁右邊的 設定->基本資料 中).

(5)
在developers.facebook.com中,
在應用程式的"應用程式審查"(左邊選單)中, 將 "是否發佈..." 選項設定為 "是".
```



#####檔案放置方式:
```
 <app> 
   |___<manifests>
   |     |___AndroidManifest.xml
   |            
   |___<java>
   |     |___<com.abc.myapplication>
   |            |___MainActivity.java
   | 
   |___<res>
         |    
         |___<layout>
         |      |___content_main2.xml    
         |   
         |___<values>
                |___colors.xml  
                |
                |___strings.xml  
                
 <Gradle Scripts>
         |___build.gradle(Module:app)
```



#####檔案名稱: build.gradle
```
apply plugin: 'com.android.application'

android {
    compileSdkVersion 25
    buildToolsVersion "23.0.2"

    defaultConfig {
        applicationId "com.abc.myapplication"
        minSdkVersion 21
        targetSdkVersion 25
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

//--加入--------------
repositories {
    mavenCentral()
}
dependencies {
    compile 'com.facebook.android:facebook-android-sdk:4.+'
}
//--------------------

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:25.0.1'
    compile 'com.android.support:design:25.0.1'
}
```



#####檔案名稱: AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.abc.myapplication">

    <!-- 加入-->
    <uses-permission android:name="android.permission.INTERNET"/>
    <!-- ... -->

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">

        <!-- 加入-->
        <meta-data android:name="com.facebook.sdk.ApplicationId" android:value="@string/facebook_app_id"/>
        <!-- ... -->
        
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:theme="@style/AppTheme.NoActionBar">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>


        <activity android:name="com.facebook.FacebookActivity"
            tools:replace="android:theme"
            android:configChanges=
                "keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:theme="@android:style/Theme.Translucent.NoTitleBar"
            android:label="@string/app_name" />
    </application>

</manifest>
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



#####檔案名稱: strings.xml
```xml
<resources>
    <string name="app_name">My Application</string>
    <string name="action_settings">Settings</string>
    <string name="facebook_app_id">請填入自己的在 https://developers.facebook.com/ 建立的應用程式編號</string>
</resources>
```



#####檔案名稱: content_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto" android:layout_width="match_parent"
    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:orientation="vertical"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"
    tools:showIn="@layout/activity_main" tools:context=".MainActivity">

    <!-- 加入一個Facebook登入/登出按鈕 -->
    <com.facebook.login.widget.LoginButton
        android:id="@+id/login_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_marginTop="-50dp"/>


</FrameLayout>
```



#####檔案名稱: MainActivity.java
```java
package com.abc.myapplication;

import android.content.Context;
import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.util.Log;
import android.view.View;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.Toast;

import com.facebook.AccessToken;
import com.facebook.AccessTokenTracker;
import com.facebook.CallbackManager;
import com.facebook.FacebookCallback;
import com.facebook.FacebookException;
import com.facebook.FacebookSdk;
import com.facebook.GraphRequest;
import com.facebook.GraphResponse;
import com.facebook.login.LoginResult;
import com.facebook.login.widget.LoginButton;

import org.json.JSONObject;

import java.util.Date;

public class MainActivity extends AppCompatActivity {
    //========================================
    Context context;
    CallbackManager callbackManager;
    //========================================


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        //=====================
        // 初始化Facebook
        //=====================
        FacebookSdk.sdkInitialize(getApplicationContext());

        setContentView(R.layout.activity_main);
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //==========================
                // 顯示儲在偏好內的id及姓名
                //==========================
                SharedPreferences sharedPreferences=getSharedPreferences();
                String id=sharedPreferences.getString("id", "***");
                String name=sharedPreferences.getString("name", "***");
                Snackbar.make(view, "stored id/name:"+id+"/"+name, Snackbar.LENGTH_LONG)
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


    //==================================================
    // 登入／登出完成後執行callbackManager管理的回呼程式
    //==================================================
    @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        callbackManager.onActivityResult(requestCode, resultCode, data);
    }



    //=============================================================================
    // 首次載入App時會執行onResume(), 下次Activity由背景回到前景時也會執行onResume()
    //=============================================================================
    @Override
    protected void onResume() {
        super.onResume();

        context=this;

        //-------------------------
        // 登出後, 偏好內容即清除
        //-------------------------
        AccessTokenTracker accessTokenTracker= new AccessTokenTracker() {
            @Override
            protected void onCurrentAccessTokenChanged(AccessToken oldAccessToken, AccessToken currentAccessToken){
                SharedPreferences.Editor editor=getSharedPreferences().edit();
                editor.putString("id", null);
                editor.putString("name", null);
                editor.putString("time", null);

                editor.apply();
            }
        };

        //------------------------------------------------------
        // 建立一個callbackManager, 用來管理登入/登出後的回呼程式
        //------------------------------------------------------
        callbackManager = CallbackManager.Factory.create();

        //----------------
        // 登入按鈕
        //----------------
        final LoginButton loginButton = (LoginButton) findViewById(R.id.login_button);

        //------------------------------------------
        // 註冊回呼程式內容, 並向Facebook發出登入請求
        //------------------------------------------
        loginButton.registerCallback(callbackManager, new FacebookCallback<LoginResult>() {
            @Override
            public void onSuccess(LoginResult loginResult) {
                GraphRequest request = GraphRequest.newMeRequest(
                        loginResult.getAccessToken(),
                        new GraphRequest.GraphJSONObjectCallback() {
                            @Override
                            public void onCompleted(JSONObject object, GraphResponse response) {
                                try {
                                    //---------------------------------------------------
                                    // 將Facebook回傳的id及name, 另系統時間加入偏好內容中
                                    //---------------------------------------------------
                                    String id=object.getString("id");
                                    String name = object.getString("name");
                                    String time=new Date(System.currentTimeMillis()).toString();

                                    Toast.makeText(context, id+","+name+","+time , Toast.LENGTH_SHORT).show();

                                    SharedPreferences.Editor editor=getSharedPreferences().edit();

                                    editor.putString("id", id);
                                    editor.putString("name", name);
                                    editor.putString("time", time);

                                    editor.apply();
                                }catch(Exception e){
                                    Toast.makeText(context, "Exception! ", Toast.LENGTH_SHORT).show();
                                }
                            }
                        }
                );

                //---------------------------------
                // 要求Facebook回覆的使用者資料項目
                //---------------------------------
                Bundle parameters = new Bundle();
                parameters.putString("fields", "id, name, about, email, gender, birthday");
                request.setParameters(parameters);

                //----------------------
                // 向Facebook發出請求
                //----------------------
                request.executeAsync();
            }

            @Override
            public void onCancel() {
                Toast.makeText(context, "登入取消", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onError(FacebookException exception) {
                Toast.makeText(context, "登入錯誤", Toast.LENGTH_SHORT).show();
            }
        });
    }


    //=====================
    // 取得偏好設定空間
    //=====================
    private SharedPreferences getSharedPreferences(){
        SharedPreferences sharedPreferences=context.getSharedPreferences("myLogin", Context.MODE_PRIVATE);
        return sharedPreferences;
    }
}
```
