> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.cnblogs.com/yangyxd/p/10449614.html

1. Flutter 是什么？
===============

Flutter 是谷歌的移动 UI 框架，可以快速在 iOS 和 Android 上构建高质量的原生用户界面。 Flutter 可以与现有的代码一起工作。在全世界，Flutter 正在被越来越多的开发者和组织使用，并且 Flutter 是完全免费、开源的。

2. Flutter 特性有哪些？
=================

快速开发（毫秒级热重载）

*   绚丽 UI（内建漂亮的质感设计 Material Design 和 Cupertino Widget 和丰富平滑的动画效果和平台感知）
*   响应式 (Reactive，用强大而灵活的 API 解决 2D、动画、手势、效果等难题)
*   原生访问功能
*   堪比原生性能

3. Flutter 和 Dart 的关系是什么？ 
==========================

Flutter 是一个使用 Dart 语言开发的跨平台移动 UI 框架，通过自建绘制引擎，能高性能、高保真地进行移动开发。Dart 囊括了多数编程语言的优点，它更符合 Flutter 构建界面的方式。

4. Dart 语言的特性？
==============

*   Productive（生产力高，Dart 的语法清晰明了，工具简单但功能强大）
*   Fast（执行速度快，Dart 提供提前优化编译，以在移动设备和 Web 上获得可预测的高性能和快速启动。）
*   Portable（易于移植，Dart 可编译成 ARM 和 X86 代码，这样 Dart 移动应用程序可以在 iOS、Android 和其他地方运行）
*   Approachable（容易上手，充分吸收了高级语言特性，如果你已经知道 C++，C 语言，或者 Java，你可以在短短几天内用 Dart 来开发）
*   Reactive（响应式编程）

5. Dart 的一些重要概念？
================

*   在 Dart 中，一切都是对象，所有的对象都是继承自 Object
*   Dart 是强类型语言，但可以用 var 或 dynamic 来声明一个变量，Dart 会自动推断其数据类型, dynamic 类似 c#
*   没有赋初值的变量都会有默认值 null
*   Dart 支持顶层方法，如 main 方法，可以在方法内部创建方法
*   Dart 支持顶层变量，也支持类变量或对象变量
*   Dart 没有 public protected private 等关键字，如果某个变量以下划线（_）开头，代表这个变量在库中是私有的

6. dart 是值传递还是引用传递？
===================

dart 是引用传递的。

7. Widget 和 element 和 RenderObject 之间的关系？
=========================================

*   Widget 是用户界面的一部分, 并且是不可变的。
*   Element 是在树中特定位置 Widget 的实例。
*   RenderObject 是渲染树中的一个对象，它的层次结构是渲染库的核心。

Widget 会被 inflate（填充）到 Element，并由 Element 管理底层渲染树。Widget 并不会直接管理状态及渲染, 而是通过 State 这个对象来管理状态。Flutter 创建 Element 的可见树，相对于 Widget 来说，是可变的，通常界面开发中，我们不用直接操作 Element, 而是由框架层实现内部逻辑。就如一个 UI 视图树中，可能包含有多个 TextWidget(Widget 被使用多次)，但是放在内部视图树的视角，这些 TextWidget 都是填充到一个个独立的 Element 中。Element 会持有 renderObject 和 widget 的实例。记住，Widget 只是一个配置，RenderObject 负责管理布局、绘制等操作。

在第一次创建 Widget 的时候，会对应创建一个 Element， 然后将该元素插入树中。如果之后 Widget 发生了变化，则将其与旧的 Widget 进行比较，并且相应地更新 Element。重要的是，Element 不会被重建，只是更新而已。

8. mixin extends implement 之间的关系?
=================================

继承（关键字 extends）、混入 mixins （关键字 with）、接口实现（关键字 implements）。这三者可以同时存在，前后顺序是 extends -> mixins -> implements。

Flutter 中的继承是单继承，子类重写超类的方法要用 @Override，子类调用超类的方法要用 super。

在 Flutter 中，Mixins 是一种在多个类层次结构中复用类代码的方法。mixins 的对象是类，mixins 绝不是继承，也不是接口，而是一种全新的特性，可以 mixins 多个类，mixins 的使用需要满足一定条件。

9. 使用 mixins 的条件是什么？
====================

