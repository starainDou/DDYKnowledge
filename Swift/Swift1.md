# Swift1-数据类型 元组 枚举 结构体     

Swift 包含了 Objective-C 上所有基本数据类型，另外还增加了高阶数据类型，如元组(Tuple)，还增加了可选类型(Optional)

#### 变量与常量声明方式

+ 变量用var声明，如 ``` var name = "LiLei" ```
+ 常量用let声明，如 ``` let home = "BeiJing" ```

#### 整型

 + 有符号整型：Int，Int8，Int16，Int32，Int64
 + 无符号整型：UInt，UInt8，UInt16，UInt32，UInt64

	一般来说，不需要专门指定整数的长度。Swift中整数类型Int的长度与当前平台的原生字长相同：
	> 在32位平台上，Int和Int32长度相同。    
	> 在32位平台上，UInt和UInt32长度相同。    
	> 在64位平台上，Int和Int64长度相同。    
	> 在64位平台上，UInt和UInt64长度相同。    

	用例
	
	```
	/// 整型用例 Int and UInt
	public class func testBasic() {
		let age: UInt = 18
		var count: Int = 10086
		count = 1008611
		print("\(age)") // 18
		print("\(count)") // 1008611
	}
	```

	随机数

	arc4random() 这个全局函数会生成10位数的随机整数（UInt32）。其生成的最大值是4294967295（2^32 - 1），最小值为0。

	```
	// 用 arc4random 函数生成一个 1~100 的随机数(包括1和100)
	let temp = Int(arc4random()%100)+1
	// 用 arc4random_uniform 函数生成一个 1~100 的随机数（包括1和100）
	let temp = Int(arc4random_uniform(100))+1
	```

#### 浮点型

+ Double: 表示64位浮点数。当你需要存储很大或者很高精度的浮点数时请使用此类型。
+ Float: 表示32位浮点数。精度要求不高的话可以使用此类型。

	用例
	
	```
	/// 浮点型用例 Float and Double
	public class func testBasic() {
		let score: Float = 99.5
		var velocity: Double = 10.333333333
		velocity = 20.666666666
		print("\(score)") // 99.5
		print("\(velocity)") // 20.666666666
	}
	```
	
	为增强大数可读性，Swift增加了下划线来分隔数字(整数、浮点数)。

	```
	let num1 = 10_000_000_000
	let num2 = 1_000_000.000_000_1
	var num3:Int = 1_0_0_0_1
	```

#### 布尔型

+ Bool: 用来表示逻辑上真(true)与假(false)，但不能用0和非0表示

	用例

	```
	/// 布尔型用例 Bool
	public class func testBasic() {
		var isSelected: Bool = false
		isSelected = true
		if isSelected {
			print("true") // true
		}
	}
	```

#### 字符型

+ Character: 一般指单个字符

	用例

	```
	let firstChar = "C"
	print("\(firstChar)")
	```

#### 字符串

+ String: 是字符的序列集合

	Swift 中 String 与 OC 中 NSString对比：     
	String 是一个结构体，性能更高     
	NSString 是一个 OC 对象，性能略差     
	String 支持直接遍历     

	用例

	```
	/// 字符串用例
	public class func testBasic() {
		var city = "BeiJing"
		// 用\(str) 方式包裹变量常量
		let message = "Welcome to \(city)"
		// 更改city为"ShangHai"但message中仍为"BeiJing"
		city = "ShangHai"
		// 大写
		let changeStr1 = city.uppercased()
		// 小写
		let changeStr2 = city.lowercased()
		// 首字母大写
		let changeStr3 = city.capitalized
		// 判断是否空字符串
		if message.isEmpty == false {
		    // message.count得到字符数量
		    print("\(message.count)") // 18
		    print("\(city)") // ShangHai
		    print(message) // Welcome to BeiJing
		    print(changeStr1) // SHANGHAI
		    print(changeStr2) // shanghai
		    print(changeStr3) // Shanghai
		}
    }
	```

	截取字符串

	```
	/// 取子字符串
	public class func testSubString() {
		let message = "Welcome to BeiJing"
		// 取前三个字符
		let prefixStr = message.prefix(3) // Wel
		// 取后三个字符
		let suffixStr = message.suffix(3) // ing
		// 取限定范围[3..<6]内字符
		let indexStart = message.index(message.startIndex, offsetBy: 3)
		let indexEnd = message.index(message.startIndex, offsetBy: 6)
		let midStr = message[indexStart..<indexEnd] // com
		// Wel ing com
		print("\(prefixStr) \(suffixStr) \(midStr)")
    }
	```

