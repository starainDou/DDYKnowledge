> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.jianshu.com/p/a8fc0dd66cbb

[![](https://upload.jianshu.io/users/upload_avatars/1910830/4fda0dde-72da-4b50-a3a9-7af5340f2e7e.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/db56e4cae85b)

0.1152019.12.24 18:33:34 字数 2,014 阅读 16

RunLoop 参考：深入理解 RunLoop
-----------------------

[ibireme：《深入理解 RunLoop》](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.ibireme.com%2F2015%2F05%2F18%2Frunloop%2F)

### Runloop 的概念

首先，让一个线程随时能处理事件，但是并不退出，这样的模型通常称作 `Event Loop`，如下：

```
funcation loop() {
    initialize();
    do {
        var message = get_next_message();
        process_message(message);
    } while (message != quit);
}


```

实现这种模型的关键点在于：如何管理事件 / 消息，如何让线程在没有消息处理时休眠以避免资源浪费，在消息到来时立刻被唤醒。

`Runloop` 实际上就是一个对象，这个对象管理了其所需要处理的事件和消息，并提供一个入口函数来执行上面的 `Event Loop` 逻辑。线程执行了这个函数后，就会一直处于这个函数内部 “接收消息 -> 等待 -> 处理” 的循环中，直到这个循环结束（如返回 `quit` 消息），函数返回。

`iOS/MacOS` 提供了两个这样的对象：`NSRunLoop` 和 `CFRunLoopRef`。

*   `CFRunLoopRef` 是在 `CoreFoundation` 框架内的，它提供了纯 `C` 的 `API`，所有这些 `API` 都是线程安全的。
*   `NSRunLoop` 是基于 `CFRunLoopRef` 的封装，提供了面向对象的 `API`，但这些 `API` 不是线程安全的。

### RunLoop 与线程的关系

1.  `RunLoop` 是通过 `p_thread` 管理的。苹果不允许直接创建 `RunLoop`，它提供了两个自动获取的方法：`CFRunLoopGetCurrent()` 和 `CFRunLoopGetMain()`。
2.  线程和 `RunLoop` 是一一对应的，其关系保存在一个全局的 `Dictionary` 里。线程刚创建时是没有 `RunLoop` 的，如果不主动获取，那它就会一直没有。`RunLoop` 的创建是在第一次获取时，销毁发生在线程结束时。

### RunLoop 对外的接口

在 `CoreFoundation` 里面关于 `RunLoop` 有五个类：

*   CFRunLoopRef
*   CFRunLoopSourceRef
*   CFRunLoopObserverRef
*   CFRunLoopTimerRef
*   CFRunLoopModeRef

其中， `CFRunLoopModeRef` 类没有对外暴露，只是通过 `CFRunLoopRef` 的接口进行了封装，他们关系如下：

![](http://upload-images.jianshu.io/upload_images/1910830-b41f9d12ac8a95e7.png)

RunLoop_0.png

一个 `RunLoop` 包含若干 `Mode`，每个 `Mode` 又包含若干 `Source/Timer/Observer`。每次调用 `RunLoop` 的主函数时，只能指定其中一个 `Mode`，这个 `Mode` 被称为 `CurrentMode`。如果需要切换 `Mode`，只能退出 `Loop`，再重新指定一个 Mode 进入。这样做主要是为了分割不同组的 `Source/Timer/Observer`，让其互不影响。这也是为啥 `ScrollView` 滑动时，默认 Mode 下计时器停止的原因。

`Source/Timer/Observer` 被统称为 `mode item`，一个 `item` 可以同时加入多个 `mode`。但一个 `item` 被重复加入同一个 `mode` 是不会有效果的。如果一个 `mode` 中一个 `item` 都没有，则 `RunLoop` 会直接退出，不进入循环。

#### CFRunLoopSourceRef

是事件产生的地方。 `Source` 有两个版本： `Source0` 和 `Source1`。

*   `Source0` 只包含一个回调（指针），它并不能主动触发事件。使用时需要先调用 `CFRunLoopSourceSignal(source)`，将这个 `source` 标记为待处理， 然后手动调用 `CFRunLoopWakeUp(runloop)` 来唤醒 `RunLoop` ，让其处理事件。
*   `Source1` 包含了一个 `mach_port` 和一个回调（指针），被用于通过内核和其他线程相互发送消息。这种 `Source` 能主动唤醒 `RunLoop` 的线程。

#### CFRunLoopTimerRef

`CFRunLoopTimerRef` 是基于时间的触发器，它和 `NSTimer` 是 `toll-free bridged` 的，可以混用。其包含一个时间长度和回调（指针）。当其加入到 `RunLoop` 时，`RunLoop` 会注册对应的时间点，当时间点到时，`RunLoop` 被唤醒执行那个回调。

#### CFRunLoopObserverRef

`CFRunLoopObserverRef` 是观察者。 每个 `Observer` 都包含了一个回调（指针），当 `RunLoop` 状态发生变化时，观察者就能通过回调接收到这个变化。可以观测的时间点有以下几个：

```
typedef CF_OPTIONS(CFOptionFlags, CFRunLoopActivity) {
    kCFRunLoopEntry         = (1UL << 0), 
    kCFRunLoopBeforeTimers  = (1UL << 1), 
    kCFRunLoopBeforeSources = (1UL << 2), 
    kCFRunLoopBeforeWaiting = (1UL << 5), 
    kCFRunLoopAfterWaiting  = (1UL << 6), 
    kCFRunLoopExit          = (1UL << 7), 
};


```

### RunLoop 的 Mode

`CFRunLoopMode` 和 CFRunLoop` 的结构如下：

```
struct __CFRunLoopMode {
    CFRuntimeBase _base;
    pthread_mutex_t _lock;  
    CFStringRef _name;
    Boolean _stopped;
    char _padding[3];
    CFMutableSetRef _sources0;
    CFMutableSetRef _sources1;
    CFMutableArrayRef _observers;
    CFMutableArrayRef _timers;
    CFMutableDictionaryRef _portToV1SourceMap;
    __CFPortSet _portSet;
    CFIndex _observerMask;
#if USE_DISPATCH_SOURCE_FOR_TIMERS
    dispatch_source_t _timerSource;
    dispatch_queue_t _queue;
    Boolean _timerFired; 
    Boolean _dispatchTimerArmed;
#endif
#if USE_MK_TIMER_TOO
    mach_port_t _timerPort;
    Boolean _mkTimerArmed;
#endif
#if DEPLOYMENT_TARGET_WINDOWS
    DWORD _msgQMask;
    void (*_msgPump)(void);
#endif
    uint64_t _timerSoftDeadline; 
    uint64_t _timerHardDeadline; 
};


struct __CFRunLoop {
    CFRuntimeBase _base;
    pthread_mutex_t _lock;          
    __CFPort _wakeUpPort;           
    Boolean _unused;
    volatile _per_run_data *_perRunData;              
    pthread_t _pthread;
    uint32_t _winthread;
    CFMutableSetRef _commonModes;
    CFMutableSetRef _commonModeItems;
    CFRunLoopModeRef _currentMode;
    CFMutableSetRef _modes;
    struct _block_item *_blocks_head;
    struct _block_item *_blocks_tail;
    CFTypeRef _counterpart;
};


```

筛选出比较关键的信息，大致如下：

```
struct __CFRunLoopMode {
    CFStringRef _name;            
    CFMutableSetRef _sources0;    
    CFMutableSetRef _sources1;    
    CFMutableArrayRef _observers; 
    CFMutableArrayRef _timers;    
    ...
};
 
struct __CFRunLoop {
    CFMutableSetRef _commonModes;     
    CFMutableSetRef _commonModeItems; 
    CFRunLoopModeRef _currentMode;    
    CFMutableSetRef _modes;           
    ...
};


```

*   苹果公开提供的 Mode 有两个：kCFRunLoopDefaultMode (NSDefaultRunLoopMode) 和 UITrackingRunLoopMode，可以用这两个 Mode Name 来操作其对应的 Mode。
*   可以自定义 Mode。RunLoop 内部 Mode 只能增加，不能减少。

### RunLoop 内部逻辑

RunLoop 内部的逻辑大致如下：

![](http://upload-images.jianshu.io/upload_images/1910830-8ffc5cdfd235981f.png)

RunLoop_1.png

### 苹果用 RunLoop 实现的功能

APP 启动时，系统默认注册了 5 个 Mode:

1.  kCFRunLoopDefaultMode: App 的默认 Mode，通常主线程是在这个 Mode 下运行的。
2.  UITrackingRunLoopMode: 界面跟踪 Mode，用于 ScrollView 追踪触摸滑动，保证界面滑动时不受其他 Mode 影响。
3.  UIInitializationRunLoopMode: 在刚启动 App 时第进入的第一个 Mode，启动完成后就不再使用。
4.  GSEventReceiveRunLoopMode: 接受系统事件的内部 Mode，通常用不到。
5.  kCFRunLoopCommonModes: 这是一个占位的 Mode，没有实际作用。

可以在 [这里](https://links.jianshu.com/go?to=http%3A%2F%2Fiphonedevwiki.net%2Findex.php%2FCFRunLoop) 看到更多的苹果内部的 Mode，但那些 Mode 在开发中就很难遇到了。

RunLoop 参考：iOS 线下分享《RunLoop》by 孙源 @sunnyxx
------------------------------------------

[iOS 线下分享《RunLoop》by 孙源 @sunnyxx](https://links.jianshu.com/go?to=https%3A%2F%2Fv.youku.com%2Fv_show%2Fid_XODgxODkzODI0.html)

### `RunLoop` 机制

为什么要有 `RunLoop`:

*   使程序一直运行并接受用户输入
*   决定程序在何时应该处理哪些 `Event`
*   调用解耦（`Message Quene`）
*   节省 `CPU` 时间

![](http://upload-images.jianshu.io/upload_images/1910830-b602783e265aeafb.png)

RunLoop 机制

*   `RunLoop` 与线程（`Thread`）一一绑定并非说是一个 `Thread` 只能对应一个 `RunLoop`, 而是对应一个在外层的 `RunLoop`,`RunLoop` 可以嵌套使用。
*   `1` 对 `n` 是通过数组结构实现的。

### `CFRunLoopTimer`

我们常用的 `Timer` 相关的，都是基于 `CFRunLoopTimer` 的封装，如：

```
+ (NSTimer *)timerWithTimeInterval:(NSTimeInterval)ti 
invocation:(NSInvocation *)invocation repeats:(BOOL)yesOrNo;
+ (NSTimer *)scheduledTimerWithTimeInterval:(NSTimeInterval)ti 
invocation:(NSInvocation *)invocation repeats:(BOOL)yesOrNo;


```

或者延迟执行：

```
- (void)performSelector:(SEL)aSelector withObject:(nullable id)anArgument
afterDelay:(NSTimeInterval)delay inModes:(NSArray<NSRunLoopMode> *)modes;


```

或者屏幕刷新频率 `CADisplayLink`：

```
+ (CADisplayLink *)displayLinkWithTarget:(id)target selector:(SEL)sel;
- (void)addToRunLoop:(NSRunLoop *)runloop forMode:(NSRunLoopMode)mode;


```

### `CFRunLoopSource`

*   `Source` 是 `RunLoop` 的数据抽象类（Protocol）
*   `RunLoop` 定义了两个 `Version` 的 `Source`，可以从堆栈信息中查看:
    *   `Source0`: 处理 `App` 内部事件、`App` 自己负责管理（触发），如 `UIEvent`，`CFSocket`等。
    *   `Source1`: 由 `RunLoop` 内核管理，`Mach Port` 驱动，如`CFMachPort`、`CFMessagePort` （都是官方文档说的。。。）
    *   如果需要，可以选择一种来实现自己的 `Source`。（这事儿知道就行了）

### `CFRunLoopObserver`

```
typedef CF_OPTIONS(CFOptionFlags, CFRunLoopActivity) {
    kCFRunLoopEntry = (1UL << 0),
    kCFRunLoopBeforeTimers = (1UL << 1),
    kCFRunLoopBeforeSources = (1UL << 2),
    kCFRunLoopBeforeWaiting = (1UL << 5),
    kCFRunLoopAfterWaiting = (1UL << 6),
    kCFRunLoopExit = (1UL << 7),
    kCFRunLoopAllActivities = 0x0FFFFFFFU
};


```

*   向外部报告当前 `RunLoop` 状态的更改。
*   框架中很多机制都由 `CFRunLoopObserver` 触发，如 `CAAnimation`。（其实这个也是猜测，并没有实质文档说明）

#### `Topic`: `CFRunLoopObserver` 与 `Autorelease Pool` 关系

> `UIKit` 通过 `RunLoopObserver` 在 `RunLoop` 循环过程中，对 `Autorelease Pool` 进行 `Pop` 和 `Push` 操作，将这次 `Loop` 中产生的 `Autorelease` 对象释放。视频中作者测试，是在两次 `Sleep` 之间。

### `CFRunLoopMode`

*   `RunLoop` 在同一段时间，只能，并且必须在一种特定的 `Mode` 下 `Run`。
*   更换 `Mode` 时，当前 `Loop` 会停止，然后重新启动 `Loop`。
*   `Mode` 是 `iOS App` 滑动顺畅的关键。
*   可以定制自己的 `Mode`。

1.  `NSDefaultRunLoopMode`: 默认状态，越是空闲时的状态。
2.  `UITrackingRunLoopMode`: 滑动时的状态 `ScrollView`。
3.  `UIInitializationRunLoopMode`: 私有（不可见，堆栈追踪能看到，其他均为猜测）
4.  `NSRunLoopCommonModes`: 包含`NSDefaultRunLoopMode` 和 `UITrackingRunLoopMode` 两种状态。:

#### `Topic`: `UITrackingRunLoopMode` 与 `Timer`

1.  这个方法，是将 `Timer` 加到默认的 `NSDefaultRunLoopMode` 模式下。

```
[NSTimer scheduledTimerWithTimeInterval:1
                                 target:self
                               selector:@selector(timeCount)
                               userInfo:nil
                                repeats:YES];


```

2.  如果存在 `ScrollView`，滑动时，`[NSRunLoop currentRunLoop].currentMode` 从 `NSDefaultRunLoopMode` 改变为 `UITrackingRunLoopMode`，此时，`Timer` 暂停，结束滑动后， `Timer` 继续。如果想要滑动时 `Timer` 正常运行，则将 `Timer` 添加到 `NSRunLoopCommonModes` 模式下即可，即：

```
[[NSRunLoop currentRunLoop] addTimer:self.timer forMode:NSRunLoopCommonModes];


```

#### `Topic`: `RunLoopMode` 的切换

*   滑动前：`NSDefaultRunLoopMode`
*   滑动中：`UITrackingRunLoopMode`
*   滑动结束后：`NSDefaultRunLoopMode`

#### `RunLoop` 与 `GCD`

> 视频中这一块也是在讨论，并没有定论

1.  `GCD` 本身与 `RunLoop` 没有关系。
2.  `GCD` 中 `dispatch` 到 `main queue` 的 `block` 被分发到 `main runloop` 中执行，`dispatch_after` 同理。

### Runloop 的等待与唤醒

*   指定用户唤醒的 `mach_port` 端口。
*   调用 `mach_msg` 监听唤醒端口，被唤醒前，系统内核将这个线程挂起，停留在 `mach_msg_trap` 状态。
*   由另一个线程（或另一个进程中的某个线程）向内核发送这个端口的 `msg` 后，`trap` 状态被唤醒， `Runloop` 继续开始干活。

"小礼物走一走，来简书关注我"

还没有人赞赏，支持一下

[![](https://upload.jianshu.io/users/upload_avatars/1910830/4fda0dde-72da-4b50-a3a9-7af5340f2e7e.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/webp)](https://www.jianshu.com/u/db56e4cae85b)

总资产 7 (约 0.74 元) 共写了 5.3W 字获得 76 个赞共 29 个粉丝

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

*   来源：『深入理解 RunLoop』RunLoop 是 iOS 和 OSX 开发中非常基础的一个概念，这篇文章将从 CFRu...
    
    [![](https://upload-images.jianshu.io/upload_images/117999-116082f8ba9bd2d9.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)](https://www.jianshu.com/p/cc5068e2a3a9)
*   转自 bireme，原地址：https://blog.ibireme.com/2015/05/18/runloop/...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/15-a7ac401939dd4df837e3bbf82abaa2a8.jpg)乜_啊_](https://www.jianshu.com/u/5d07122b5834)阅读 2
    
    [![](https://upload-images.jianshu.io/upload_images/3362328-1e574709ba42cc76.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)](https://www.jianshu.com/p/11c4c47a93af)