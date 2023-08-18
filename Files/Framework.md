当我们点击xcode工程的target时，在Build Phases这块framework可选的状态有下面几种：在xcode 10及之前，有三种状态：Do Not Embed、Embed & Sign和Embed Without Signing

Embed：嵌入，用于动态库，动态库在运行时链接，所以它们需要被打进bundle里面。如何判断呢？使用终端执行

```
file frameworkToLink.framework/frameworkToLink

current ar archive:说明是静态库，选择Do not embed
Mach-0 dynamically：说明是动态库，选择Embed
```

静态库和动态库的区别

```
静态库：链接时完整地拷贝至可执行文件中，被多次使用就有多份冗余拷贝，存在形式：.a和.framework
动态库：链接时不会拷贝至可执行文件中，程序运行时由系统动态加载到内存以供程序调用，系统只加载一次，多个程序共用，可以节省内存。存在形式：.dylib和.framework
```

Signing：只用于动态库，如果已经有签名了就不需要再签名。如何判断呢？使用终端执行

```
codesign -dv frameworkToLink.framwork

code object is not signed at all 或者 adhoc:选择Embed and sign
其它：表示已经正确签名，选择Embed Without Signing
```


[Unity2019 4.37f1 打包iOS 所遇问题与解决方案](https://www.jianshu.com/p/4381dc7b2fb8)
[C++中调用OC代码](https://blog.csdn.net/u013712343/article/details/122623800)