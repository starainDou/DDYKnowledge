# Swift2-运算符和流程控制语句

> ## 运算符

Swift支持大多数标准C运算符，并有所改进。  
 
* 赋值运算符(=)不返回值，以防止在相等运算(==)意图时错误地使用它。
* 算术运算符(+，-，*，/，%等)检测和禁止值溢出，避免数据比类型允许范围更大或更小
* 在面对数值溢出行为可以选择使用溢出运算符，在溢出运算符中描述。
* 提供了在C中不存在的区间运算符，如a..<b和a...b表示一系列值范围的快捷方式。


### 按目标数量分类

按操作数分为:一元运算符、二元运算符、三元运算符(也有习惯成目，如三目运算符)

* 一元运算符 作用于单个目标(在前面则前缀一元运算符，后面则后缀一元运算符)，如：-a !a a!
* 二元运算符 作用于两个目标，是中缀运算符， 如：a+b
* 三元运算符 作用于三个目标(Swift同OC一样只有一个三元运算符 a?b:c)


### 按功能作用分类

* 赋值运算符 =
* 算术运算符 + - * / % (Swift默认禁止值溢出，但可使用溢出符号选择溢出行为: a &+ b，另外+可用于String连接)
* 一元加减运算符 + - (let a = -10086)
* 复合赋值运算符 += -=
* 比较运算符 == != > < >= <= (Swift还提供了恒等运算符 === !==)
* 三元条件运算符 ?: (可以将if else 进行简化)
* Nil-Coalescing运算符 (a??b 表示 (a!=nil)?(a!):b, a!表示必然有值强制解包，a不为nil则不管b，这也称为短路评估)
* 区间运算符 (a...b 区间表示[a,b]，a..<b区间表示[a,b)，单向区间a...和..<b,分别表示a到结尾和起始到b但不包含b)
* 逻辑运算符 ! && || (分别非 且 或)

注：
reversed()结合区间运算符则倒序

```
for i in (0..<10).reversed() {
    print(i)
}
// 9 8 7 6 5 4 3 2 1 0
```

字符串范围结合区间运算符

```
let msg = "www.google.com"
// 不使用区间运算符
let index1 = msg.index(msg.startIndex, offsetBy: 4)
let index2 = msg.index(msg.startIndex, offsetBy: 6)
let range1 = Range(uncheckedBounds: (lower: index1, upper: index2))
let subStr1 = msg.substring(with: range) 
print(subStr1)
// 使用区间运算符
let range2 = msg.index(msg.startIndex, offsetBy: 4)..<msg.index(msg.startIndex, offsetBy: 6)
let subStr2 = msg.substring(with: range2) 
print(subStr2)
```
区间运算符除了返回一个Range外，还可以接受Comparable的输入，返回ClosedInterval或HalfOpenInterval

```
// 我们可以用这个特性检查字符串里的字符是不是都是合法字符（如是不是都是小写）
let words = "Welcome!"
let interval = "a"..."z"
for c in words.characters {
    if !interval.contains(String(c)){
            print("\(c)不是小写字母")
    }
}
// W不是小写字母 !不是小写字母
```

> ## 流程控制语句

### if... (if...else...)

```
public class func testIf() {
        let a = 1
        let b = 2
        // 可以不带括号
        if a < b {
            print("a>b")
        }
    }
let settingURL = URL(string: UIApplication.openSettingsURLString)!
if UIApplication.shared.canOpenURL(settingURL) {
	if #available(iOS 10, *) {
		UIApplication.shared.open(settingURL, options: [:], completionHandler: nil)
	} else {
		UIApplication.shared.openURL(settingURL)
	}
}
```

### switch

