# 配置

1. PROJECT ——> Info ——> Configurations
2. 点击 + 添加新的，如 OnlineDebug
3. TARGET ——> Build Setting ——> Active Compilation Conditions
4. 修改新加的OnlineDebug的值为ONLINEDEBUG
5. TARGET ——> Build Setting ——> Other Swift Flags
6. 修改新加的OnlineDebug的值为-D ONLINEDEBUG
7. 修改新加的Debug的值为-D DEBUG
8. 如果使用CocoaPods可以执行更新命令

# 使用

1. 打印方式

Objective-C

```
// 自定义打印 宏定义
#ifdef DEBUG
#define DDYInfoLog(fmt, ...) NSLog((@"\nDebug\nFileName:%s\nMethodName:%s\nLineNumber:%d\n" fmt),__FILE__, __FUNCTION__, __LINE__, ##__VA_ARGS__)
#elseif ONLINEDEBUG
#define DDYInfoLog(fmt, ...) NSLog((@"\nOnlineDebug\nFileName:%s\nMethodName:%s\nLineNumber:%d\n" fmt),__FILE__, __FUNCTION__, __LINE__, ##__VA_ARGS__)
#else
#define DDYInfoLog(fmt, ...) NSLog((@"\nRelease\nFileName:%s\nMethodName:%s\nLineNumber:%d\n" fmt),__FILE__, __FUNCTION__, __LINE__, ##__VA_ARGS__)
#endif
```

Swift

```
// 自定义打印 全局函数
func ddyInfoLog<T>(_ message: T, fileName: String = #file, methodName: String = #function, lineNumber: Int = #line) {
    #if DEBUG
    print("\nDebug\nFileName:\(fileName as NSString)\nMethodName:\(methodName)\nLineNumber:\(lineNumber)\n\(message)")
    #elseif ONLINEDEBUG
    print("\nOnlineDebug\nFileName:\(fileName as NSString)\nMethodName:\(methodName)\nLineNumber:\(lineNumber)\n\(message)")
    #else
    print("\nRelease\nFileName:\(fileName as NSString)\nMethodName:\(methodName)\nLineNumber:\(lineNumber)\n\(message)")
    #endif
}
```

2. 网络请求Host

Objective-C

```
#ifdef DEBUG
#define requestHost @"http://10.10.8.22:9214"
#elseif ONLINEDEBUG
#define requestHost @"http://online-test.idengyun.com"
#else 
#define requestHost @"http://online-test.idengyun.com"
#endif
```

Swift

```
func requestHost() -> String {
    #if DEBUG
    return "http://10.10.8.22:9214"
    #elseif ONLINEDEBUG
    return "http://online-test.idengyun.com"
    #else
    return "http://app.iblln.com"
    #endif
}
```

3. AppIcon

* 使用Asset Catalog 分别命名为 <br>
	AppIconDebug <br>
	AppIconOnlineDebug <br>
	AppIconRelease <br>

* Target ——> Build Settings 搜索 Asset Catalog App Icon Set Name <br>
	对应的修改选项值 <br>
	
4. AppName

* Target ——> Build Settings里点+号，添加一个User-Defined Setting
* 建立key值 APP_DISPLAY_NAME, 然后对应的修改选项值
* 进到 Target ——> Info里，修改 Bundle Name 为$(APP_DISPLAY_NAME)

4. 打包配置

	将Release环境以外的Skip Install配置成NO。




