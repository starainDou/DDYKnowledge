Swift 包含了 Objective-C 上所有基本数据类型，另外还增加了高阶数据类型，如元组(Tuple)，还增加了可选类型(Optional)

# 变量与常量声明方式

+ 变量用var声明，如 ``` var name = "LiLei" ```
+ 常量用let声明，如 ``` let home = "BeiJing" ```

# 整型

 + 有符号整型：Int，Int8，Int16，Int32，Int64
 + 无符号整型：UInt，UInt8，UInt16，UInt32，UInt64

一般来说，不需要专门指定整数的长度。Swift中整数类型Int的长度与当前平台的原生字长相同：
> 在32位平台上，Int和Int32长度相同。<br>
> 在32位平台上，UInt和UInt32长度相同。<br>
> 在64位平台上，Int和Int64长度相同。<br>
> 在64位平台上，UInt和UInt64长度相同。<br>

用例
	
```
let age = 18
var count = 10086
count = 1008611
print("\(age) \(count)") 
```

# 浮点型

+ Double: 表示64位浮点数。当你需要存储很大或者很高精度的浮点数时请使用此类型。
+ Float: 表示32位浮点数。精度要求不高的话可以使用此类型。

用例
	
```
let score = 99.5
var velocity = 10.333333333
velocity = 20.666666666
print("\(score) \(velocity)") 
```

# 布尔型

+ Bool: 用来表示逻辑上真(true)与假(false)，但不能用0和非0表示

用例

```
var isSelected = false
isSelected = true
if isSelected {
	print("true")
}
```

# 字符型

+ Character: 一般指单个字符

用例

```
let firstChar = "C"
print("\(firstChar)")
```

# 字符串

+ String: 是字符的序列集合

用例

```
var city = "BeiJing"
let message = "Welcome to \(city)"
city = "ShangHai"
print("city:\(city) message:\(message)")
//city:ShangHai message:Welcome to BeiJing
```

判断是否为空 str.isEmpty <br>
获取字符数量 str.count <br>
判断特定前缀/后缀 str.hasPrefix/str.hasSuffix <br>
大写/小写/首字母大写 str.uppercased()/str.lowercased()/str.capitalized <br>

截取字符串

```
let message = "Welcome to BeiJing"
let prefixStr = message.prefix(3) // 截取前三个字符
let suffixStr = message.suffix(3) // 截取后三个字符
print("\(prefixStr) \(suffixStr)")

let indexStart = message.index(message.startIndex, offsetBy: 3)
let indexEnd = message.index(message.startIndex, offsetBy: 6)
let midStr = message[indexStart..<indexEnd] // 截取
print("\(indexStart) \(midStr)")
```

# 数组

+ Array: 是有序数据的集,分配常量得到不可变数组，分配变量得到可变数组

```
// 一个数组的完成类型为：Array<ElementType>。这里的Element表示存放到该数组中元素的类型，还有一种精简的表示法：Array[ElementType]。
// 声明一个存放整型元素的数组对象array，并对它创建一个空数组
var array1 = Array<Int>()
var array2 = [Int]()
// 声明一个Double类型常量数组，创建10个元素，每个元素都是2.0
let array3 = [Double](repeating: 2.0, count: 10)
// 不过我们更常用的是直接使用数组的字面量[1，2，3，4，5],这样数组的类型为Int。
// 声明一个有4个元素的Double类型数组
let array4 = [1.0, 2.0, 3.0, 4.0]
// 声明一个有3个元素的String类型数组
var array5 = ["none","warning","error"]

var types = ["none","warning","error"]
var menbers = [String]() //声明一个空数组
menbers.append("six")  //添加元素
menbers += ["seven"] //添加元素
menbers.insert("one", at:0)  //指定位置添加元素
menbers[0] = "message"  //通过下标修改数组中的数据
menbers[0...2] = ["message","hangge","com"]  //通过小标区间替换数据（前3个数据）
menbers.count  //获取数组元素个数
menbers.isEmpty  //判断数组是否为空
// 交换元素位置（第2个和第3个元素位置进行交换）
menbers.swapAt(1, 2)
menbers.remove(at: 2)  //删除下标为2的数组
menbers.removeLast()  //删除最后一个元素
menbers.removeAll(keepingCapacity: true)  //删除数组中所有元素
let addStringArr = types + menbers // 数组组合(+号)
// 使用for in 实现数组遍历
for value in menbers{
    print("\(value)");
}
// 通过enumerate函数同时遍历数组的所有索引与数据
for (index,value) in menbers.enumerated() {
    print("索引：\(index) 数据：\(value)");
}
// 过滤数组元素
let newTypes = types.filter { $0.count < 6 } //["none", "error"]
// 创建包含100个元素的数组 ["条目0", "条目1" ... "条目99"]
let items = Array(0..<100).map{ "条目\($0)"}
```

