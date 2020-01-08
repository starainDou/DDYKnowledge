> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.jianshu.com/p/9856cf221c2b

[![](https://upload.jianshu.io/users/upload_avatars/1910830/4fda0dde-72da-4b50-a3a9-7af5340f2e7e.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/db56e4cae85b)

0.1052019.12.23 15:57:49 字数 2,655 阅读 26

前言
--

这是三个没多少关系的知识点，只是今天，2019 年 12 月 23 号，突然就想把它们放到一起，来简单梳理下。

消息转发机制
------

参考:

[iOS Runtime 消息转发机制原理和实际用途](https://www.jianshu.com/p/fdd8f5225f0c)

[iOS - 动态添加方法和消息转发](https://www.jianshu.com/p/c31c70854e7b)

调用一个对象的方法，如果方法已经实现，则会接收消息并响应。如果方法未实现，如果没有做预防措施，则会运行时崩溃并报错 `unrecognized selector sent to instance 0x1c4015330`。这中间经历了什么呢？参考下面这张原理图:

![](http://upload-images.jianshu.io/upload_images/1910830-41b8ad7eede55a03.jpg)

iOS 消息转发机制. jpeg

下面结合源码分析：

```
#import "Person.h"
#import <objc/runtime.h>

@implementation Person

+ (BOOL)resolveClassMethod:(SEL)sel {
    if ([NSStringFromSelector(sel) isEqualToString:@"testFuncation"]) {
          
          
        const char *classChar = [NSStringFromClass([self class]) UTF8String];
        Class metaClass = objc_getClass(classChar);
        IMP imp = class_getMethodImplementation(self, @selector(createClassFun));
        class_addMethod(metaClass, sel, imp, "v@:");
        return YES;
      }

    return [super resolveClassMethod:sel];
}

+ (BOOL)resolveInstanceMethod:(SEL)sel {
    NSLog(@"resolveInstanceMethod： %@",NSStringFromSelector(sel));
    if ([NSStringFromSelector(sel) isEqualToString:@"testFuncation"]) {
        
        
        IMP imp = class_getMethodImplementation(self, @selector(createTestFuncation));
        class_addMethod([self class], sel, imp, "v@:");
        return YES;
    }
    
    return [super resolveInstanceMethod:sel];
}

- (id)forwardingTargetForSelector:(SEL)aSelector {
    NSLog(@"----- forwardingTargetForSelector： %@",NSStringFromSelector(aSelector));
    
    
    
    
    return [super forwardingTargetForSelector:aSelector];
}


- (NSMethodSignature *)methodSignatureForSelector:(SEL)aSelector {
    
    if ([super methodSignatureForSelector:aSelector] == nil) {
        NSMethodSignature *sign = [NSMethodSignature signatureWithObjCTypes:"v@:"];
        return sign;
    }
    return [super methodSignatureForSelector:aSelector];
}

- (void)forwardInvocation:(NSInvocation *)anInvocation {
    
    SEL sel = anInvocation.selector;

    Person *p = [Person new];
    if ([p respondsToSelector:sel]) {
        
        [anInvocation invokeWithTarget:p];
    } else {
        
        [self doesNotRecognizeSelector: sel];
    }
}


+(void)createClassFun {
    NSLog(@"createClassFun 被调用");
}


-(void)createTestFuncation {
    NSLog(@"createTestFuncation  被调用");
}


```

调用：

```
Person *p = [[Person alloc] init];
[p performSelector:@selector(testFuncation)];


```

### resolveClassMethod & resolveInstanceMethod

动态添加类方法和实例方法。给对象发送消息，未找到对应方法时首先调用的方法。在这里动态添加方法，防止崩溃。  
关于动态添加方法：

```
OBJC_EXPORT BOOL
class_addMethod(Class _Nullable cls, SEL _Nonnull name, IMP _Nonnull imp, 
                const char * _Nullable types) 


```

*   Class：需要添加方法的类。
*   SEL：调用时未实现的方法。
*   IMP：防止 Crash 添加的方法的指针。
*   const char：方法描述：
    *   "v@:": 这是一个 void 类型方法，没有参数传入。
    *   "i@:": 这是一个 int 类型方法，没有参数传入。
    *   "i@:@": 这是一个 int 类型方法，有一个参数传入。

每一个方法都会默认隐藏两个参数：self 和 _cmd。其中 self 代表方法调用者，_cmd 标识这个方法的 SEL，是用来描述这个方法的返回值、参数的。[详见官网](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.apple.com%2Flibrary%2Farchive%2Fdocumentation%2FCocoa%2FConceptual%2FObjCRuntimeGuide%2FArticles%2FocrtTypeEncodings.html)。

另外，用这种方式添加的方法是无法直接调用的，要用 `performSelector:` 调用。因为 performSelector 方法是运行时去寻找方法的，在编译时不做校验，目前 Xcode 对未实现的方法会给出警告。 `class_addMethod` 是在运行时添加方法的。

### forwardingTargetForSelector

方法重定向，是 NSObject 的函数，用来决定谁执行方法。

### methodSignatureForSelector & forwardInvocation

1.  `[NSMethodSignature signatureWithObjCTypes:"v@:"];` 是方法签名。参数规则同上面所说的 `const char` 。
2.  `forwardInvocation` 是一个， 不能识别的消息的分发中心，在这里对最终未响应的消息做处理。该方法只有对象无法正常响应消息时才会被调用。

### 小结

1.  调用 resolveClassMethod 或者 resolveInstanceMethod，给个机会让类添加这个方法的实现。
2.  调用 forwardingTargetForSelector 让别的对象去实现这个函数。
3.  调用 methodSignatureForSelector（函数签名） 和 forwardInvocation（函数分发执行） 灵活地将目标函数以其他方式实现。
4.  如果以上三步仍未解决问题，则调用 doesNotRecognizeSelector 抛出异常。

响应者链和事件传递
---------

参考：

[iOS 响应链和事件传递](https://www.jianshu.com/p/410ca78faa98)

[iOS 响应者及响应者链](https://www.jianshu.com/p/89d20caabf41)

### 响应者

响应者对象是 `UIResponder`。只有继承 `UIResponder` 的类，才能处理事件。如 `UIApplication`、`UIView` 及其子类等。 CALayer 不是 UIResponder 的子类，无法处理事件。

### 查找响应者、事件的分发和传递

1.  当 iOS 程序发生触摸事件后，系统会利用 RunLoop 将事件加入到 UIApplication 管理的一个任务队列中。此时，该触摸事件被封装成一个 UIEvent 对象。具体可参考 [深入理解 RunLoop](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.ibireme.com%2F2015%2F05%2F18%2Frunloop%2F)。
2.  UIApplication 将处于任务队列最前端的事件向下分发，即 分发给 UIWindow 处理。
3.  UIWindow 将事件向下分发。此时调用 `hitTest:withEvent:` 在视图层次结构中找到一个合适的 UIView 来处理触摸事件。
4.  UIView 首先要看自己是否能够处理事件，触摸点是否在自己身上。如果能。则继续寻找子视图。
5.  遍历子控件，重复以上两步。
6.  如果没有找到，那么自己就是事件处理者。
7.  如果自己不能处理，则不做任何处理。

UIView 不接受事件处理的情况有以下三种：

*   alpha < 0.01
*   userInteractionEnabled = NO
*   hidden = YES

找响应者，即是从父 View 到 子 View 的查找过程。主要用到了 UIView 的 `hitTest:withEvent:` 以及 `pointInside:withEvent:` 两个方法：

![](http://upload-images.jianshu.io/upload_images/1910830-faf1f0911f0c649f.png)

iOS 查找响应者对象. png

```
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event


- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event


```

#### hitTest

点击检测方法，实现方法大致如下:

```
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event {
    
    if (self.userInteractionEnabled == NO || self.hidden == YES || self.alpha <= 0.01) return nil;
    
    if ([self pointInside:point withEvent:event] == NO) return nil;
    
    NSInteger count = self.subviews.count;
    for (NSInteger i = count - 1; i >= 0; i--) {
        UIView *childView = self.subviews[i];
        
        CGPoint childP = [self convertPoint:point toView:childView];
        UIView *fitView = [childView hitTest:childP withEvent:event];
        if (fitView) { 
            return fitView;
        }
    }
    
    return self;
}


```

遍历是从后往前的顺序，即先遍历最底部的父视图。所以当父视图的 userInteractionEnabled 为 NO 时，子视图无法寻找最合适的 view，无法做出响应。

#### pointInside

判断一个点是否在触摸范围内。这个方法可以用来扩展按钮的点击范围：

```
- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent*)event {
    CGRect bounds = self.bounds;
     bounds = CGRectInset(bounds, -10, -10);
   
    return CGRectContainsPoint(bounds, point);
}


```

也可以为不规则的按钮定制点击区域：

```
- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event {
    
    
    CGRect bounds = CGRectInset(self.bounds, -20, -20);
    NSLog(@"point = %@",NSStringFromCGPoint(point));
    UIBezierPath *path1 = [UIBezierPath bezierPathWithRect:CGRectMake(-20, 0, 40, 120)];
    UIBezierPath *path2 = [UIBezierPath bezierPathWithRect:CGRectMake(self.frame.size.width - 20, 0, 40, 120)];
    if (([path1 containsPoint:point] || [path2 containsPoint:point])&& CGRectContainsPoint(bounds, point)){
        
        return YES;
    }
    return NO;
}


```

### 响应者链

响应者链是从最合适的 view 开始，将处理事件传递给下一个响应者。响应者链的传递方法是事件传递的反方法，如果所有响应者都不处理，则事件被丢弃。通常用 UIResponder 的 nextResponder 方法来寻找上级响应者。

下图响应者链来自 [官网](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.apple.com%2Fdocumentation%2Fuikit%2Ftouches_presses_and_gestures%2Fusing_responders_and_the_responder_chain_to_handle_events%3Flanguage%3Dobjc)：

![](http://upload-images.jianshu.io/upload_images/1910830-4098123368ab45ae.png)

响应者链. png

也可以通过下面代码查看响应者链：

```
- (void)viewDidLoad {
    [super viewDidLoad];

    UIButton *b = [UIButton new];
    b.frame = CGRectMake(100, 100, 100, 100);
    b.backgroundColor = [UIColor redColor];
    [b addTarget:self action:@selector(clickedBtn:) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:b];
}

-(void)clickedBtn: (UIButton *)btn {
    UIResponder *res = btn.nextResponder;
    while (res != nil) {
        NSLog(@"res = %@",res);
        res = res.nextResponder;
    }
    NSLog(@"结束");
}


```

用文字来表述执行流程为：

1.  如果 hitTest view 或者 first responder 不处理此事件，则将事件传递给其 nextResponder 处理。
2.  重复步骤 1，直到找到视图层级的顶级视图 UIWindow 对象。如果 window 仍不处理此事件，则传递给 UIApplication。
3.  如果 UIApplication 对象不处理此事件，则事件被丢弃。

下面通过完整的图来感受下：

![](http://upload-images.jianshu.io/upload_images/1910830-b5258308a4ba4cf8.png)

iOS 响应者链. png

App 启动时做了什么
-----------

**通过添加环境变量可以打印出 App 的启动时间分析:**

Edit scheme -> Run -> Arguments -> Environment Variables:

*   添加：DYLD_PRINT_STATISTICS，设置为 1。
*   如果需要更详细的信息，那就添加：DYLD_PRINT_STATISTICS_DETAILS，设置为 1。

以下手动 copy 自：[戴铭 iOS 开发高手课 02 | App 启动速度怎么做优化与监控？](https://links.jianshu.com/go?to=https%3A%2F%2Ftime.geekbang.org%2Fcolumn%2Farticle%2F85331)

这里不对优化做过多细节分析。主要了解 APP 从点击到渲染出来第一个页面，中间做了什么。

### App 启动类型

一般情况下，App 的启动分为冷启动和热启动：

*   冷启动是指，App 点击，它的进程不在系统里，需要系统创建一个进程分配给它启动的情况。这是一次完整的启动过程。
*   热启动是指，App 在冷启动后用户将 App 退入后台，在 App 的进程还在系统里的情况下，用户重新启动进入 App 的过程，这个过程做的事情非常少。

这里只展开讲一下 App 冷启动的优化。

一般而言，App 的启动时间，指的是从用户点击 App 开始，到用户看到第一个界面的时间。总得来说，App 的启动主要包括三个阶段：

1.  main() 函数执行前；
2.  main() 函数执行后；
3.  首屏渲染完成后。

整个启动过程示意图，如下所示：

![](http://upload-images.jianshu.io/upload_images/1910830-1de31482d4e088c1.png)

APP 启动过程示意图. png

#### main() 函数执行前

在 main() 函数执行前，系统主要会做以下几件事情：

*   加载可执行文件（App 的 .o 文件的集合）；
*   加载动态链接库，进行 rebase 指针调整和 bind 符号绑定；
*   Objc 运行时的初始处理，包括 Objc 相关类的注册、Category 注册、selector 唯一性检查等；
*   初始化，包括 +load() 方法、attribute((constructor)) 修饰的函数调用、创建 C++ 静态全局变量。

相应的，这个阶段对于启动优化来说，可以做的事情包括：

*   减少动态库加载。每个库本身都有依赖关系，苹果公司建议使用更少的动态库，并且建议在使用动态库的数量较多时，尽量将多个动态库进行合并。数量上，苹果公司最多可以支持 6 个非系统动态库合并为一个。
*   减少加载启动后不会去使用的类或者方法。
*   +load() 方法里的内容可以放到首屏渲染完成后再执行。或使用 +initialize() 方法替换掉。因为在一个 +load() 方法里，进行运行时方法替换会带来 4 毫秒的消耗。不要小看这 4 毫秒，积少成多，执行 +load() 方法对启动速度的影响会越来越大。
*   控制 C++ 全局变量的数量。

#### main() 函数执行后

main() 函数执行后的阶段，指的是从 main() 函数执行开始，到 AppDelegate 的 didFinishLaunchingWithOptions 方法里首屏渲染的相关方法执行完成。

首页业务代码都是要在这个阶段，也就是首屏渲染前执行的，主要包括了：

*   首屏初始化所需配置文件的读写操作；
*   首屏列表大数据的读取；
*   首屏渲染的大量计算等。

很多时候，开发者会把各种初始化工作都放到这个阶段执行，导致渲染完成滞后。**更加优化的开发方式，应该是**从功能上梳理出哪些是首屏渲染必要的初始化功能，哪些是 App 启动必要的初始化功能，而哪些是只需要在对应功能开始使用时才需要初始化的。梳理完之后，将这些初始化功能分别放到合适的阶段进行。

#### 首屏渲染完成后

首屏渲染后的这个阶段，主要完成的是，非首屏其他业务服务模块的初始化、监听的注册、配置文件的读取等。从函数上来看，这个阶段指的就是截止到 didFinishLaunchingWithOptions 方法作用域内执行首屏渲染之后的所有方法执行完成。简单说的话，这个阶段就是从渲染完成时开始，到 didFinishLaunchingWithOptions 方法作用域结束时结束。

这个阶段用户已经能够看到 App 的首页信息了，所以优化的优先级排在最后。但是，那些会卡住主线程的方法还是需要最优先处理的，不然还是会影响到用户后面的交互操作。

"小礼物走一走，来简书关注我"

还没有人赞赏，支持一下

[![](https://upload.jianshu.io/users/upload_avatars/1910830/4fda0dde-72da-4b50-a3a9-7af5340f2e7e.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/webp)](https://www.jianshu.com/u/db56e4cae85b)

总资产 7 (约 0.75 元) 共写了 5.3W 字获得 79 个赞共 29 个粉丝

### 被以下专题收入，发现更多相似内容