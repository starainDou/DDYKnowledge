# Swift4-类 属性 协议 范型 扩展

> ## 类

类和结构体区别：

+ 1，类可以继承，结构体不可以
+ 2，可以让一个类的实例来反初始化，释放存储空间，结构体做不到
+ 3，类的对象是引用类型，而结构体是值类型。所以类的赋值是传递引用，结构体则是传值。

相同点：
+ 1，类和结构体都可以扩展
+ 2，定义属性用于储存值
+ 3，定义方法用于提供功能
+ 4，定义下标用于通过下标语法访问值
+ 5，定义初始化器用于生成初始化值

类的构造方法(初始化方法)

```
/********************************
使用默认构造函数
********************************/
class Student{
   	//类属性
   	var name:String = ""
   	var number:Int = 0
}
var student = Student()
 
/********************************
自定义构造函数
********************************/
class Person{
   	//类属性
   	var name:String
   	var age:Int
 
   	//类构造函数
   	init(newName:String, newAge:Int){
   	   	self.name = newName
   	   	self.age = newAge
   	}
 
   	//成员函数（实例方法）
   	func say() -> String{
   	   	return "我叫\(name)"
   	}
}
var p = Person(newName: "hangge",newAge: 32)
print(p.say())
```
类的析构方法(反初始化方法)

```
class DBClass{
   	var conn:Connection? = Connection()
   	deinit{
   	   	//可以做一些清理工作
   	   	self.conn!.close()
   	   	self.conn = nil
   	}
}
 
var db:DBClass? = DBClass()
db = nil //设置nil后即可执行deinit()方法
```

> ## 属性

属性分：计算属性，存储属性，类型属性

* 计算属性

计算（而不是存储）一个值，用于类、结构体和枚举，以及扩展

计算属性，即使用get和set来间接获取/改变其他属性的值      
```
class Calcuator{
   	var a:Int = 1;
   	var b:Int = 1;
 
   	var sum:Int{
   	   	get {
   	   	    return a + b
        }
        set(val) {
   	   	   	b = val - a
   	   	}
   	}
}
let cal = Calcuator();
print(cal.sum) //2
cal.sum = 5
print(cal.b) //4
```

对于set有简写方法，简写时，新赋的值默认为newValue      
```
class Calcuator{
    var a:Int = 1;
    var b:Int = 1;
 
    var sum:Int{
        get{
            return a + b
        }
        set{
            b = newValue - a
        }
    }
}
```     

只读计算属性

只有 getter 没有 setter 的计算属性就是只读计算属性。只读计算属性总是返回一个值，可以通过点(.)运算符访问，但不能设置新的值。

如果只要get，不要set方法时可以简写成如下代码     
```
class Calcuator{
    var a:Int = 1;
    var b:Int = 1;
 
    var sum:Int{
        return a + b
    }
}
```

* 存储属性

存储常量或变量作为实例的一部分，用于类和结构体

存储属性可以是变量存储属性（用关键字var定义），也可以是常量存储属性（用关键字let定义）。        
可以在定义存储属性的时候指定默认值，也可以在构造过程中设置或修改存储属性的值，甚至修改常量存储属性的值

```
struct Number {
   var digits: Int
   let pi = 3.1415
}

var n = Number(digits: 12345)
n.digits = 67

print("\(n.digits)")
print("\(n.pi)")
```

延迟存储属性(懒加载属性)

延迟存储属性是指当第一次被调用的时候才会计算其初始值的属性。在属性声明前使用 lazy var来标示一个延迟存储属性。

```
class sample {
    lazy var no = number() // `var` 关键字是必须的
    lazy var scanBackView: UIImageView = {
        let imageView = UIImageView.init(frame: self.bounds)
        return imageView
    }()
}
```

属性观察器(属性观察者)

属性观察器，类似于触发器。用来监视属性的除初始化之外的属性值变化，当属性值发生改变时可以对此作出响应。有如下特点：
1，不仅可以在属性值改变后触发didSet，也可以在属性值改变前触发willSet。
2，给属性添加观察者必须要声明清楚属性类型，否则编译器报错。
3，willSet可以带一个newName的参数，没有的话，该参数默认命名为newValue。
4，didSet可以带一个oldName的参数，表示旧的属性，不带的话默认命名为oldValue。
5，属性初始化时，willSet和didSet不会调用。只有在初始化上下文之外，当设置属性值时才会调用。
6，即使是设置的值和原来值相同，willSet和didSet也会被调用