# 字典

+ Dictionary: 是无序的键值对的集,分配常量得到不可变字典，分配变量得到可变字典

# 集合

+ Set: 是无序无重复数据的集,分配常量得到不可变集合，分配变量得到可变集合

# 结构体

+ struct

# 枚举

+ enum

# 可选类型

+ Optional: 用来处理值可能缺失的情况,表示有值或没有值。

# 元组

目前Swift中唯一的一种复合类型，他可以将指定有限个数的任何类型一次整理为一个对象，元组中的每一种类型都可以是任何的结构体、枚举或类类型，甚至也可以是一个元组以及空元组。元组中的每个元素我们称为一个“分量”

```
// 声明一个元组常量tuple，类型为(Int, Double, Bool)
let tuple1 = (10, 0.5, false)
// 访问元组中的第一个元素(索引下标从0开始)
let subTuple1 = tuple1.0

// 我们也可以给元组指定标签，通过访问标签来访问元素
let tuple2 = (age: 18, money: 0.5, isSelected: false)
let subTuple2 = tuple2.age

print("\(subTuple1) \(subTuple2)")
// 10 18
```

注意: 一旦一个元组的某个元素带上标签，那么该元组类型相应的位置上也必须加上标签，一个元组中不需要给所有的元素都加上标签，可以加一部分，指定标签的元素，我们可以通过索引位置访问也可以通过标签访问; 元组往往用于一个函数返回多个不同类型的对象的场合。

我们能很容易的定义一个元组对象，我们也可以很容易的同时萃取一个元组中的相应元素，这也被称为元组分解

```
// 声明一个元组常量tuple
let tuple = (10, 0.5, true)
// 这里同时声明三个常量：a, b, c
// tuple的第一个元素对 a 初始化，b, c同理
let (a1, b1, c1) = tuple
// 上面代码 let (a, b, c) 不是在声明元组，而是同时声明三个常量，用于萃取一个元组中的值，元组也可以看作一个对象，上面的tuple就是名副其实的一个元组对象
// 如果我们不想萃取元组中的某个元素，用_代替
// a的值为：5.5， c的值为：false
let(a2, _, c2) = (5.5, "hello", false)

// 因为元组分解这一语法特性，可以通过元组字面量来交换两个对象的值
// 定义 a和b两个Int类型对象
var a3 = 1, b3 = 2
// 交换两个对象的值，一个特殊用法，不用引入中间变量
(a3, b3) = (b3, a3) 
print("\(a1) \(b1) \(c1) \(a2) \(c2) \(a3) \(b3)")
// 10 0.5 true 5.5 false 2 1
```

元组比较

```
let tupleCompare1 = (1, 2, 3)
let tupleCompare2 = (1, 2, 3)
let tupleCompare3 = (6, 9, 3, 4)
let tupleCompare4 = (6, 2, 5, 8)
let tupleCompare5 = (true, 9, 3, 4)
let tupleCompare6 = (false, 2, 5, 8)
// 判断元组 t1和t2 是否相等，结果为：true
print("t1 == t2? \(tupleCompare1 == tupleCompare2)")
// 判断元组 t3和t4 是否为小于关系，结果为：false 因为t3的第二个元素大于t4的第二个元素
print("t3 < t4? \(tupleCompare3 < tupleCompare4)")
// 这里t1和t3比较，会直接报错，因为不属于同一个类型
// print("tupleCompare1 = tupleCompare3? \(tupleCompare1 == tupleCompare3)")
// 这里的tupleCompare5和tupleCompare6比较也会报错，虽然元组类型一样，因为Bool类型不遵循comparable协议
// print("t5 < t6? \(tupleCompare5 < tupleCompare6)")
```

