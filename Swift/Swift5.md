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

父类又叫超类    
如果要重写父类中的方法，需要加 override     
如果要访问父类中的方法(子类对象访问父类方法)用super(此时super表示self当前对象)

重写属性    
你可以提供定制的 getter（或 setter）来重写任意继承来的属性，无论继承来的属性是存储型的还是计算型的属性。

子类并不知道继承来的属性是存储型的还是计算型的，它只知道继承来的属性会有一个名字和类型。所以你在重写一个属性时，必需将它的名字和类型都写出来。

注意点：    
如果你在重写属性中提供了 setter，那么你也一定要提供 getter。     
如果你不想在重写版本中的 getter 里修改继承来的属性值，你可以直接通过     super.someProperty来返回继承来的值，其中someProperty是你要重写的属性的名字。  

重写属性观察器     
你可以在属性重写中为一个继承来的属性添加属性观察器。这样一来，当继承来的属性值发生改变时，你就会监测到。    
注意：你不可以为继承来的常量存储型属性或继承来的只读计算型属性添加属性观察器 

防止重写    
我们可以使用 final 关键字防止它们被重写。    
如果你重写了final方法，属性或下标脚本，在编译时会报错。   
你可以通过在关键字class前添加final特性（final class）来将整个类标记为 final 的，这样的类是不可被继承的，否则会报编译错误。    

\_\_weak 与\_\_unretained有何区别？   
\_\_weak修饰的弱引用，如果指向的对象被销毁，那么指针会立马指向nil   
\_\_unretained修饰的弱引用，如果指向的对象被销毁，它的指针依然会指向之前的内存地址，很容易产生野指针（僵尸对象）