#### 数组

+ Array: 是有序数据的集,分配常量得到不可变数组，分配变量得到可变数组

	数组使用有序列表存储同一类型的多个值。相同的值可以多次出现在一个数组的不同位置中。

	Swift数组与OC数组区别：     
	Array是一个结构体，而不是一个类     
	可以放普通类型     

	用例

	```
	public class func testBasic() {
		// var arrayM = [String]()
		// var arrayM1:[String]
		// var arrayM2 = Array()
		// 一个数组的完成类型为：Array<ElementType>。ElementType表示数组中元素的类型
		let array1 = Array<Int>()
		// 一种精简的表示法：Array[ElementType]
		let array2 = [Int]()
		// 声明一个Double类型常量数组，创建10个元素，每个元素都是2.0
		let array3 = [Double](repeating: 2.0, count: 10)
		// 字面量方式声明一个有4个元素的Int类型数组常量
		let array4 = [1, 2, 3, 4]
		// 声明一个有2个元素的 Any 类型数组常量
		let array5 = [1, "two", true, 1.1] as [Any]
		print("\(array1) \(array2) \(array3) \(array4) \(array5)")
	}
	```

	基本操作

	```
	public class func testHandle() {
		// 声明一个空数组变量（let声明为常量, var声明变量，即可变数组）
		var testArray = [String]()
		// 追加元素 姿势1
		testArray.append("six")
		// 追加元素 姿势2
		testArray += ["seven"]
		// 指定位置添加元素
		testArray.insert("one", at:0)
		// 通过下标修改数组中的数据
		testArray[0] = "message"
		// 通过小标区间替换数据（前3个数据），没有则追加
		testArray[0...2] = ["message","Apple","com"]
		// 交换元素位置
		testArray.swapAt(1, 2)
		// 删除下标为2的数组
		testArray.remove(at: 2)
		// 删除最后一个元素
		testArray.removeLast()
		// 删除数组中所有元素 keepingCapacity：保持最大容量
		testArray.removeAll(keepingCapacity: true)
		// 数组组合
		let addStringArr = testArray + ["1", "2"]
		// 使用for in 实现数组遍历
		for value in addStringArr {
		    print("\(value)");
		}
		// 通过enumerate函数同时遍历数组的所有索引与数据
		for (index, value) in addStringArr.enumerated() {
		    print("index：\(index) data：\(value)");
		}
		// 过滤数组元素(元素长度小于6)
		let newTypes = addStringArr.filter { $0.count < 6 }
		// 创建包含100个元素的数组 ["条目0", "条目1" ... "条目5"]
		let intArray1 = Array(0..<6).map{ "条目\($0)"}
		// 创建1-10连续整数数组 姿势1 闭区间
		let intArray2 = [Int](1...10)
		// 创建1-10连续整数数组 姿势2 半闭半开区间
		let intArray3 = [Int](1..<11)
		// 获取数组元素个数
		let testArrayCount = testArray.count
		// 判断数组是否为空
		if testArray.isEmpty == false {
		    print("\(testArray)")
		    print("\(testArrayCount)")
		    print("\(addStringArr)")
		    print("\(newTypes)")
		    print("\(intArray1)")
		    print("\(intArray2)")
		    print("\(intArray3)")
		}
	}
	```

	注意：数组字面量(例如:[1,2,3])，本身是一个常量，如果我们对数组字面量进行添加等操作，则会引起编译器报错。

#### 字典

