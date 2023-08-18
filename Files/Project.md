> #### 工程初始化配置

1. 在指定目录下建立 cocosProject 和 cocos_build 文件夹，以Desktop路径为例

```
cd ~/Desktop

mkdir cocosProject

mkdir cocos_build

cd ~/Desktop/cocos_build

mkdir market
```

2. 进入 cocosProject 目录，拉取 cocos 代码（若无访问权限则需让管理员添加）

```
cd ~/Desktop/cocosProject

git clone https://gitlab.huixuanjiasu.com/hongbaogame/market.git

cd ~/Desktop/cocosProject/market

git checkout ios-test
```

3. 安装 CocosCreator 并用其打开 market 工程

```
open -a CocosCreator

CocosCreator 菜单栏 打开其他项目 选择 ~/Desktop/cocosProject/market 目录
```

4. 导出 CocosCreator 工程为原生工程

```
菜单栏 - 项目 - 构建发布
```

5. 填写导出配置信息

```
游戏名称 market
发布平台 iOS
发布路径 ~/Desktop/cocos_build/market
包名 com.kdsjge.imarket
```
6. 删除导出工程 jsb-link/frameworks/runtime-src/proj.ios_mac 文件夹

```
rm -rf ~/Desktop/cocos_build/market/jsb-link/frameworks/runtime-src/proj.ios_mac/
```

7. 进入 runtime-src 并克隆原生壳工程命名为 proj.ios_mac，并检出分支

```
cd  ~/Desktop/cocos_build/market/jsb-link/frameworks/runtime-src/

git clone https://gitlab.huixuanjiasu.com/hongbaogame/iossdk.git proj.ios_mac

cd ~/Desktop/cocos_build/market/jsb-link/frameworks/runtime-src/proj.ios_mac/

git checkout feature-market
```

8. 安装 pod 库 (如果本地版本低则需要先 pod repo update)

```
cd ~/Desktop/cocos_build/market/jsb-link/frameworks/runtime-src/proj.ios_mac/

pod install
```

