* 方法一：通过苹果官方文档里面UIDevice public API来获取 (不够精确)

```
NSLog(@"电池电量：%.2f", [UIDevice currentDevice].batteryLevel);
[UIDevice currentDevice].batteryMonitoringEnabled = YES;
[[NSNotificationCenter defaultCenter] addObserverForName:UIDeviceBatteryLevelDidChangeNotification object:nil queue:[NSOperationQueue mainQueue] usingBlock:^(NSNotification *notification) {
NSLog(@"电量改变为：%.2f", [UIDevice currentDevice].batteryLevel);
}];
```

* 方法二：利用私有IOKit.framework (不够精确且违规)

* 方法三:通过runtime 获取iOS 6+ StatusBar上电池电量控件类私有变量的值 (相对精准)

MRC: Build Phase —> Compile Source -> Compiler Flags : -fno-objc-arc

```
- (int)getCurrentBatteryLevel {
    if ([UIApplication sharedApplication].applicationState != UIApplicationStateBackground) {
        void *result = nil;
        object_getInstanceVariable([UIApplication sharedApplication], "_statusBar", &result);
        id status  = result;
        for (id aview in [status subviews]) {
            for (id bview in [aview subviews]) {
                int batteryLevel = 0;
                if ([NSStringFromClass([bview class]) caseInsensitiveCompare:@"UIStatusBarBatteryItemView"] == NSOrderedSame) {
                    object_getInstanceVariable(bview, "_capacity", &result);
                    batteryLevel = (int)result;
                    NSLog(@"电池电量:%d",batteryLevel);
                    if (batteryLevel > 0 && batteryLevel <= 100) {
                        return batteryLevel;
                    } else {
                        return 0;
                    }
                }
            }
        }
    }
    return 0;
}
```

ARC: Build Phase —> Compile Source -> Compiler Flags : -fobjc-arc

```
- (int)getCurrentBatteryLevel {
    if ([UIApplication sharedApplication].applicationState != UIApplicationStateBackground) {
        Ivar ivar = class_getInstanceVariable([[UIApplication sharedApplication] class],"_statusBar");
        id status = object_getIvar([UIApplication sharedApplication], ivar);
        for (id aview in [status subviews]) {
            int batteryLevel = 0;
            for (id bview in [aview subviews]) {
                if ([NSStringFromClass([bview class]) caseInsensitiveCompare:@"UIStatusBarBatteryItemView"] == NSOrderedSame) {
                    Ivar ivar =  class_getInstanceVariable([bview class],"_capacity");
                    if(ivar) {
                        batteryLevel = ((int (*)(id, Ivar))object_getIvar)(bview, ivar);
                        if (batteryLevel > 0 && batteryLevel <= 100) {
                            return batteryLevel;
                        } else {
                            return 0;
                        }
                    }
                }
            }
        }
    }
    return 0;
}

```

条件判断

```
#if !__has_feature(objc_arc)

#else

#endif
```
