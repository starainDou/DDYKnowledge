> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.jianshu.com/p/a7cc623132b0

> 此文章是 v1.0 + 时编写，年代久远，小心有毒，谨慎食用！！！

一些重要概念
------

*   所有的东西都是对象，所有的对象都是类的实例。即使 数字、函数、`null`  
    也都是对象。所有的对象都继承自 `Object` 类。
*   指定静态类型表明你的意图，并使检查类型检查成为可能。在 Dart 1.x 指定类型是可选的，然而 Dart 正在朝着完全类型安全的语言发展。
*   在 Dart 1.x 中，强类型 _strong mode_ 是可选的，但在 Dart 2 中，默认就是 _strong mode_。
*   Dart 在运行前解析所有的代码，可以使用些小技巧，例如：通过使用类型或编译时常量，来捕捉错误或使代码运行的更快。
*   Dart 支持顶级的函数，也支持类或对象的静态和实例方法。也可以在函数内部嵌套函数或本地函数。
*   同样，Dart 支持顶级的变量，也支持类或对象的静态变量和实例变量 (也被称作字段或属性)。
*   Dart 没有 `public`、`protected`、`private` 等关键字，如果一个标识符以 `_`开头则表示私有。
*   标识符以小写字母或下划线`_`开头，后面跟着字符和数字的任意组合。
*   明确区分表达式和语句。
*   Dart tools 会报告两种类型的问题：警告 (warnings) 和错误(errors)。警告仅标志着你的代码可能不会工作，但并不会阻止程序执行；错误可能是编译时错误，也可能是运行时错误。编译时错误会阻止程序执行；运行时错误会在程序执行时抛出异常。
*   Dart 1.x 有两种运行时模式：生产模式和检查模式。推荐在开发和 debug 时使用检查模式，部署到生产模式。生产模式时 Dart 程序默认的运行时模式。
*   Dart 2 废弃了 _checked mode_ 默认 _strong mode_。
*   Dart 2 不再使用 _Dartium_，改为 _dartdevc_(DDC)。
*   没有初始化的变量都会被赋予默认值 `null`。
*   `final`的值只能被设定一次。`const` 是一个编译时的常量，可以通过 `const` 来创建常量值，`var c=const[];`，这里 `c` 还是一个变量，只是被赋值了一个常量值，它还是可以赋其它值。实例变量可以是 `final`，但不能是 `const`。

### 关键字（Keywords）

| abstract | continue | false | new | this |
| --- | --- | --- | --- | --- |
| as | default | final | null | throw |
| assert | deferred | finally | operator | true |
| async | do | for | part | try |
| async* | dynamic | get | rethrow | typedef |
| await | else | if | return | var |
| break | enum | implements | set | void |
| case | export | import | static | while |
| catch | external | in | super | with |
| class | extends | is | switch | yield |
| const | factory | library | sync | yield |

内置类型（Built-in types）
--------------------

*   numbers
*   strings
*   booleans
*   lists (also known as arrays)
*   maps
*   runes (for expressing Unicode characters in a string)
*   symbols

`is is！`操作符判断对象是否为指定类型，如 num、String 等。

`as`用来类型断言。

*   num
    *   int 取值范围：-2^53 to 2^53
    *   double

```
var one = int.parse('1');


var onePointOne = double.parse('1.1');


String oneAsString = 1.toString();


String piAsString = 3.14159.toStringAsFixed(2);


```

*   String
    
    *   Dart 的 String 是 UTF-16 编码的一个队列。
    *   `'...'，"..."`表示字符串。
    *   `'''...'''，"""..."""`表示多行字符串。
    *   `r'...',r"..."`表示 “raw” 字符串。
*   bool
    
    *   Dart 是强 bool 类型检查，只有 bool 类型的值是`true` 才被认为是 true。
*   List
    

```
var vegetables = new List();


var fruits = ['apples', 'oranges'];


fruits.add('kiwis');


fruits.addAll(['grapes', 'bananas']);


assert(fruits.length == 5);


fruits.first;


fruits.last;


assert(fruits[0] == 'apples');


assert(fruits.indexOf('apples') == 0);


fruits.removeAt(index);


fruits.remove('apples');


fruits.removeLast();


fruits.removeRange(start,end);


fruits.removeWhere((item) => item.length >6)；


fruits.clear();
assert(fruits.length == 0);


fruits.sort((a, b) => a.compareTo(b));


```

