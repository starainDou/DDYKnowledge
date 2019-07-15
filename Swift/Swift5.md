# Swift5-转型 可选链 其他知识点 

> ## 转型

is 用于检测值的类型，as 用于转换类型。

```
import UIKit

class MyBaseClass {
    var baseName: String
    init(name: String) {
        self.baseName = name
    }
}

class MyAppleClass: MyBaseClass {
    var myPrice: Float
    init(name: String, price: Float) {
        self.myPrice = price
        super.init(name: name)
    }
}

class MyPenClass: MyBaseClass {
    var myColor: UIColor
    init(name: String, color: UIColor) {
        self.myColor = color
        super.init(name: name)
    }
}

class CastingTest: NSObject {

    lazy var myClassArray = [MyAppleClass(name: "富士", price: 2.5),
                             MyPenClass(name: "好得利", color: UIColor.blue),
                             MyAppleClass(name: "金帅", price: 3.3)]

    public class func testBasic() {
        CastingTest().testIs()
        CastingTest().testAs()
    }

    private func testIs() {

        for item in myClassArray {
            if item is MyAppleClass {
                let apple = item as! MyAppleClass
                print("\(apple.baseName) \(apple.myPrice)")
            } else if item is MyPenClass {
                let pen = item as! MyPenClass
                print("\(pen.baseName) \(pen.myColor)")
            }
        }
    }

    private func testAs() {
        // 向下转型，用类型转换操作符(as? 或 as!)
        // 当不确定向下转型是否成功时，用类型转换的条件形式(as?)。
        // 只确定向下转型一定会成功时，才使用强制形式(as!)。当试图向下转型为一个不正确的类型时，强制形式会触发一个运行时错误。
        for item in myClassArray {
            if let apple = item as? MyAppleClass {
                print("\(apple.baseName) \(apple.myPrice)")
            } else if let pen = item as? MyPenClass {
                print("\(pen.baseName) \(pen.myColor)")
            }
        }
    }

}
```

> ## 可选链

通过在属性、方法、或下标脚本的可选值后面放一个问号(?)，即可定义一个可选链。    
   
* 如果目标有值，调用就会成功，返回该值
* 如果目标为nil，调用将返回nil

感叹号（!）强制展开方法，属性，下标脚本可选链

* 如果目标有值，调用就会成功，返回该值
* 如果目标为nil，强制展开则报错

fatal error: unexpectedly found nil while unwrapping an Optional value

> ## 其他知识点


