## 知识概要

> #### 术语

* Dalvik:Android特有的虚拟机，比JVM更适合移动端使用
* AVD:Android Virtual Device,安卓虚拟设备，模拟器
* ADT:Android Development Tools,安卓开发工具
* SDK:Software Development Kit,软件开发工具包
* DDMS:Dalvik Debug Monitor Sevice,安卓调试监控工具
* adb:安卓调试桥，在platform-tools目录
* DX工具:将.class转.dex的工具
* AAPT:Android Asset Packing Tool,安卓资源打包工具
* R.java文件:由aapt工具根据App中的资源文件自动生成,可以理解为资源字典
* AndroidManifest.xml：app包名+组件声明+程序兼容的最低版本+所需权限等程序的配置文件

> #### adb命令

* windows上需要配置环境变量

1. 新建 ``` ANDROID_HOME ```环境变量，添加SDK地址(如 D:\Android\android_sdks)
2. 更新Path环境变量，添加 ``` % ANDROID_HOME%\tools ```

* 一些命令

1. 安装软件: adb install [-r] xx.apk -r:重装
2. 卸载软件: adb uninstall [-k] <包名> -k:保留数据和缓存目录
3. 取出手机文件: adb pull xxx.xxx
4. 发送文件到手机: adb push xxx.xxx
5. 进入手机终端: adb shell
6. 列出所有连接的设备: adb devices
7. 重启adb: adb:reset adb
8. 杀死adb: adb kill-server
9. 启动adb: adb start-server
10. 查看端口号占用的进程: netstat -ano

> #### 工程说明

* java目录

	写业务功能的java代码

* res目录
	
	存放各种资源文件的地方，如图片、字符串、动画、音频、视频及各种XML等
	
* 自建assets目录

	自己创建的存放资源的地方。res下所有的资源文件在R.java文件下生成对应资源id，通过资源id访问对应资源；自建assets不会在R.java生成id，通过AssetManager以二进制流形式读取。（R文件可理解成资源字典，res下每个资源在这里生成唯一id）
	
* drawable
	
	存放各种位图文件，包括png、jpg、.9png、gif及一些其他drawable类型的XML文件等

* mipmap-hdpi

	高分辨率，一般图片放这里

* mipmap-mdpi

	中等分辨率，兼容旧型号手机，很少用了

* mipmap-xhdpi

	超高分辨率

* mipmap-xxhdpi

	超超高分辨率

* layout

	布局文件资源所在目录。另外在一些特定机型上，我们做屏幕适配，如480*320手机，创建另外一套布局layout-480x320文件夹

* menu

	有物理按键menu的手机用，菜单相关资源xml放这里

* values

	* demens.xml: 定义尺寸资源
	* string.xml: 定义字符串资源
	* style.xml: 定义样式资源
	* colors.xml: 定义颜色资源
	* arrays.xml: 定义数组资源
	* attri.xml: 自定义控件时用到，自定义控件的属性
	* theme主题文件，和style类似，但是会对整个应用的Activity或指定Activity起作用，一般改变窗口外观。可在Java代码中通过setTheme使用，或在Androidmanifest.xml中为<application..>添加theme的属性。有时候可能看到这样的values目录：values-w820dp,values-v11等,前者w代表平板设备，820dp代表屏幕宽度，后者v11代表在API(11)后才能用到。

* raw目录

	存放各种原生资源，包括音频、视频、一些XML文件等，可通过openRawResource(int id)获得资源的二进制流，和Assets类似，不过这里的资源会在R文件生成资源id
	
* 动画目录

	* animator:存放属性动画的XML文件
	* anim: 存放补间动画的XML文件