*   Map

```
var hawaiianBeaches = {
    'oahu' : ['waikiki', 'kailua', 'waimanalo'],
    'big island' : ['wailea bay', 'pololu beach'],
    'kauai' : ['hanalei', 'poipu']
};
var searchTerms = new Map();


var nobleGases = new Map<int, String>();


nobleGase[54] = 'dart';



nobleGases[54] = 'xenon';
assert(nobleGases[54] == 'xenon');


assert(nobleGases.containsKey(54));


nobleGases.remove(54);
assert(!nobleGases.containsKey(54));


```

*   Runes  
    Dart 中 runes 是 UTF-32 字符集的 string 对象。

```
string = 'Dart';
string.codeUnitAt(0); 
string.codeUnits;     


```

*   Symbol

symbol 字面量是编译时常量，在标识符前面加`#`。如果是动态确定，则使用 Symbol 构造函数，通过`new`来实例化。

### 函数（Functions）

所有的函数都会有返回值。如果没有指定函数返回值，则默认的返回值是`null`  
。没有返回值的函数，系统会在最后添加隐式的`return` 语句。

函数可以有两种类型的参数：

*   必须的——必须的参数放在参数列表的前面。
*   可选的——可选的参数跟在必须的参数后面。

##### 可选的参数

可以通过**名字**或**位置**指定可选参数，但是两个不能同时存在。

*   命名可选参数使用`{}`，默认值使用冒号`:`，调用时需指明参数名，与顺序无关。
*   位置可选参数使用`[]`，默认值使用等号`=`，调用时参数按顺序赋值。

### 操作符（Operators）

表中，操作符的优先级依次降低。

| 描述 | 操作符 |
| --- | --- |
| 一元后置操作符 | expr++ expr-- () [] . ?. |
| 一元前置操作符 | -expr !expr ~expr ++expr --expr |
| 乘除 | * / % ~/ |
| 加减 | + - |
| 位移 | <<>> |
| 按位与 | & |
| 按位异或 | ^ |
| 按位或 |  |
| 关系和类型判断 | >= > <= < as is is! |
| 等 | == != |
| 逻辑与 | && |
| 逻辑或 |  |  |
| 若为 null | ?? |
| 条件表达式 | expr1 ? expr2 : expr3 |
| 级联 | .. |
| 赋值 | = *= /= ~/= %= += -= <<=>>= &= ^= | = ??= |

### 流程控制语句（Control flow statements）

*   if...else
*   for
*   while do-whild
*   break continue
*   switch...case
*   assert（仅在 checked 模式有效）

### 异常（Exceptions）

##### throw

*   抛出固定类型的异常：

```
throw new FormatException('Expected at least 1 section');


```

*   抛出任意类型的异常：

```
throw 'Out of llamas!';


```

*   因为抛出异常属于表达式，可以将 throw 语句放在 => 语句中，或者其它可以出现表达式的地方：

```
distanceTo(Point other) =>
    throw new UnimplementedError();


```

##### catch

将可能出现异常的代码放置到`try`语句中，可以通过 `on`语句来指定需要捕获的异常类型，使用`catch`来处理异常。

```
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  
  buyMoreLlamas();
} on Exception catch (e) {
  
  print('Unknown exception: $e');
} catch (e, s) {
  print('Exception details:\n $e');
  print('Stack trace:\n $s');
}


```

可以向`catch()`传递 1 个或 2 个参数。第一个参数表示：捕获的异常的具体信息，第二个参数表示：异常的堆栈跟踪 (stack trace)。

##### rethrow

`rethrow`语句用来处理一个异常，同时希望这个异常能够被其它调用的部分使用。

```
final foo = '';

void misbehave() {
  try {
    foo = "1";
  } catch (e) {
    print('2');
    rethrow;
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('3');
  }
}


```

##### finally

