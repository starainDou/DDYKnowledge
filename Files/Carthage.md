> #### 一、准备(按需执行)

* ##### 升级homebrew 

	1. 查看版本 ``` brew --version ```
	2. 卸载brew ``` ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)" ```  
	3. 安装brew ``` ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" ```
	4. 查看版本 ``` brew --version ``` 
	5. 安装目录	 ``` which brew ```

> #### 二、安装与使用

* ##### 安装

	1. 安装 ``` brew install carthage ```
	2. 版本 ``` carthage version ```
	3. 升级 ``` brew upgrade carthage ```

* ##### 使用

	1. cd 工程目录
	2. 建立Cartfile文件 ``` touch Cartfile ```
	3. 打开Cartfile ``` open -e Cartfile ``` 或 ``` open -a Xcode Cartfile ```
	4. 添加github的三方库，如 ``` github "ReactiveCocoa/ReactiveCocoa" # GitHub.com ```
	5. 添加GitHubEnterprise的三方库，如 ``` 
github "https://enterprise.local/ghe/desktop/git-error-translations" # GitHub Enterprise ``` 
	6. 添加其他git，如私有git ``` git "git@git.idengyun.com:iOSTeam/KNNewRetail.git" "1.0.80" ```
	7. 只支持二进制文件发framework ``` binary "https://my.domain.com/release/MyFramework.json" ```
	8. 版本号

	```
	github "aaa/bbb"  // 不指定版本号，永远获取最新的版本
	github "aaa/bbb" == 1.0   // 只1.0版本
	github "aaa/bbb" ~> 1.0.0 // 1.0及以上的兼容版本
	"some-branch-or-tag-or-commit" //特定的分支、tag、或提交
	```
	9. 更新库 
```
// 指定平台
carthage update --platform iOS
// 不适用二进制
carthage update --no-use-binaries --platform ios
// 更新指定库
carthage update Alamofire
// 更新指定多个库空格分开
carthage update Alamofire SnapKit
```

> #### 三、让自己的库支持cocoapods管理

* ##### Carthage 仅支持动态库
* ##### Carthage 提供给用户的动态库是根据项目中的 动态库 Target 编译打包后生成的
* ##### 步骤

1. 新建Target，选择Cocoa Touch Framework，并设置Deployment Target
2. 添加 .h 文件到 Build Phases -> Headers -> Public
3. 添加 .m 文件到 Build Phases -> Compile Sources
4. 添加其它资源文件到 Build Phases -> Copy Bundle Sources
5. Carthage只构建从 .xcodeproj 分享出来的scheme，所以需要在 Manage Schemes 中将新生成的 scheme 设置为 shared
6. 通过 ```carthage build --no-skip-current``` 来检测 scheme 是否能够构建成功，然后检查 Carthage/Build 目录
7. Carthage 通过搜索 Git tag 来决定用户可以下载哪个版本的项目资源，所以还需要在提交代码后添加 tag 并 push 到 GitHub，格式建议写成 1.2.0 或 v1.2
8. 通过添加下面内容，用来在项目 Readme.md 文件上显示  [![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)

 ``` 
 [![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
 ```
 
 <!--http://lijingcheng.github.io/blog/2016/12/23/shi-yong-carthage-zuo-yi-lai-guan-li/-->
 <!--https://www.jianshu.com/p/30246a000bc6-->
 <!--http://www.cocoachina.com/cms/wap.php?action=article&id=20107-->
 <!--https://www.jianshu.com/p/ee477f659858-->
 <!--https://www.jianshu.com/p/7a0634e14332-->
 <!--https://github.com/Carthage/Carthage-->
 <!--https://www.jianshu.com/p/7a361e9bc6c2-->


