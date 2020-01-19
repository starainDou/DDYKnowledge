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


[adb常用命令小结](https://zhuanlan.zhihu.com/p/68571291)
[菜鸟教程](https://www.runoob.com/) 