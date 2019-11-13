##### Xcode11后打包不允许i386和x86_64，否则报错 IPA processing failed。

* i386:模拟器32位，iPhone 5s及以下
* x86_64:模拟器64位, iPhone 6及以上
* armv7:真机32位, iPhone 4及以下
* armv7s:真机32位，iPhone 5, iPhone 5s
* arm64:真机64位，iPhone 6及以上


##### 方案一:

* 暂时使用Xcode 10打包

##### 方案二:

* 去除不支持的架构。

1. 进入SDK内 ``` cd aa.framework ``` 
2. 查看支持的架构 ``` lipo -inof aa ```
3. 可能显示 Architectures in the fat file: aa are: i386 x86_64 armv7 arm64
4. 删除不支持的架构 

```
lipo -remove i386 aa -o aa
lipo -remove x86_64 aa -o aa
```
5. 重新打包

