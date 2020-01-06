> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.flutter123.net/Article/Detail/35

本文主要介绍 Flutter 布局中的 Padding、Align 以及 Center 控件，详细介绍了其布局行为以及使用场景，并对源码进行了分析。

1.  Padding  
    A widget that insets its child by the given padding.

1.1 简介  
Padding 在 Flutter 中用的也挺多的，作为一个基础的控件，功能非常单一，给子节点设置 padding 属性。写过其他端的都了解这个属性，就是设置内边距属性，内边距的空白区域，也是 widget 的一部分。

Flutter 中并没有单独的 Margin 控件，在 Container 中有 margin 属性，看源码关于 margin 的实现。

if (margin != null)  
current = new Padding(padding: margin, child: current);  
不难看出，Flutter 中淡化了 margin 以及 padding 的区别，margin 实质上也是由 Padding 实现的。

1.2 布局行为  
Padding 的布局分为两种情况：

当 child 为空的时候，会产生一个宽为 left+right，高为 top+bottom 的区域；  
当 child 不为空的时候，Padding 会将布局约束传递给 child，根据设置的 padding 属性，缩小 child 的布局尺寸。然后 Padding 将自己调整到 child 设置了 padding 属性的尺寸，在 child 周围创建空白区域。  
1.3 继承关系  
Object > Diagnosticable > DiagnosticableTree > Widget > RenderObjectWidget > SingleChildRenderObjectWidget > Padding  
从继承关系可以看出，Padding 控件是一个基础控件，不像 Container 这种组合控件。Container 中的 margin 以及 padding 属性都是利用 Padding 控件去实现的。

1.3.1 关于 SingleChildRenderObjectWidget  
SingleChildRenderObjectWidget 是 RenderObjectWidgets 的一个子类，用于限制只能有一个子节点。它只提供 child 的存储，而不提供实际的更新逻辑。

1.3.2 关于 RenderObjectWidgets  
RenderObjectWidgets 为 RenderObjectElement 提供配置，而 RenderObjectElement 包含着（wrap）RenderObject，RenderObject 则是在应用中提供实际的绘制（rendering）的元素。

1.4 示例代码  
实例代码直接上官方的例子，非常的简单：

new Padding(  
padding: new EdgeInsets.all(8.0),  
child: const Card(child: const Text('Hello World!')),  
)  
例子中对 Card 设置了一个宽度为 8 的内边距。

1.5 源码解析  
构造函数如下：

const Padding({  
Key key,  
@required this.padding,  
Widget child,  
})  
包含一个 padding 属性，相当的简单。

1.5.1 属性解析  
padding：padding 的类型为 EdgeInsetsGeometry，EdgeInsetsGeometry 是 EdgeInsets 以及 EdgeInsetsDirectional 的基类。在实际使用中不涉及到国际化，例如适配阿拉伯地区等，一般都是使用 EdgeInsets。EdgeInsetsDirectional 光看命名就知道跟方向相关，因此它的四个边距不限定上下左右，而是根据方向来定的。

1.5.2 源码  
@override  
RenderPadding createRenderObject(BuildContext context) {  
return new RenderPadding(  
padding: padding,  
textDirection: Directionality.of(context),  
);  
}  
Padding 的创建函数，实际上是由 RenderPadding 来进行的。

关于 RenderPadding 的实际布局表现，当 child 为 null 的时候：

if (child == null) {  
size = constraints.constrain(new Size(  
_resolvedPadding.left + _resolvedPadding.right,  
_resolvedPadding.top + _resolvedPadding.bottom  
));  
return;  
}  
返回一个宽为 _resolvedPadding.left+_resolvedPadding.right，高为 _resolvedPadding.top+_resolvedPadding.bottom 的区域。

当 child 不为 null 的时候，经历了三个过程，即调整 child 尺寸、调整 child 位置以及调整 Padding 尺寸，最终达到实际的布局效果。

// 调整 child 尺寸  
final BoxConstraints innerConstraints = constraints.deflate(_resolvedPadding);  
child.layout(innerConstraints, parentUsesSize: true);

// 调整 child 位置  
final BoxParentData childParentData = child.parentData;  
childParentData.offset = new Offset(_resolvedPadding.left, _resolvedPadding.top);

// 调整 Padding 尺寸  
size = constraints.constrain(new Size(  
_resolvedPadding.left + child.size.width + _resolvedPadding.right,  
_resolvedPadding.top + child.size.height + _resolvedPadding.bottom  
));  
到此处，上面介绍的 padding 布局行为就解释的通了。

1.6 使用场景  
Padding 本身还是挺简单的，基本上需要间距的地方，它都能够使用。如果在单一的间距场景，使用 Padding 比 Container 的成本要小一些，毕竟 Container 里面包含了多个 widget。Padding 能够实现的，Container 都能够实现，只不过，Container 更加的复杂。

2.  Align  
    A widget that aligns its child within itself and optionally sizes itself based on the child's size.

2.1 简介  
在其他端的开发，Align 一般都是当做一个控件的属性，并没有拿出来当做一个单独的控件。Align 本身实现的功能并不复杂，设置 child 的对齐方式，例如居中、居左居右等，并根据 child 尺寸调节自身尺寸。

2.2 布局行为  
Align 的布局行为分为两种情况：

