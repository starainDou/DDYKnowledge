# DDYScratchView

* 跟随系统国际化

单纯根据系统语言，只需要把DDYLanguageTool文件夹拖入工程，然后在PCH或者AppDelegate 引入即可

* 应用内切换语言

这种方式可以在不重启App情况下国际化，但是局限在应用内控件，对displayName 和 系统弹窗(如权限弹窗)无效

使用时用 label.text = NSLocalizedString(@"string文件中字符串", nil);
或者 label.text = DDYLocalStr(@"string文件中字符串");


![DDYLanguageToolCN.png](https://github.com/starainDou/DDYDemoImage/blob/master/DDYLanguageToolCN.png)  ![DDYLanguageToolEN.png](https://github.com/starainDou/DDYDemoImage/blob/master/DDYLanguageToolEN.png)
  


> # NSLocalizedString

Xcode中有本地化的内置支持

* NSLocalizedString的定义：

```
#define NSLocalizedString(key, comment) \
[[NSBundle mainBundle] localizedStringForKey:(key) value:@"" table:nil]
```
第一个参数key是从Localizable.strings这个文件中读取对应的key-value值。
第二个参数comment可以是nil，可以是一段为空的字符串，也可以是对key的注释。


* 本地化字符串 

添加语言支持

![AddLanguage.png](http://upload-images.jianshu.io/upload_images/1465510-df8357aea2b0ec6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

建立String File文件，命名命名为“Localizable，生成一个Localizable.strings文件(名字不要其他的) 

![Localizable.png](http://upload-images.jianshu.io/upload_images/1465510-451ef24a5833160c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选中新建的Localizable.strings文件，点击右边的Localize，添加English和Chinese

![Localize.png](http://upload-images.jianshu.io/upload_images/1465510-0421e964609dedb8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在Localizable.strings(English)  文件中写 "OK" = "OK";（不要忘记写分号） 
在Localizable.strings(Chinese) 文件中写 "OK" = "确定";（不要忘记写分号）

使用 
#define OKText NSLocalizedString(@"OK", nil)


如果你的strings文件名字不是Localizable而是自定义的话，如hel.strings，那么你就得使用NSLocalizedStringFromTable()来读取本地化字符串 NSLocalizedStringFromTable(@"hello",@"hel", nil)

> # 如果不能添加语言支持（localizations--choose files and reference language to create...为空白）

打开工程配置，搜 /* Begin XCBuildConfiguration section */
在之前添加段落

/* Begin PBXVariantGroup section */
		4C0E56D11F09E6B900583071 /* Localizable.strings */ = {
			isa = PBXVariantGroup;
			children = (
				4C0E56D01F09E6B900583071 /* en */,
				4C0E56D21F09E6C200583071 /* zh-Hans */,
			);
			name = Localizable.strings;
			sourceTree = "<group>";
		};
		ABCB97AC1D71D063000DEBEF /* LaunchScreen.storyboard */ = {
			isa = PBXVariantGroup;
			children = (
				ABCB97AD1D71D063000DEBEF /* Base */,
				4C0E56CC1F09E37B00583071 /* zh-Hans */,
			);
			name = LaunchScreen.storyboard;
			sourceTree = "<group>";
		};
/* End PBXVariantGroup section */

然后添加直接添加Localizable.strings，方法见上，不考虑别的问题，然后回到配置文件搜索
/* Localizable.strings in Resources */
复制ID
搜/* Begin PBXVariantGroup section */，将/* Localizable.strings */ 前面的ID 替换保存即可

XCBuildConfiguration