> #### 获取资源

	对于R.java中生成唯一资源id的资源，可通过id完成资源访问，可在java代码或XML中使用
	
	* java代码中使用
	
	```
	// java文本
	texName.setText(getResources().getText(R.string.name));
	
	// 图片
	imgIcon.setBackgroundDrawableResource(R.drawable.icon);
	
	// 颜色
	textName.setTextColor(getResources().getColor(R.color.red));
	
	// 布局
	setContentView(R.layout.main);
	
	// 控件
	textName = (TextView)findViewById(R.id.text_name);
	```
	
	* XML代码中使用
	
	```
	通过@xxx即可得到，比如获取文本和图片
	<TextView 
		android:text = "@string/hello_world"
		android:layout_width = "wrap_content"
		android:layout_height = "wrap_content"
		android:backgroud = "drawable/img_back" />
	```
	
> #### 深入了解三个文件

* MainAcrivity.java

```
package jay.com.example.firstapp

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

// 1.定义一个MainActivity 继承自AppCompatActivity 要实现抽象方法onCreate()
public class MainActivity extends AppCompatActivity {
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		// 2.调用父类方法
		super.onCreate(saveInstanceState);
		// 3.加载R.layout.activity_main的布局文件
		setContentView(R.layout.activity_main);
	}
}
// 4.在AndroidManifest.xml中进行声明，否则报错
```

* 布局文件 activity_main.xml

```
<RelativeLayout 
	xmlns:android = "http://schemas.android.com/apk/res/android"
	xmlns:tools = "http://schemas.android.com/tools"
	android:layout_width = "match_parent"
	android:layout_height = "match_parent"
	tools:context = ".MainActivity">
	
	<TextView
		android:layout_width = "wrap_content"
		android:layout_height = "wrap_content"
		android:text = "@string/hello_world" />
	
</RelativeLayout>
```

我们定义一个LinearLayout线性布局，在xml命名空间中定义我们所需要使用的架构，来自于

```
1.有了xmlns:android 和 xmlns:tools 就可以按alt+/作为提示(语法文件且无需联网)
2.android:id = "@+id/linearLayout1" 为这个布局设置一个id标记为LinearLayout1，并添加到R文件的id内部类中
3. android:layout_width和android:layout_height 控制组件的宽高，有三个属性可以选wrap_content/fill_content/match_content
4. android:orientation = "vertical" 为线性布局指定方式(水平horizontal 竖直vertical)
5. tools:context = ".MyActivity" 这句不会打包进Apk,只是ADT的的Layout Editor在当前的Layout文件里设置对应的渲染上下文，说明当前Layout所在的渲染上下文是activiry name对应的那个activity，如果这个activity在manifest文件中设置了Theme，那么ADT的Layout Editor会根据这个Theme来渲染当前的Layout。
6. android:text = "@string/hello_world" 为这个textview设置文本，引用string资源里hello_world字符串文本
```

* AndroidManifest.xml配置文件

```
<?xml version = "1.0" encoding = "utf-8"?>
<manifest xmlns:android = "http://schemas.android.com/apk/res/android"
	package = "jay.com.example.firstapp">
	
	<application 
		android:allowBackup = "true"
		android:icon = "@mipmap/ic_launcher"
		android:label = "@string/app_name"
		android:theme = "@style/AppTheme" >
		
		<activity 
			android:name = ".MainActivity"
			android:label = "@string/app_name">
			
			<intent-filter>
				
				<action android:name = "android.intent.action.MAIN">
				<category android:name = "android.intent.catrgoty.LAUNCHER">
				
			</intent-filter>
	
	</application>
	
</manifest>
```

代码详细分析