Dart 的`finally`用来执行那些无论异常是否发生都执行的操作。

```
final foo = '';

void misbehave() {
  try {
    foo = "1";
  } catch (e) {
    print('2');
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('3');
  } finally {
    print('4'); 
  }
}



```

### 类（Classes）

Dart 是一种面向对象的语言，并且支持基于 mixin 的继承方式：**一个类可以继承自多个父类**。

使用`new`语句来构造一个类。构造函数的名字可能是 ClassName，也可以是`ClassName.identifier`。例如：

```
var jsonData = JSON.decode('{"x":1, "y":2}');


var p1 = new Point(2, 2);


var p2 = new Point.fromJson(jsonData);


```

*   使用`.`来调用实例的变量或者方法。
*   使用 `?.` 来避免左边操作数为`null`引发异常。
*   使用`const`替代`new`来创建编译时的常量构造函数。
*   两个使用`const`构建的同一个构造函数，实例相等。
*   获取对象的运行时类型使用：`o.runtimeType`。

##### 实例化变量

在类定义中，所有没有初始化的变量都会被初始化为`null`。

所有实例变量会生成一个隐式的`getter`方法，不是`final`或`const`的实例变量也会生成一个隐式的`setter`方法。

##### 构造函数

```
class Point {
  num x;
  num y;

  Point(num x, num y) {
    // 这不是最好的方式.
    this.x = x; // this关键字指向当前类的实例
    this.y = y;
  }
}


```

```
class Point {
  num x;
  num y;

  // 推荐方式
  Point(this.x, this.y);
}


```

###### 默认构造函数

没有声明构造函数，dart 会提供默认构造函数。默认构造函数没有参数，并且调用父类的无参数构造函数。

###### 构造函数不能被继承

子类不会继承父类的构造函数。如果不显式提供子类的构造函数，系统就提供默认的构造函数。

###### 命名构造函数

```
class Point {
  num x;
  num y;

  Point(this.x, this.y);

  
  Point.fromJson(Map json) {
    x = json['x'];
    y = json['y'];
  }
}


```

使用命名构造函数可以实现一个类多个构造函数。构造函数不能被继承，父类中的命名构造函数不能被子类继承。如果想要子类也拥有一个父类一样名字的构造函数，必须在子类实现这个构造函数。

###### 调用父类的非默认构造函数

默认情况下，子类调用父类的无参数的非命名构造函数。父类的构造函数会在子类的构造函数体之前（大括号前）调用。如果也使用了**初始化列表** ，则会先执行**初始化列表** 中的内容，即下面的顺序：

1.  初始化列表
2.  父类无参数构造函数
3.  主类无参数构造函数

如果父类不显式提供无参的非命名构造函数，在子类中必须手动调用父类的一个构造函数。在子类构造函数名后，大括号`{`前，使用`super.`调用父类的构造函数，中间使用`:`分割。

**父类构造函数参数不能访问`this`，例如，参数可以调用静态方法但不能调用实例方法。**

```
class Person {
  String firstName;

  Person.fromJson(Map data) {
    print('in Person');
  }
}

class Employee extends Person {
  
  Employee.fromJson(Map data) : super.fromJson(data) {
    print('in Employee');
  }
}

main() {
  var emp = new Employee.fromJson({});

  
  
  
  if (emp is Person) {
    
    emp.firstName = 'Bob';
  }
  (emp as Person).firstName = 'Bob';
}


```

因为父类构造函数的参数是在被调用之前确认值的，所以参数可以是一个表达式，像一个函数的调用。

```
class Employee extends Person {
  
  Employee() : super.fromJson(findDefaultData()); 
}


```

当在构造函数初始化列表中使用`super()`时，要把它放在最后。

```
View(Style style, List children)
    : _children = children,
      super(style) {}


```

###### 初始化列表

除了调用父类的构造函数，也可以通过**初始化列表** 在子类的构造函数体前（大括号前）来初始化实例的变量值，使用逗号`,`分隔。如下所示：

**一个初始化器的右边不能访问`this`**。

