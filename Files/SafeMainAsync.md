很多时候需要返回主线程执行操作，此时最简单粗暴的方式就是直接

```
// OC
dispatch_async(dispatch_get_main_queue(), ^{
	// code
});
// Swift
DispatchQueue.main.async {
	// code
}
```

但是这样并不友好，比如当前如果已经时主线程，所以就有了safe的形式

```
// OC 早期SDWebImage
#define dispatch_main_sync_safe(block)\
	if ([NSThread isMainThread]) {\
		block();\
	} else {\
		dispatch_sync(dispatch_get_main_queue(), block);\
	}
#define dispatch_main_async_safe(block)\
	if ([NSThread isMainThread]) {\
		block();\
	} else {\
		dispatch_async(dispatch_get_main_queue(), block);\
	}

// Swift 早期Kingfisher
extension DispatchQueue {
    func safeAsync(_ block: @escaping ()->()) {
        if self === DispatchQueue.main && Thread.isMainThread {
            block()
        } else {
            async { block() }
        }
    }
}
```

后来有人遇到主线程非主队列[issue](http://blog.benjamin-encz.de/post/main-queue-vs-main-thread/)

所以后来SDWebImage中更改了判断方式

```
// OC SDWebImage
#ifndef dispatch_queue_async_safe
#define dispatch_queue_async_safe(queue, block)\
    if (dispatch_queue_get_label(DISPATCH_CURRENT_QUEUE_LABEL) == dispatch_queue_get_label(queue)) {\
        block();\
    } else {\
        dispatch_async(queue, block);\
    }
#endif

#ifndef dispatch_main_async_safe
#define dispatch_main_async_safe(block) dispatch_queue_async_safe(dispatch_get_main_queue(), block)
#endif
```


Kingfisher中貌似没改

理应这个样子

```
// Swift 自己瞎写的，认真你就输了
import Foundation

extension DispatchQueue {

    fileprivate static var currentQueueLabel: String? {
        let cString = __dispatch_queue_get_label(nil)
        return String(cString: cString)
    }

    fileprivate static var isMainQueue: Bool {
        return currentQueueLabel == self.main.label
    }
}

func ddyMainAsyncSafe(_ execute: @escaping () -> Void) {
    DispatchQueue.isMainQueue ? execute() : DispatchQueue.main.async(execute: execute)
}
```
