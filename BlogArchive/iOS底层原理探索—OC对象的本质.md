> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.jianshu.com/p/ffd742041946

在探索 OC 对象的本质之前，我们要明白 Objective-C 的代码，底层的实现都是 C/C++ 代码。

![](http://upload-images.jianshu.io/upload_images/1760191-5502bdaeca8dd513.png)

OC 代码转化过程. png

而 OC 中的对象、类则是基于 C/C++ 的结构体来实现的。

我们可以通过将创建好的 OC 文件，转化成 C++ 文件来看一下 OC 对象的底层结构。

OC 代码转换成 C++
------------

通过命令行将 OC 的 main.m 文件转换成 C++，生成 main.cpp。

```
   clang -rewrite-objc main.m -o main.cpp 



```

需要注意这种方式没有指定运行平台和架构模式，我们可以通过命令行设置参数，来指定运行平台和架构模式

```
   xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main-arm64.cpp 



```

生成的 main-arm64.cpp 文件就是 main.m 转换 c++ 后的文件，直接拖拽到工程中，就可以查看底层实现了。我们在 main-arm64.cpp 文件中搜索 NSObjcet，可以找到 NSObjcet_IMPL（IMPL 即 implementation 的缩写，代表实现）。

```
struct NSObject_IMPL {
    Class isa;
};


```

可以看到，NObject 的底层实现就是一个结构体。我们进一步查看 Class

```
typedef struct objc_class *Class;


```

我们发现 Class 其实就是一个指针，指向了 objc_class 类型的结构体。

> 通过以上探索我们可以得出总结，OC 中对象的底层实现都是通过 C\C++ 的结构体实现的。那么一个 OC 对象占据了多少内存空间呢？下面让我们用代码实际验证一下。

![](http://upload-images.jianshu.io/upload_images/1760191-aba7c6c8f33dbc32.png)

NSObject 对象占多少内存空间. png

通过代码我们创建了一个 NSObject 对象，分别调用了

`class_getInstanceSize`

和

`malloc_size`

方法来查看占据的内存空间。我们看到打印输出了 8 和 16，这是怎么回事呢？

我们通过查看 OC 源码可知，

`class_getInstanceSize`

是

**返回类的成员变量占据内存的大小**

。而

`malloc_size`

是

**获取 obj 指针指向内存的大小**

我们上文创建的 NSObject 对象 obj 的结构体只有一个成员：isa 指针，而指针在 64 位架构中占 8 个字节，所以第一次打印输出了 8。但是系统为 obj 对象分配了 16 个字节的内存，第二次打印输出 16。

![](http://upload-images.jianshu.io/upload_images/1760191-10906f567eef7cca.jpeg)

NSObject 对象占据内存空间示意图. jpeg

上图中黄色区域系统为 obj 对象分配的 16 个字节的内存，但是 obj 对象结构体中只有一个 isa 指针，所以只占据了绿色区域的 8 个字节。这一点可以通过查看

`alloc`

底层源码也可以证实：

![](http://upload-images.jianshu.io/upload_images/1760191-9c678a6b920a6e35.jpeg)

alloc 源码. jpeg

通过

`alloc`

底层源码可以看到，如果一个对象占据的内存小于 16 时，则会直接返回 16，也就是说，系统在为对象分配内存空间时，最小为 16 个字节。

至此我们知道了系统在创建一个对象的时候会为其分配最少 16 字节的内存空间，其中对象的`isa`指针占据 8 个字节。我们通过下面一道面试题继续深入了解 OC 对象的底层本质。

> 在 64 位环境下，下面的代码会输出什么？

```
@interface Person : NSObject
{
    int _age;
}
@end

@implementation Person
@end


@interface Student : Person
{
    int _no;
}
@end

@implementation Student
@end

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        
        NSLog(@"%zd  %zd",
              class_getInstanceSize([Person class]),
              class_getInstanceSize([Student class])
              );
    }
    return 0;
}


```

> 经过代码分析，这道面试题想问的其实就是一个 Person 对象和一个 Student 对象分别占据多少内存空间。但是需要注意的是 Person 对象和 Student 对象分别有一个实例变量、Student 继承自 Person 对象。

经过综合上文简单分析，我们可以简单画出 Person 对象和 Student 对象的内存结构示意图：  

![](http://upload-images.jianshu.io/upload_images/1760191-958ccf23632f9df8.png)

Person 对象和 Student 对象内存结构示意图. png

我们可以看到，在 Person 对象结构体中，有一个父类类型的结构体指针和一个

`int`

类型成员变量

`_age`

，分别占据内存空间为 8 字节和 4 字节。根据底层源码对象分配的最小内存空间为 16 可得，Person 对象占据的内存空间为 16。此处值得注意的是，根据

**内存对齐**

，也可得出 Person 对象占据 16 个字节。

![](http://upload-images.jianshu.io/upload_images/1760191-ff9e84976d094a1d.jpeg)

Person 对象内存示意图. jpeg

那么根据这个分析逻辑，Student 对象占据的内存空间是不是 20 或者 24 呢？答案当然是不对，虽然 Student 结构体中有 Person 类型的指针（16 个字节）和`int`类型成员变量`_no`（4 个字节），但是要注意的是上面 Person 对象占据的 16 个字节是没有完全占据完的，还留有 4 个字节的空白地址，系统会自动的把`int`类型成员变量`_no`放在这 4 个字节的空白地址中，所以 Student 对象也占据 16 个字节的内存空间。  

![](http://upload-images.jianshu.io/upload_images/1760191-ab775e74d63ccde4.jpeg)

Student 内存示意图. jpeg

> 内存对齐:
> 
> 1.  前面的地址必须是后面的地址整数倍, 不是就补齐。
> 2.  整个结构体的地址必须是最大字节的整数倍。

> OC 对象分为三种：  
> **实例对象（instance 对象）**  
> **类对象（class 对象）**  
> **元类对象（meta-class 对象）**

1、实例对象（instance 对象）
-------------------

**实例对象（instance 对象）**就是通过类的`alloc`出来的对象，每次调用`alloc`都会产生新的实例对象。例如：

```
NSObjcet *obj1 = [[NSObjcet alloc] init];
NSObjcet *obj2 = [[NSObjcet alloc] init];


```

obj1 和 obj2 都是 NSObject 的实例对象，但是它们是不同的两个实例对象，分别占用两块不同的内存地址。  
实例对象在内存中存储的信息包括：  
1、`isa`指针  
2、其他`成员变量`  

![](http://upload-images.jianshu.io/upload_images/1760191-2e82a3c57ba5ec45.png)

实例对象在内存中存储信息示意图. png

2、类对象（class 对象）
---------------

**类对象（class 对象）就是**通过`class`方法或者 runtime 的`object_getClass`方法得到的 class 对象

```
Class objClass1 = [obj1 class];
Class objClass2 = [obj2 class];
Class objClass3 = [NSObject class];


Class objClass4 = object_getClass(obj1);
Class objClass5 = object_getClass(obj2);


```

objClass1-objClass5 都是 NSObject 的类对象（class 对象），且它们是同一个对象。

> 每个类在内存中有且只有一个 class 对象

类对象在内存中存储的信息包括：  
1、`isa`指针  
2、`superClass`指针  
3、类的`属性`信息（`@property`），类的成员变量信息（`ivar`）  
4、类的`对象方法`信息（`instance method`），类的`协议`信息（`protocol`）  

![](http://upload-images.jianshu.io/upload_images/1760191-b0a49c8c145defd9.png)

类对象在内存中存储信息示意图. png

3、元类对象（meta-class 对象）
---------------------

**元类对象（meta-class 对象）**就是通过 RunTime 的`object_getClass`方法得到的对象

```
Class objectMetaClass = object_getClass([NSObject class]);


```

objectMetaClass 就是 NSObject 的元类对象

> 每个类在内存中有且只有一个元类对象

元类对象和类对象的内存结构是一样的，但是用途不一样，元类对象在内存中存储的信息包括：  
1、`isa`指针  
2、`superClass`指针  
3、类的`类方法`信息（`class method`）  

![](http://upload-images.jianshu.io/upload_images/1760191-59d94b3303724f26.png)

元类对象在内存中存储信息示意图. png

以上我们了解了实例对象、类对象和元类对象的含义以及包含的内容，那么它们当中的`isa`指针和`superClass`指针分别指向哪里呢?

isa 指针的指向
---------

isa 指针指向用一张示意图来简单概括一下：

![](http://upload-images.jianshu.io/upload_images/1760191-9f0d589ac77c87b1.jpeg)

isa 指针指向示意图. jpeg

实例对象（instance 对象）的`isa`指针指向`class`。当调用对象方法时，通过实例对象的`isa`找到`class`，最后找到对象方法的实现进行调用  
类对象（class 对象）的`isa`指针指向 meta-class。当调用类方法时，通过类对象的`isa`找到 meta-class，最后找到类方法的实现进行调用。

class 的 superClass 指针的指向
------------------------

class 的 superClass 指针指向用一张示意图来简单概括一下：  

![](http://upload-images.jianshu.io/upload_images/1760191-0cf0a692554c9673.jpeg)

superClass 指针指向示意图. jpeg

图中举例 Student 继承自 Person，Person 继承自 NSObject。

当 Student 的实例对象要调用父类 Person 的对象方法时，会先通过

`isa`

找到 Student 的

`class`

，然后通过

`class`

中的 superClass 找到父类 Person 的

`class`

，最后找到对象方法的实现进行调用。

meta-class 对象的 superClass 指针指向
------------------------------

![](http://upload-images.jianshu.io/upload_images/1760191-cf604bd592f17c58.jpeg)

meat-class 的 superClass 指针指向示意图. jpeg

同上，当 Student 的 class 要调用 Person 的类方法时，会先通过 isa 找到 Student 的 meta-class，然后通过 superClass 找到 Person 的 meta-class，最后找到类方法的实现进行调用。

这里当然要提一下非常经典的 isa 指向图，做进一步的总结：

![](http://upload-images.jianshu.io/upload_images/1760191-16c6237e389628a0.jpeg)

超神图. jpeg

> 1、instance 的 isa 指向 clas  
> 2、class 的 isa 指向 meta-class  
> 3、meta-class 的 isa 指向基类的 meta-class，基类的 isa 指向自己  
> 4、class 的 superClass 指向父类的 class，如果没有父类，则 superClass 指针为 nil  
> 5、meta-class 的 superClass 指向父类的 meta-class，基类的 meta-class 的 superClass 指向基类的 class  
> 6、instance 调用对象方法的轨迹：通过 isa 找到 class，方法不存在，就通过 supercla 逐层父类里找，有就实现，如果找到基类仍没有找到，就会抛出`unrecognized selector sent to instance`异常  
> 7、class 调用类方法的轨迹：通过 isa 找到 meta-class，方法不存在，就通过 superClass 逐层父类里找。

**补充：**  
相信很多人在查看源码或者看一些底层博客的时候，经常会看到下面一段代码，来讲述 class 的内部结构：

```
typedef struct objc_class *Class;

struct objc_class {
    Class _Nonnull isa  OBJC_ISA_AVAILABILITY;

#if !__OBJC2__
    Class _Nullable super_class                              OBJC2_UNAVAILABLE;
    const char * _Nonnull name                               OBJC2_UNAVAILABLE;
    long version                                             OBJC2_UNAVAILABLE;
    long info                                                OBJC2_UNAVAILABLE;
    long instance_size                                       OBJC2_UNAVAILABLE;
    struct objc_ivar_list * _Nullable ivars                  OBJC2_UNAVAILABLE;
    struct objc_method_list * _Nullable * _Nullable methodLists                    OBJC2_UNAVAILABLE;
    struct objc_cache * _Nonnull cache                       OBJC2_UNAVAILABLE;
    struct objc_protocol_list * _Nullable protocols          OBJC2_UNAVAILABLE;
#endif

} OBJC2_UNAVAILABLE;


```

这段源码其实讲述的也是 class 内部结构，包含成员变量列表、方法列表、方法缓存以及协议列表。细心的人可能会发现，这段代码里面是有`if`判断条件的:

判断条件是非 OC2.0 版本，也就是说在 OC2.0 之前的版本中，class 底层的结构体中包含上面代码所讲述的，但我们现在所用的最新版肯定是 OC2.0 版本了，所以这段代码就不再使用了。

新的 class 底层的 objc_class 结构体：

![](http://upload-images.jianshu.io/upload_images/1760191-cc7eeaf32f98a523.jpeg)

objc_class 结构体. jpeg

可以看到结构体中只包含`isa`、`superclass`、`cache`和`bits`。而`bits`经过`& FAST_DATA_MASK`之后，会得到`struct class_rw_t`这样一个结构体：  

![](http://upload-images.jianshu.io/upload_images/1760191-23aaa5b83c9c99ec.jpeg)

class_rw_t.jpeg

> rw 代表 readwrite，可读可写  
> t 代表 table，列表

`struct class_rw_t`结构体中就包含了方法列表、属性列表以及协议列表，这些都是可读可写的。其中还包含一个`struct class_ro_t`的结构体：  

![](http://upload-images.jianshu.io/upload_images/1760191-f9b2f5c4063f6af1.jpeg)

class_ro_t.jpeg

> ro 代表 readonly，只读

`struct class_ro_t`的结构体中包含了 instance 对象占用的内存空间、类名以及成员变量列表，当然这些都是只读的。

**一个 NSObject 对象占用多少内存？**  
答：系统会为一个 NSObject 对象分配最少 16 个字节的内存空间。一个指针变量所占用的大小（64bit 占 8 个字节，32bit 占 4 个字节）  
**对象的 isa 指针指向哪里？**  
答：instance 对象的`isa`指针指向 class 对象，class 对象的`isa`指针指向 meta-class 对象，meta-class 对象的`isa`指针指向基类的 meta-class 对象，基类自己的`isa`指针指向自己。  
**OC 的类信息存放在哪里？**  
答：成员变量的具体值存放在实例对象（instance 对象）；对象方法，协议，属性，成员变量信息存放在类对象（class 对象）；类方法信息存放在元类对象（meta-class 对象）。

> 本文如果对你有所帮助，点亮喜欢支持一下  
> 更多底层知识，扫码关注公众号  
> iOS 进阶
> 
> ![](http://upload-images.jianshu.io/upload_images/1760191-6a8057dc9afbe7e4.jpg)
> 
> 我的二维码. jpg