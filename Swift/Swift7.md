# Swift7-高阶函数 map flatMap compactMap filter reduce

> ## 相关概念

Swift1中接触了高阶类型--元组(Tuple)，现在再了解一个新概念--高阶函数。     
Swift作为一门多范式编程语言，其对函数式编程的支持成就了对高阶函数的应用。       
高阶函数(Higher order functions)，顾名思义，仍是函数，但这种函数能接受函数作为参数或者返回一个函数操作其他函数。    
由此可见我们常用的闭包(closure)也算一种高阶函数       
现在了解一下Swift中最典型的4个高阶函数:Map/FlatMap/Filter/Reduce  

> ## map

干嘛的？ 对集合类型的数据进行循环，并对每个元素采取相同操作，然后返回同维集合

例如，要对一个一维整型数组内所有数字进行+1

```
private class func testMap() {
	let numbersArray = [1, 3, 5, 7, 9]
	var newNumArray1 = [Int]()
	// for循环方式
	for item in numbersArray {
	    newNumArray1.append(item+1)
	}
	// 高阶函数Map接受闭包方式
	let newNumArray2 = numbersArray.map { $0+1 }
	print("加1后数组1：\(newNumArray1)")
	print("加1后数组2：\(newNumArray2)")
	// 加1后数组1：[2, 4, 6, 8, 10]
	// 加1后数组2：[2, 4, 6, 8, 10]
}
```

附加尾随闭包几种不同写法

```
let result1 = numbersArray.map { (item: Int) -> Int in return item + 1 }
let result2 = numbersArray.map { (item: Int) in return item + 1 }
let result3 = numbersArray.map { item in return item + 1 }
let result4 = numbersArray.map { item in item + 1 }
let result5 = numbersArray.map { $0+1 }
print("\(result1) \(result2) \(result3) \(result4) \(result5)")
```

> ## flatMap & compactMap

flatMap 对集合类型的数据进行循环，并对每个元素采取相同操作，然后返回一个一维集合
compactMap 对集合类型的数据进行循环，并对每个元素采取相同操作，然后返回一个同维集合

例如，要对一个二维整型数组内所有数字进行+1

```
private class func testFlatMap() {
	let numbersArray = [[1, 2, 3], [4, 5, 6]]
	let newNumArray1 = numbersArray.flatMap { $0.map { $0+1 }}
	let newNumArray2 = numbersArray.compactMap{ $0.map { $0+1 }}
	let newNumArray3 = numbersArray.map { $0.map { $0+1 }}
	print("加1后数组1：\(newNumArray1)")
	print("加1后数组2：\(newNumArray2)")
	print("加1后数组3：\(newNumArray3)")
	// 加1后数组1：[2, 3, 4, 5, 6, 7]
	// 加1后数组1：[[2, 3, 4], [5, 6, 7]]
	// 加1后数组2：[[2, 3, 4], [5, 6, 7]]
}
```

> ## Filter

干嘛的？对集合类型的数据进行循环，并对每个元素进行筛选，然后返回一个一维集合

```
private class func testFilter() {
	let numbersArray = [1, 2, 3, 4, 5, 6, 7, 8, 9]
	let newNumArray1 = numbersArray.filter { $0 % 2 == 0}
	let newNumArray2 = numbersArray.filter { $0 % 2 == 1}
	print("删除奇数后的数组1：\(newNumArray1)")
	print("删除偶数后的数组2：\(newNumArray2)")
	// 删除奇数后的数组1：[2, 4, 6, 8]
	// 删除偶数后的数组2：[1, 3, 5, 7, 9]
}
```

> ## Reduce

干嘛的？接受一个初始化值，并且接受一个闭包作为规则，自动遍历集合的每一个元素，使用闭包的规则去处理这些元素，合并处理结果

```
private class func testReduce() {
	let numbersArray = [1, 2, 3, 4, 5, 6, 7, 8, 9]
	let sum = numbersArray.reduce(0) { $0 + $1}
	print("数组中数字之和：\(sum)")
	// 数组中数字之和：45
}	
```

> ## 综合起来，链式调用

```
private class func testAllFunc() {
	let numbersArray = [1, 2, 3, 4, 5, 6, 7, 8, 9]
	let result = numbersArray.filter { $0 % 2 == 0 }.map { $0+1}.reduce(0) { $0 + $1 }
	print("数组数据去除奇数得到的数组中所有元素加1后的求和结果:\(result)")
	// 数组数据去除奇数得到的数组中所有元素加1后的求和结果:24
}
```

* flatMap 与 map 不同之处是

	- flatMap 去nil,返回后的数组中不存在 nil 同时它会把Optional解包;
	- flatMap 降纬，把数组中存有数组的数组一同打开变成一个新的数组;
	- flatMap 也能把两个不同的数组合并成一个数组 这个合并的数组元素个数是前面两个数组元素个数的乘积

* 学习这些高阶函数有什么意义？

	- 阅读和理解复杂的函数式编程   
	- 编写更优美、更易于维护的代码，具有更好的可读性   
	- 提高我们的Swift语言能力   
   

[函数式思想--Swift中Map、Filter、Reduce函数实现原理及仿写](www.cocoachina.com/articles/20075)    
[Swift 四种高阶函数简介](https://www.jianshu.com/p/7f4472c1b039)  
[Swift之Map与CompactMap区别](https://www.jianshu.com/p/07b59f4f0071)    
[swift中高阶函数map、flatMap、filter、reduce](https://www.cnblogs.com/muzijie/p/6542650.html)    

