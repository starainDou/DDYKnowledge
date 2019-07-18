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
        get{
            return a + b
        }
        set(val){
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


未完待续