+ Dictionary: 是无序的键值对的集,分配常量得到不可变字典，分配变量得到可变字典

	字典是由键值 key:value 对组成的集合     
	字典中的元素之间是无序的     
	字典是由两部分集合构成的，一个是键集合，一个是值集合     
	字典是通过访问键间接访问值的     
	键集合是不能有重复元素的，而值集合是可以重复的     
	Swift中的字典类型是Dictionary，也是一个泛型集合     
	使用let修饰的字典是不可变字典     
	使用var修饰的字典是可变字典     

	用例

	```
	public class func testBasic() {
		// 建立个空字典变量（let声明为常量, var声明变量，即可变字典）
		var fruitPriceDict = [String: Int]() // Dictionary<String, Int>()
		fruitPriceDict = ["apple":10, "pear":9, "banana":8, "peach":11, "strawberry":30, "lemon":1]
		// 声明一个字典变量，其key为String类型 value为Any类型
		var personDict = ["name":"LiLei", "age":18, "nickName":"XiaoLi", "score":100] as [String : Any]
		// 修改key对应value值（不存在则添加）姿势1
		personDict.updateValue("city", forKey: "BeiJing China")
		// 修改key对应value值（不存在则添加）姿势2
		personDict["city"] = "BeiJing"
		// 删除key值及对应value值 姿势1
		personDict.removeValue(forKey: "score")
		// 删除key值及对应value值 姿势2
		personDict["nickName"] = nil
		// 访问字典的key集合
		let keysSet = personDict.keys
		// 访问字典的values数组
		let valueArray = personDict.values
		print("\(keysSet)  \(valueArray)")
	}
	```

	遍历字典

	```
	public class func testEnumerated() {
		var personDict = ["name":"LiLei", "age":18, "nickName":"XiaoLi", "score":100] as [String : Any]
		// 遍历字典 姿势1
		for (key, value) in personDict {
		    print("\(key):\(value)");
		}
		// 遍历字典 姿势2
		for keyAndValue in personDict {
		    print("keyAndValue: \(keyAndValue)")
		}
		// 只遍历字典的键（key）
		for key in personDict.keys {
		    print("\(key)");
		}
		// 只遍历字典的值（value）
		for value in personDict.values {
		    print("\(value)");
		}
	}
	```
    
	过滤和合并
    
	```
 	public class func testFilterAndMerge() {
		// 建立个空字典变量（let声明为常量, var声明变量，即可变字典）
		let fruitPriceDict = ["apple":10, "pear":9, "banana":8, "peach":11, "strawberry":30, "lemon":1]
		// 过滤字典元素
		let fruitPriceDict2 = fruitPriceDict.filter { $0.value < 10 }
		print("\(fruitPriceDict2)")
		// 合并 姿势1
		var dict1 = ["name":"000","age":18,"title":"888"] as [String : Any]
		let dict2 = ["name":"da","hegiht":190] as [String : Any]

		for e in dict2 {
		    dict1[e.key] = dict2[e.key]
		}
		// 如果key存在会修改，key不存在会新增
		print(dict1)
		var dic = ["one": 10, "two": 20]
		// merge方法合并
		let tuples = [("one", 5),  ("three", 30)]
		dic.merge(tuples, uniquingKeysWith: min)
		print("dic：\(dic)")
		// merging方法合并
		let dic2 = ["one": 0, "four": 40]
		let dic3 = dic.merging(dic2, uniquingKeysWith: min)
		print("dic3：\(dic3)")
	}
	```
    
	字典合并

	+ merge(_: uniquingKeysWith:)：这种方法会修改原始Dictionary
	+ merging(_: uniquingKeysWith:)：这种方法会创建并返回一个全新的Dictionary
   
	另类创建方式   
    
	```
	public class func testCreateDictSomeMethod() {
		// 通过元组创建字典
		let tupleKeyValueArray = [("Monday", 30),  ("Tuesday", 25),  ("Wednesday", 27)]
		let dictFromTuple = Dictionary(uniqueKeysWithValues: tupleKeyValueArray)
		print(dictFromTuple) // ["Monday": 30, "Tuesday": 25, "Wednesday": 27]
		// 通过键值序列创建字典
		let keyArrayToDict = ["Apple", "Pear"]
		let valueArrayToDict = [7, 6]
		let keyValueArrayToDict = Dictionary(uniqueKeysWithValues: zip(keyArrayToDict, valueArrayToDict))
		print(keyValueArrayToDict)
		// 用键序列/值序列创建字典
		let arrayKeyOrValue = ["Monday", "Tuesday", "Wednesday"]
		let indexKeyDict = Dictionary(uniqueKeysWithValues: zip(1..., arrayKeyOrValue))
		let indexValueDict = Dictionary(uniqueKeysWithValues: zip(arrayKeyOrValue, 1...))
		print("\(indexKeyDict) \(indexValueDict)")
		// 数组分组成字典（比如下面生成一个以首字母分组的字典）
		let nameGroupArray = ["LiLei", "LiXiaolong", "LiuDehua", "HanMeimei", "HanLei", "SunWukong", "ErLangshen"]
		let dictFromNameGroup = Dictionary(grouping: nameGroupArray) { $0.first! }
		print(dictFromNameGroup)
	}
	```

	重复键处理

	```
	public class func testHandleRepeatKey() {
		// 重复键的处理
		// zip配合速记+可以用来解决重复键的问题（相同的键值相加）
		let array = ["Apple", "Pear", "Pear", "Orange"]
		let dic1 = Dictionary(zip(array, repeatElement(1, count: array.count)), uniquingKeysWith: +)
		print(dic1)

		// 下面使用元组创建字典时，遇到相同的键则取较小的那个值
		let duplicatesArray = [("Monday", 30),  ("Tuesday", 25),  ("Wednesday", 27), ("Monday", 28)]
		let dic2 = Dictionary(duplicatesArray, uniquingKeysWith: min)
		print(dic2)
	}
	```

	默认值以及妙用用来统计字符串中每个单词出现个数

	```
 	public class func testDefaultValue() {
		// swift4之前自己判断并赋值
		let dic1 = ["apple": 1, "banana": 2]
		var orange1:Int
		if let value1 = dic1["orange"] {
		    orange1 = value1
		}else{
		    orange1 = 0
		}
		print(orange1)
		// swift4之后  给定即可
		let dic2 = ["apple": 1, "banana": 2 ]
		let orange2 = dic2["orange", default:0]
		print(orange2)
    }
    // 下面是统计一个字符串中所有单词出现的次数。可以看到了有了默认值，实现起来会简单许多
    public class func testCharacterCountInStr() {
		let str = "apple banana orange apple banana"
		var wordsCount: [String: Int] = [:]
		for word in str.split(separator: " ") {
		    wordsCount["\(word)", default: 0] += 1
		}
		print(wordsCount)
    }
	```

