> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.flutter123.net/Article/Detail/36

本文主要介绍 Flutter 布局中的 FittedBox、AspectRatio、ConstrainedBox，详细介绍了其布局行为以及使用场景，并对源码进行了分析。

1.  FittedBox  
    Scales and positions its child within itself according to fit.

1.1 简介  
按照其官方的介绍，它主要做了两件事情，缩放（Scale）以及位置调整（Position）。

FittedBox 会在自己的尺寸范围内缩放并且调整 child 位置，使得 child 适合其尺寸。做过移动端的，可能会联想到 ImageView 控件，它是将图片在其范围内，按照规则，进行缩放位置调整。FittedBox 跟 ImageView 是有些类似的，可以猜测出，它肯定有一个类似于 ScaleType 的属性。

1.2 布局行为  
FittedBox 的布局行为还算简单，官方没有给出说明，我在这里简单说一下。由于 FittedBox 是一个容器，需要让其 child 在其范围内缩放，因此其布局行为分两种情况：

如果外部有约束的话，按照外部约束调整自身尺寸，然后缩放调整 child，按照指定的条件进行布局；  
如果没有外部约束条件，则跟 child 尺寸一致，指定的缩放以及位置属性将不起作用。  
1.3 继承关系  
Object > Diagnosticable > DiagnosticableTree > Widget > RenderObjectWidget > SingleChildRenderObjectWidget > FittedBox  
从继承关系可以看出，FittedBox 控件是一个基础控件。

1.4 示例代码  
new Container(  
color: Colors.amberAccent,  
width: 300.0,  
height: 300.0,  
child: new FittedBox(  
fit: BoxFit.contain,  
alignment: Alignment.topLeft,  
child: new Container(  
color: Colors.red,  
child: new Text("FittedBox"),  
),  
),  
)  
写了一个很简单的例子，加入 Container 是为了加颜色显示两个区域，读者可以试着修改 fit 以及 alignment 查看其不同的效果。

1.5 源码解析  
const FittedBox({  
Key key,  
this.fit: BoxFit.contain,  
this.alignment: Alignment.center,  
Widget child,  
})  
1.5.1 属性解析  
fit：缩放的方式，默认的属性是 BoxFit.contain，child 在 FittedBox 范围内，尽可能的大，但是不超出其尺寸。这里注意一点，contain 是保持着 child 宽高比的大前提下，尽可能的填满，一般情况下，宽度或者高度达到最大值时，就会停止缩放。

BoxFit 布局表现

alignment：对齐方式，默认的属性是 Alignment.center，居中显示 child。

1.5.2 源码  
构造函数如下：

@override  
RenderFittedBox createRenderObject(BuildContext context) {  
return new RenderFittedBox(  
fit: fit,  
alignment: alignment,  
textDirection: Directionality.of(context),  
);  
}  
FittedBox 具体实现是由 RenderFittedBox 进行的。不知道读者有没有发现，目前的一些基础控件，继承自 RenderObjectWidget 的，widget 本身都只是存储了一些配置信息，真正的绘制渲染，则是由内部的 createRenderObject 所调用的 RenderObject 去实现的。

RenderFittedBox 具体的布局代码如下：

if (child != null) {  
child.layout(const BoxConstraints(), parentUsesSize: true);  
// 如果 child 不为 null，则按照 child 的尺寸比率缩放 child 的尺寸  
size = constraints.constrainSizeAndAttemptToPreserveAspectRatio(child.size);  
_clearPaintData();  
} else {  
// 如果 child 为 null，则按照最小尺寸进行布局  
size = constraints.smallest;  
}  
1.6 使用场景  
FittedBox 在目前的项目中还未用到过。对于需要缩放调整位置处理的，一般都是图片。笔者一般都是使用 Container 中的 decoration 属性去实现相应的效果。对于其他控件需要缩放以及调整位置的，目前还没有遇到使用场景，大家只需要知道有这么一个控件，可以实现这个功能即可。