当 widthFactor 和 heightFactor 为 null 的时候，当其有限制条件的时候，Align 会根据限制条件尽量的扩展自己的尺寸，当没有限制条件的时候，会调整到 child 的尺寸；  
当 widthFactor 或者 heightFactor 不为 null 的时候，Aligin 会根据 factor 属性，扩展自己的尺寸，例如设置 widthFactor 为 2.0 的时候，那么，Align 的宽度将会是 child 的两倍。  
Align 为什么会有这样的布局行为呢？原因很简单，设置对齐方式的话，如果外层元素尺寸不确定的话，内部的对齐就无法确定。因此，会有宽高因子、根据外层限制扩大到最大尺寸、外层不确定时调整到 child 尺寸这些行为。

2.3 继承关系  
Object > Diagnosticable > DiagnosticableTree > Widget > RenderObjectWidget > SingleChildRenderObjectWidget > Align  
可以看出，Align 跟 Padding 一样，也是一个非常基础的组件，Container 中的 align 属性，也是使用 Align 去实现的。

2.4 示例代码  
new Align(  
alignment: Alignment.center,  
widthFactor: 2.0,  
heightFactor: 2.0,  
child: new Text("Align"),  
)  
例子依旧很简单，设置一个宽高为 child 两倍区域的 Align，其 child 处在正中间。

2.5 源码解析  
const Align({  
Key key,  
this.alignment: Alignment.center,  
this.widthFactor,  
this.heightFactor,  
Widget child  
})  
Align 的构造函数基本上就是宽高因子、对齐方式属性。日常使用中，宽高因子属性基本上用的不多。如果是复杂的布局，Container 内部的 align 属性也可以实现相同的效果。

2.5.1 属性解析  
alignment：对齐方式，一般会使用系统默认提供的 9 种方式，但是并不是说只有这 9 种，例如如下的定义。系统提供的 9 种方式只是预先定义好的。

/// The top left corner.  
static const Alignment topLeft = const Alignment(-1.0, -1.0);  
Alignment 实际上是包含了两个属性的，其中第一个参数，-1.0 是左边对齐，1.0 是右边对齐，第二个参数，-1.0 是顶部对齐，1.0 是底部对齐。根据这个规则，我们也可以自定义我们需要的对齐方式，例如

/// 居右高于底部 1/4 处.  
static const Alignment rightHalfBottom = alignment: const Alignment(1.0, 0.5),  
widthFactor：宽度因子，如果设置的话，Align 的宽度就是 child 的宽度乘以这个值，不能为负数。

heightFactor：高度因子，如果设置的话，Align 的高度就是 child 的高度乘以这个值，不能为负数。

2.5.2 源码  
@override  
RenderPositionedBox createRenderObject(BuildContext context) {  
return new RenderPositionedBox(  
alignment: alignment,  
widthFactor: widthFactor,  
heightFactor: heightFactor,  
textDirection: Directionality.of(context),  
);  
}  
Align 的实际构造调用的是 RenderPositionedBox。

RenderPositionedBox 的布局表现如下：

// 根据 _widthFactor、_heightFactor 以及限制因素来确定宽高  
final bool shrinkWrapWidth = _widthFactor != null || constraints.maxWidth == double.infinity;  
final bool shrinkWrapHeight = _heightFactor != null || constraints.maxHeight == double.infinity;

if (child != null) {  
// 如果 child 不为 null，则根据规则设置 Align 的宽高，如果需要缩放，则根据 _widthFactor 是否为 null 来进行缩放，如果不需要，则尽量扩展。 child.layout(constraints.loosen(), parentUsesSize: true); size = constraints.constrain(new Size(shrinkWrapWidth ? child.size.width * (_widthFactor ?? 1.0) : double.infinity,  
shrinkWrapHeight ? child.size.height * (_heightFactor ?? 1.0) : double.infinity));  
alignChild();  
} else {  
// 如果 child 为 null，如果需要缩放，则变为 0，否则就尽量扩展  
size = constraints.constrain(new Size(shrinkWrapWidth ? 0.0 : double.infinity,  
shrinkWrapHeight ? 0.0 : double.infinity));  
}  
2.6 使用场景  
一般在对齐场景下使用，例如需要右对齐或者底部对齐之类的。它能够实现的功能，Container 都能实现。

3.  Center  
    Center 继承自 Align，只不过是将 alignment 设置为 Alignment.center，其他属性例如 widthFactor、heightFactor，布局行为，都与 Align 完全一样，在这里就不再单独做介绍了。Center 源码如下，没有设置 alignment 属性，是因为 Align 默认的对齐方式就是居中。

class Center extends Align {  
/// Creates a widget that centers its child.  
const Center({Key key, double widthFactor, double heightFactor, Widget child})  
: super(key: key, widthFactor: widthFactor, heightFactor: heightFactor, child: child);  
}

4.  后话  
    笔者建了一个 flutter 学习相关的项目，github 地址，里面包含了笔者写的关于 flutter 学习相关的一些文章，会定期更新，也会上传一些学习 demo，欢迎大家关注。
    
5.  参考  
    Padding class  
    EdgeInsetsGeometry class  
    EdgeInsets class  
    EdgeInsetsDirectional class  
    RenderPadding class  
    Align class  
    Center class