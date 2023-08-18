##### Mac OSX 校验文件 MD5/SHA1

```
# MD5
md5 filePath
# SHA1
shasum filePath 
# SHA1 
shasum -a 1 filePath
# SHA 256
shasum -a 256 filePath
```

##### 终端 sqlite3

```
// 创建或打开
sqlite3 SongListCache.sqlite

// 数据库文件所包含的所有自定义数据表
.tables

// 查看某个数据表所有内容
select * from MusicListTable_1000;

// https://www.cnblogs.com/Camier-myNiuer/p/5716826.html
```

##### 字符

```
半角空格（0×20）
制表符(t)
全角空格 (u3000)
u0009 CHARACTER TABULATION u000A LINE FEED u000D CARRIAGE RETURN u0020 SPACE u0085 NEXT LINE u00A0 NBSP u1680 OGHAM SPACE MARK u180E MONGOLIAN VOWEL SEPARATOR u2000 EN QUAD u200A HAIR SPACE u200B ZERO WIDTH SPACE u2028 LINE SEPARATOR u2029 PARAGRAPH SEPARATOR u202F NARROW NO-BREAK SPACE u205F MEDIUM MATHEMATICAL SPACE u3000 IDEOGRAPHIC SPACE
```

##### 文件已损坏解决

```
sudo spctl --master-disable

sudo xattr -d com.apple.quarantine /Applications/xxxx.app
```

##### 在Mac下使用crontab执行定时脚本

[在Mac下使用crontab执行定时脚本](https://tool.4xseo.com/article/311355.html)

##### Jenkins 打包

https://www.cnblogs.com/somepark/p/16813250.html

##### 在APP下载链接里查看

```
https://itunes.apple.com/lookup?id=951937596.
```
##### 黑苹果

https://apple.sqlsec.com/5-%E5%AE%9E%E6%88%98%E6%BC%94%E7%A4%BA/5-7.html

##### 消除更新红点

```
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0

killall Dock
```

##### MAC 命令行工具获取手机信息

```
brew install libimobiledevice
brew install ideviceinstaller
https://cloud.tencent.com/developer/article/1536056
[使用libimobiledevice + ifuse提取iOS沙盒文件](https://cloud.tencent.com/developer/article/1521371)
```

##### 查看图片元数据

```
[Exiftool 命令行操作图片元信息](https://blog.csdn.net/weixin_34393428/article/details/88679127)
```

##### 系统设置扩展没有Xcode

```
PATH=/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support:"$PATH"
lsregister -f /Applications/Xcode.app
```
[修复Xcode Source Editor在masOS的扩展中不显示](https://www.sunyazhou.com/2020/10/XcodeSourceEditorNotWork/)