2.  AspectRatio  
    A widget that attempts to size the child to a specific aspect ratio.

2.1 简介  
AspectRatio 的作用是调整 child 到设置的宽高比，这种控件在其他移动端平台上一般都不会提供，Flutter 之所以提供，我想最大的原因，可能就是自定义起来特别麻烦吧。

2.2 布局行为  
AspectRatio 的布局行为分为两种情况：

AspectRatio 首先会在布局限制条件允许的范围内尽可能的扩展，widget 的高度是由宽度和比率决定的，类似于 BoxFit 中的 contain，按照固定比率去尽量占满区域。  
如果在满足所有限制条件过后无法找到一个可行的尺寸，AspectRatio 最终将会去优先适应布局限制条件，而忽略所设置的比率。  
2.3 继承关系  
Object > Diagnosticable > DiagnosticableTree > Widget > RenderObjectWidget > SingleChildRenderObjectWidget > AspectRatio  
从继承关系看，AspectRatio 是基础的布局控件。

2.4 示例代码  
new Container(  
height: 200.0,  
child: new AspectRatio(  
aspectRatio: 1.5,  
child: new Container(  
color: Colors.red,  
),  
),  
);  
示例代码是定义了一个高度为 200 的区域，内部 AspectRatio 比率设置为 1.5，最终 AspectRatio 的是宽 300 高 200 的一个区域。

2.5 源码解析  
构造函数如下：

const AspectRatio({  
Key key,  
@required this.aspectRatio,  
Widget child  
})  
构造函数只包含了一个 aspectRatio 属性，其中 aspectRatio 不能为 null。

2.5.1 属性解析  
aspectRatio：aspectRatio 是宽高比，最终可能不会根据这个值去布局，具体则要看综合因素，外层是否允许按照这种比率进行布局，只是一个参考值。

2.5.2 源码  
@override  
RenderAspectRatio createRenderObject(BuildContext context) => new RenderAspectRatio(aspectRatio: aspectRatio);  
经过前面一些控件的解析，我想大家对这种构造应该不会再陌生了，绘制都是交由 RenderObject 去完成的，这里则是由 RenderAspectRatio 去完成具体的绘制工作。

RenderAspectRatio 的构造函数中会对 aspectRatio 做一些检测（assert）

aspectRatio 不能为 null；  
aspectRatio 必须大于 0；  
aspectRatio 必须是有限的。  
接下来我们来看一下 RenderAspectRatio 的具体尺寸计算函数：

如果限制条件为 isTight，则返回最小的尺寸（constraints.smallest）；  
if (constraints.isTight)  
return constraints.smallest;  
如果宽度为有限的值，则将高度设置为 width / _aspectRatio。 如果宽度为无限，则将高度设为最大高度，宽度设为 height * _aspectRatio；  
if (width.isFinite) {  
height = width / _aspectRatio;  
} else {  
height = constraints.maxHeight;  
width = height * _aspectRatio;  
}  
接下来则是在限制范围内调整宽高，总体思想则是宽度优先，大于最大值则设为最大值，小于最小值，则设为最小值。  
如果宽度大于最大宽度，则将其设为最大宽度，高度设为 width / _aspectRatio；

if (width> constraints.maxWidth) {  
width = constraints.maxWidth;  
height = width / _aspectRatio;  
}  
如果高度大于最大高度，则将其设为最大高度，宽度设为 height * _aspectRatio；

if (height> constraints.maxHeight) {  
height = constraints.maxHeight;  
width = height * _aspectRatio;  
}  
如果宽度小于最小宽度，则将其设为最小宽度，高度设为 width / _aspectRatio；

if (width < constraints.minWidth) {  
width = constraints.minWidth;  
height = width / _aspectRatio;  
}  
如果高度小于最小高度，则将其设为最小高度，宽度设为 height * _aspectRatio。