因为 mixins 使用的条件，随着 Dart 版本一直在变，这里讲的是 Dart2.1 中使用 mixins 的条件：

mixins 类只能继承自 object  
mixins 类不能有构造函数  
一个类可以 mixins 多个 mixins 类  
可以 mixins 多个类，不破坏 Flutter 的单继承

10. mixin 怎么指定异常类型？
===================

on 关键字可用于指定异常类型。 on 只能用于被 mixins 标记的类，例如 mixins X on A，意思是要 mixins X 的话，得先接口实现或者继承 A。这里 A 可以是类，也可以是接口，但是在 mixins 的时候用法有区别.

on 一个类：

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

```
class A {
  void a(){
    print("a");
  }
}


mixin X on A{
  void x(){
    print("x");
  }
}


class mixinsX extends A with X{
}

```

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

on 的是一个接口： 得首先实现这个接口，然后再用 mix

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

```
class A {
  void a(){
    print("a");
  }
}

mixin X on A{
  void x(){
    print("x");
  }
}

class implA implements A{
  @override
  void a() {}
}

class mixinsX2 extends implA with X{
}

```

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

11. Flutter main future mirotask 的执行顺序? 
========================================

普通代码都是同步执行的，结束后会开始检查 microtask 中是否有任务，若有则执行，执行完继续检查 microtask，直到 microtask 列队为空。最后会去执行 event 队列（future）。

12. Future 和 Isolate 有什么区别？
===========================

future 是异步编程，调用本身立即返回，并在稍后的某个时候执行完成时再获得返回结果。在普通代码中可以使用 await 等待一个异步调用结束。

isolate 是并发编程，Dartm 有并发时的共享状态，所有 Dart 代码都在 isolate 中运行，包括最初的 main()。每个 isolate 都有它自己的堆内存，意味着其中所有内存数据，包括全局数据，都仅对该 isolate 可见，它们之间的通信只能通过传递消息的机制完成，消息则通过端口 (port) 收发。isolate 只是一个概念，具体取决于如何实现，比如在 Dart VM 中一个 isolate 可能会是一个线程，在 Web 中可能会是一个 Web Worker。

13. Stream 与 Future 是什么关系？
==========================

Stream 和 Future 是 Dart 异步处理的核心 API。Future 表示稍后获得的一个数据，所有异步的操作的返回值都用 Future 来表示。但是 Future 只能表示一次异步获得的数据。而 Stream 表示多次异步获得的数据。比如界面上的按钮可能会被用户点击多次，所以按钮上的点击事件（onClick）就是一个 Stream 。简单地说，Future 将返回一个值，而 Stream 将返回多次值。Dart 中统一使用 Stream 处理异步事件流。Stream 和一般的集合类似，都是一组数据，只不过一个是异步推送，一个是同步拉取。

14. Stream 两种订阅模式？
==================

Stream 有两种订阅模式：**单订阅 (single) **和 **多订阅（broadcast）**。单订阅就是只能有一个订阅者，而广播是可以有多个订阅者。这就有点类似于消息服务（Message Service）的处理模式。单订阅类似于点对点，在订阅者出现之前会持有数据，在订阅者出现之后就才转交给它。而广播类似于发布订阅模式，可以同时有多个订阅者，当有数据时就会传递给所有的订阅者，而不管当前是否已有订阅者存在。  
Stream 默认处于单订阅模式，所以同一个 stream 上的 listen 和其它大多数方法只能调用一次，调用第二次就会报错。但 Stream 可以通过 transform() 方法（返回另一个 Stream）进行连续调用。通过 Stream.asBroadcastStream() 可以将一个单订阅模式的 Stream 转换成一个多订阅模式的 Stream，isBroadcast 属性可以判断当前 Stream 所处的模式。

15. await for 如何使用?
===================

await for 是不断获取 stream 流中的数据，然后执行循环体中的操作。它一般用在直到 stream 什么时候完成，并且必须等待传递完成之后才能使用，不然就会一直阻塞。

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

```
Stream<String> stream = new Stream<String>.fromIterable(['不开心', '面试', '没', '过']);
main() async{
  print('上午被开水烫了脚');
  await for(String s in stream){
    print(s);
  }
  print('晚上还没吃饭');
}

```

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