```
class Point {
  num x;
  num y;

  Point(this.x, this.y);

  
  Point.fromJson(Map jsonMap)
      : x = jsonMap['x'],
        y = jsonMap['y'] {
    print('In Point.fromJson(): ($x, $y)');
  }
}


```

###### 重定向构造函数

有时候构造函数的目的只是重定向到该类的另一个构造函数。重定向构造函数没有函数体，使用冒号`:`分隔。

```
class Point {
  num x;
  num y;

  
  Point(this.x, this.y) {
    print("Point($x, $y)");
  }

  
  Point.alongXAxis(num x) : this(x, 0);
}

void main() {
  var p1 = new Point(1, 2);
  var p2 = new Point.alongXAxis(4);
}


```

###### 常量构造函数

如果类的对象不会发生变化，可以构造一个编译时的常量构造函数。定义格式如下：

*   定义所有的实例变量是`final`。
*   使用`const`声明构造函数。

```
class ImmutablePoint {
  final num x;
  final num y;
  const ImmutablePoint(this.x, this.y);
  static final ImmutablePoint origin =
      const ImmutablePoint(0, 0);
}


```

###### 工厂构造函数

当实例化了一个构造函数后，不想每次都创建该类的一个新的实例的时候使用`factory`关键字，定义工厂构造函数，从缓存中返回一个实例，或返回一个子类型的实例。

**工厂构造函数不能访问`this`**

```
class Logger {
  final String name;
  bool mute = false;
  static final Map<String, Logger> _cache = <String, Logger>{}; 
  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name];
    } else {
      final logger = new Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }
  Logger._internal(this.name);
  void log(String msg) {
    if (!mute) {
      print(msg);
    }
  }
}

main() {
  var p1 = new Logger("1");
  p1.log("2");

  var p2 = new Logger('22');
  p2.log('3');
  var p3 = new Logger('1');
}



```

##### 方法

###### 实例方法

实例方法可以访问实例变量和`this`。

###### Getters and setters

`get()`和`set()`方法是 Dart 语言提供的专门用来读取和写入对象的属性的方法。每一个类的实例变量都有一个隐式的`getter`和可能的`setter`（如果字段为`final`或`const`，只有`getter`）。

像`++`之类的操作符，无论是否明确定义了 getter，都按预期的方式工作。为了避免意想不到的副作用，操作符调用 getter 一次，将其值保存在临时变量中。

```
class Rectangle {
  num left;
  num top;
  num width;
  num height;

  Rectangle(this.left, this.top, this.width, this.height);

  
  num get right             => left + width;
      set right(num value)  => left = value - width;
  num get bottom            => top + height;
      set bottom(num value) => top = value - height;
}

main() {
  var rect = new Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -8);
}


```

###### 抽象方法

抽象方法，只有函数声明，没有函数体，以分号`;`结尾，作为接口在其他类中实现。调用抽象方法会导致运行时错误。如果在不是抽象类中定义抽象方法会引发`warning`，但不会阻止实例化。

```
abstract class Doer {
  ...

  void doSomething(); 
}

class EffectiveDoer extends Doer {
  void doSomething() {
    
  }
}


```

###### 可重写的运算符

下面是可重写的运算符，在运算符前面使用`operator`关键字。

如果重写`==`，需要重写`Object`的`hashCode`getter。

