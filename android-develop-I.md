title: Android开发笔记 - I
date: 2015-03-15 22:29:27
tags: Android
categories: Android
---

Note for Android
<br/>

<!--more-->

#**0x00**

最近太懒了....
国赛涉及到Android平台的东西,所以就来插一脚Android开发

<br/>
<br/>

#**0X01**

###Android系统架构,自己看
![](http://evilddog.qiniudn.com/image.9KPCVX.png)

###Android系统四大组件:<br/>

>**活动(Activity)**   是所有 Android 应用程序的门面,凡是在应用中你看得到的东西,都是放在活动中

>**服务(Service)**   可以一直在后台默默运行,即使用户退出了应用,服务仍然是可以继续运行的

>**广播接收器(Broadcast Receiver)**   允许应用接收来自各处的广播消息,比如电话、短信等,同样也可以向外发出广播消息

>**内容提供器(Content Provider)**   为应用程序之间共享数据提供了可能,比如你想要读取系统电话簿中的联系人,就需要通过内容提供器来实现

###Android开发工具
[Android Studio <del>===Android SDK + AVD + Eclipse](http://developer.android.com/sdk/index.html)足够,一流程序员炫耀代码,二流程序员炫耀工具

<br/>
<br/>

#**0x02**

###Android项目文件目录结构

因为各种原因,目录结构可能不会完全相同,但是基本一致


**工程总目录以Test为例**
```shell
Test
├── ./Test.iml
├── ./app
├── ./build
├── ./build.gradle
├── ./gradle
├── ./gradle.properties
├── ./gradlew
├── ./gradlew.bat
├── ./local.properties
└── ./settings.gradle
```
<del>**核心文件夹就是app,这里面的是我们所要写项目需要的重要文件,其他目录暂时没有涉及,所以不再介绍**

**app文件目录**
```shell
app
├── ./build	
├── ./libs
├── ./src
├── ./app.iml
├── ./build.gradle
└── ./proguard-rules.pro
```

---
>**./src**中的main文件夹包括/res, /java等文件夹,还有一个重要的文件----AndroidManifest.xml
<br/>
>**./src/res**存放在项目中使用到的所有图片、布局、字符串等觉得部分静态资源资源,之后的R.java中的内容也是根据这个目录下的文件自动生成。图片在*drawabl*e目录下,布局放在*layout*目录下,字符串放在*values*目录下
<br/>
>**./src/java**包括程序最核心的Java代码
<br/>
>**./src/AndroidManifest.xml**是程序中至关重要的文件,在程序中定义的所有四大组件都需要在这个文件里注册。另外还可以在这个文件中给应用程序添加权限声明,也可以重新指定你创建项目时指定的程序最低兼容版本和目标版本。
<br/>
>**./libs**如果使用第三方 Jar 包,就需要把这些 Jar 包都放在 libs 目录下,这些 Jar 包都会被自动添加到构建路径里

---

<br/>
<br/>

#0x03

###Activity 七个回调方法
 >**onCreate()**
每个活动中都y要重写了这个方法,它会在活动第一次被创建的时候调用,应该在这个方法中完成活动的初始化操作,比如说加载布局、绑定事件等
<br/>
 >**onStart()**
这个方法在活动由不可见变为可见的时候调用
<br/>
 >**onResume()**
这个方法在活动准备好和用户进行交互的时候调用。此时的活动一定位于返回栈的栈顶,并且处于运行状态
<br/>
 >**onPause()**
这个方法在系统准备去启动或者恢复另一个活动的时候调用,但这个方法的执行速度一定要快,不然会影响到新的栈顶活动的使用
<br/>
 >**onStop()**
这个方法在活动完全不可见的时候调用。它和 onPause()方法的主要区别在于,如果启动的新活动是一个对话框式的活动,那么 onPause()方法会得到执行,而 onStop()方法并不会执行。
<br/>
 >**onDestroy()**
这个方法在活动被销毁之前调用,之后活动的状态将变为销毁状态。
<br/>
 >**onRestart()**
这个方法在活动由停止状态变为运行状态之前调用,也就是活动被重新启动了。在活动由不可见变为可见的时候调用

###7个回调方法的生命周期
![](http://evilddog.qiniudn.com/ActivityLifeCycle.jpg)


<br/>
<br/>

#0x04

###创建活动

**这里还要提到一个概念Intent**
>**Intent** 是 Android 程序中各组件之间进行交互的一种重要方式,它不仅可以指明当前组件想要执行的动作,还可以在不同组件之间传递数据。Intent 一般可被用于启动活动、启动服务、以及发送广播等场景,大致可以分为两种,显式 Intent 和隐式 Intent

前面提到在./src/java目录下包含核心代码,每个app都会有一个主要活动,由于我使用的是ADT自己生成的活动,所以是 _MainActivity_
接下来修改MainActivity


```java
package com.hduisa.activitylifecycletest;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.util.Log;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.Window;
import android.widget.Button;



/**
 * Author: evilddog
 * Time: 2015.03.15
 * Gmail: error0320@gmail.com
 */


public class MainActivity extends Activity {		#继承名为 MainActivity 的类,并继承自 Activity

    public static final String TAG = "MainActivity";

    @Override
    protected void onCreate(Bundle saveInstanceState){		//重写 onCreate()
        super.onCreate(saveInstanceState);
        Log.e(TAG, "onCreate");
        requestWindowFeature(Window.FEATURE_NO_TITLE);		//隐藏标题栏,一定要在 setContentView()之前

        //调用ContentView()方法为当前活动加载布局,此处的R包代表com.hduisa.activitylifecycletest
        setContentView(R.layout.activity_main);
        //点击第一个按钮会启动NormalActivity,点击第二个按钮会启动 DialogActivity
        Button startNormalActivity = (Button) findViewById(R.id.start_normal_activity);
        Button startDialogActivity = (Button) findViewById(R.id.start_dialog_activity);
        startNormalActivity.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(MainActivity.this, NormalActivity.class);  
                //使用显式 Intent,传入 FirstActivity.this 作为上下文,传入 SecondActivity.class作为目标活动
                //如果是隐式 Intent,就需要在配置文件中的<intent-filter>中指定当前活动可以响应的action
                //intent.putExtra("extra_data", data);
                //通过 putExtra()方法传递字符串,前面的为键名,后面为内容
                startActivity(intent);
                //startActivity()方法将这个 category 添加到 Intent
        });


        startDialogActivity.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(MainActivity.this, DialogActivity.class);
                startActivity(intent);
            }
        });
    }

    @Override
    protected void onStart(){
        super.onStart();
        Log.e(TAG, "onStart");
    }


    @Override
    protected void onResume(){
        super.onResume();
        Log.e(TAG, "onResume");
    }


    @Override
    protected  void onPause(){
        super.onPause();
        Log.e(TAG, "onPause");
    }

    @Override
    protected void onStop(){
        super.onStop();
        Log.e(TAG, "onStop");
    }

    @Override
    protected void onDestroy(){
        super.onDestroy();
        Log.e(TAG, "onDestroy");
    }

    @Override
    protected void onRestart(){
        super.onRestart();
        Log.e(TAG, "onReatart");
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
接下来我们要创建两个活动,分别是 _NormalActivity_ 
```java
package com.hduisa.activitylifecycletest;

import android.app.Activity;
import android.os.Bundle;
import android.view.Window;

import com.hduisa.activitylifecycletest.R;

/**
 * Author: evilddog
 * Time: 2015.03.15
 * Gmail: error0320@gmail.com
 */


public class NormalActivity extends Activity {

    @Override
    protected void onCreate(Bundle saveInstanceState){
        super.onCreate(saveInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.normal_layout);
        //Intent intent = getIntent();
        //String data = intent.getStringExtra("extra_data");
        //getStringExtra()方法,传入键值,就可以得到数据
        //Log.d("SecondActivity", data);
        //取出MainActivity中的传递的数据并打印
    }
}
```
 和 _DialogActivity_
```Java
package com.hduisa.activitylifecycletest;

import android.app.Activity;
import android.os.Bundle;
import android.view.Window;

import com.hduisa.activitylifecycletest.R;

/**
 * Author: evilddog
 * Time: 2015.03.15
 * Gmail: error0320@gmail.com
 */


public class DialogActivity extends Activity {

    @Override
    protected void onCreate(Bundle saveInstanceState){
        super.onCreate(saveInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.dialog_layout);
    }
}
```

<br/>
<br/>

#0x05

###在MainActivity中注册活动
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.hduisa.activitylifecycletest" >

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        	
        <!--android:name 来指定具体注册哪一个活动-->

        <!--由于最外层的<manifest>通过package 属性
        指定了程序的包名是com.hduisa.activitylifecycletest
        因此在注册活动时这一部分就可以省略了,直接使用.MainActivity-->
        
        <!--android:label 指定活动中标题栏的内容-->

        <activity
            android:name=".MainActivity"
            android:label="@string/app_name" >

                <!-- <action android:name="android.intent.action.MAIN" />和
                        <category android:name="android.intent.category.LAUNCHER" />
                        表示 HelloWorldActivity 是这个项目的主活动,在手机上点击应用图标
                        首先启动的就是这个活动-->


            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>


        <activity android:name=".DialogActivity"
                  android:theme="@android:style/Theme.Dialog">
        </activity>
 
        <!--@android:style/Theme.Dialog让DialogActivity 使用对话框式的主题-->

        <activity android:name=".NormalActivity">
        </activity>

    </application>

</manifest>

```

<br/>
<br/>

#0x06
###定义三种活动的布局

**activity_main.xm**
```xml
<LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:orientation="vertical" >
    	
        <!--@+id/start_normal_activity在XML中定义一个id-->
        <!--match_parent 表示让当前元素和父元素一样宽-->
        <!--android:layout_height指定了当前元素的高度-->
        <!--wrap_content表示当前元素的高度只要能刚好包含里面的内容-->
        
        <Button
        android:id="@+id/start_normal_activity"		
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Start NormalActivity" />


    <Button
        android:id="@+id/start_dialog_activity"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Start DialogActivity" />

        <!--加入了两个按钮,一个用于启动 NormalActivity,一个用于启动 DialogActivity-->

</LinearLayout>
```
**dialog_layout.xml**
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:orientation="vertical" >
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="This is a dialog activity"
        />
</LinearLayout>
```

**normal_layout.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:orientation="vertical">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="This is a normal activity..."
        />

</LinearLayout>

```

>开始调试,MainActivity 第一次被创建时会依次执行 onCreate()、 onStart()和 onResume()方法
![]()
```shell
03-16 17:59:01.843 E/MainActivity( 5249): onCreate
03-16 17:59:01.847 E/MainActivity( 5249): onStart
03-16 17:59:01.847 E/MainActivity( 5249): onResume
```

>点击NormalActivity执行onPause和onStop
```shell
03-16 17:59:09.702 E/MainActivity( 5249): onPause
03-16 17:59:10.223 E/MainActivity( 5249): onStop
```
>按下Back返回,重启MainActivity活动
```shell
03-16 17:59:13.614 E/MainActivity( 5249): onReatart
03-16 17:59:13.615 E/MainActivity( 5249): onStart
03-16 17:59:13.615 E/MainActivity( 5249): onResume
```
>点击DialogActivity,此时MainActivity暂停执行onPause活动
```shell
03-16 17:59:15.152 E/MainActivity( 5249): onPause
```
>Back 键返回 MainActivity,执行onResume
```shell
03-16 17:59:16.070 E/MainActivity( 5249): onResume
```
>继续点击Back键,结束进程,执行onPause,onStop,onDestroy
```shell
03-16 17:59:18.475 E/MainActivity( 5249): onPause
03-16 17:59:18.925 E/MainActivity( 5249): onStop
03-16 17:59:18.925 E/MainActivity( 5249): onDestroy
```

就先写到这里