可以为除了延迟存储属性(懒加载属性)之外的其他存储属性添加属性观察器，也可以通过重载属性的方式为继承的属性（包括存储属性和计算属性）添加属性观察器。

```
class Samplepgm {
    var counter: Int = 0{
        willSet(newTotal){
            print("计数器: \(newTotal)")
        }
        didSet{
            if counter > oldValue {
                print("新增数 \(counter - oldValue)")
            }
        }
    }
}
let NewCounter = Samplepgm()
NewCounter.counter = 100
NewCounter.counter = 800
/**
计数器: 100
新增数 100
计数器: 800
新增数 700
*/
```

类型属性

类型属性：属于类型固有的，实例不能调用     
使用关键字 static 来定义值类型的类型属性，关键字 class 来为类定义类型属性。

```
struct Structname {
   static var storedTypeProperty = " "
   static var computedTypeProperty: Int {
      // 这里返回一个 Int 值
   }
}

enum Enumname {
   static var storedTypeProperty = " "
   static var computedTypeProperty: Int {
      // 这里返回一个 Int 值
   }
}

class Classname {
   class var computedTypeProperty: Int {
      // 这里返回一个 Int 值
   }
}
```


> ## 协议

 协议规定了用来实现某一特定功能所必需的方法和属性。   
 任意能够满足协议要求的类型被称为遵循这个协议。   
 类，结构体或枚举类型都可以遵循协议，并提供具体实现来完成协议定义的方法和功能。
 
 Swift中协议类似于别的语言里的接口，协议里只做方法的声明，包括方法名、返回值、参数等信息，而没有具体的方法实现。
 
 
	```
 	protocol ddyBasePersonProtocol {
		// 协议中通常用var声明变量属性，然后指明读写权限，不用指定存储型属性还是计算型属性
		var nickName: String { get set }
   		// 只读属性
   		var age: Int { get }
   		// 类型方法
   		static func runMethod()
   		// 实例方法
   		func eatMethod(food: String) -> Bool
   		// 突变方法
   		mutating func changeNickNameMethod(_ newNickName: String)
	}
	// 值类型(协议，结构体，枚举)的实例方法中，将mutating关键字作为函数的前缀，表示可以在该方法实现中修改它所属的实例及其实例属性的值。

	protocol ddyStudentProtocol: ddyBasePersonProtocol {
   		func studyMethod()
	}

	struct student: ddyStudentProtocol {
   		func studyMethod() {
   	   		print("studyMethod")
   		}
   	
   		var nickName: String
   	
   		var age: Int
   	
   		static func runMethod() {
   	   		print("runMethod")
   		}
   	
   		mutating func changeNickNameMethod(_ newNickName: String) {
   	   		self.nickName = newNickName
   		}
   	
   		func eatMethod(food: String) -> Bool {
   	   		return food == "dinner" ? true : false
   		}
	}

	class ProtocolTest: NSObject {
   	
   		public class func testBasic() {
   	   		student.runMethod()
   	   		var liLei = student(nickName: "xiao li", age: 18)
   	   		liLei.studyMethod()
   	   		let isEated = liLei.eatMethod(food: "dinner")
   	   		liLei.changeNickNameMethod("Lao Li")
   	   		print("\(liLei.age) \(liLei.nickName) \(isEated)")
   		}
	}
	// runMethod
	// studyMethod
	// 18 Lao Li true
	// image
	// image
	// image
	```
	
	* 对构造器的规定
	
	协议可以要求它的遵循者实现指定的构造器(在协议的定义里写下构造器的声明，但不需实现)
	
	* 协议类型
	
	尽管协议本身并不实现任何功能，但是协议可以被当做类型来使用。

	协议可以像其他普通类型一样使用，使用场景:

	作为函数、方法或构造器中的参数类型或返回值类型   
	作为常量、变量或属性的类型    
	作为数组、字典或其他容器中的元素类型 

> ## 范型


