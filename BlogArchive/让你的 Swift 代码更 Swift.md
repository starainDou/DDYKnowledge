> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [juejin.cn](https://juejin.cn/post/7028506186743808008)

> 这是我参与 11 月更文挑战的第 5 天，活动详情查看：[2021 最后一次更文挑战](https://juejin.cn/post/7023643374569816095 "https://juejin.cn/post/7023643374569816095")

引言
--

Swift 有很多其他语言所没有的独特的结构和方法，因此很多刚开始接触 Swift 的开发者并没有发挥它本身的优势。

所以，我们就来看一看那些让你的 Swift 代码更 Swift 的写法吧~

有条件的 for 循环
-----------

现在，我们要对`view.subviews`中的`UIButton`做一些不可描述的事情，用 for 循环怎么来遍历呢？

在下面的写法中，更推荐后面两种写法：

```
❌
for subView in view.subviews {
    if let button = subView as? UIButton {
        //不可描述的事情
    }
}

✅
for case let button as UIButton in view.subviews {
    //不可描述的事情
}

✅
for button in view.subviews where button is UIButton {
    //不可描述的事情
}

复制代码

```

enumerated()
------------

在 Swift 中进行 for 循环，要拿到下标值，一般的写法要么`定义局部变量记录下标值`，要么`遍历 0..<view.subviews.count`。其实还有个更方便的写法：`enumerated()`，可以一次性拿到下标值和遍历的元素。

*   ❌ 第一种肯定是不推荐的，因为还要定义额外的局部变量，容易出错，pass
    
*   ✅ 第二种在只需要用到下标值的时候，是可以用的，但如果还要用到下标值对应的元素，就还得再取一次，麻烦，pass
    
*   ✅ 第三种就比较完美，虽然一次性可以拿到下标值和元素，但其中一个用不到就可以用 `_`
    

```
❌
var index: Int = 0
for subView in view.subviews {
    //不可描述的事情
    index += 1
}

✅
for index in 0..<view.subviews.count {
    let subView = view.subviews[index]
    //不可描述的事情
}

✅
//index 和 subView 在循环体中都能使用到
for (index, subView) in view.subviews.enumerated() {
    //不可描述的事情
}

//只用到 index
for (index, _) in view.subviews.enumerated() {
    //不可描述的事情
}

//只用到 subView
for (_, subView) in view.subviews.enumerated() {
    //不可描述的事情
}
复制代码

```

first(where:)
-------------

`filter` 是 Swift 中几个高级函数之一，过滤集合中的元素时非常的好用，不过在某些情况下，比如获取集合中满足条件的第一个元素时，有一个更好的选择`first(where: )`

```
let article1 = ArticleModel(title: "11", content: "内容1", articleID: "11111", comments: [])

let article2 = ArticleModel(title: "11", content: "内容2", articleID: "22222", comments: [])

let article3 = ArticleModel(title: "33", content: "内容3", articleID: "3333", comments: [])

let articles = [article1, article2, article3]

❌
if let article = articles.filter({ $0.articleID == "11111" }).first {
    print("\(article.title)-\(article.content)-\(article.articleID)")
}

✅
if let article = articles.first(where: {$0.articleID == "11111"}) {
    print("\(article.title)-\(article.content)-\(article.articleID)")    //11-内容1-11111
}
复制代码

```

contains(where:)
----------------

这个和上面的`first(where: )`几乎一样，比如这里要判断文章列表里是否包含 articleID 为 11111 的文章：

```
❌
if !articles.filter({ $0.articleID == "11111" }).isEmpty {
    //不可描述的事情
}

✅
if articles.contains(where: { $0.articleID == "11111"}) {
    //不可描述的事情
}
复制代码

```

forEach
-------

当循环体内的逻辑比较简单时，forEach 往往比 for...in... 来的更加简洁：

```
func removeArticleBy(ID: String) {
    //删库跑路
}

❌
for article in articles {
    removeArticleBy(ID: $0.articleID)
}

✅
articles.forEach { removeArticleBy(ID: $0.articleID) }
复制代码

```

计算属性 vs 方法
----------

我们知道计算属性本身不存储数据，而是在 get 中返回计算后的值，在 set 中设置其他属性的值，所以和方法很类似，但比方法更简洁。一起来看下面的示例：

```
❌
class YourManager {
    static func shared() -> YourManager {
        //不可描述的事情
    }
}

let manager = YourManager.shared()

❌
extension Date {
    func formattedString() -> String {
        //不可描述的事情
    }
}

let string = Date().formattedString()


✅
class YourManager {
    static var shared: YourManager {
        //不可描述的事情
    }
}

let manager = YourManager.shared

✅
extension Date {
    var formattedString: String {
        //不可描述的事情
    }
}

let string = Date().formattedString
复制代码

```

协议 vs 子类化
---------

尽量使用协议而不是继承。协议可以让代码更加灵活，因为类可同时遵守多个协议。

此外，结构和枚举不能子类化，但是它们可以遵守协议，这就更加放大了协议的好处

Struct vs Class
---------------

尽可能使用 Struct 而不是 Class。Struct 在多线程环境中更安全，更快。

它们最主要的区别， Struct 是值类型，而 Classe 是引用类型，这意味着 Struct 的每个实例都有它自己的唯一副本，而 Class 的每个实例都有对数据的单个副本的引用。

这个链接是苹果官方的文档，解释如何在 Struct 和 Class 之间做出选择。 [Choosing Between Structures and Classes | Apple Developer Documentation](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.apple.com%2Fdocumentation%2Fswift%2Fchoosing-between-structures-and-classes "https://developer.apple.com/documentation/swift/choosing-between-structures-and-classes")

结语
--

让我们的 Swift 代码更 Swift 的方法远不止上面这些，这里要说的是，平时写代码时，要刻意的使用 Swift 强大的特性，才能发挥它本身的价值。

而这些特性就需要大家去多看看官网的例子，或者一些主流的 Swift 第三方库，看看他们是如何运用 Swift 的特性的。