```
// manifest
<?xml version = "1.0" encoding = "utf-8"?> 定义了使用的xml的版本和编码方式
xmlns:android = "http://schemas.android.com/apk/res/android" 定义架构来源
package = "jay.com.example.firstapp"> 程序所在Java包(包名是应用唯一标识)

// users-sdk
android:versionCode = "1" android:versionName = "1.0" 定义程序的版本号和版本名称
<user-sdk android:minSdkVersion = "8" android:targetSdkVersion = "21"> 使用该app的最低sdk版本为2.2 开发的系统版本为4.2，后面的编号代表每个版本的sdk

// Application
android:allowBackup = "true" 是否允许备份文件(这里允许)
android:icon = "@drawable/ic_launcher" 定义应用的图标，引用drawable中的ic_launcher
android:label = "@string/app_name" 定义应用名称，应用string中的app_name
Android:theme = "@style/AppTheme" 定义应用使用的主题(app风格)

// Activity
android:name = "com.example.helloworld.MainActivity" 声明一个activity，包括activity所在的包和app名称，前面部分可用 . 表示来缩写
android:label = "@string/app_name" Activity名称，显示给用户看的

Intent-filter(意图过滤器)
<action android:name = "android.intent.action.MAIN" /> 
<category android:name = "android.intent.category.LAUNCHER" />
这两行代码决定了程序的入口，且app会被显示在Home的应用程序列表中

除上述内容外：
1.如果app中包含其他组件的话，都要使用类型说明语法在该文件中进行声明Server元素 BroadcastReceiver元素ContentProvoder元素 IntentFilter<intent-filter>元素
2.权限的声明：在该文件中显式地声明程序需要的权限，防止app错误地使用服务或不恰当地访问资源，最终提高健壮性
```

> #### 签名

* 签名好处[详见](https://blog.csdn.net/fyh2003/article/details/6911967)

1. 应用程序升级：如果你希望用户无缝升级到新的版本，那么你必须用同一个证书进行签名。这是由于只有以同一个证书签名，系统才会允许安装升级的应用程序。如果你采用了不同的证书，那么系统会要求你的应用程序采用不同的包名称，在这种情况下相当于安装了一个全新的应用程序。如果想升级应用程序，签名证书要相同，包名称要相同！ 
2. 应用程序模块化：Android系统可以允许同一个证书签名的多个应用程序在一个进程里运行，系统实际把他们作为一个单个的应用程序，此时就可以把我们的应用程序以模块的方式进行部署，而用户可以独立的升级其中的一个模块
3. 代码或者数据共享：Android提供了基于签名的权限机制，那么一个应用程序就可以为另一个以相同证书签名的应用程序公开自己的功能。以同一个证书对多个应用程序进行签名，利用基于签名的权限检查，你就可以在应用程序间以安全的方式共享代码和数据了。

不同的应用程序之间，想共享数据，或者共享代码，那么要让他们运行在同一个进程中，而且要让他们用相同的证书签名。

* 命令行签名方式(keytool在jdk/jre/bin目录，jarsigner在jdk/bin目录)

1. 通过以下命令可以获取证书和私钥：keytool -genkey -v -keystore doudou.keystore -alias doudou -keyalg RSA -validity 10000
2. 通过以下命令可以对apk签名：jarsigner -verbose -keystore doudou.keystore doudou.apk doudou.keystore
3. 验证是否签名 jarsigner -verbose -certs -vertify doudou.apk

```
可以更改顺序如：
keytool -genkey -alias demo.keystore -keyalg RSA -validity 40000 -keystore demo.keystore
-genkey 产生密钥
-alias demo.keystore 别名demo.keystore
-validity 40000 有效期40000天

jarsigner -verbose -keystore demo.keystore -signedjar demo_signed.apk demo.apk demo.keystore
-verbose 输出签名的详细信息
-keystore  demo.keystore 密钥库位置
-signedjar demor_signed.apk demo.apk demo.keystore 正式签名，三个参数中依次为签名后产生的文件demo_signed，要签名的文件demo.apk和密钥库demo.keystore
```

打包Android APK的方法还有很多，命令行，或者Gradle，ANT，MAVEN等等


[传送门](https://www.runoob.com/w3cnote/android-tutorial-sign-package.html)

[adb常用命令小结](https://zhuanlan.zhihu.com/p/68571291)   

[菜鸟教程](https://www.runoob.com/) 

[Android学习总结](https://www.jianshu.com/p/5a2419132c4c)

[Android 2011](https://www.cnblogs.com/qianxudetianxia/default.html?page=4)