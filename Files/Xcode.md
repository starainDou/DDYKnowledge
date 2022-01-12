> ### 键盘符号


* ⌘ Command / Cmd
* ⌥ Option / Alt
* ⇧ Shift / Caps Loc                                
* ⌃ Control / Ctrl
* ⏎ Return
* ⌫ Delete
* ← Left
* → Right
* ↑ top
* ↓ down

> ### 快捷键

##### 文件

* 新建文件: Command + N
* 新建空文件: Command + Ctrl + N
* 新建项目: Command + Shift + N
* 打开文件: Command + O
* 保存文件: Command + S
* 保存所有文件: Command + Alt + S
* 文件另存为: Command + Shift + S
* 关闭窗口: Command + W
* 退出程序: Command + Q
* 关闭文件: Command + Shift + W
* 内容全选: Command + A
* 自动对齐: 选中后Ctrl + I

##### 编辑

* 向左缩进: Command + [ 
* 向右缩进: Command + ] 
* 项目中查找: Command + Shift + F 
* 查找下一个: Command + G
* 查找上一个: Command + Shift + G
* 前移光标: Ctrl + F
* 后移光标: Ctrl + B
* 移动光标到上一行: Ctrl + P
* 移动光标到下一行: Ctrl + N
* 移动光标到本行行首(替换Home键): Ctrl + A
* 移动光标到本行行尾(替换end键): Ctrl + E 
* 交换光标左右两边的字符: Ctrl + T 
* 删除光标右边单个字符(如果选中则删除选中): Ctrl + D
* 删除本行右边所有字符(如果选中则删除选中): Ctrl + K
* 将插入点置于窗口正中: Ctrl + L
* 参数提示: Ctrl + . 
* 显示代码提示菜单: Esc 
* 向上移动: Command + Option + [
* 向下移动: Command + Option + ]
* 局部折叠: Command + Option + Left
* 局部展开: Command + Option + Right
* 全局折叠: Command + Option + Shift + Left
* 全局展开: Command + Option + Shift + Right

##### 调试

* 运行程序: Command + R 
* 停止运行: Command + . 
* 编译程序: Command + B 
* 清除工程: Command + Shift + K
* 带提示清除: Command + Shift + Option + K
* 切换.h和.m文件: Command + Ctrl + Top/Down 
* 设置或取消断点: Command + \
* 允许或禁用当前断点: Command + Alt + \
* 查看全部断点: Command + Alt + B
* 编译并运行(根据设置决定是否启用断点): Command + Enter
* 编译并调试(触发断点): Command + Y
* 终止运行或调试: Command + SHIFT + Enter
* 继续(在调试中): Command + Alt + P
* 跳过: Command + Alt + 0
* 跳入: Command + Alt + I
* 跳出: Command + Alt + T

> ### 一些默认路径

* 快捷代码段 Libary/Developer/Xcode/UserData/CodeSnippets
* 真机调试包 /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport

> ### Error

1. 签名问题
```
object file format unrecognized, invalid, or unsuitable
/usr/bin/codesign failed with exit code 1
```
解决方案
```
sudo mv /usr/bin/codesign_allocate /usr/bin/codesign_allocate_old
sudo ln -s ...Xcode.app/Platforms/iPhoneOS.platform/Developer/usr/bin/codesign_allocate /usr/bin
```

> ### 优化

// 不每次重复全量编译
user-defined HEADERMAP_USES_VFS YES

// Command PhaseScriptExecution failed with a nonzero exit code
SWIFT_ENABLE_BATCH_MODE NO


> ### 打包前添加Action判断是否release

Edit Scheme -> Archive -> Pre-actions -> New Run Script Action -> provide build setting from

```
if [ $CONFIGURATION != "Release" ]; then

osascript -e 'tell app "Xcode" to display dialog "正在非Release打包！当前环境'$CONFIGURATION'"'

fi
```

> ### 打包前自增build号

[持续集成时指定构建号](https://www.jianshu.com/p/eba178ee4150)

```
if [ $CONFIGURATION == Release ]; then
    echo "Bumping build number..."
    plist=${PROJECT_DIR}/${INFOPLIST_FILE}

# increment the build number (ie 115 to 116)
    buildnum=$(/usr/libexec/PlistBuddy -c "Print CFBundleVersion" "${plist}")
    if [[ "${buildnum}" == "" ]]; then
        echo "No build number in $plist"
        exit 2
    fi

    buildnum=$(expr $buildnum + 1)
    /usr/libexec/Plistbuddy -c "Set CFBundleVersion $buildnum" "${plist}"
    echo "Bumped build number to $buildnum"

else
    echo $CONFIGURATION " build - Not bumping build number."
fi
```

<!--https://www.jianshu.com/p/4dd914b93555-->