| < | + | { | [] |
| --- | --- | --- | --- |
| > | / | ^ | []= |
| <= | ~/ | & | ~ |
| >= | * | << | == |
| – | % | >> |  |

##### 抽象类

使用`abstract`关键字定义一个抽象类，抽象类不能实例化。抽象类通常用来定义接口。

假如需要将抽象类实例化，需要定义一个`factory constructor`。

抽象类通常会包含一些抽象的方法。

```
abstract class AbstractContainer { 
  

  void updateChildren(); 
}


```

##### 隐式接口

每一个类都隐式的定义一个接口，这个接口包含了这个类的所有实例成员和它实现的所有接口。

一个类可以实现一个或多个（用`,`隔开）接口，通过`implements`关键字。

```
class Person {
  final _name;
  Person(this._name);
  String greet(who) => 'hello,$who,i am $_name';
}

class Imposter implements Person {
  final _name = '';
  String greet(who) => 'hi $who.do you know who i am.';
}

greetBob(Person p) => p.greet('bob');
main(List<String> args) {
  print(greetBob(new Person('lili')));
  print(greetBob(new Imposter()));
}



```

##### 继承

使用`extends`来创造子类，使用`super`来指向父类。

```
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  
}

class SmartTelevision extends Television {
  void turnOn() {
    super.turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  
}


```

子类可以重载实例方法，getter 和 setter。使用`@override`注解重载方法。

```
class A {
  @override
  void noSuchMethod(Invocation mirror) {
    print('You tried to use a non-existent member:' +
          '${mirror.memberName}');
  }
}


```

如果使用`noSuchMethod()`来实现每一个可能的 getter，setter 和一个或多个类型的方法，可以使用`@proxy`注解来避免警告：

```
@proxy
class A {
  void noSuchMethod(Invocation mirror) {
    
  }
}


```

如果知道编译时类型，可以不使用`@proxy`，只需要使类实现这些类型。

```
class A implements SomeClass, SomeOtherClass {
  void noSuchMethod(Invocation mirror) {
    
  }
}


```

##### 枚举类型

枚举类型是一种特殊的类，通常用来表示一组固定数字的常量值。

使用`enum`关键字声明枚举类型。

```
enum Color {
  red,
  green,
  blue
}


```

每个枚举类型都有一个`index`的 getter，返回以 0 开始的位置索引，每次加 1。

```
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);


```

使用`values`关键字获取枚举的所有值，返回一个列表。

```
print(Color.values); 


```

在 switch 语句中使用枚举，必须在 case 语句中判断所有的枚举，否则会获得警告。

```
enum Color {
  red,
  green,
  blue
}

Color aColor = Color.blue;
switch (aColor) {
  case Color.red:
    print('Red as roses!');
    break;
  case Color.green:
    print('Green as grass!');
    break;
  default: 
    print(aColor);  
}


```

枚举类型有以下限制：

*   不能继承，mixin，或实现一个枚举。
*   不能显式的实例化一个枚举。

##### minxins

mixins 是一种在多个类层次结构中，重用一个类的代码的方法。使用`with`关键字后跟多个 mixin 名。

```
class Musician extends Performer with Musical {
  
}

class Maestro extends Person
    with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}


```

##### class 变量和方法

使用`static`关键字来实现类范围内变量和方法。

*   静态变量

静态变量在使用时初始化。

```
class Color {
  static const red =
      const Color('red'); 
  final String name;      
  const Color(this.name); 
}

main() {
  assert(Color.red.name == 'red');
}


```

*   静态方法

静态方法不能被实例调用，不能访问`this`，只能被类名调用。

```
import 'dart:math';

class Point {
  num x;
  num y;
  Point(this.x, this.y);

  static num distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}

main() {
  var a = new Point(2, 2);
  var b = new Point(4, 4);
  var distance = Point.distanceBetween(a, b);
  print(distance); 
}


```

**对于通用或广泛使用的工具和功能，应该使用顶级函数，而不是静态方法。**

可以使用静态方法作为编译时常量。例如，可以给一个常量构造函数，传递一个静态方法作为参数。

### 泛型（Generics）

使用`<...>` 的方式来定义泛型。

##### 为什么使用泛型

*   虽然 Dart 语言中类型是可选的，但是明确的指明使用的是泛型，会让代码更好理解。

```
 var names = new List<String>();
 names.addAll(['Seth', 'Kathy', 'Lars']);
 
 names.add(42); 


```

*   使用泛型减少代码重复。

代码片段一：

```
 abstract class ObjectCache {
   Object getByKey(String key);
   setByKey(String key, Object value);
 }


```

代码片段二：

```
 abstract class StringCache {
   String getByKey(String key);
   setByKey(String key, String value);
 }


```

以上的两段代码可以使用泛型简化如下：

```
 abstract class Cache<T> {
   T getByKey(String key);
   setByKey(String key, T value);
 }


```

##### 用于集合类型 (Using collection literals)

泛型用于`List` 和 `Map` 类型参数化。

*   List: `<type>`
*   Map: `<keyType, valueType>`

```
var names = <String>['Seth', 'Kathy', 'Lars'];
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};


```

##### 在构造函数中参数化类型

```
var names = new List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
var nameSet = new Set<String>.from(names);
var views = new Map<int, View>();


```

##### 泛型集合及它们所包含的类型

dart 的泛型类型是具体的，在运行时包含它们的类型信息。

```
var names = new List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); 


```

然而，`is`表达式检查的是集合的类型，而不是里面的对象。在生产模式下，一个`List<Stirng>`可能有很多不是`string`的条目在里面。解决办法是检查每一项，或者包裹在一个异常处理程序中。

##### 限制参数化类型

当实现一个泛型时，如果需要限制它参数的类型，可以使用`extends`关键字。

```
class Foo<T extends SomeBaseClass> {...}

class Extender extends SomeBaseClass {...}

void main() {
  
  var someBaseClassFoo = new Foo<SomeBaseClass>();
  var extenderFoo = new Foo<Extender>();

  
  var foo = new Foo();

  
  
  
}


```

### 库和可见性

使用`import` 和 `library` 指令可以方便的创建一个模块或分享代码。一个 Dart 库不仅能够提供相应的 API，还可以包含一些以`_`开头的私有变量仅在库内部可见。

每一个 Dart 应用都是一个库，即使它没有使用`library`指令。库可以方便是使用各种类型的包。

##### 使用库（Libraries and visibility）

使用`import`指定怎样的命名空间，从一个库引用另一个库。

`import`唯一要求的参数是指定库的 URI。

*   dart 内置库，URI 组合`dart:`
*   其他库，使用文件路径或`package:`组合。`package:`组合式通过包管理工具提供的。

```
import 'dart:html';
import 'package:mylib/mylib.dart';


```

###### 指定一个库的前缀

如果导入的库拥有相互冲突的名字，使用`as`为其中一个或几个指定不一样的前缀。

```
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

Element element1 = new Element();           
lib2.Element element2 = new lib2.Element(); 


```

###### 导入库的一部分

如果只需要使用库的一部分内容，使用`show`或`hide`有选择的导入。

```
import 'package:lib1/lib1.dart' show foo;


import 'package:lib2/lib2.dart' hide foo;


```

###### 延迟加载库

延迟加载，也叫懒加载，允许应用程序按需加载库。使用延迟加载的场景：

*   减少程序初始化启动时间。
*   执行 A/B 测试——尝试替换一个算法的实现。
*   加载很少用的功能，比如可选的屏幕和对话框。

要延迟加载一个库，首先必须使用`deferred as`导入它。

```
import 'package:deferred/hello.dart' deferred as hello;


```

当需要使用库的时候，使用库名调用`loadLibrary()`。

```
greet() async {
  
  await hello.loadLibrary();
  hello.printGreeting();
}


```

可以在代码中多次调用`loadLibrary()`方法。但是实际上它只会被执行一次。

使用延迟加载的注意事项：

*   延迟加载的内容只有在加载后才存在。
*   Dart 隐式的将`deferred as`改为了`deferred as namespace`。`loadLibrary()`返回值是`Future`。

### 异步支持（Asynchrony support）

使用`async`函数和`await`表达式实现异步操作。

当需要使用一个从`Future`返回的值时，有两个选择：

*   使用`async`和`await`。
*   使用`Future API`。

当需要从一个`Stream`获取值时，有两个选择：

*   使用`async`和异步的循环 (`await for`)。
*   使用`Stream API`。

代码使用了`async`和`await`就是异步的，虽然看起来像同步代码。

```
await lookUpVersion()


```

要使用`await`，代码必须在函数后面标记为`async`：

```
checkVersion() async {
  var version = await lookUpVersion();
  if (version == expectedVersion) {
    
  } else {
    
  }
}


```

可以使用`try，catch和finally`配合`await`处理错误。

```
try {
  server = await HttpServer.bind(InternetAddress.LOOPBACK_IP_V4, 4044);
} catch (e) {
  
}


```

##### 声明异步函数

异步函数是一个被`async`标记的函数。

虽然异步的函数中可能执行耗时的操作，但是函数本身在调用后将会立即返回，即使函数体一条语句也没执行。

```
checkVersion() async {
  
}

lookUpVersion() async => ;


```

给函数添加`async`关键字将使函数返回一个`Future`类型。

```
String lookUpVersionSync() => '1.0.0';



Future<String> lookUpVersion() async => '1.0.0';


```

##### 在 Future 中使用 await 表达式

```
await expression


```

可以在异步函数中多次使用`await`

```
var entrypoint = await findEntrypoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);


```

在`await`表达式中，表达式通常是一个`Future`。如果表达式不是`Future` 类型，它将自动被包装为`Future`类型。`await expression`的返回值是一个对象。await 表达式使执行暂停，直到对象可用。

如果`await`不工作，确保`await`处于`async`函数中。即使是在 main 函数中，也要标记为`async`。

```
main() async {
  checkVersion();
                          // 这里使用了 await
  print('In main: version is ${await lookUpVersion()}');
}


```

##### 在 Stream 中使用异步循环

```
 
await for (variable declaration in expression) {
  
}


```

异步循环的执行流程如下：

1.  等待 stream 发出数据。
2.  执行循环体，并将变量的值设置为发出的数据。
3.  重复 1.，2. 直到 stream 对象被关闭。

结束监听 stram 可以使用`break` 或`returen` 语句，跳出`for`循环，取消订阅 stream。

如果异步循环不工作，确保是在一个`async`函数中，如果使用异步循环在`main()`函数中，也要确保`main()`函数被标记为`async`。

```
main() async {
  ...
  await for (var request in requestServer) {
    handleRequest(request);
  }
  ...
}


```

### 可调用类（Callable classes）

Dart 语言中为了能够让类像函数一样能够被调用，可以实现`call()`方法。

```
class WannabeFunction {
  call(String a, String b, String c) => '$a $b $c!';
}

main() {
  var wf = new WannabeFunction();
  var out = wf("Hi","there,","gang");
  print('$out'); 
  print(wf.runtimeType); 
  print(out.runtimeType); 
  print(wf is Function); 
}


```

### 隔离（Isolates）

Dart 代码都运行在独立的隔离空间中，每个隔离空间都有自己的内存堆栈，确保每个隔离空间的状态不会被其它的隔离空间访问。

### 类型定义（Typedefs）

`typedef`关键字，用来声明一种类型，当一个函数类型分配给一个变量时，保留类型信息。

```
// 声明一种 Compare类型
typedef int Compare(int a, int b);

int sort(int a, int b) => a - b;

main() {
  assert(sort is Compare); // True!
}


```

**当前 typedef 仅用于函数类型**。

### 元数据（Metadata）

元数据是以`@`开始的修饰符。在`@` 后面接着编译时的常量或调用一个常量构造函数。

所有 dart 代码中可用的三个注解：

*   @deprecated 被弃用的
*   @override 重载
*   @proxy 代理

##### 定义自己的元数据

通过`library`来定义一个库，在库中定义一个相同名字的`class`，然后在类中定义`const` 构造方法。

```
library todo;

class todo {
  final String who;
  final String what;

  const todo(this.who, this.what);
}


import 'todo.dart';

@todo('seth', 'make this do something')
void doSomething() {
  print('do something');
}


```

元数据可以修饰`library`(库), `class`(类), `typedef`(类型定义), `type parameter`(类型参数), `constructor`(构造函数), `factory`(工厂函数), `function`(函数), `field`(作用域), `parameter`(参数), 或者 `variable declaration`(变量声明)。

可以使用`reflection`反射，在运行时检查元数据。

### 注释（Comments）

*   单行注释：`/`
*   多行注释：`/*...*/`
*   文档注释：`/**...*/`或`///` 使用`[]`引用参考生成 API 文档链接

```
class Llama {
  String name;// 这是单行注释
  void feed(Food food) {
    /*
    这是多行注释
    /
  }

/**
这里是文档注释
*/
  /// Exercises your llama with an [activity] for
  /// [timeLimit] minutes.
  void exercise(Activity activity, int timeLimit) {
    // ...
  }
}


```