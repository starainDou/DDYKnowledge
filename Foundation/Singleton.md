单例模式:一个类只有一个实例对象
使用场合:在整个应用程序共享一份资源

* 写法一：GCD的一次函数（dispatch_once_t）

ARC
```
#pragma mark - 单例对象

static DDYDebugTool *_instance;

+ (instancetype)sharedManager {
    return [[self alloc] init];
}

+ (instancetype)allocWithZone:(struct _NSZone *)zone {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        _instance = [super allocWithZone:zone];
    });
    return _instance;
}

- (id)copyWithZone:(NSZone *)zone {
    return _instance;
}

- (id)mutableCopyWithZone:(NSZone *)zone {
    return _instance;
}
```

MRC 添加下面三个方法
```
- (oneway void)release {
}
- (instancetype)retain {
    return _instance;
}
- (NSUInteger)retainCount {
    return MAXFLOAT;
}
```

[一劳永逸宏定义](https://www.jianshu.com/p/4867dc92337e)
```
#define singleH(name) +(instancetype)share##name;

#if __has_feature(objc_arc)

#define singleM(name) static id _instance;\
+(instancetype)allocWithZone:(struct _NSZone *)zone\
{\
    static dispatch_once_t onceToken;\
    dispatch_once(&onceToken, ^{\
        _instance = [super allocWithZone:zone];\
    });\
    return _instance;\
}\
\
+(instancetype)share##name\
{\
    return [[self alloc]init];\
}\
-(id)copyWithZone:(NSZone *)zone\
{\
    return _instance;\
}\
\
-(id)mutableCopyWithZone:(NSZone *)zone\
{\
    return _instance;\
}
#else
#define singleM static id _instance;\
+(instancetype)allocWithZone:(struct _NSZone *)zone\
{\
static dispatch_once_t onceToken;\
dispatch_once(&onceToken, ^{\
_instance = [super allocWithZone:zone];\
});\
return _instance;\
}\
\
+(instancetype)shareTools\
{\
return [[self alloc]init];\
}\
-(id)copyWithZone:(NSZone *)zone\
{\
return _instance;\
}\
-(id)mutableCopyWithZone:(NSZone *)zone\
{\
return _instance;\
}\
-(oneway void)release\
{\
}\
\
-(instancetype)retain\
{\
    return _instance;\
}\
\
-(NSUInteger)retainCount\
{\
    return MAXFLOAT;\
}
#endif
```


* 写法二：线程锁+懒加载

```
static DDYDebugTool *_instance;
+ (instancetype)allocWithZone:(struct _NSZone *)zone {
    //线程锁
    @synchronized (self) {
        if (_instance == nil) {
            _instance = [super allocWithZone:zone];
        }
    }
    return _instance;
}

+ (instancetype)sharedManager {
    return [[self alloc] init];
}
```

* 写法三：直接懒加载(超级不安全，不推荐)