swift中[Error](https://developer.apple.com/documentation/swift/error)

```
public protocol Error {
}

extension Error {
}

extension Error where Self.RawValue : SignedInteger {
}

extension Error where Self.RawValue : UnsignedInteger {
}
```

苹果文档第一个例子

```
// 枚举对Int值处理可能的Error分类
enum IntParsingError: Error {
    case overflow
    case invalidInput(Character)
}

// 扩展Int
extension Int {
    init(validating input: String) throws {
        // ...
        let c = _nextCharacter(from: input)
        if !_isValid(c) {
            throw IntParsingError.invalidInput(c)
        }
        // ...
    }
}

// 具体使用(捕获)
do {
    let price = try Int(validating: "$100")
} catch IntParsingError.invalidInput(let invalid) {
    print("Invalid character: '\(invalid)'")
} catch IntParsingError.overflow {
    print("Overflow error")
} catch {
    print("Other error")
}
// Prints "Invalid character: '$'"
```

第二个例子

```
// 自定义错误输出结构体
struct XMLParsingError: Error {
    enum ErrorKind {
        case invalidCharacter
        case mismatchedTag
        case internalError
    }

    let line: Int
    let column: Int
    let kind: ErrorKind
}
// 抛出错误
func parse(_ source: String) throws -> XMLDoc {
    // ...
    throw XMLParsingError(line: 19, column: 5, kind: .mismatchedTag)
    // ...
}
// 具体应用
do {
    let xmlDoc = try parse(myXMLData)
} catch let e as XMLParsingError {
    print("Parsing error: \(e.kind) [\(e.line):\(e.column)]")
} catch {
    print("Other error: \(error)")
}
// Prints "Parsing error: mismatchedTag [19:5]"
```

Alamofire中的错误示例

Alamofire中的错误处理，用单独的一个文件AFError来管理使用中的错误，其中自定义了枚举

```
public enum AFError: Error {
                                encoding process.
    public enum ParameterEncodingFailureReason {
        case missingURL
        case jsonEncodingFailed(error: Error)
        case propertyListEncodingFailed(error: Error)
    }

    
    public enum MultipartEncodingFailureReason {
        case bodyPartURLInvalid(url: URL)
        case bodyPartFilenameInvalid(in: URL)
        case bodyPartFileNotReachable(at: URL)
        case bodyPartFileNotReachableWithError(atURL: URL, error: Error)
        case bodyPartFileIsDirectory(at: URL)
        case bodyPartFileSizeNotAvailable(at: URL)
        case bodyPartFileSizeQueryFailedWithError(forURL: URL, error: Error)
        case bodyPartInputStreamCreationFailed(for: URL)

        case outputStreamCreationFailed(for: URL)
        case outputStreamFileAlreadyExists(at: URL)
        case outputStreamURLInvalid(url: URL)
        case outputStreamWriteFailed(error: Error)

        case inputStreamReadFailed(error: Error)
    }

    
    public enum ResponseValidationFailureReason {
        case dataFileNil
        case dataFileReadFailed(at: URL)
        case missingContentType(acceptableContentTypes: [String])
        case unacceptableContentType(acceptableContentTypes: [String], responseContentType: String)
        case unacceptableStatusCode(code: Int)
    }

    
    public enum ResponseSerializationFailureReason {
        case inputDataNil
        case inputDataNilOrZeroLength
        case inputFileNil
        case inputFileReadFailed(at: URL)
        case stringSerializationFailed(encoding: String.Encoding)
        case jsonSerializationFailed(error: Error)
        case propertyListSerializationFailed(error: Error)
    }

    case invalidURL(url: URLConvertible)
    case parameterEncodingFailed(reason: ParameterEncodingFailureReason)
    case multipartEncodingFailed(reason: MultipartEncodingFailureReason)
    case responseValidationFailed(reason: ResponseValidationFailureReason)
    case responseSerializationFailed(reason: ResponseSerializationFailureReason)
}
```

作者还将AFError进行了扩展，增加部分判断的方法，比如判断是否是无效的URL:

```
// MARK: - Error Booleans

extension AFError {
    /// Returns whether the AFError is an invalid URL error.
    public var isInvalidURLError: Bool {
        if case .invalidURL = self { return true }
        return false
    }
}
```

同时拓展中也遵守了LocalizedError协议来重写errorDescription属性，并且对已经分类enum的自定义错误类型进行拓展重写了属性localizedDescription,这样就构成了项目中完整的错误处理机制。比如在Alamofire编码解析方法方法encode中，有这么一段代码

```
do {
	let data = try JSONSerialization.data(withJSONObject: jsonObject, options: options)
	if urlRequest.value(forHTTPHeaderField: "Content-Type") == nil {
		urlRequest.setValue("application/json", forHTTPHeaderField: "Content-Type")
	}
	urlRequest.httpBody = data
} catch {
	throw AFError.parameterEncodingFailed(reason: .jsonEncodingFailed(error: error))
}
```

其中就是对于JSON序列化时，通过 do { try } catch 的方式来捕获错误，如果捕获到，将抛出已经自定义好的 AFError.parameterEncodingFailed(reason: .jsonEncodingFailed(error: error)) 类型错误。

致命错误

喵神举例的两个使用的场景：

1.父类中的某些方法，不想让别人调用，可以在方法中加上fatalError，这样子类如果想到用必须重写
2.对于其他一切我们不希望别人随意调用，但是又不得不去实现的方法，我们都应该使用 fatalError 来避免任何可能的误会。

```
required init?(coder aDecoder: NSCoder) {
	fatalError("init(coder:) has not been implemented")
}
```

[原文请移步](https://www.jianshu.com/p/3f3c32ed4aa4)


图片放入工程两个方式

1.Assets.xcassets内，同时生成描述文件Contents.json。且在打包后以Assets.car的形式存在，不能直接打开（据说工具ThemeEngine可以打开）。    
以此方式放入的图片并不在mainBundle中，不能使用contentOfFile这样的API来加载图片，interface builder中使用图片时不需要后缀和倍数标识（@2x这样的）
2.在工程建立专门放图片的文件夹，然后放入图片

Assets优势:      

1.性能好，节省Disk。Asset Catalogs会用一个高度优化的特殊格式来存所有图片，而不是一个一个的单独的图片资源，会更少的涉及频繁Disk I/O操作，且会按需下载适合你机型的合适分辨率的图片资源； 
2.相对安全。图片资源得到一定程度保护（Asset.car不易打开）


swift中Data获取bytes数组

方法1:使用 [UInt8] 新的构造函数

```
/// 使用(data as NSData).bytes 并不优雅  毕竟要尽量脱离OC的框架
/// data是结构体  使用[UInt8]构造方法得到data的byte数组
let bytes = [UInt8](data)
```

方法2:通过 Pointer 指针获取

```
let bytes = data.withUnsafeBytes {
    [UInt8](UnsafeBufferPointer(start: $0, count: data.count))
}
print(bytes)
```


封包与拆包

* 拆包概念    
	就是一个可选类型有值的时候，打印结果带有可选类型（Optional）标记，当我们去掉可选类型（Optional）的过程就成为拆包，例如：将Optional String类型强制转换为String类型的过程，就是一种拆包过程
	
* 强制拆包    
	当我们在进行拆包过程中，如果对可选类型（Optional）是否有值不做处理，进行拆包的过程，就是一种强制拆包的过程，如果某一个可选类型（Optional）没有值，而我们又进行强制拆包的操作，就会崩溃(fatal error: unexpectedly found nil while unwrapping an Optional value) 

* 封包    
	简单来就是将一种确定的数据类型转换为可选类型（Optional）的过程，我们称之为封包。例如：将String类型转换为Optional String类型的过程，其实就是一种封包过程。
	
	
pngData()和jpegData() 为nil

文档中指明：May return nil if image has no CGImageRef or invalid bitmap format


