> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.jianshu.com/p/93b2d495f580

[![](https://upload.jianshu.io/users/upload_avatars/1910830/4fda0dde-72da-4b50-a3a9-7af5340f2e7e.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/db56e4cae85b)

0.5432019.10.31 21:10:31 字数 3,646 阅读 120

前言
--

本篇博客手动 `copy` 自 [iOS 开发：彻底理解 iOS 内存管理（MRC、ARC）](https://www.jianshu.com/p/48665652e4e4)。谢谢原作者的详细分析。

正文
--

#### 一、什么是内存管理

*   程序运行过程中，通常以下行为，会增加程序的内存使用：
    *   创建一个对象
    *   定义一个变量
    *   调用一个函数或者方法
*   而一个设备的内存是有限的，程序能占用的内存也是有限的
*   当程序所占用的内存较多时，系统就会发出内存警告，这时就得回收一些不需要的内存空间。比如回收一些不需要使用的对象、变量等
*   如果程序占用系统内存过大，系统就可能强制关闭程序，造成程序崩溃、闪退现象，影响用户体验

> 综上所述，我们要对内存进行合理的内存分配、清除内存，回收哪些不需要在使用的对象，从而保证程序的稳定性。

那么，哪些对象需要我们进行内存管理呢？

*   任何继承了 `NSObject` 的对象，都需要进行内存管理。
*   而其他非对象类型，如 `int` 、 `char` 、 `float` 、 `double` 、 `struct` 、 `enum` 等，不需要进行内存管理。

这是因为：

*   继承了 `NSObject` 的对象，存储在操作系统到 **堆** 里边。
*   操作系统的 **堆**：一般由程序员分配释放，若程序员不释放，结束时可能由系统回收，分配方式类似数据结构的链表。
*   非 `OC` 对象一般都放在操作系统的 **栈** 里边。
*   操作系统的 **栈**：由操作系统自动分配释放内存，存放函数的参数值、局部变量值等。其操作方式类似数据结构中的 **栈（先进后出）**。

示例：

```
int main(int argc, const char *argv []) {
    @autoreleasepool {
        int a = 10;  
        int b = 20;  
        
        
        Person *p = [[Person alloc] init];
    }
    
    
    return 0;
}


```

![](http://upload-images.jianshu.io/upload_images/1910830-50a2f3806917203c.png)

iOS 内存管理 - 1

> 上述说法过于片面，其实并非所有 继承了 `NSObject` 的对象，存储在操作系统到 **堆** 里边，譬如创建的字符串有时候根据创建方式、位置，也会存储到 **常量区**。

#### 二、内存管理模型

提供给 `Objective-C` 程序员的内存管理模型有一下三种：

*   自动垃圾收集（`iOS` 运行环境不支持）
*   手工引用计数和自动释放池（`MRC`）
*   自动引用计数（`ARC`）

#### 三、 `MRC` 手动管理内存（`Manual Reference Counting`）

##### 1. 引用计数器

系统就是根据一个对象的引用计数器来判断什么时候需要回收一个对象的内存。

*   引用计数器是一个整数
*   从字面意思，可以理解为一个对象被引用的次数。也可以理解为，有多少人正在引用这个对象
*   每个 `OC` 都有自己的引用计数器
*   任何一个对象，创建初始的引用计数都是 1
*   当使用 `alloc` 、`new` 、`copy` 等关键词创建一个对象时，对象的引用计数默认就是 1
*   当没有任何人使用这个对象时，该对象就会被回收，也就是说：
    *   当对象的引用计数为 0 时，该对象的内存地址就会被回收。
    *   如果对象的引用计数不为 0，那么在整个程序运行过程，它占用的内存就不可能被回收（除非整个程序已经退出）

##### 2. 引用计数操作

*   为保证对象的存在，每当创建一个该对象的引用时，需要该对象发送一条 `retain` 消息，使该对象的引用计数 +1。（`retain` 方法返回对象本身）。
*   当不再需要该对象时，通过给对象发送 `release` 消息，可使对象的引用计数器 -1。
*   给对象发送 `retainCount` 消息，可以获得当前的引用计数器值
*   当对象的引用计数器为 0 时，系统就知道该对象已经不被引用了，可以回收该对象的内存了。这时会主动调用对象的 `dealloc` 方法发起整个回收过程。
*   需要注意的是，`release` 并不代表销毁或者回收队形，仅仅是引用计数器 `-1`。

##### 3. `dealloc` 方法

*   当一个对象的引用计数为 0 时，该对象即将被销毁，内存被回收。
*   对象即将被销毁时，系统会给对象发送一条 `dealloc` 消息，因此，也能够通过对象是否调用 `dealloc` 方法来判断是否被销毁。
*   `dealloc` 方法的重写
    *   一般都会重写 `dealloc` 方法，在这里释放相关资源。`dealloc` 就是对象的遗言。
    *   一旦重写了 `dealloc` 方法，就必须调用 `dealloc` 方法，并且放到最后面。
*   注意，不能直接调用 `dealloc` 方法。一旦对象被回收了，其占用的内存就不能使用了，坚持使用会导致程序崩溃，即野指针错误。

##### 4. 野指针和空指针

*   只要一个对象被销毁了，我们就称这个对象是 `僵尸对象（不能再使用的对象）`。
*   当一个指针指向僵尸对象（不可用内存），我们就称这个指针为野指针。
*   只要给一个野指针发送消息，就会引发崩溃。（`EXC_BAD_ACCESS`）
    
    ```
    int main(int argc, const char * argv[]) {
        @autoreleasepool {
            Person *p = [[Person alloc] init]; 
            [p release]; 
            [p release]; 
            [p release];
        }
        return 0;
    }   
    
    
    ```
    
*   为了防止野指针错误，一般情况下，当一个对象被销毁时，我们就将该对象的引用指针设为 `空指针`。
*   空指针
    
    *   没有指向内存空间的指针。里面存的是 `nil` ，也就是 `0`。
    *   给空指针发送消息是没有任何回应的。
    
    ```
    int main(int argc, const char * argv[]) {
        @autoreleasepool {
            Person *p = [[Person alloc] init]; 
            [p release]; 
            p = nil; 
            [p release]; 
            [p release];
        }
        return 0;
    }
    
    
    ```
    

##### 5. 内存管理规律

`单个对象内存管理规律`

*   谁创建，谁 `release`
    
    *   如果通过 `alloc` 、 `new` 、 `copy` 、 `mutableCopy` 来创建一个对象，那么就必须调用 `release` 或者 `autorelase`。
*   谁 `retain` ，谁 `release`
    
    *   只要调用了 `retain`，就必须调用 `release` 。
*   总结一下：
    
    *   有加就有减
    *   曾经让对象的引用计数 `+1` ，就必须在最后将对象的引用计数 `-1` 。

`多个对象内存管理规律`

*   其实就是 `内存管理规律` 的多个使用。

##### 6. `@property` 参数

*   在成员变量前加上 `@property` ，系统就会帮我们生成基本的 `getter/setter` 方法。
*   如果在 `property` 属性用 `retain` 修饰，系统就会帮我们生成 `getter/setter` 方法的内存管理代码，但是仍需要我们自己重写 `dealloc` 代码。
*   如果 `property` 属性用 `assgin` 修饰，系统就不会帮我们生成 `setter` 方法的内存管理代码，仅仅只会生成普通的 `getter/setter` 方法。默认什么都不写就是 `assgin` 。

##### 7. 自动释放池

当我们不在使用一个对象的时候应该将其内存释放掉。但是有时候我们不知道应该何时将其释放，为了解决这个问题， `Objective-C` 提供了 `autorelease` 方法

*   `autorelease` 是一种支持引用计数的内存管理方式，只要给对象发送一条 `autorelease`消息，会将对象放到一个自动释放池中，当自动释放池被销毁时，会对池子里的所有对象做一次 `release` 操作。
    
    > 需要特别注意的是：这里只是发送 `release` 消息，如果对象的引用计数不为 0，该对象依然不会被销毁。
    
*   `autorelease` 方法会返回对象自身，且调用完 `autorelease` 后，对象的计数器不变。

###### 7.1 使用 `autorelease` 有什么好处呢？

*   不在关心对象的释放时间
*   不在关心什么时候调用 `release`

###### 7.2 `autorelease` 的原理实质上是什么？

`autorelease` 实际上只是把 `release` 的调用延迟了，对于每一个 `autorelease` , 系统只是把对象放入了当前的 `autorelease pool` 中，当该 `pool` 被释放时，该 `pool` 中所有对象都会调用 `release` 。

###### 7.3 `autorelease` 的创建方法

*   使用 `NSAutoreleasePool` 来创建：
    
    ```
    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init]; 
    [pool release]; 
    
    
    ```
    
*   使用 `@autoreleasepool` 创建：

###### 7.4 `autorelease` 的使用方法

*   `NSAutoreleasePool` 用法：
    
    ```
    NSAutoreleasePool *autoreleasePool = [[NSAutoreleasePool alloc] init];
    Person *p = [[[Person alloc] init] autorelease];
    [autoreleasePool drain];
    
    
    ```
    
*   `@autoreleasepool` 用法:
    
    ```
    @autoreleasepool
    { 
        Person *p = [[Person new] autorelease];
        
    } 
    
    
    ```
    

###### 7.5 `autorelease` 的注意事项

*   并不是放到自动释放池代码中，就会自动加入自动释放池
    
    ```
    @autoreleasepool {
        
        Person *p = [[Person alloc] init];
        [p run];
    }
    
    
    ```
    
*   在自动释放池的外部调用 `autorelease` 不会被加入到自动释放池中。`autorelease` 是一个方法，只有在自动释放池中调用才有效。
    
    ```
    @autoreleasepool {
    }
    
    Person *p = [[[Person alloc] init] autorelease];
    [p run];
    
    
    @autoreleasepool {
        Person *p = [[[Person alloc] init] autorelease];
    }
    
    
    Person *p = [[Person alloc] init];
    @autoreleasepool {
        [p autorelease];
    }
    
    
    ```
    

###### 7.6 自动释放池的嵌套调用

*   自动释放池是以栈的形式存在。
*   由于栈只有一个入口，所以调用 `autorelease` 时会将对象放到栈顶的自动释放池。栈顶就是离调用 `autorelease` 最近的自动释放池。
    
    ```
    @autoreleasepool { 
        @autoreleasepool {
            @autoreleasepool { 
                Person *p = [[[Person alloc] init] autorelease];
            }
            Person *p = [[[Person alloc] init] autorelease];
        }
    }
    
    
    ```
    
*   自动释放池内不宜放占用内存比较大的对象
    
    *   尽量避免对大内存使用该方法，对这种延迟释放机制，还是尽量少用。
    *   不要把大量循环操作放到一个 `autoreleasepool` 之间，这样会造成内存峰值的上升。
    
    ```
    @autoreleasepool {
        for (int i = 0; i < 99999; ++i) {
            Person *p = [[[Person alloc] init] autorelease];
        }
    }
    
    
    for (int i = 0; i < 99999; ++i) {
        @autoreleasepool {
            Person *p = [[[Person alloc] init] autorelease];
        }
    }
    
    
    ```
    

###### 7.7 `autorelease` 错误用法

*   不要连续使用 `autorelease`
    
    ```
    @autoreleasepool {
        
        Person *p = [[[[Person alloc] init] autorelease] autorelease];
    }
    
    
    ```
    
*   调用 `autorelease` 后又调用 `release`
    
    ```
    @autoreleasepool {
        Person *p = [[[Person alloc] init] autorelease];
        [p release]; 
    }
    
    
    ```
    

##### 8. `MRC` 中避免循环 `retian`

*   不要让 `A` `retain` `B`，`B` `retain` `A`。
*   让其中一方不要做 `retain` 操作即可。
*   当两端互相引用时，应该一端用 `retain`，一端用 `assign` 。

#### 四、`ARC` 自动管理内存（`Automatic Reference Counting`）

*   `Automatic Reference Counting`，自动引用计数，即 `ARC`，`WWDC2011` 和 `iOS5` 所引用的最大变革和最激动人心的变化。`ARC` 是新的 `LLVM3.0` 编译器的一项特性，使用 `ARC`，可以说是一举解决了广大 `iOS` 开发者所憎恶的手动内存管理的麻烦。
*   使用 `ARC` 后，系统会检测出何时需要保持对象，何时需要自动释放对象，何时需要释放对象，编译器会管理好对象的内存，会在合适的地方插入`retain` 、 `release` 、 `autorelease` ，通过生成正确的代码，去自动释放或者保持对象。

##### 4.1 `ARC` 的判断原则

`ARC` 通过判断对象是否被 `强指针` 引用来决定引用计数是否增加。

*   强指针
    *   默认所有对象的指针变量都是强指针
    *   被 `__strong` 修饰的指针
*   弱指针
    *   被 `__weak` 修饰的指针

##### 4.2 `ARC` 的使用

```
int main(int argc, const char * argv[]) {
    
    Person *p = [[Person alloc] init];

    return 0;
}


```

##### 4.3 `ARC` 的注意点

*   不允许调用对象的 `release` 方法
*   不允许调用 `autorelease` 方法
*   重写父类的 `dealloc` 时，不能在调用 `[super deallock]`

##### 4.4 `ARC` 下对象内存管理

*   局部变量释放，对象随之被释放
    
    ```
    int main(int argc, const char * argv[]) {
        @autoreleasepool {
            Person *p = [[Person alloc] init];
        } 
        
        return 0;
    }
    
    
    ```
    
*   清空指针，对象随之被释放
    
    ```
    int main(int argc, const char * argv[]) {
        @autoreleasepool {
            Person *p = [[Person alloc] init];
            p = nil; 
        }
        return 0;
    }
    
    
    ```
    
*   默认所有指针都是强指针
    
    ```
    int main(int argc, const char * argv[]) {
        @autoreleasepool {
            
            Person *p1 = [[Person alloc] init];
            __strong Person *p2 = [[Person alloc] init];
        }
        return 0;
    }
    
    
    ```
    
*   弱指针需要明确说明 (注意：千万不要使用弱指针保存薪创建的对象)
    
    ```
    int main(int argc, const char * argv[]) {
        @autoreleasepool {
            
            __weak Person *p1 = [[Person alloc] init];
        }
        return 0;
    }
    
    
    ```
    

##### 4.5 `ARC` 下多对象内存管理

`ARC` 和 `MRC` 一样，想拥有某个对象必须用强指针保存对象，但是不需要在 `dealloc` 中 `release`。

```
@interface Person : NSObject




@property (nonatomic, strong) Dog *dog;
@end


```

##### 4.6 `ARC` 中的 `@property` 参数

*   `strong`: 用于 `OC` 对象，相当于 `MRC` 中的 `retain`
*   `weak`: 用于 `OC` 对象，相当于 `MRC` 中的 `assign`
*   `assign`: 用于基本数据类型，跟 `MRC` 中 `assign` 一样。

##### 4.7 `ARC` 下循环引用问题

`ARC` 跟 `MRC` 一样，如果 `A` 拥有 `B`，`B` 拥有 `A`，那么必须一方使用弱指针。

```
@interface Person : NSObject
@property (nonatomic, strong) Dog *dog;
@end

@interface Dog : NSObject




@property (nonatomic, weak) Person *owner;
@end


```

### 后记

最近两年一直在使用 `Swift`，对 `OC` 的一些基础知识遗忘了不少，通过这篇博客算是温习一些。接下来打算在细读一遍 《Objective-C 高级编程》，来对 `ARC` 、 `Blocks` 、 `GCD` 做一个系统的复习。

"小礼物走一走，来简书关注我"

还没有人赞赏，支持一下

[![](https://upload.jianshu.io/users/upload_avatars/1910830/4fda0dde-72da-4b50-a3a9-7af5340f2e7e.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/webp)](https://www.jianshu.com/u/db56e4cae85b)

总资产 8 (约 0.80 元) 共写了 5.3W 字获得 84 个赞共 29 个粉丝

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

*   今天看到一篇不错的文章关于 OC 内存管理的, 转载一下与你共享概述我们知道在程序运行过程中要创建大量的对象，和其他高级...
    
*   本文首发于我的个人博客：『不羁阁』 https://bujige.net 文章链接：https://bujige.n...
    
    [![](https://upload.jianshu.io/users/upload_avatars/1877784/4f6fb03e-c9bc-47ad-b2c6-8d60f4a8fb86.jpeg?imageMogr2/auto-orient/strip|imageView2/1/w/48/h/48/format/webp)行走少年郎](https://www.jianshu.com/u/6d09d915f1bf)阅读 148
    
    [![](https://upload-images.jianshu.io/upload_images/1877784-bb4c8de7c285874e.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)](https://www.jianshu.com/p/48665652e4e4)
*   Swift1> Swift 和 OC 的区别 1.1> Swift 没有地址 / 指针的概念 1.2> 泛型 1.3> 类型严谨 对...
    
*   内存管理简介 引用计数器 1. 什么是引用计数器 系统是如何判断什么时候需要回收一个对象所占用的内存? 根据对象的引用...
    
    [![](https://upload-images.jianshu.io/upload_images/16111993-51f7586e27b2edfa.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)](https://www.jianshu.com/p/527b74d87e3b)
*   今天上午我有三节英语课，慌乱中竟记错课表，把五年级的第三节当做了第二节课，拿起书进班就准备讲课，课代表提醒我这...
    
    [![](https://upload-images.jianshu.io/upload_images/17524913-d37beb43dbcd7126.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)](https://www.jianshu.com/p/4d3b8f541fd7)