#### 集合

+ Set: 是无序无重复数据的集,分配常量得到不可变集合，分配变量得到可变集合

	一个集合也能存放多个相同类型的元素，与数组不同的是：     
	1:一个集合不允许出现两个相同的元素     
	2:集合中的元素是无序的     
	3:并不是所有的类型对象都能作为集合的元素，不过swift的基本类型都可以     

	```
	public class func testInit() {
		// 一个集合的完整类型为：Set<Element: Hashable>，集合没有精简表示法。
		var set1 = Set<Int>()
		// 通过集合的不定参数个数的构造方法来创建一个含有指定元素的集合
		let set2 = Set<Int>(arrayLiteral: 1, 2, 3, 4)
		// 通过数组字面量来创建一个集合常量
		let set3: Set<Float> = [1.0, 0.5, 2.0]
		// 我们也可以用数组字面量来赋值一个集合
		set1 = [1, 2]
		print("\(set1) \(set2) \(set3)")
	}
	```

	元素集合的访问不能像数组通过索引值，因为它是无序的，可以通过for-in循环，也可以通过flatMap方法，有选择性的将集合元素取出。

	集合的数学操作

	```
	public class func testMathCalculation() {
		let setA: Set<Float> = [1.0, 0.5, 2.0]
		let setB: Set<Float> = [1.0, 1.5, 2.0]
		print(setA.union(setB).sorted()) // 并集 [0.5, 1.0, 1.5, 2.0]
		print(setA.intersection(setB).sorted()) // 交集 [1.0, 2.0]
		print(setA.subtracting(setB).sorted()) // 取差值 [0.5]
		print(setA.symmetricDifference(setB).sorted()) // 去掉相同值 [0.5, 1.5]
	}
	```

	集合遍历

	```
	for item in favorite {
    	print(item)
	}
	//按照首字母的顺序输出
	for item1 in favorite.sorted() {
   		print(item1)
	}
	```

	集合成员关系

	用 ==来判断两个集合是否包含全部相同的值    
	用 isSubset(of:)来判断一个集合中的值是否也被包含在另外一个集合中    
	用 isSuperset（of:）来判断一个集合中包含另一个集合所有的值   
	用isStrictSubset(of:)或者isStrictSuperset(of:)方法来判断一个集合是否是另外一个集合的子集合	或父集合并且两个集合不相等    