16. Flutter 中的 Widget、State、Context 的核心概念？是为了解决什么问题？
====================================================

**Widget**: 在 Flutter 中，几乎所有东西都是 Widget。将一个 Widget 想象为一个可视化的组件（或与应用可视化方面交互的组件），当你需要构建与布局直接或间接相关的任何内容时，你正在使用 Widget。

**Widget 树**: Widget 以树结构进行组织。包含其他 Widget 的 widget 被称为父 Widget(或 widget 容器)。包含在父 widget 中的 widget 被称为子 Widget。

**Context**: 仅仅是已创建的所有 Widget 树结构中的某个 Widget 的位置引用。简而言之，将 context 作为 widget 树的一部分，其中 context 所对应的 widget 被添加到此树中。一个 context 只从属于一个 widget，它和 widget 一样是链接在一起的，并且会形成一个 context 树。

**State**: 定义了 StatefulWidget 实例的行为，它包含了用于” **交互 / 干预** “Widget 信息的行为和布局。应用于 State 的任何更改都会强制重建 Widget。

这些状态的引入，主要是为了解决多个部件之间的交互和部件自身状态的维护。

17. Widget 的两种类型是什么？
====================

**StatelessWidget**: 一旦创建就不关心任何变化，在下次构建之前都不会改变。它们除了依赖于自身的配置信息（在父节点构建时提供）外不再依赖于任何其他信息。比如典型的 Text、Row、Column、Container 等，都是 StatelessWidget。它的生命周期相当简单：初始化、通过 build() 渲染。

**StatefulWidget**: 在生命周期内，该类 Widget 所持有的数据可能会发生变化，这样的数据被称为 **State**，这些拥有动态内部数据的 Widget 被称为 StatefulWidget。比如复选框、Button 等。State 会与 Context 相关联，并且此关联是永久性的，State 对象将永远不会改变其 Context，即使可以在树结构周围移动，也仍将与该 context 相关联。当 **state 与 context 关联时**，state 被视为**已挂载**。StatefulWidget 由两部分组成，在初始化时必须要在 createState() 时初始化一个与之相关的 State 对象。

18. State 对象的初始化流程？
===================

**initState**() : 一旦 State 对象被创建，initState 方法是第一个（构造函数之后）被调用的方法。可通过重写来执行额外的初始化，如初始化动画、控制器等。重写该方法时，应该首先调用 super.initState()。在 initState 中，无法真正使用 context，因为框架还没有完全将其与 state 关联。initState 在该 State 对象的生命周期内将不会再次调用。

**didChangeDependencies**(): 这是第二个被调用的方法。在这一阶段，context 已经可用。如果你的 Widget 链接到了一个 InheritedWidget 并且 / 或者你需要初始化一些 listeners（基于 context），通常会重写该方法。

**build(BuildContext context)**: 此方法在 didChangeDependencies()、didUpdateWidget() 之后被调用。每次 State 对象更新（或当 InheritedWidget 有新的通知时）都会调用该方法！我们一般都在 build 中来编写真正的功能代码。为了强制重建，可以在需要的时候调用 setState((){...}) 方法。

**dispose()**: 此方法在 Widget 被废弃时调用。可重写该方法来执行一些清理操作（如解除 listeners），并在此之后立即调用 super.dispose()。

19. Widget 唯一标识 Key 有那几种？
=========================

在 flutter 中，每个 widget 都是被唯一标识的。这个唯一标识在 build 或 rendering 阶段由框架定义。该标识对应于可选的 Key 参数，如果省略，Flutter 将会自动生成一个。

在 flutter 中，主要有 4 种类型的 Key：GlobalKey（确保生成的 Key 在整个应用中唯一，是很昂贵的，允许 element 在树周围移动或变更父节点而不会丢失状态）、LocalKey、UniqueKey、ObjectKey。

20. 什么是 Navigator? MaterialApp 做了什么？
====================================

Navigator 是在 Flutter 中负责管理维护页面堆栈的导航器。MaterialApp 在需要的时候，会自动为我们创建 Navigator。Navigator.of(context)，会使用 context 来向上遍历 Element 树，找到 MaterialApp 提供的_NavigatorState 再调用其 push/pop 方法完成导航操作。