* Swift 提供了泛型让你写出灵活且可重用的函数和类型。
* Swift 标准库是通过泛型代码构建出来的。
* Swift 的数组和字典类型都是泛型集。

 
	```
	// 交换两个字符串
 	public class func swapTwoString(_ value1: inout String,_ value2: inout String) {
     	(value1, value2) = (value2, value1)
    }
    // 交换两个浮点数
   	public class func swapTwoDouble(_ value1: inout Double,_ value2: inout Double) {
    	(value1, value2) = (value2, value1)
    }
   // 以上函数功能相同只是类型不同，所以使用范型来避免重复代码
   public class func swapTwoValues<T>(_ value1: inout T,_ value2: inout T) {
  		(value1, value2) = (value2, value1)
   }
 	```
 	
 	函数名后面跟着用尖括号括起来占位类型名 T，尖括号告诉Swift编译器那个 T 是函数定义内的一个占位类型名，因此编译器不会去查找名为 T 的实际类型。
 	
 	
 	* 范型类型
 	
 	Swift 允许定义自己的泛型类型
 	
 	```
	// 模仿栈操作
	struct DDYStack<Element> {
   		var items = [Element]()
    	mutating func push(_ item: Element) {
        	items.append(item)
    	}
    	mutating func pop() -> Element {
        	return items.removeLast()
    	}
	} 	

	public class func testGenerics() {
        var stackString = DDYStack<String>()
        stackString.push("one")
        stackString.push("two")
        let removedElement = stackString.pop()
        print("\(stackString) \(removedElement)")
        
        var stackDouble = DDYStack<Double>()
        stackDouble.push(3.14)
        print("\(stackDouble)")
    }
 	```
 	
 	* 扩展泛型类型
 	
 	当用extension扩展一个泛型类型的时候，并不需要在扩展的定义中提供类型参数列表。更加方便的是，原始类型定义中声明的类型参数列表在扩展里是可以使用的，并且这些来自原始类型中的参数名称会被用作原始定义中类型参数的引用。
 	
 	```
 	extension Stack {
    	var topItem: Element? {
       	return items.isEmpty ? nil : items[items.count - 1]
    	}
	}
 	```
	
	* 类型约束
	类型约束指定了一个必须继承自指定类的类型参数，或者遵循一个特定的协议或协议构成。

	* 类型约束语法
	你可以写一个在一个类型参数名后面的类型约束，通过冒号分割，来作为类型参数链的一部分。这种作用于泛型函数的类型约束的基础语法如下所示（和泛型类型的语法相同）：
	
	```
	// 第一个类型参数T(T必须是DDYClass子类的类型约束，第二个参数U(符合DDYProtocol协议的类型约束)
	func ddyFunc<T: DDYClass, U: DDYProtocol>(someT: T, someU: U) {
    	// TODO:
	}
	```
	
	* 关联类型
	
	associatedtype关键字设置关联类型实例
	
	```
	// 遵循该协议就要满足协议内的条件内容
	protocol DDYBaseStackProtocol {
		// 使用associatedtype关键字来设置关联类型实例
    	associatedtype DDYItemType
    	// 添加一个新元素到容器
    	mutating func append(_ item: DDYItemType)
    	// 获取容器中元素个数
    	var count: Int { get }
    	// 通过索引类型下标检索容器中每一个元素
    	subscript(i: Int) -> DDYItemType { get }
	}

	// 模仿栈操作
	struct DDYStack<Element>: DDYBaseStackProtocol {
    	// 原始部分
    	var items = [Element]()
    	mutating func push(_ item: Element) {
        	items.append(item)
    	}
    	mutating func pop() -> Element {
        	return items.removeLast()
    	}
    	// DDYBaseStackProtocol协议实现部分
    	mutating func append(_ item: Element) {
        	self.push(item)
    	}
    	var count: Int {
        	return items.count
    	}
    	subscript(i: Int) -> Element {
        	return items[i]
    	}
	}
	
	// 使用
	var stackInt = DDYStack<Int>()
   	stackInt.push(1)
	stackInt.append(2)
   	print("打印 \(stackInt.count) \(stackInt.items) \(stackInt[1])")
	```
	
> ## 扩展