#### 结构体

+ struct

	```
	//创建一个结构体
	struct BookInfo{
    	var ID:Int = 0
    	var Name:String = "Defaut"
    	var Author:String = "Defaut"
	}
 
	var book1:BookInfo //默认构造器创建结构体实例
	var book2 = BookInfo(ID:0021,Name:"苹果",Author:"Apple")  //调用逐一构造器创建实例
	book2.ID = 1234  //修改内部值
	```

#### 枚举

+ enum

 与OC只能整型不同，Swift支持更多枚举方案:   

 + 整型
 + 浮点数
 + 字符串
 + 布尔类型
 + 嵌套枚举

	声明

	```
	// 常规枚举
	enum DDYSeason {
    	case spring
    	case summer
    	case autumn
    	case winter
	}
	// 整型枚举
	enum DDYIndex: Int {
    	case first  = 1
    	case second = 2
    	case nine   = 9
	}
	// 带内部方法的枚举
	enum DDYDirection {
    	case up
    	case down

    	func description() -> String{
        	switch(self) {
        	case .up: return "向上"
        	case .down: return "向下"
        	}
    	}
	}
	// 嵌套枚举
	enum Area {
	 	enum DongGuan {
	    	case NanCheng
        	case DongCheng
    	}
    
    	enum GuangZhou {
        	case TianHe
        	case CheBei
    	}
	}
	print(Area.DongGuan.DongCheng)

	// 关联值(case 传值)
	enum Trade {
    	case Buy(stock:String,amount:Int)
    	case Sell(stock:String,amount:Int)
	}

	let trade = Trade.Buy(stock: "003100", amount: 100)

	switch trade {
		case .Buy(let stock,let amount):
    
    		print("stock:\(stock),amount:\(amount)")
    
		case .Sell(let stock,let amount):
    		print("stock:\(stock),amount:\(amount)")
		default:
    	()
	}

	// 递归枚举
	// 枚举成员的关联值为当前枚举类型时称为递归枚举。indirect修饰整个枚举时,所有成员均可递归(也可不递归)
	indirect enum MyCalculateEnum {
    	case number(Int)
    	case addition(MyCalculateEnum, MyCalculateEnum)
    	case multi(MyCalculateEnum, MyCalculateEnum)
	}
	// 调用
	private class func testIndirectEnum() {
		let five = MyCalculateEnum.number(5)
		let four  = MyCalculateEnum.number(4)
  		let sum = MyCalculateEnum.addition(five, four)
		let product = MyCalculateEnum.multi(sum, MyCalculateEnum.number(2))
		print("(5+4)*2 = \(product)")
	}
	```
	应用

	```
	// 作为参数
	private func testEnum(_ mySeason: DDYSeason) {
		print("\(mySeason)")
	}
	``` 
	调用   

	```

	// 给变量(常量)赋值
	var mySeason = DDYSeason.spring
	mySeason = DDYSeason.summer
	// 调用作为参数的方法
	testEnum(DDYSeason.spring)
	testEnum(mySeason)
	testEnum(.winter) // 当明确声明的枚举类型后可以省略类型
 
	// rawValue来获取原始值
	let indexValue = DDYIndex.nine.rawValue
	// 由原始值获取枚举成员
	let ddyIndex = DDYIndex(rawValue: 2)
	// 9 Optional(DDYTest.DDYIndex.second)
	print("\(indexValue) \(String(describing: ddyIndex))")
	// 向上
	print("\(DDYDirection.up.description())") 
	```

	用struct 遵循 OptionSet 协议 表示位移枚举(OC中NS_OPTIONS)

	OC中的位移枚举

	```
	// 声明定义
	typedef NS_OPTIONS(NSInteger, DDYCertificateType) {
		DDYCertificateIDCard    = 1 << 0, // 身份证
    	DDYCertificateLicense   = 1 << 1, // 营业执照
	};
	// 作为参数
	- (void)refreshWithType:(DDYCertificateType)type {
		// 判断
		if (type & DDYCertificateIDCard) {
		NSLog(@"身份证");
		}
	}
	// 调用
	[self refreshWithType: DDYCertificateIDCard | DDYCertificateLicense];
	```
	
	Swift中目前没提供直接的位移枚举   
	但可以使用结构体(struct) 并结构体要遵从 OptionSet协议,以引入选项集合   
	不能直接用枚举(enum)表示
	
	```
	struct DDYCertificateType: OptionSet {
		let rawValue: Int
		static let IDCard   = DDYCertificateType(rawValue: 1)
		static let License  = DDYCertificateType(rawValue: 2)
		static let BankCard = DDYCertificateType(rawValue: 4)
    	static let Passport = DDYCertificateType(rawValue: 8)
    	static let VehiclePlate = DDYCertificateType(rawValue: 16)
	}

	// 添加多个枚举值条件不再是用| 而是用[.xxx,.xxx] 类似于数组的方式
	let certificateType: DDYCertificateType = [.IDCard, .License, .BankCard]
	// 判断位移枚举的变量是否存在不再是用& 而是用.contains()
	print("\(certificateType.contains(.IDCard))") // true
	print("\(certificateType.contains(.Passport))") // false
	```

