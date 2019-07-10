# Swift3-访问修饰词 函数 闭包

> ## 访问限制词

在 Swift 语言中，访问修饰符有五种，分别为 fileprivate，private，internal，public 和 open。   
权限从高到底 open > public > interal > fileprivate > private

* private 访问级别所修饰的属性或者方法只能在当前类里访问(swift4开始 extension 里也可以访问 private 的属性)。
* fileprivate 访问级别所修饰的属性或者方法在当前的 Swift 源文件里可以访问。
* internal 访问级别所修饰的属性或方法在源代码所在的整个模块都可以访问。
* internal 访问级别所修饰的属性或方法在源代码所在的整个模块都可以访问。
	- 如果是框架或者库代码，则在整个框架内部都可以访问，框架由外部代码所引用时，则不可以访问。
	- 如果是 App 代码，也是在整个 App 代码，也是在整个 App 内部可以访问。
	- 默认访问级别，internal修饰符可写可不写
* public 可以被任何人访问。但其他 module 中不可以被重写(override)和继承，而在 module 内可以。
* open 可以被任何人使用，包括 override 和继承


> ## 函数

* 一个完整的函数由 访问修饰符+类型+func+方法名+参数标签+参数名称+返回值 组成
* 面向对象上我们习惯称 方法


```
// 定义 
// 多个参数可以(但最好不要)有相同的参数标签，不可以有相同的参数名称 
// "_"下划线用于隐藏标签 
// 参数可以写进固定值
// 返回值有多个时可以利用元组，也可以用数组，字典的形式
public class func test(_ fruit: String, priceLabel price: Float, count: Int, shop: String = "超市") -> (String, Float, Int) {
        return (fruit, price, count)
 }
 // 使用
 test2("pear", priceLabel: 1.25, count: 1)
```

* 类方法

```
public class func test1() {
	print("类方法")
}
```

* 静态方法

```
public class func test2() {
	print("静态方法")
｝
```

* 实例方法(对象方法)

```
func test3() {
	print("实例方法")
｝
```

* 无参数无返回值(Void而已，省略)

```
func test4() {
	print("无参数无返回值")
｝
```

* 可变形参(可变参数:参数数量不定)

```
func test5(nums: Int...) {
    let sum = nums.reduce(0) { (result, num) -> Int in
        return result + num
    }
    print("\(nums) 的和: \(sum)")
}
test5(nums: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

* 输入输出参数

默认情况下，函数参数是常量。尝试从该函数体内更改函数参数的值会导致编译时错误。这意味着无法更改参数的值。   
通过将inout关键字放在参数类型之前来编写输入输出参数。一个输入输出参数传递进函数的值，可以由函数修改并替换原来的值。   
将变量作为输入输出参数的参数传递。不能传递常量或文字值作为参数，因为不能修改常量和文字。当使用输入输出参数调用函数时在变量的名称前直接放置一个&，以指示该函数可以修改它。   
输入输出参数不能具有默认值，并且可变参数不能标记为inout。  


```
func inoutFunc(argument1: inout Int, argument2: inout Int) {
    let temp = argument1
    argument1 = argument2
    argument2 = temp
}
var inout1 = 1
var inout2 = 2
inoutFunc(argument1: &inout1, argument2: &inout2)
print("inout1: \(inout1), inout2: \(inout2)")
```

### 函数类型

每个函数都有一个特定的函数类型，由函数的参数类型和返回类型组成。形式上表示为：(参数类型) -> 返回值类型。

* 函数类型作为参数类型

可以使用函数类型，作为另一个函数的参数类型。这使得可以为函数调用者保留函数实现的某些方面，以便在调用函数时提供：

```
func testAdd(a:Int, b:Int) -> Int {
    return a + b
}
func testFuncParam(_ addFunction: (Int, Int) -> Int, a:Int, b:Int) {
    print("Result:\(addFunction(a,b))")
}
func testBasic() {
    testFuncParam(testAdd, a: 5 ,b: 7)
}
```

* 函数类型作为返回类型

可以使用函数类型作为另一个函数的返回类型。通过在返回函数的返回箭头（->）之后立即编写完整的函数类型来完成此操作:

```
// 定义个自增函数
func testIncrease(input:Int) -> Int {
    return input + 1
}
// 定义个自减函数
func testReduce(input:Int) -> Int {
    return input - 1
}
// 定义一个返回函数类型的函数
func testFunctionBack(backwards:Bool) -> (Int) -> Int {
    return backwards ? testReduce : testIncrease
}
```

* 嵌套函数

在函数体内定义的函数称为嵌套函数，嵌套函数对外界是隐藏的，但它们的封闭函数仍然可以调用它们。封闭函数也可以返回其嵌套函数，以允许嵌套函数在另一个范围内使用：

```
func sumFunc() -> (Int...) -> Int {
    func sumInts(nums: Int...) -> Int {
        return nums.reduce(0) { (result, item) -> Int in
            result + item
        }
    }
    return sumInts(nums:)
}
print(sumFunc()(1, 2, 3, 4, 5))
```

* 构造函数和析构函数

```
// 属性
var name: String
// 构造函数(初始化)
init(newName:String){
    self.name = newName
    print("My name is \(newName)")
}
// 析构函数(反初始化，功能跟oc的dealloc一样做善后)
deinit {
    name = ""
}
```

* class与staitc关键字的区别与使用

	- static 可以在类、结构体、枚举、扩展中使用。而 class 只能在类中使用。   
	- static 可以修饰存储属性，static 修饰的存储属性称为静态变量(常量)。而 class 不能修饰存储属性。   
	- static 修饰的计算属性不能被重写。而 class 修饰的可以被重写。   
	- static 修饰的静态方法不能被重写。而 class 修饰的类方法可以被重写。    
	- class 修饰的计算属性被重写时，可以使用 static 让其变为静态属性。    
	- class 修饰的类方法被重写时，可以使用 static 让方法变为静态方法。   

> ## 闭包


