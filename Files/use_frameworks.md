\# 在Podfiel中是注释即使用和不使用

> # 使用\#use_frameworks!：

如果在Podfile文件里不使用 use_frameworks! 则是会生成相应的 .a文件（静态链接库），通过 static libraries 这个方式来管理pod的代码。

Linked:libPods-xxx.a包含了其它用pod导入的第三方框架的.a文件。

> # 而使用use_frameworks!：

则cocoapods 会生成相应的 .frameworks文件（动态链接库：实际内容为 Header + 动态链接库 + 资源文件），使用 dynamic frameworks 来取代 static libraries 方式。

Linked:Pods_xxx.framework包含了其它用pod导入的第三方框架的.framework文件。


即：

use_frameworks! -> dynamic frameworks 方式 -> .framework
\#use_frameworks! -> static libraries 方式 -> .a

> # 注意

Swift项目cocoapods 默认 use_frameworks!
OC项目cocoapods 默认 \#use_frameworks!
用cocoapods 导入Swift 框架到 swift项目和OC项目都必须要 use_frameworks!
使用 dynamic frameworks，必须要在Podfile文件中添加 use_frameworks!
