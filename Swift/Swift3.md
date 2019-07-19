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

闭包是swift中非常重要的一个知识点,类似于objective-c中的block。   
闭包的本质是代码块，是函数的升级版，函数是有名称、可复用的代码块，闭包则是比函数更加灵活的匿名代码块。     
(函数相当于一个特殊的闭包(有名字的闭包)，或者说闭包是一个特殊的函数(带有自动变量的匿名函数))

由上面括号内语句， 可以归纳三种闭包形式：   

* 全局函数：具名函数，但不捕获任何值
* 嵌套函数：在函数内部嵌套定义具名函数，可捕获包含函数中的值
* 闭包表达式：匿名函数类型实例，不具名的代码块，轻量级语法，可捕获上下文的值

其中闭包表达式才是我们一般意义上说的闭包

Swift的闭包表达式具有干净，清晰的风格，闭包的优势包括：

* 从上下文中推断参数和返回值类型
* 单表达式闭包的隐式返回
* 速记参数名称
* 尾随闭包语法
 
闭包定义的类型

```
{ (参数1，参数2... )->返回值类型 in
     语句块
}
```

闭包表达式以及简化形式：

```
import UIKit
/// 起别名
typealias DDYTestClosureTypealias = (String, Bool) -> Void

class ClosureTest: NSObject {

    public class func testBasic() {
        testTypealias { (city: String, success: Bool) in
            print("\(city) \(success)") // Beijing true
        }
        testParams()
    }

    private class func testTypealias(_ closure: DDYTestClosureTypealias) {
        closure("Beijing", true);
    }

    private class func testParams() {
        let numbersArray = [1, 3, 5, 9, 7, 2, 8, 6, 0]

        // 参数加类型
        let sorted1 = numbersArray.sorted { (num1: Int, num2: Int) -> Bool in
            return num1 > num2 // 降序
        }
        // 参数省略类型(编译器推断类型)
        let sorted2 = numbersArray.sorted { num1, num2 in
            return num1 < num2 // 升序
        }
        // 省略参数名和类型，用$加数字引用每个参数
        let sorted3 = numbersArray.sorted {
            return $1 > $0
        }
        // 如果闭包只包含一行代码，省略return都可以
        let sorted4 = numbersArray.sorted {
            $0 > $1
        }
        // 闭包还可以存储在变量中，类似函数一样调用
        let comparator = {(a: Int, b: Int) in a<b }
        let result = comparator(1, 2)
        print("\(sorted1)")
        print("\(sorted2)")
        print("\(sorted3)")
        print("\(sorted4)")
        print("\(result)")
        // [9, 8, 7, 6, 5, 3, 2, 1, 0]
        // [0, 1, 2, 3, 5, 6, 7, 8, 9]
        // [0, 1, 2, 3, 5, 6, 7, 8, 9]
        // [9, 8, 7, 6, 5, 3, 2, 1, 0]
        // true
    }
}

```

将操作符函数自动推导为函数类型

* 尾随闭包：当闭包表达式为函数最后一个参数，可将其写在括号后。

尾随闭包用于需要将一个很长的闭包表达式作为最后一个参数传递给函数。也就是说如果按时的最后一个参数是闭包，那么在调用它的时候就可以把这个闭包写在括号外面，并紧跟括号，函数的其他参数则仍然写在括号之中

```
private class func testCloseClosure(paramStr: String, paramClosure: (String) -> Void) {
    paramClosure("输出"+paramStr)
}
private class func testCloseClosurePrint() {
    // 普通调用
    testCloseClosure(paramStr: "1 普通调用", paramClosure: { (closureStr) in
    print(closureStr)
    })
    // 尾随闭包
    testCloseClosure(paramStr: "2 尾随闭包") { (closureStr) in
    print(closureStr)
    }
    // 输出1 普通调用
    // 输出2 尾随闭包
}
```

* 自动闭包：不接受任何参数，直接返回表达式的值。允许延迟计算。

