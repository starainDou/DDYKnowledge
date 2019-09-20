> # nil

nil -> Null-pointer to objective- c object

nil 特对于表示Object-C的除Class类型外的对象为NULL,表示其对象指针不指向任何对象。NSArray末用nil来表示结束。

> # Nil

NIL -> Null-pointer to objective- c class

NIL 特对于表示Object-C的Class类型对象为NULL,表示其对象指针不指向任何对象。


> # Null

NULL其实就是C\C++的用法，用来表示一个对象指针不指向任何对象，其值为0，

> # NSNull

NSNull是一个类，它只有一个方法：+ (NSNull *) null;　



* nil与Nil可以简单的理解为Object-C自己搞的对objective- c的NULL表示，用来表示一个对象指针不指向任何对象。但是当在对象指针为nil 或NIL的对象上调用方法或访问成员变量时，返回NO，不会抛异常，程序将继续执行下去。发给nil对象的消息返回值取决于其方法的返回类型。如果返回方法返回某个指针类型（例如对象指针），则返回值是nil.表示安全地将消息传递给了nil对象-nil仅起着传递作用。如果返回方法是返回类型长度和指针类型相等或更小的int话，返回值是零。如果返回值是浮点类型或结构体的话，将得到某个未定义的结果。

* 当在对象指针为NULL的对象上调用方法或访问成员变量时，会抛异常

* [NSNull null]用来在NSArray和NSDictionary中加入非nil（表示列表结束）的空值.   [NSNull null]是一个对象，用来表示空，他用在不能使用nil的场合。