```
public class func testSwitch() {
        // Swift中不需在用break跳出switch。若想用C风格的落入特性，需给case分支插入fallthrough语句
        let fruit = "apple"
        switch fruit {
        case "apple": print("good"); fallthrough // 用分号结束写到同一行
        case "banana","orange": print("great")
        default: print("bad")
        }
        // case分支还可以进行区间匹配
        let age = 5
        switch age {
        case 0...11: print("少儿")
        case 12...18: print("少年")
        default: print("其他")
        }
        // case分支同样支持单侧区间匹配
        let num = -5
        switch num {
        case ..<0: print("负数")
        case 0...: print("正数")
        default: print("0")
        }
        // 使用元组匹配（如：判断属于哪个象限）
        let point = (2,2)
        switch point {
        case (0,0): print("坐标在原点")
        case (_,0): print("坐标在x轴上")
        case (0,_): print("坐标在y轴上")
        default: print("在象限区域")
        }
        // case中还可以使用where关键字来做额外的判断条件
        let height = 1.72
        switch height{
        case 1...3 where height == 1.72:  print("case 1")
        case 1...3 where height == 2: print("case 2")
        default: print("default")
        }
        // 值绑定
        let anotherPoint = (2, 0)
        switch anotherPoint {
        case (let x, 0):
            print("on the x-axis with an x value of \(x)")
        case (0, let y):
            print("on the y-axis with a y value of \(y)")
        case let (x, y):
            print("somewhere else at (\(x), \(y))")
        }
        // on the x-axis with an x value of 2
        // 复合案例
        let someCharacter: Character = "e"
        switch someCharacter {
        case "a", "e", "i", "o", "u":
            print("\(someCharacter) is a vowel")
        case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
             "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
            print("\(someCharacter) is a consonant")
        default:
            print("\(someCharacter) is not a vowel or a consonant")
        }
        // e is a vowel
        // 复合案例 结合 值绑定
        let stillAnotherPoint = (9, 0)
        switch stillAnotherPoint {
        case (let distance, 0), (0, let distance):
            print("On an axis, \(distance) from the origin")
        default:
            print("Not on an axis")
        }
        // On an axis, 9 from the origin
    }
```

### for

```
public class func testFor() {
        // C-style for statement has been removed in Swift 3
        //for var i=1; i<100; i++ {
        //  print("\(i)")
        //}
        // for-in
        for chare in "Google" {
            print(chare)
        }
        // forEach
        (1...10).forEach {
            print($0)
        }
    }
```

### 控制转移语句

* continue

continue语句告诉循环停止当前循环不再往下执行，进入下次循环。即“完成了当前的循环迭代”而没有离开循环。

```
public class func testContinue() {
        let startStr = "www.google.com www.apple.com"
        var endStr = ""
        let charactersToRemove: [Character] = ["a", "e", "i", "o", "u", " "]
        for character in startStr {
            if charactersToRemove.contains(character) {
                continue
            }
            endStr.append(character)
        }
        print(endStr)  // www.ggl.cmwww.ppl.cm
    }
```

* break

break语句立即结束整个控制流语句的执行，尽管break在Swift中不需要，但仍可用来中断匹配。

```
public class func testBreak() {
        let numberSymbol: Character = "三"
        var possibleIntegerValue: Int?
        switch numberSymbol {
        case "1", "١", "一", "๑":
            possibleIntegerValue = 1
        case "2", "٢", "二", "๒":
            possibleIntegerValue = 2
        case "3", "٣", "三", "๓":
            possibleIntegerValue = 3
        case "4", "٤", "四", "๔":
            possibleIntegerValue = 4
        default:
            break
        }
        if let integerValue = possibleIntegerValue {
            print("The integer value of \(numberSymbol) is \(integerValue).")
        } else {
            print("An integer value could not be found for \(numberSymbol).")
        }
        // The integer value of 三 is 3.
    }
```

* fallthrough

在Swift中，switch语句不会落入每个案例的底部并进入下一个案例。也就是说，switch一旦第一个匹配的案例完成，整个语句就完成了它的执行。相反，C要求break在每个switch案例的末尾插入一个明确的语句，以防止通过。避免默认的下降意味着Swift switch语句比C中的对应语句更简洁和可预测，因此它们避免switch错误地执行多个案例。
如果需要C样式的直通行为，则可以使用fallthrough关键字逐个选择加入此行为。以下示例fallthrough用于创建数字的文本描述。

