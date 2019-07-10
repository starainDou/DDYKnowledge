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
let a = 1
let b = 2
// 可以不带括号
if a > b {
	print("a>b")
}

if a >= b {
    print("yes")
} else {
    print("no")
}

```

### switch

```
```


