performSelector传参一般不能超过2个，但我们有时需要传递两个以上的参数，那就不能直接用performSelector了。这里提供了两种方案：

1.用NSInvocation代替performSelector

2.将参数进行封装，如:结构体、Class、字典等，然后定义一个传递单参的方法即可


```
- (void)test {
    // 方法签名(方法的描述)
    NSMethodSignature *singture = [[self class] instanceMethodSignatureForSelector:@selector(test1:test2:test3:test4:)];
    // 包装方法
    NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:singture];
    // 设置方法调用者
    [invocation setTarget:self];
    // 设置需要调用的方法，必须和NSMethodSignature里面的SEL方法一样
    [invocation setSelector:@selector(test1:test2:test3:test4:)];
    NSString *test1 = @"invocation test";
    int test2 = 2;
    NSNumber *test3 = @12;
    NSArray *test4 = @[@"test1" ,@"test2" ,@"test3" ,@"test4"];
    // 0表示target, 1为_cmd,  所以保存从索引2幵始
    [invocation setArgument:&test1 atIndex:2];
    [invocation setArgument:&test2 atIndex:3];
    [invocation setArgument:&test3 atIndex:4];
    [invocation setArgument:&test4 atIndex:5];
    [invocation retainArguments];
    // retain所有参数,防止参数被释放dealloc
    [invocation invoke];
}

- (void)test1:(NSString *)test1 test2:(int)test2 test3:(NSNumber *)test3 test4:(NSArray *)test4 {
    NSLog(@"\n%@\n%d\n%@\n%@", test1, test2, test3, test4);
}
```


有返回值情况

```
- (void)test {
    // 方法签名(方法的描述)
    NSMethodSignature *singture = [[self class] instanceMethodSignatureForSelector:@selector(test1:test2:test3:test4:)];
    // 包装方法
    NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:singture];
    // 设置方法调用者
    [invocation setTarget:self];
    // 设置需要调用的方法，必须和NSMethodSignature里面的SEL方法一样
    [invocation setSelector:@selector(test1:test2:test3:test4:)];
    NSString *test1 = @"invocation test";
    int test2 = 2;
    NSNumber *test3 = @12;
    NSArray *test4 = @[@"test1" ,@"test2" ,@"test3" ,@"test4"];
    // 可以有返回参数 这里举例子为NSString类型返回值
    NSString *returnValue = @"我是原始值";
    // 0表示target, 1为_cmd,  所以保存从索引2幵始
    [invocation setArgument:&test1 atIndex:2];
    [invocation setArgument:&test2 atIndex:3];
    [invocation setArgument:&test3 atIndex:4];
    [invocation setArgument:&test4 atIndex:5];
    [invocation retainArguments];
    // retain所有参数,防止参数被释放dealloc
    [invocation invoke];
    [invocation getReturnValue:&returnValue];
    NSLog(@"%@", returnValue);
}

- (NSString *)test1:(NSString *)test1 test2:(int)test2 test3:(NSNumber *)test3 test4:(NSArray *)test4 {
    NSLog(@"\n%@\n%d\n%@\n%@", test1, test2, test3, test4);
    return @"我是返回值";
}
```

