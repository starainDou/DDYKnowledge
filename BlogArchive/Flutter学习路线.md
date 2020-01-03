> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.jianshu.com/p/6a7bbae6aa19

0.4252019.03.09 11:53:10 字数 620 阅读 83

一、学习路线
------

> 10 步，每一个学习步骤都以自己亲身实现写出了 Demo 为准，光看资料不算完成

1.  参考 [https://flutterchina.club/setup-macos/](https://flutterchina.club/setup-macos/)，搭建 Flutter 开发环境，安装默认项目到设备
2.  通读 Dart 语法一遍：[https://juejin.im/post/5c52a386f265da2de25b5c36](https://juejin.im/post/5c52a386f265da2de25b5c36)，要求上面所有代码，必须自己敲一遍
3.  实现一个简单的 HelloWorld 页面，目的是熟悉 Flutter 的项目结构和 StatelessWidget 组件，通读 Flutter 的所有 UI 组件一遍：[https://juejin.im/post/5c18d181f265da611f07a128](https://juejin.im/post/5c18d181f265da611f07a128)，要求记住 flutter 的常用 UI 组件的继承关系
4.  实现页面跳转、传参、接收返回值，目的是熟悉 Navigator 组件
5.  实现一个 ezbuy-Mine 页面，暂不要求交互，但是页面的样式要大致实现，主要目的是熟悉基本 UI 组件，尤其是布局、文本、图片，
6.  实现一个 ezbuy-Home 页面，要求包含交互：下拉刷新和上拉加载更多，主要目的是熟悉复杂列表视图、滚动嵌套、手势
7.  实现一个简单登录页面的 Http 请求，服务端自己写，主要目的是熟悉 Flutter 的输入框、Dart 的异步编程 (async/await/Future)、网络请求、json 解析、响应式编程 (StatefulWidget/setState)
8.  实现一个简单登录页面的 grpc 请求，protobuf 文件和服务端也自己写，主要目的是熟悉 grpc 插件的用法
9.  实现读写本地缓存（Android:SharedPreferences，ios:NSUserDefaults）的简单页面，不准用第三方插件，自己用 MethodChannel 实现 Flutter 调用原生的功能
10.  实现 Flutter 项目接入宿主 Android 容器，主要目的是熟悉 Flutter 静态路由和 Android 原生传参给 FlutterView

二、下一步的调研计划
----------

> 调研必须有产出，实践性的要有代码产出，理论性的要有 wiki 产出，没有产出的调研，就是耍流氓

*   写一个国际化的 demo，实现多语言切换
*   写一个自定义 View 的 Demo，熟悉绘图三剑客：canvas、path、paint
*   写一个网络图片缓存的 demo，并了解其实现原理和内存占用
*   阅读源码，了解 Flutter 的渲染机制，如何从 Widget->Element->RenderObject
*   了解 Dart 的异步机制，深入理解 async/await/Future 的原理

[![](https://upload.jianshu.io/users/upload_avatars/1432234/01887fa87a37?imageMogr2/auto-orient/strip|imageView2/1/w/120/h/120/format/webp)](https://www.jianshu.com/u/5b37e90e81f7)

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

*   先看效果： 实现： 这边主要是用到了 NotificationListener 这个 widget，借助这个 widget...
    
*   学习 Flutter 也有一阵子了。闲着没事，用了公司一个已经凉凉的 App 设计图来练手。当然了接口不可能用的了，所以都...
    
*   一、路由跳转 1、直接跳转 2、先声明注册路由 后跳转 二、路由参数传递方法：使用 arguments 接收方法：在新...
    
*   在开发过程中, 屏幕适配是很重要的一件事. 再 RN 中我们可以获取屏幕的像素, 按照特定机型来适配屏幕大小. 还要自己手写类...
    
*   写在前面 Flutter 是 Google 推出并开源的移动应用开发框架，主打跨平台、高保真、高性能。开发者可以通过...