> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://juejin.im/post/5bd1270ff265da0ac07c951b

[TOC]

前言
--

​ 从 Flutter 的架构图中可以看出，Flutter Platform SDK 处于整个 Flutter 框架的上层，连接了 Java 与 Dart 代码。那么作为 “最上层” 的它，到底扮演了哪些角色，以及是如何扮演好这些角色的呢？Google 工程师用一个封装好的 flutter.jar 包 "show me the answer"。

Platform SDK 的角色扮演
------------------

​ 通过对 Platform SDK(以下简称 Platform) 源码的阅读，可以大致将它分成三个角色：Creator（创建者），Transmitter（传递者），Registrant（注册人）。

### Creator

创建者

##### FlutterMain

##### 时序图

![](https://user-gold-cdn.xitu.io/2019/1/31/168a2e485d84c7b9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

FlutterMain 担任着 flutter 的初始化工作，在被 Application 的 onCreate 调起后

*   initConfig
    
    必须的配置文件名称和路径
    
    *   通过命令行 flutter build aot 生成的文件
        
        | 名称 | 内容 |
        | --- | --- |
        | isolate_snapshot_instr | 应用程序指令段 |
        | isolate_snapshot_data | 应用程序数据段 |
        | vm_snapshot_instr | VM 虚拟机指令段 |
        | vm_snapshot_data | VM 虚拟机数据段 |
        
    
    ​ 注: 详细含义参见[官方说明](https://link.juejin.im?target=https%3A%2F%2Fgithub.com%2Fflutter%2Fengine%2Fwiki%2FFlutter-Engine-Operation-in-AOT-Mode)
    
    *   路径信息
        
        flutter_assets 等文件的路径信息
        
*   initAot
    
    1.  初始化标记位: snapshot 文件集成的方式。
    2.  sIsPrecompiledAsSharedLibrary 代表的是把所有的 snapshot 文件打包成一个动态库 (一种类似 ios 的集成方式)。
    3.  禁止同时采用两种集成方式。
*   initResource
    
    1.  创建了一个 ResourceExtractor 对象, 他是 Resource 文件的搬运工
    2.  通过 ResourceExtractor 对象的 addResource 方法初始化需要搬运的文件
    3.  ResourceExtractor 启动异步任务把 asset 下面的文件搬运到 DataDir 的 flutter 目录下面

#### FlutterActivityDelegate

##### WhatDelegate

```
//FlutterActivity.java
......
private final FlutterActivityEvents eventDelegate;
private final Provider viewProvider;
private final PluginRegistry pluginRegistry;   
private final FlutterActivityDelegate delegate = new FlutterActivityDelegate(this, this);
......
public FlutterActivity() {
        this.eventDelegate = this.delegate;
        this.viewProvider = this.delegate;
        this.pluginRegistry = this.delegate;
    }
......
复制代码

```

我们可以从上面 FlutterActivity 的构造函数中看出 Delegate 到底代理了哪些职责

*   FlutterActivityEvents：将 Activity 生命周期的具体处理逻辑传递给 FlutterView
    
*   Provider：在 onCreate 中创建的 FlutterView，通过实现 Provider 的接口暴露出来
    
*   PluginRegistry: 插件信息的注册与查询传递给 FlutterView
    
    从上面看来，其实 FlutterActivityDelegate 直接交互的对象就是 FlutterView，通过这样的设计，提高了每一个类的内聚性
    

![](https://user-gold-cdn.xitu.io/2019/1/31/168a2e4c5ec9a63b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### FlutterView

可以先简单的把 FlutterView 看成是负责显示的，而把后面的 FlutterNativeView 看成是负责通信的。

*   解偶系统控件
    
    首先我们可以看到 FlutterView 是继承自 SurfaceView 的，提到 SurfaceView 我们肯定会想到 “挖洞”，“双缓冲” 这些词，也正是因为这些特性 FlutterView 可以很好的把 UI 渲染工作交给 Flutter（ 后者通过 dart->组装 LayerTree->Skia 完成绘制 )。
    

![](https://user-gold-cdn.xitu.io/2019/1/31/168a2e4ec2bba490?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

*   传递状态消息给 Dart UI
    
    这些消息大致可以将它们分成七个模块，两大类
    
    1.  通过反射原生系统的 api 进行数据通讯
        
        多语言模块、系统信息和用户设置
        
    2.  将数据打包成特殊格式以消息的形式和使用 Dart 编写的 flutter 控件交互
        
        生命周期、按键消息、路由导航和平台插件
        

#### FlutterNativeView

可以先简单的把 FlutterNativeView 看成是负责通信的。它在系统层面上实现了 BinaryMessenger 接口 [这在下文 **Transmitter** 中详细介绍]。

### Transmitter

传递者

#### 消息渠道

*   Flutter 针对不同的应用场景封装了 3 类 Channel
    
    1.  MethodChannel：调用方法
    2.  BasicMessageChannel：自定义结构信息
    3.  EventChannel：事件的通知
*   3 类 Channel 拥有相似的结构类型
    
    1.  信使 BinaryMessenger：我们自己创建的 channel 一般就是 FlutterNativeView
    2.  channel 名：channel 的 key 值，不可重复
    3.  MethodCodec /MessageCodec 解码器: 针对不同的 channle 解码二进制应答数据

![](https://user-gold-cdn.xitu.io/2019/1/31/168a2e5150caeec2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 消息编解码

​ 标准平台通道使用一个标准的消息编解码器。简单的 JSON 类值（如布尔值）的高效二进制序列化，数字、字符串、字节缓冲区以及这些列表和映射。(查看细节 [`StandardMessageCodec`](https://link.juejin.im?target=https%3A%2F%2Fdocs.flutter.io%2Fflutter%2Fservices%2FStandardMessageCodec-class.html))。这些值的序列化和反序列化在消息发送和接收值时自动发生。（对应的数据转化关系如下）

![](https://user-gold-cdn.xitu.io/2019/1/31/168a2e54d0772ed8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 传递流程

​ 通过 Flutter Engine 的数据转换, 使得 Dart 和 Android 之间可以进行通信

![](https://user-gold-cdn.xitu.io/2019/1/31/168a2e57e62d4062?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### Registrant

注册人

#### 情境转换

​ 首先这里有三个形似得英文单词 registry, registrar and registrant 分别对应注册局，注册商和注册人。把它们翻译到现实的生活场景中的角色其实是一个 “注册人通过注册商，更新注册信息后，注册商把信息传递给注册局进行保存” 的过程。下面我们把这个过程再翻译回代码：

首先我们新建一个 plugin 插件, 作为说明的对象：

```
// 实现 PluginRegistry.ActivityResultListener
public class FlutterMusicPlugin implements MethodCallHandler, PluginRegistry.ActivityResultListener {
    ...
    public static void registerWith(Registrar registrar) {
        //传入Activity
        final FlutterMusicPlugin plugin = new FlutterMusicPlugin(registrar.activity());
        ...
        // 注册ActivityResult回调
        registrar.addActivityResultListener(plugin);
    }
    
    @Override
    public void onMethodCall(MethodCall call, Result result) {
           ...
    }
    
    @Override
    public boolean onActivityResult(int requestCode, int resultCode, Intent data) {
           ...
        return false;
    }
}

复制代码

```

同时在 GeneratedPluginRegistrant 类中会自动生成

```
public final class GeneratedPluginRegistrant {
  public static void registerWith(PluginRegistry registry) {
    if (alreadyRegisteredWith(registry)) {
      return;
    }
 FlutterMusicPlugin.registerWith(registry.registrarFor("com.plugin.FlutterMusicPlugin"));
  }

  private static boolean alreadyRegisteredWith(PluginRegistry registry) {
    final String key = GeneratedPluginRegistrant.class.getCanonicalName();
    if (registry.hasPlugin(key)) {
      return true;
    }
    registry.registrarFor(key);
    return false;
  }
}
复制代码

```

#### 注册人

​ 它对应的是代码中的 GeneratedPluginRegistrant 类

*   发起注册
    
    GeneratedPluginRegistrant 类中的
    
    ```
     FlutterMusicPlugin.registerWith(registry.registrarFor("com.plugin.FlutterMusicPlugin"));//即为发起注册点
    复制代码
    
    ```
    
*   通过注册商更新注册
    
    FlutterMusicPlugin 类中的
    
    ```
    registrar.addActivityResultListener(plugin);//即为通过注册商更新了需要Activity回调的信息
    复制代码
    
    ```
    
*   同步给注册局
    
    sdk 中的 FlutterRegistrar 类
    
    ```
    FlutterPluginRegistry.this.mActivityResultListeners.add(listener);//即为把信息同步给了注册局
    复制代码
    
    ```
    

#### 注册商

​ 它对应的是代码中的 FlutterPluginRegistry 内部类 FlutterRegistrar。

​ 不看不知道，"注册商" 其实给我们提供了很多功能，比如获取 activity，viewDestory 的生命周期的回调，获取 surfaceTexture 等等，真是一个能力强大的 "注册商"。

```
      
        Activity activity();//返回 Host app的Activity
        Context context();//返回 Application Context.
        Context activeContext();//返回 活动Context
        //返回 BinaryMessenger 主要用来注册Platform channels
        BinaryMessenger messenger();
        //返回 TextureRegistry，从里面可以拿到SurfaceTexture 
        TextureRegistry textures();
        //返回 当前Host app创建的FlutterView
        FlutterView view();
        //返回Asset对应的文件路径
        String lookupKeyForAsset(String var1);
        //返回Asset对应的文件路径
        String lookupKeyForAsset(String var1, String var2);
        //插件对外发布的一个"值"
        PluginRegistry.Registrar publish(Object var1);
        //注册权限相关的回调
        PluginRegistry.Registrar addRequestPermissionsResultListener(PluginRegistry.RequestPermissionsResultListener var1);
        //注册ActivityResult回调
        PluginRegistry.Registrar addActivityResultListener(PluginRegistry.ActivityResultListener var1);
        //注册NewIntent回调
        PluginRegistry.Registrar addNewIntentListener(PluginRegistry.NewIntentListener var1);
        //注册UserLeaveHint回调
        PluginRegistry.Registrar addUserLeaveHintListener(PluginRegistry.UserLeaveHintListener var1);
        //注册View销毁回调
        PluginRegistry.Registrar addViewDestroyListener(PluginRegistry.ViewDestroyListener var1);
复制代码

```

#### 注册局

​ 它对应的是代码中的 FlutterPluginRegistry

![](https://user-gold-cdn.xitu.io/2019/1/31/168a2e5bef9b4783?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

从成员变量可以看出 FlutterPluginRegistry 主要维护两类信息:

1.  注册插件唯一标识信息汇总
2.  FlutterRegistrar 所开放的那些功能 (比如：addActivityResultListener) 的信息汇总