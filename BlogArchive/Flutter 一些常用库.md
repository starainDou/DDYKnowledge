> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.cnblogs.com/yangyxd/p/9232308.html

Flutter 社区和资源传送门
================

 新： [慕课网《Flutter 入门与案例实战》](https://www.imooc.com/learn/1090)   |   [中文网《Flutter 实战》电子书](https://book.flutterchina.club/)
------------------------------------------------------------------------------------------------------------------------

*   ### 字体图标生成 [http://fluttericon.com/](http://fluttericon.com/)
    
*   ### Flutter 中文网  [https://flutterchina.club](https://flutterchina.club/) 
    
*   ### Flutter 官网 [https://flutter.io](https://flutter.io)
    
*   ### Flutter 中文开发者论坛 [http://flutter-dev.cn/](http://flutter-dev.cn/)
    
*   ### Flutter|Dart 语言中文社区 [http://www.cndartlang.com/flutter](http://www.cndartlang.com/flutter)
    
*   ### Dart 开源包 [https://pub.dartlang.org/packages](https://pub.dartlang.org/packages)
    
*   ### Dart SDK 文档 [https://api.dartlang.org/stable/1.24.3/index.html](https://api.dartlang.org/stable/1.24.3/index.html)
    
*   ### 学习资料 [https://marcinszalek.pl/](https://marcinszalek.pl/)
    
*   ### Flutter 布局控件 [https://juejin.im/post/5bab35ff5188255c3272c228](https://juejin.im/post/5bab35ff5188255c3272c228)
    
*   ### Flutter 开发者 [http://flutter.link/](http://flutter.link/)
    
*   ### Flutter 开源 APP [https://itsallwidgets.com/](https://itsallwidgets.com/)
    
*   ### 深入理解（Flutter Platform Channel ）[https://www.jianshu.com/p/39575a90e820](https://www.jianshu.com/p/39575a90e820)
    
*   ### 简书 - 闲鱼技术 [https://www.jianshu.com/u/cf5c0e4b1111](https://www.jianshu.com/u/cf5c0e4b1111)
    

网络请求
====

http  
------

^0.11.3+16

[https://pub.dartlang.org/packages/http](https://pub.dartlang.org/packages/http)

[https://github.com/dart-lang/http](https://github.com/dart-lang/http)

该软件包包含一组高级函数和类，可以轻松使用 HTTP 资源。它与平台无关，可以在命令行和浏览器上使用。

dio
---

0.0.14

[https://pub.dartlang.org/packages/dio](https://pub.dartlang.org/packages/dio)

Dart 的一个强大的 Http 客户端，支持拦截器、全局配置、FormData、请求取消、文件下载、超时等。

http_multi_server
-----------------

2.0.5

[https://pub.dartlang.org/packages/http_multi_server](https://pub.dartlang.org/packages/http_multi_server)

dart:io HttpServer 包装器，用于处理来自多个服务器的请求

图像加载
====

可使用 Image.network 、 FadeInImage.memoryNetwork 或下面的库加载。

cached_network_image
--------------------

"^0.4.1"

[https://pub.dartlang.org/packages/cached_network_image](https://pub.dartlang.org/packages/cached_network_image)

Flutter 库来加载和缓存网络图像。也可以与占位符和错误小部件一起使用。

flutter_advanced_networkimage
-----------------------------

0.2.8

[https://pub.dartlang.org/packages/flutter_advanced_networkimage](https://pub.dartlang.org/packages/flutter_advanced_networkimage)

高级图像缓存加载和缩放控制。

transparent_image
-----------------

^0.1.0

[https://pub.dartlang.org/packages/transparent_image](https://pub.dartlang.org/packages/transparent_image)

简单的透明图像，表示为 Uint8List。在加载图片时可以用来做为占位符。

图像处理
====

image_jpeg
----------

0.0.1

[https://pub.dartlang.org/packages/image_jpeg](https://pub.dartlang.org/packages/image_jpeg)

[https://github.com/yangyxd/image_jpeg](https://github.com/yangyxd/image_jpeg)

用于图像上传之前转 jpeg 缩放压缩，调用 Android 或 iOS 原生功能进行处理，性能较高，支持的源图像格式也更多。

image_picker
------------

0.4.5

[https://pub.dartlang.org/packages/image_picker](https://pub.dartlang.org/packages/image_picker)

用于从 Android 和 iOS 图像库中选择图像，并使用相机拍摄新照片。

photo
-----

0.1.4

[https://pub.dartlang.org/packages/photo](https://pub.dartlang.org/packages/photo)

用于选择图像，支持多选，而且这个是用 Flutter 做的 UI，可以很方便的自定义修改（强烈推荐）。

image
-----

2.0.4

[https://pub.dartlang.org/packages/image](https://pub.dartlang.org/packages/image)

DART 库，提供以各种不同的文件格式加载、保存和操作图像的能力。该库不依赖于 DART：IO，因此它可以用于服务器和 Web 应用程序。

flutter_svg
-----------

0.6.3

[https://pub.dartlang.org/packages/flutter_svg](https://pub.dartlang.org/packages/flutter_svg)

加载 svg 图像。

zoomable_image
--------------

1.2.0  
[https://pub.dartlang.org/packages/zoomable_image](https://pub.dartlang.org/packages/zoomable_image)

提供图像查看和手势缩放操作功能。

image_carousel
--------------

0.4.1

[https://pub.dartlang.org/packages/image_carousel](https://pub.dartlang.org/packages/image_carousel)

Flutter 图像展示控件，可以左右划动切换上一张下一张图像，还结合了 zoomable_image 可以点击后缩放查看。支持 Asset 和网络图像。

carousel_slider
---------------

0.0.4

[https://pub.dartlang.org/packages/carousel_slider](https://pub.dartlang.org/packages/carousel_slider)

一个支持手势划动和自动播放的图像展示控件。

parallax_image
--------------

0.3.0

[https://pub.dartlang.org/packages/parallax_image](https://pub.dartlang.org/packages/parallax_image)

视差图像可以与任何可滚动（例如 ListVIEW）一起使用。说白了就是让放在滚动区域内的图像滚动时看起来更平滑。

camera
------

0.2.1

[https://pub.dartlang.org/packages/camera](https://pub.dartlang.org/packages/camera)

用于在 Android 和 iOS 上获取有关和控制相机的信息。支持预览相机馈送和捕捉图像。

UI 相关
=====

fluttertoast
------------

2.0.3

[https://pub.dartlang.org/packages/fluttertoast](https://pub.dartlang.org/packages/fluttertoast)

用于 Android 和 ios 的 toast 库。

flutter_html_view
-----------------

0.5.2

[https://pub.dartlang.org/packages/flutter_html_view](https://pub.dartlang.org/packages/flutter_html_view)

Flutter 没有默认的支持来显示 html，所以需要三方的包来显示。这个包可以将 html 呈现给原生的 Widget。（目前支持的标签比较少）

flutter_html_textview
---------------------

0.2.6

[https://pub.dartlang.org/packages/flutter_html_textview](https://pub.dartlang.org/packages/flutter_html_textview)

将 html 呈现为一个 Widget, 在 textview 中呈现 html。

markdown
--------

2.0.0

[https://pub.dartlang.org/packages/markdown](https://pub.dartlang.org/packages/markdown)

用 Dart 编写的便携式 Markdown 库。它可以在客户端和服务器上将 Markdown 解析为 HTML。

html2md
-------

0.1.7

[https://pub.dartlang.org/packages/html2md](https://pub.dartlang.org/packages/html2md)

将 html 转换为 Dart 中的 MarkDown.

flutter_calendar  （日历）
----------------------

0.0.3

[https://pub.dartlang.org/packages/flutter_calendar](https://pub.dartlang.org/packages/flutter_calendar)

日历组件.

flutter_picker
--------------

0.0.1

[https://pub.dartlang.org/packages/flutter_picker](https://pub.dartlang.org/packages/flutter_picker)

[https://github.com/yangyxd/flutter_picker](https://github.com/yangyxd/flutter_picker)

选择器。可以根据 json 或自定义数据生成选择器。

flutter_spinkit
---------------

[https://github.com/jogboms/flutter_spinkit](https://github.com/jogboms/flutter_spinkit)

加载动画。支持多种常用效果，非常酷炫。 

extended_nested_scroll_view
---------------------------

[https://github.com/zmtzawqlp/Flutter_Candies/tree/master/extended_nested_scroll_view](https://github.com/zmtzawqlp/Flutter_Candies/tree/master/extended_nested_scroll_view)

 一个扩展 NestedScrollView，能够更好的处理列表、TabView、Sliver 混合的情况（但是这个插件没有在 pub 找到）

badge
-----

0.0.3

[https://pub.dartlang.org/packages/badge](https://pub.dartlang.org/packages/badge)

小红点插件，可以用来显示小红点、未读消息数量等，非常方便

flutter_staggered_grid_view （瀑布流）
---------------------------------

0.3.0

[https://pub.dev/packages/flutter_staggered_grid_view](https://pub.dev/packages/flutter_staggered_grid_view)

瀑布流列表插件，可以支持不同大小的列。

视频 & 音频
=======

chewie
------

0.6.1

[https://pub.dartlang.org/packages/chewie](https://pub.dartlang.org/packages/chewie)

视频播放器，在 video_player 的基础上包装了控制 UI。

video_player
------------

0.6.0

[https://pub.dartlang.org/packages/video_player](https://pub.dartlang.org/packages/video_player)

用于在 Android 和 iOS 上与其他 Flutter 窗口小部件一起显示内嵌视频。

video_launcher
--------------

0.3.0

[https://pub.dartlang.org/packages/video_launcher](https://pub.dartlang.org/packages/video_launcher)

视频播放器，可播放本地文件和字节流。

flute_music_player
------------------

0.0.6

[https://pub.dartlang.org/packages/flute_music_player](https://pub.dartlang.org/packages/flute_music_player)

基于 Flutter 的材料设计音乐播放器与音频插件播放本地音乐文件。自带华丽的播放界面。

audioplayer
-----------

0.5.0

[https://pub.dartlang.org/packages/audioplayer](https://pub.dartlang.org/packages/audioplayer)

一个播放远程或本地音频文件 Flutter 音频插件

audioplayers
------------

0.5.2

[https://pub.dartlang.org/packages/audioplayers](https://pub.dartlang.org/packages/audioplayers)

这是 rxlabz 的 audioplayer 的一个分支，不同之处在于它支持同时播放多个音频并显示音量控制。

spritewidget
------------

0.9.17

[https://pub.dartlang.org/packages/spritewidget](https://pub.dartlang.org/packages/spritewidget)

SpriteWidget 是用于构建复杂、高性能动画和带有 2D 游戏的插件包，可与其它小部件无缝混合。您可以使用 SpriteWidget 创建任何东西，从动画图标到成熟的游戏。

路由和消息传递
=======

fluro
-----

1.3.1

[https://pub.dartlang.org/packages/fluro](https://pub.dartlang.org/packages/fluro)

[https://github.com/theyakka/fluro](https://github.com/theyakka/fluro)

最好用的路由导航框架。功能：简单的路线导航；函数处理程序（映射到函数而不是路径）；通配符参数匹配；查询字符串参数解析；内置常用转换；简单的定制转换创建。

flutter_local_notifications
---------------------------

0.3.0

[https://pub.dartlang.org/packages/flutter_local_notifications](https://pub.dartlang.org/packages/flutter_local_notifications)

一个跨平台的显示本地 notifications 的插件。

local_notifications
-------------------

0.0.6

[https://pub.dartlang.org/packages/local_notifications](https://pub.dartlang.org/packages/local_notifications)

这个库能让你在 Android 和 iOS 上创建 Notifications 很简单。

url_launcher
------------

3.0.2

[https://pub.dartlang.org/packages/url_launcher](https://pub.dartlang.org/packages/url_launcher)

用于在 Android 和 iOS 上启动 URL。支持网络，电话，短信和电子邮件方案。

firebase_messaging
------------------

1.0.2

[https://pub.dartlang.org/packages/firebase_messaging](https://pub.dartlang.org/packages/firebase_messaging)

一款跨平台的消息传递解决方案，可让您在 Android 和 iOS 上可靠地传递消息。

flutter_msg_engine
------------------

0.0.1

[https://pub.dartlang.org/packages/flutter_msg_engine](https://pub.dartlang.org/packages/flutter_msg_engine)

[https://github.com/yangyxd/flutter_msg_engine](https://github.com/yangyxd/flutter_msg_engine)

消息引擎, 注册一个消息，在任何地方响应并处理。。

event_bus
---------

0.4.1

[https://pub.dartlang.org/packages/event_bus](https://pub.dartlang.org/packages/event_bus)

一个使用 Dart 流进行解耦应用程序的简单事件总线的库。

数据存储、缓存有关的库
===========

sqflite
-------

**"^0.11.0"**

[https://pub.dartlang.org/packages/sqflite](https://pub.dartlang.org/packages/sqflite)

SQLite 的 Flutter 插件，一个自包含的高可靠性嵌入式 SQL 数据库引擎。

file_cache
----------

0.0.1

[https://pub.dartlang.org/packages/file_cache](https://pub.dartlang.org/packages/file_cache)

为 flutter package 项目缓存 Json,Buffer,FileCacheImage。

flutter_cache_manager
---------------------

0.1.1

[https://pub.dartlang.org/packages/flutter_cache_manager](https://pub.dartlang.org/packages/flutter_cache_manager)

管理你的 app 下载到本地的文件缓存。它使用缓存控制 HTTP 报头有效地检索文件。

类型编解码的库
=======

html_unescape
-------------

1.0.0

[https://pub.dartlang.org/packages/html_unescape](https://pub.dartlang.org/packages/html_unescape)

用于解决 HTML 编码字符串的 Dart 库。支持所有命名字符引用（如`&nbsp;`），小数字符引用（如`&#225;`）和十六进制字符引用（如`&#xE3;`）。

html
----

0.13.3

[https://pub.dartlang.org/packages/html](https://pub.dartlang.org/packages/html)

HTML 解析库。

crypto
------

2.0.6

[https://pub.dartlang.org/packages/crypto](https://pub.dartlang.org/packages/crypto)

在纯 DART 中实现的加解密函数库。支持 SHA-1、SHA-256、MD5、HMAC

序列化
===

*   ### 手动序列化：
    

使用 `dart:convert` 的内置解码器。包括传入 JSON 原始字符串给 JSON.decode() 方法，然后从 Map<String, dynamic> 中查询你需要的数据。

*   ### 自动序列化：
    

| 库名 | 版本号 | 链接 | 描述 |
| --- | --- | --- | --- |
| json_serializable | 0.5.7 | [https://pub.dartlang.org/packages/json_serializable](https://pub.dartlang.org/packages/json_serializable) |   |
| built_value | 5.5.1 | [https://pub.dartlang.org/packages/built_value](https://pub.dartlang.org/packages/built_value) | runtime 依赖项 |
| built_value_generator | 5.5.1 | [https://pub.dartlang.org/packages/built_value_generator](https://pub.dartlang.org/packages/built_value_generator) | dev 依赖项 |
| built_value_test | 5.5.1 | [https://pub.dartlang.org/packages/built_value_test](https://pub.dartlang.org/packages/built_value_test) | test 依赖项 |

Json 解析
-------

*   [https://github.com/javiercbk/json_to_dart](https://github.com/javiercbk/json_to_dart) 根据 json 生成 Dart 实体类
    
*   [https://pub.dartlang.org/packages/json_schema](https://pub.dartlang.org/packages/json_schema) json 解析
*   [https://github.com/debuggerx01/JSONFormat4Flutter](https://github.com/debuggerx01/JSONFormat4Flutter) 这是一个 AS 的辅助插件，将 JSONObject 格式的 String 解析成 Dart 的实体类
    
*   Dson 0.13.2 下载地址 [https://pub.dartlang.org/packages/dson](https://pub.dartlang.org/packages/dson)  
    描述：Dson 的 github 地址 [https://github.com/drails-dart/dson](https://github.com/drails-dart/dson) DSON 是一个将 Dart 对象转换为 JSON 的库。(用于 web) 这个库是 Dartson 的一个分支，但又有不同。
    
*   Dartson 是一个 Dart 库，可用于将 Dart 对象转换为 JSON 字符串。 [https://github.com/eredo/dartson](https://github.com/eredo/dartson) （用于 web）
    
*   几个 Json 库的比较：[https://github.com/drails-dart/dart-serialise](https://github.com/drails-dart/dart-serialise)
    

| 方式 | 大小 (js) | 序列化 (dart) | 反序列化 (dart) | 序列化 (js) | 反序列化 (js) |
| --- | --- | --- | --- | --- | --- |
| json_serializable | 80 KB | 9.09 ms | 6.61 ms | 8.23 ms | 8.12 ms |
| Serializable | 79 KB | 6.1 ms | 6.92 ms | 4.37 ms |   |
| DSON | 94 KB | 12.72 ms | 11.15 ms | 16.64 ms | 17.94 ms |
| Dartson | 86 KB | 9.61 ms | 6.81 ms | 8.58 ms | 7.01 ms |
| Manual | 86 KB | 8.29 ms | 5.78 ms | 10.7 ms | 7.9 ms |
| Interop | 70 KB | 61.55 ms | 14.96 ms | 2.49 ms | 2.93 ms |
| Jaguar_serializer | 88 KB | 8.57 ms | 6.58 ms | 10.31 ms | 8.59 ms |
| Jackson (Groovy) |   | 496 ms | 252 ms | n/a | n/a |

国际化和本地化
=======

intl
----

0.15.6

[https://pub.dartlang.org/packages/intl](https://pub.dartlang.org/packages/intl)

这个包提供国际化和本地化功能，包括消息翻译、复数和性别、日期 / 数字格式和解析以及双向文本。

rx 系列
=====

rxdart
------

0.17.0

[https://pub.dartlang.org/packages/rxdart](https://pub.dartlang.org/packages/rxdart)

RxDart 是一种基于 ReactiveX 的谷歌 Dart 反应性函数编程库。谷歌 Dart 自带了一个非常不错的流 API;RxDart 没有尝试提供这个 API 的替代方案，而是在它上面添加了一些功能。

rx_widgets
----------

1.0.3

[https://pub.dartlang.org/packages/rx_widgets](https://pub.dartlang.org/packages/rx_widgets)

rx_widgets 是一个包含基于流的 Flutter Widgets 和 Widget 帮助程序 / 便利类的程序包，它们有助于反应式编程风格，特别是与 RxDart 和 RxCommands 结合使用。

rx_command
----------

2.0.0

[https://pub.dartlang.org/packages/rx_command](https://pub.dartlang.org/packages/rx_command)

RxCommand 是针对事件处理程序的基于 Reactive Extensions（Rx）的抽象。它基于 ReactiveUI 框架的 ReactiveCommand。它大量使用了 RxDart 包。

系统平台有关的库
========

path_provider  (获取本地文件)
-----------------------

0.4.1

[https://pub.dartlang.org/packages/path_provider](https://pub.dartlang.org/packages/path_provider)

用于获取 Android 和 iOS 文件系统上的常用位置，例如 temp 和 app 数据目录。

shared_preferences  (读写 sp 文件)
------------------------------

0.4.2

[https://pub.dartlang.org/packages/shared_preferences](https://pub.dartlang.org/packages/shared_preferences)

用于读写简单键值对的 Flutter 插件。包装 iOS 上的 NSUserDefaults 和 Android 上的 SharedPreferences。

connectivity  (网络状态)
--------------------

0.3.1

[https://pub.dartlang.org/packages/connectivity](https://pub.dartlang.org/packages/connectivity)

用于发现 Android 和 iOS 上的网络状态（WiFi 和移动 / 蜂窝）连接。

device_info  (设备信息)
-------------------

0.2.1

[https://pub.dartlang.org/packages/device_info](https://pub.dartlang.org/packages/device_info)

提供有关设备（品牌，型号等）的详细信息，以及应用程序正在运行的 Android 或 iOS 版本。

flutter_blue  (蓝牙)
------------------

0.3.3

[https://pub.dartlang.org/packages/flutter_blue](https://pub.dartlang.org/packages/flutter_blue)

这是跨平台的蓝牙 sdk.

share  (分享)
-----------

0.5.1

[https://pub.dartlang.org/packages/share](https://pub.dartlang.org/packages/share)

支持分享的 flutter 插件

open_file  (打开文件)
-----------------

1.1.1

[https://pub.dartlang.org/packages/open_file](https://pub.dartlang.org/packages/open_file)

调用平台打开文件。比如直接打开一个 apk 文件会调起平台的安装向导。

权限库
===

simple_permissions
------------------

0.1.5

[https://pub.dartlang.org/packages/simple_permissions](https://pub.dartlang.org/packages/simple_permissions)

用于 android 和 ios 的请求权限的库

flutter_simple_permissions
--------------------------

0.0.1

[https://pub.dartlang.org/packages/flutter_simple_permissions](https://pub.dartlang.org/packages/flutter_simple_permissions)

地图（地图显示、定位、经纬度等）
================

map_view
--------

0.0.12

[https://pub.dartlang.org/packages/map_view](https://pub.dartlang.org/packages/map_view)

一个用于在 iOS 和 Android 上显示谷歌地图的 Flutter 插件

flutter_map
-----------

0.0.10

[https://pub.dartlang.org/packages/flutter_map](https://pub.dartlang.org/packages/flutter_map)

基于 leaflet 的 Flutter 地图包

location
--------

1.3.4

[https://pub.dartlang.org/packages/location](https://pub.dartlang.org/packages/location)

这个插件 处理 Android 和 iOS 上的位置。它还提供位置更改时的回调。

latlong
-------

0.5.3

[https://pub.dartlang.org/packages/latlong](https://pub.dartlang.org/packages/latlong)

LatLong 是一个计算通用的纬度和经度的轻量级库。

flutter_amap
------------

0.0.1

[https://pub.dartlang.org/packages/flutter_amap](https://pub.dartlang.org/packages/flutter_amap)

高德地图 3d flutter 组件。展示原生 android、ios 高德地图，并与 flutter 交互。

二维码
===

barcode_scan
------------

0.0.4

[https://pub.dartlang.org/packages/barcode_scan](https://pub.dartlang.org/packages/barcode_scan)

用于扫描 2D 条形码和 QRCodes 的 Flutter 插件。

qrcode_reader
-------------

0.3.3

[https://pub.dartlang.org/packages/qrcode_reader](https://pub.dartlang.org/packages/qrcode_reader)

使用相机读取二维码的 Flutter 插件。

WebView
=======

flutter_webview_plugin
----------------------

0.1.6

[https://pub.dartlang.org/packages/flutter_webview_plugin](https://pub.dartlang.org/packages/flutter_webview_plugin)

允许 Flutter 与原生 Webview 进行通信的插件。

图表库
===

charts-common
-------------

0.3.0

[https://pub.dartlang.org/packages/charts_flutter](https://pub.dartlang.org/packages/charts_flutter)

Material Design 风格的图表库

charts-flutter
--------------

0.3.0

[https://pub.dartlang.org/packages/charts_common](https://pub.dartlang.org/packages/charts_common)

通用的图表库组件

flutter_circular_chart
----------------------

0.0.3

[https://pub.dartlang.org/packages/flutter_circular_chart](https://pub.dartlang.org/packages/flutter_circular_chart)

一个让你使用 flutter 轻松创建的动画圆形图控件的库。

其它
==

pwa
---

0.1.10

[https://pub.dartlang.org/packages/pwa](https://pub.dartlang.org/packages/pwa)

基于 Dart 的 PWA 应用程序的库

fluwx (微信)
----------

0.5.5

[https://pub.dartlang.org/packages/fluwx](https://pub.dartlang.org/packages/fluwx)

 适用于 Flutter 的微信 SDK，方便快捷。 QQ 群：892398530。

alipay_me (支付宝)
---------------

1.0.0

[https://pub.dartlang.org/packages/alipay_me](https://pub.dartlang.org/packages/alipay_me)

支付宝插件，支持登录、支付。android 还支持本地计算签名。

flutter_qq  (QQ)
----------------

 0.0.4

[https://pub.dartlang.org/packages/flutter_qq](https://pub.dartlang.org/packages/flutter_qq)

QQ 登录、分享到 QQ、分享到 QQ 空间。

学习资料
====

[https://github.com/AweiLoveAndroid/Flutter-learning](https://github.com/AweiLoveAndroid/Flutter-learning)

网友阿韦整理的学习资料和 demo.

**注**：版本号会随时间更新，请自行修改。本文集合了 Flutter-learning 中的内容，感谢原作者阿韦 ([https://github.com/AweiLoveAndroid/Flutter-learning](https://github.com/AweiLoveAndroid/Flutter-learning))。转载请注明出处。