```
var cities = ["Beijing","Shanghai","New York", "Paris","London"]
print(cities.count)  //此时的城市数是5

let filter = { cities.removeLast() } //()->String 将闭包赋值给filter，此时还没有执行removeLast()

print(cities.count) //由于上一句并没有执行removeLast()此时的城市数还是5

print("Deleting \(filter())!") //执行了filter的闭包。 显示Deleting London!

print(cities.count) //此时的城市数是4, London被删除掉了
```

函数类型与闭包的变量捕获    
函数类型和闭包可以捕获其所在上下文的任何值：    
* 函数参数    
* 局部变量    
* 对象实例属性    
* 全局变量    
* 类的类型属性   

```
//捕获实例属性
class Rectangle{
      var width = 0
      var length = 0
     
      func getComputHandler()-> ()->Int {
          return {
               return self.width*self.length //这个闭包就捕获了实例属性self.width和self.length
          }
      }
}

//捕获参数或局部变量
func addHandler(step: Int)-> ()->Int{
        var sum = 0
      return{
          sum +=step  //这里捕获了参数step和局部变量sum 
          return sum
      }
}

let addByTen = addHandler(10)

print(addByTen())  //显示结果 10
print(addByTen())  //显示结果 20
print(addByTen())  //显示结果 30
```

如果捕获值生存周期小于闭包对象（参数和局部变量），系统会将被捕获的值封装在一个临时对象里，然后再闭包对象上创建一个对象指针，指向该临时对象。

临时对象和闭包对象之间是强引用关系，生存周期跟随闭包对象。

起别名

```
// 光强检测闭包起别名
typealias DDYQRBrightnessClosure = (_ brightnessValue: Double) -> Void

// 光强检测数据闭包回调
public var brightnessClosure: DDYQRBrightnessClosure?
```

使用闭包需要注意内存管理，当闭包为参数时如果以脱离函数则需要声明为逃逸闭包(类型前加@escaping)

逃逸闭包

当一个闭包作为参数传到一个函数中，但是该闭包要在函数返回之后才被执行，于是就称这样的闭包为逃逸闭包。也就是说闭包逃离了函数的作用域。写法是在这个闭包参数前加一个@escaping用来指明这个闭包是允许逃逸出该函数的。

```
//定义数组，里面的元素都是闭包类型的
    var callBackArray : [(Int)->Void] = []
    //定义一个接收闭包的函数
    private func testEscapingClosure() {
        testEscapingClosureChange()
        print("111: \(closureX)")
        callBackArray.first?(5)
        print("222: \(closureX)")
        let closure = callBackArray.first
        closure?(6)
        print("333: \(closureX)")
    }

    var closureX = 10
    private func testEscapingClosureChange() {
        testEscapingClosureCallBack { (closureParam) in
            self.closureX = self.closureX+closureParam
        }
    }
    private func testEscapingClosureCallBack(callBack:@escaping (Int)-> Void) {
        callBackArray.append(callBack)
    }
```


逃逸闭包实际应用

```
/// 相机权限
///
/// - Parameter closure: 闭包回调
public static func cameraAuth(_ closure: @escaping (Bool) -> Void) -> Void {
    let authStatus = AVCaptureDevice.authorizationStatus(for: AVMediaType.video)
    switch authStatus {
    case .notDetermined:
    AVCaptureDevice.requestAccess(for: AVMediaType.video) { (granted: Bool) in
        DispatchQueue.main.async {
            closure(granted)
        }
    }
        break
    case .authorized:
    DispatchQueue.main.async {
        closure(true)
        }
    break
    default: // case .restricted // case .denied
        DispatchQueue.main.async {
        closure(false)
    }
        break
    }
}
```

[解决循环引用的三种方式](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)

```
// 1、可以使用weak关键字将对象之间的联系变为弱引用
weak var weakself = self

// 2、第一种方式的简化
[weak self]

// 3、使用unowned解决
[unowned self]
// 但是该方法十分危险，要确保数据一定有值，否则会Crash
```



[闭包资料](http://www.cocoachina.com/articles/23496)
[闭包资料](https://www.runoob.com/swift/swift-closures.html)