扩展(extension)就是向一个已有的类、结构体或枚举类型添加新功能，但不能重写已有的功能。

Swift 中的扩展可以：

* 添加计算型属性和计算型静态属性
* 定义实例方法和类型方法
* 提供新的构造器
* 定义下标
* 定义和使用新的嵌套类型
* 使一个已有类型符合某个协议

```
extension MyClass: MyProtocolOne, MyProtocolTwo {
    // 协议实现
}
```

注意：扩展不能直接添加存储属性

间接方式用扩展给类(结构体，枚举)添加存储属性    

```
extension UILabel {

    var edgeInsets: UIEdgeInsets {
        get {
            guard let edgeInsets = objc_getAssociatedObject(self, &edgeInsetsKey) as? UIEdgeInsets else {
                return UIEdgeInsets.init(top: 0, left: 0, bottom: 0, right: 0)
            }
            return edgeInsets
        }
        set {
            objc_setAssociatedObject(self, &edgeInsetsKey, newValue, objc_AssociationPolicy.OBJC_ASSOCIATION_COPY_NONATOMIC)
        }
    }
    
    public static func ddySwizzleMethod() {
        ddySwizzle(#selector(draw(_:)), newSel: #selector(ddyDraw(_:)))
        ddySwizzle(#selector(getter: UILabel.intrinsicContentSize), newSel: #selector(getter: UILabel.ddyIntrinsicContentSize))
    }
    private static func ddySwizzle(_ oldSel: Selector, newSel: Selector) {
        guard let m1 = class_getInstanceMethod(self, oldSel) else {
            return
        }
        guard let m2 = class_getInstanceMethod(self, newSel) else {
            return
        }
        
        if (class_addMethod(self, newSel, method_getImplementation(m2), method_getTypeEncoding(m2))) {
            class_replaceMethod(self, newSel, method_getImplementation(m1), method_getTypeEncoding(m1))
        } else {
            method_exchangeImplementations(m1, m2)
        }
    }
    
    @objc func ddyDraw(_ rect: CGRect) {
        
    }
    
    @objc var ddyIntrinsicContentSize: CGSize {
        get {
            let superSize = super.intrinsicContentSize
            return CGSize(width: superSize.width + edgeInsets.left + edgeInsets.right, height: superSize.height + edgeInsets.top + edgeInsets.bottom)
        }
    }
}
```


[where语句](http://www.hangge.com/blog/cache/detail_1826.html)

类型约束能够确保类型符合泛型函数或类的定义约束。      
在在类型参数列表后面，where语句后跟一个或者多个针对关联类型的约束，以及（或）一个或多个类型和关联类型间的等价(equality)关系。

```

// 扩展的protocol 用于扩展具体功能
protocol DDYBaseExtensionProtocol {
    associatedtype DDYT
    /// 访问原来的value
    var ddyValue: DDYT { get }
}

// 扩展点protocol 作为扩展和访问点
protocol DDYBaseExtensionPoint {
    associatedtype DDYT
    /// 访问扩展功能
    var ddy: DDYT { get }
}

// 扩展基础实现
public final class DDYBaseExtensionImpl<T>: DDYBaseExtensionProtocol {
    
    typealias DDYT = T
    
    public let ddyValue: T
    
    public init(ddyValue: T) {
        self.ddyValue = ddyValue
    }
}

extension String: DDYBaseExtensionPoint {
    var ddy: DDYBaseExtensionImpl<String> {
        return DDYBaseExtensionImpl.init(ddyValue: self)
    }
}

extension DDYBaseExtensionProtocol where DDYT == String {
    internal func ddyTestTestTest() -> String {
        return ddyValue.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines)
    }
}


class ExtensionWhereTest: NSObject {
    
    public class func testBasic() {
        ExtensionWhereTest().testExtensionProtocol()
    }
    
    private func testExtensionProtocol() {
        let testStr: String = "  1234567890  "
        let finalStr = testStr.ddy.ddyTestTestTest()
        print("\(finalStr)")
    }
}
```



 [swift泛型整理](https://www.jianshu.com/p/36448c6312a3)  
	   
 [.](https://www.runoob.com/swift/swift-extensions.html)    
 [.](https://www.jianshu.com/p/325aa6168013)
 