if (height < constraints.minHeight) {  
height = constraints.minHeight;  
width = height * _aspectRatio;  
}  
2.6 使用场景  
AspectRatio 适用于需要固定宽高比的情景下。笔者最近使用这个控件的场景是相机，相机的预览尺寸都是固定的几个值，因此不能随意去设置相机的显示区域，必须按照比率进行显示，否则会出现拉伸的情况。除此之外，倒是用的不多。

3.  ConstrainedBox  
    A widget that imposes additional constraints on its child.

3.1 简介  
这个控件的作用是添加额外的限制条件（constraints）到 child 上，本身挺简单的，可以被一些控件替换使用。Flutter 的布局控件体系，梳理着发现确实有点乱，感觉总体思想是缺啥就造啥，哈哈。

3.2 布局行为  
ConstrainedBox 的布局行为非常简单，取决于设置的限制条件，而关于父子节点的限制因素生效优先级，可以查看之前的文章，在这里就不做具体叙述了。

3.3 继承关系  
Object > Diagnosticable > DiagnosticableTree > Widget > RenderObjectWidget > SingleChildRenderObjectWidget > ConstrainedBox  
ConstrainedBox 也是一个基础的布局控件。

3.4 示例代码  
new ConstrainedBox(  
constraints: const BoxConstraints(  
minWidth: 100.0,  
minHeight: 100.0,  
maxWidth: 150.0,  
maxHeight: 150.0,  
),  
child: new Container(  
width: 200.0,  
height: 200.0,  
color: Colors.red,  
),  
);  
例子也挺简单的，在一个宽高 200.0 的 Container 上添加一个约束最大最小宽高的 ConstrainedBox，实际的显示中，则是一个宽高为 150.0 的区域。

3.5 源码解析  
构造函数如下：

ConstrainedBox({  
Key key,  
@required this.constraints,  
Widget child  
})  
包含了一个 constraints 属性，且不能为 null。

3.5.1 属性解析  
constraints：添加到 child 上的额外限制条件，其类型为 BoxConstraints。BoxConstraints 的作用是干啥的呢？其实很简单，就是限制各种最大最小宽高。说到这里插一句，double.infinity 在 widget 布局的时候是合法的，也就说，例如想最大的扩展宽度，可以将宽度值设为 double.infinity。

3.5.2 源码  
@override  
RenderConstrainedBox createRenderObject(BuildContext context) {  
return new RenderConstrainedBox(additionalConstraints: constraints);  
}  
RenderConstrainedBox 实现其绘制。其具体的布局表现分两种情况：

如果 child 不为 null，则将限制条件加在 child 上；  
如果 child 为 null，则会尽可能的缩小尺寸。  
具体代码如下：

@override  
void performLayout() {  
if (child != null) {  
child.layout(_additionalConstraints.enforce(constraints), parentUsesSize: true);  
size = child.size;  
} else {  
size = _additionalConstraints.enforce(constraints).constrain(Size.zero);  
}  
}  
3.6 使用场景  
需要添加额外的约束条件可以使用此控件，例如设置最小宽高，尽可能的占用区域等等。笔者在实际开发中使用的倒不是很多，倒不是说这个控件不好使用，而是好多约束因素是综合的，例如需要额外的设置 margin、padding 属性能，因此单独再套个这个就显得很繁琐了。

3.7 关于 UnconstrainedBox  
这个控件不做详细介绍了，它跟 ConstrainedBox 相反，是不添加任何约束条件到 child 上，让 child 按照其原始的尺寸渲染。

很神奇是吧，我也觉得，其实它的作用就是给 child 一个尽可能大的空间，不加约束的让其显示。用处我暂时木有想到。只能说 Flutter 生产 Widget 很随性。

4.  后话  
    笔者建的一个 Flutter 学习相关的项目，Github 地址，里面包含了笔者写的关于 Flutter 学习相关的一些文章，会定期更新，也会上传一些学习 demo，欢迎大家关注。
    
5.  参考  
    FittedBox class  
    BoxFit enum  
    AspectRatio class  
    ConstrainedBox class  
    BoxConstraints class  
    UnconstrainedBox class