* return

用于提前退出，不再执行代码段中代码

```
public class func testReturn(_ fruit:[String: String]) {
        guard let price = fruit["price"] else {
            return
        }
        print("price is \(price)")
    }
```

* throw

### 守护关键字 guard

其实是if...else...变种，满足guard语句的条件则跳过else向下执行，否则执行else中语句   
else分支必须转移控制以退出guard语句出现的代码块,如return，break，continue，或throw，也可以调用一个函数或方法不返回，如fatalError()。

```
public class func testReturn(_ fruit:[String: String]) {
        guard let price = fruit["price"] else {
            return
        }
        guard let name = fruit["name"] else {
            fatalError("致命错误:不存在name键值")
        }
        print("price is \(price)")
    }
```

自定义致命错误fatalError

```
public class func testFatalError(){
        // (1) fatal error发生时，defer是不会执行的
        // (2) catch 不到 fatal error
        defer {
            print("defer here") // 不执行
        }
        do {
            try _throwsMyFatalError() // 产生fatal error
        } catch let err {
            print("in MyFatalError catch section \(err)") // 这一行进不了
        }
    }
    private class func _throwsMyFatalError() throws {
        fatalError("my fatal error here!")
    }
```

### 标签语句

在Swift中，可以在其他循环和条件语句中嵌套循环和条件语句，以创建复杂的控制流结构。但是，循环和条件语句都可以使用break语句过早地结束执行。因此，有时候明确要求break语句终止的循环或条件语句是有用的。类似地，如果有多个嵌套循环，那么明确该continue语句应该影响哪个循环可能很有用。    
要实现这些目标，可以使用语句标签标记循环语句或条件语句。使用条件语句，可以使用带标签语句的break语句来结束带标签语句的执行。使用循环语句，可以使用带有标签语句的break、continue语句来结束或继续执行带标签的语句。       
标签语句通过在语句相同行上关键字前放置标签名来指示，后跟冒号。这是while循环的这种语法的一个例子，然而所有循环和switch语句的原理是相同的：

```
public class func testLabel() {
        let finishIndex = 25
        var currentIndex = 0
        var diceRoll = 0
        gameLoop: while currentIndex != finishIndex {
            diceRoll += 1
            if diceRoll == 7 { diceRoll = 1 }
            switch currentIndex + diceRoll {
            case finishIndex:
                break gameLoop
            case let newSquare where newSquare > finishIndex:
                continue gameLoop
            default:
                currentIndex += diceRoll
            }
        }
    }
```

### 运算符重载

让已有的运算符对自定义的类(结构)进行运算或重新定义已有运算符的运算规则，这种机制被称为运算符重载。

* 通过重载加号运算符，使自定义的两个坐标结构体对象实现相加

```
struct CenterPointer{
    var x=0, y=0
}
 
func + (left:CenterPointer, right:CenterPointer) -> CenterPointer{
    return CenterPointer(x:left.x+right.x, y:left.y+right.y)
}
 
let pointer1 = CenterPointer(x:2, y:3)
let pointer2 = CenterPointer(x:4, y:5)
let pointer3 = pointer1 + pointer2
```

* 重载判断运算符，实现判断自定义类型是否相等

```
func == (left:CenterPointer, right:CenterPointer) -> Bool {
    return (left.x == right.x) && (left.y == right.y)
}
 
func != (left:CenterPointer, right:CenterPointer) -> Bool {
    return !(left == right)
}
```

* 组合运算符，即将其他运算符和赋值运算符组合在一起，注意把运算符左参数设置成inout类型

```
func += (left:inout CenterPointer, right:CenterPointer){
    left = left + right
}
 
var pointer1 = CenterPointer(x:2, y:3)
var pointer2 = CenterPointer(x:4, y:5)
pointer1 += pointer2
```

[extension及fatalError](https://www.cnblogs.com/Jepson1218/p/5277682.html)