[cocoaPods安装](https://github.com/starainDou/DDYKnowledge/blob/master/Files/CocoaPods.md)

9. 安装 node，安装 Python3 环境并执行热更命令

```
brew install node

cd ~/Desktop/cocosProject/market

修改 updateCfg.template 为 updateCfg.yaml

mv updateCfg.template updateCfg.yaml

open -e updateCfg.yaml

并修改其内容 source_folder: "/Users/chuangaofeng/Desktop/cocos_build/market/jsb-link/" # 这里用绝对路径

python3 hotupdate.py -n -i 1.1.0.6
```

如果报错

```
ModuleNotFoundError: No module named 'yaml'
sudo python3 -m pip install pyyaml

ModuleNotFoundError: No module named 'click'
sudo python3 -m pip install click
``` 

```
[Client] Synchronous remote object proxy returned error: Error Domain=NSCocoaErrorDomain Code=4099 "The connection to service named com.apple.commcenter.coretelephony.xpc was invalidated." UserInfo={NSDebugDescription=The connection to service named com.apple.commcenter.coretelephony.xpc was invalidated.}

终端运行

xcrun simctl spawn booted log config --mode "level:off"  --subsystem com.apple.CoreTelephony

//https://www.codenong.com/52127186/

[Client] Updating selectors failed with: Error Domain=NSCocoaErrorDomain Code=4099 "The connection to service on pid 0 named com.apple.commcenter.coretelephony.xpc was invalidated." UserInfo={NSDebugDescription=The connection to service on pid 0 named com

这个错误时，说明是你开启了全局代理，而去访问局域网资源。
```


> #### 添加空framework以支持模拟器

```
以 du.framework为例

打开Xcode shift + command + N 新建 Framework，命名为du

创建framework中所有同名文件.h，如若需要还有.m，将所有方法进行空实现或返回特定值

设置Build Setting参数将Build Active Architecture only设置为NO

Mach-o type 修改为Static Libray

在Build Settings下配置，删除arm64架构 Excluded Architecture arm64

设置Headers，将要公开的头文件拖到Public下

Target 点击下面加号 other Aggregate，名字为du_script

点击 du_script Target，选择加号，New Run Script Phase 

内容为

# Type a script or drag a script file from your workspace to insert its path.

UNIVERSAL_OUTPUTFOLDER=../Framework/

# 创建输出目录，并删除之前的framework文件

mkdir -p "${UNIVERSAL_OUTPUTFOLDER}"

rm -rf "${UNIVERSAL_OUTPUTFOLDER}/${PROJECT_NAME}.framework"

# 分别编译模拟器和真机的Framework

# xcodebuild -target "${PROJECT_NAME}" ONLY_ACTIVE_ARCH=NO -configuration ${CONFIGURATION} -sdk iphoneos  BUILD_DIR="${BUILD_DIR}" BUILD_ROOT="${BUILD_ROOT}" clean build

xcodebuild -target "${PROJECT_NAME}" ONLY_ACTIVE_ARCH=NO -configuration ${CONFIGURATION} -sdk iphonesimulator BUILD_DIR="${BUILD_DIR}" BUILD_ROOT="${BUILD_ROOT}" clean build

# 定义真机、模拟器Build文件夹路径变量

# IPHONE_BUILD=${BUILD_DIR}/${CONFIGURATION}-iphoneos/${PROJECT_NAME}.framework

SIMULATOR_BUILD=${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${PROJECT_NAME}.framework

# 拷贝framework到univer目录

# cp -R "${IPHONE_BUILD}" "${UNIVERSAL_OUTPUTFOLDER}/"

cp -R "${SIMULATOR_BUILD}" "${UNIVERSAL_OUTPUTFOLDER}/"

# 定义输出路径变量

OUTPUT_PATH=${UNIVERSAL_OUTPUTFOLDER}/${PROJECT_NAME}.framework

# 合并framework，输出最终的framework到build目录

# lipo -create "${IPHONE_BUILD}/${PROJECT_NAME}" "${SIMULATOR_BUILD}/${PROJECT_NAME}" -output "${OUTPUT_PATH}/${PROJECT_NAME}"
lipo -create "${SIMULATOR_BUILD}/${PROJECT_NAME}" -output "${OUTPUT_PATH}/${PROJECT_NAME}"

运行选择 du_script

xcodebuild -create-xcframework \
-framework "刚才的空framework路径" \
-framework "实际只真机的du.framework路径" \
-output "~/Desktop/du.xcframework"

xcodebuild -create-xcframework \
-framework "/Users/chuangaofeng/Desktop/duEmptySDKSimulator/iphoneOS/du.framework" \
-framework "/Users/chuangaofeng/Desktop/duEmptySDKSimulator/simulator/du.framework" \
-output "/Users/chuangaofeng/Desktop/duEmptySDKSimulator/merge/du.xcframework"

用 du.xcframework 替换掉 du.framework

// https://www.jianshu.com/p/5c54b6e18f1f
// https://www.jianshu.com/p/448529b53bf9
// https://www.codenong.com/js20a92f83f968/
// [xcframework生成与使用](https://www.jianshu.com/p/a4e7c65d0f45)
[iOS_SDK开发之Framework打包](https://juejin.cn/post/6844904166012239886)
https://www.jianshu.com/p/541553f8bce1
```

区分静态库和动态库

```
cd .../Bugly.framework
file Bugly

current ar archive random library : 表示为静态库
Mach-O dynamically linked shared library: 表示为动态库
```

[Framework概念](https://www.jianshu.com/p/51a3279c75cb)


区分模拟器

```
#if targetEnvironment(simulator)
// 在模拟器的情况下
#else
// 在真机情况下
#endif

#if (TARGET_IPHONE_SIMULATOR)
// 在模拟器的情况下
#else
// 在真机情况下
#endif
```

[Cocos Creator游戏集成到原生iOS及JS和OC交互](https://www.jianshu.com/p/4382c37f5ec4)


https://easeapi.com/blog/blog/126-podspec-configurations.html


如果打开提示->XXX.app 已损坏,打不开。您应该将它移到废纸篓 ” 或 “ 打不开 XXX.app,因为它来自身份不明的开发者

打开终端（Terminal.app）
拷贝粘贴 sudo spctl --master-disable,按回车键
输入你的账户密码,按回车键确认执行（该密码不会显示,直接输入即可）

在系统的“安全与隐私”中允许“任何来源”,再打开终端
输入以下命令,回车(注意路径根据自己的实际情况替换) sudo xattr -d com.apple.quarantine /Applications/ALL_RoundU.app
重启App即可.


https://book.flutterchina.club/
https://github.com/flutter/samples
https://github.com/iampawan/FlutterExampleApps

https://gitee.com/chenxiancai/STCObfuscator

[iOS App多环境配置Configuration](https://www.jianshu.com/p/a578555cf339)
[为什么在 Xcode 12.3 及之后版本中使用 iOS 模拟器构建项目会失败](https://docs.agora.io/cn/All/faq/ios_simulator_problem)


[添加iOS文件链接,操作.xcodeproj](https://www.jianshu.com/p/e1b8c58cabe2)
[使用代码为 Xcode 工程添加文件](https://blog.csdn.net/sun1018974080/article/details/107641744)
[Xcode工程文件的格式说明](https://www.jianshu.com/p/bd4e3c1a7276)
[iOS自动打包之xcodeproj（二）](https://www.jianshu.com/p/6a7d0a6d9656)
[ruby库xcodeproj使用心得](https://blog.csdn.net/darya_1/article/details/78095821)
[iOS 使用脚本自动化复制target](https://zhuanlan.zhihu.com/p/31514291)


http://docs.cocos.com/creator/manual/zh/advanced-topics/oc-reflection.html
[iOS 分类中同名方法的调用顺序](https://www.jianshu.com/p/3907eed7f9f3)