#### 可选类型

+ Optional: 用来处理值可能缺失的情况,表示有值或没有值 [.](https://www.jianshu.com/p/31cf5b81ad6d)

	可选类型是Swift的一个特色。它表示一个变量有可能有值，也可能没有值（nil）。
	声明的方式是“数据类型+问号”。
	而当要使用一个可选类型的变量是，要在后面加感叹号“!”。

	```
	var height:Int?  //声明一个可选类型，默认值为nil
	if height == nil{
		print("你有多高？")
	} else {
    	print("你有\(height!)高。")
	}
	height = 199 //给可选类型赋值
	```

#### 元组

目前Swift中唯一的一种复合类型，他可以将指定有限个数的任何类型一次整理为一个对象，元组中的每一种类型都可以是任何的结构体、枚举或类类型，甚至也可以是一个元组以及空元组。元组中的每个元素我们称为一个“分量”,一般以”元素“统称

用例

```
/// 元组用例
    public class func testBasic() {
        // 给元组元素加标签(不能数字开头)，可通过标签或索引下标来访问元素
        let tupleTest1 = (gameName:"game", age:18, _:99, 10086, s0090990s:"666")
        let tupleSub0 = tupleTest1.gameName
        let tupleSub1 = tupleTest1.0
        // 声明一个类型为(Int, Double, Bool)的元组常量,通过索引访问元素(索引下标从0开始)
        let tupleTest2 = (10, 0.5, false)
        let tupleSub2 = tupleTest2.0
        print("\(tupleTest1)") // (gameName: "game", age: 18, 99, 10086, s0090990s: "666")
        print("\(tupleSub0)") // game
        print("\(tupleSub1)") // game
        print("\(tupleTest2)") // (10, 0.5, false)
        print("\(tupleSub2)") // 10
        // 一旦一个元组的某个元素带上标签，那么该元组类型相应的位置上也必须加上标签
        // 一个元组中不需要给所有的元素都加上标签，可以加一部分，指定标签的元素
        // 如果给定标签，我们可以通过标签访问也可以通过索引位置访问
        // 元组往往用于一个函数返回多个不同类型的对象的场合
    }
```

萃取元素

```
/// 元组萃取元素
    public class func testGetElement() {
        // 声明一个元组常量tupleTest, 元组也可以看作一个对象
        let tupleTest = (10, 0.5, true)
        // 这里同时声明三个常量：a, b, c tupleTest的第一个元素对 a 初始化，b, c同理
        // let (a, b, c) 不是在声明元组，而是同时声明三个常量，用于萃取一个元组中的值
        let (a1, b1, c1) = tupleTest
        // 如果我们不想萃取元组中的某个元素，用_代替
        let (a2, _, c2) = (5.5, "hello", false)
        // 因为元组分解这一语法特性，可以通过元组字面量来交换两个对象的值
        var a3 = 1, b3 = 2
        // 交换两个对象的值，a3 ＝ 2， b3 ＝ 1
        (a3, b3) = (b3, a3)
        // a1:10 b1:0.5 c1:true a2:5.5 c2:false a3:2 b31
        print("a1:\(a1) b1:\(b1) c1:\(c1) a2:\(a2) c2:\(c2) a3:\(a3) b3\(b3)")
    }
```

元组比较

```
/// 元组比较
    public class func testCompara() {
        let tupleCompare1 = (1, 2, 3)
        let tupleCompare2 = (1, 2, 3)
        let tupleCompare3 = (6, 9, 3, 4)
        let tupleCompare4 = (6, 2, 5, 8)
        let tupleCompare5 = (true, 9, 3, 4)
        let tupleCompare6 = (false, 2, 5, 8)
        // 判断元组 t1和t2 是否相等，结果为：t1 == t2? true
        print("t1 == t2? \(tupleCompare1 == tupleCompare2)")
        // 判断元组 t3和t4 是否为小于关系，结果为：t3 < t4? false 因为t3的第二个元素大于t4的第二个元素
        print("t3 < t4? \(tupleCompare3 < tupleCompare4)")
        // 这里t1和t3比较，会直接报错，因为不属于同一个类型
        // print("tupleCompare1 = tupleCompare3? \(tupleCompare1 == tupleCompare3)")
        // 这里的tupleCompare5和tupleCompare6比较也会报错，虽然元组类型一样，因为Bool类型不遵循comparable协议
        // print("t5 < t6? \(tupleCompare5 < tupleCompare6)")
    }
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

简单使用

```
// 写法一:
let error = (404, "Not Found")
//下标访问
print(error.0)
print(error.1)

// 写法二:
let error = (errorCode : 404, errorInfo : "Not Found")
//别名访问
print(error.errorCode)
print(error.errorInfo)

// 写法三:
//定义元组变量接收元素的值
let (errorCode, errorInfo) = (404, "Not Found")
print(errorCode)
print(errorInfo)
```

Any、AnyObject

Any是一个空协议集合的别名，它表示没有实现任何协议，因此它可以是任何类型，包括类实例与结构体实例。可以表示任何类型，包括函数类型。
AnyObject是一个成员为空的协议，任何对象都实现了这个协议。可以表示任何类类型的实例。

类型转化符号

is : 使用类型检查操作符 （ is ）来检查一个实例是否属于一个特定的子类。如果实例是该子类类型，类型检查操作符返回 true ，否则返回 false 。

as : 类型转换操作符      
as 从派生类转换为基类，向上转型（upcasts）
as!  向下转型（Downcasting）时使用。由于是强制类型转换，如果转换失败会报 runtime 运行错误。
as? 和 as! 操作符的转换规则完全一样。但 as? 如果转换不成功的时候便会返回一个 nil 对象。成功的话返回可选类型值。由于 as? 在转换失败的时候也不会出现错误，所以对于如果能确保100%会成功的转换则可使用 as!，否则使用 as?
[.](https://www.cnblogs.com/dukework/p/6553714.html)

Swift是强类型的语言，强类型语言也称为强类型定义语言，是一种总是强制类型定义的语言，要求变量的使用要严格符合定义，所有变量都必须先定义后使用
Swift中任何一个变量/常量都有明确的类型
注意:

如果定义一个标识符时有直接进行赋值，那么标识符后面的类型可以省略
Swift有类型推导，会自动根据后面的赋值来决定前面的标识符的数据类型
可以通过option+鼠标左键来查看变量的数据类型

* [Swift字符串中Unicode](https://blog.csdn.net/jiuchabaikaishui/article/details/92837988)

* 字面量，指能够直接指出自己的类型并为变量进行赋值的值
[参考](https://blog.csdn.net/potato512/article/details/52573766)
[参考](http://www.hangge.com/blog/cache/detail_553.html)

* [学习Swift www.runoob.com](https://www.runoob.com/swift/swift-tutorial.html) 
* [学习Swift www.hangge.com](http://www.hangge.com/blog/cache/category_72_42.html)
* [学习Swift www.swift51.com](http://www.swift51.com/swift.html)

<br><br><br><br><br>


[下一页 Swift2 运算符和流程控制语句](https://github.com/starainDou/DDYKnowledge/blob/master/Swift/Swift2.md)