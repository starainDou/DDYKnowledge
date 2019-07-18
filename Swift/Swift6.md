# Swift6-多线程

> ## 相关概念

* 进程      
指在系统中正在运行的一个应用程序，进程拥有独立运行所需的全部资源（例如：正在运行的QQ就是一个进程）。

* 线程     
指程序中独立运行的代码段（例如：接收QQ消息的代码），一个进程是由一或多个线程组成。

* 多线程     
1个进程中可以开启多条线程，每条线程可以并行（同时）执行不同的任务，进程只负责资源的调度和分配，线程才是程序真正的执行单元，负责代码的执行。

* 单线程与多线程对比
 - 单线程程序：只有一个线程，代码顺序执行，容易出现代码阻塞（页面假死）。
 - 多线程程序：有多个线程，线程间独立运行，能有效的避免代码阻塞，并且提高程序的运行性能。

* 线程相关
 - 同步线程：同步线程会阻塞当前线程去执行线程内的任务，执行完之后才会反回当前线程。
 - 异步线程：异步线程不会阻塞当前线程，会开启其他线程去执行线程内的任务。
 - 串行队列：线程任务按先后顺序逐个执行（需要等待队列里面前面的任务执行完之后再执行新的任务）。
 - 并发队列：多个任务按添加顺序一起开始执行（不用等待前面的任务执行完再执行新的任务），但是添加间隔往往忽略不计，所以看着像是一起执行的。
 - 并发VS并行：并行是基于多核设备的，并行一定是并发，并发不一定是并行。

* 死锁     
死锁是指两个或两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，它们都将无法推进下去

例如主线程串行队列同步执行任务引起死锁     

```
DispatchQueue.main.sync {
	print("死锁了不执行")
}
```

自定义串行队列同步任务 嵌套 该自定义串行队列同步任务，产生死锁

```
let serialQueue = DispatchQueue(label: "com.ddy.serialQueue")
serialQueue.sync {
	print("执行了1")
	serialQueue.sync {
		print("死锁了不执行")
	}
}

```

自定义串行队列异步任务 嵌套 该自定义串行队列同步任务， 产生死锁

```
let serialQueue = DispatchQueue(label: "com.ddy.serialQueue")
serialQueue.async {
	print("执行了1")
	serialQueue.sync {
		print("死锁了不执行")
	}
}
print("执行了3")
```

总结：遇到串行同步要小心

* 死锁的四个必要条件

 - 互斥条件：一个资源每次只能被一个进程使用。
 - 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
 - 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。
 - 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。

* 避免死锁的方法    
 1. 破坏“互斥”条件:就是在系统里取消互斥。若资源不被一个进程独占使用，那么死锁是肯定不会发生的。但一般“互斥”条件是无法破坏的。因此，在死锁预防里主要是破坏其他三个必要条件，而不去涉及破坏“互斥”条件。
 2. 破坏“请求和保持”条件:在系统中不允许进程在已获得某种资源的情况下，申请其他资源。即要想出一个办法，阻止进程在持有资源的同时申请其他资源。
方法：要求每个进程提出新的资源申请前，释放它所占有的资源。这样，一个进程在需要资源S时，须先把它先前占有的资源R释放掉，然后才能提出对S的申请，即使它可能很快又要用到资源R。
 3. 破坏“不可抢占”条件：允许对资源实行抢夺。
方法一：如果占有某些资源的一个进程进行进一步资源请求被拒绝，则该进程必须释放它最初占有的资源，如果有必要，可再次请求这些资源和另外的资源。
方法二：如果一个进程请求当前被另一个进程占有的一个资源，则操作系统可以抢占另一个进程，要求它释放资源。只有在任意两个进程的优先级都不相同的条件下，该方法才能预防死锁。
 4. 破坏“循环等待”条件：将系统中的所有资源统一编号，进程可在任何时刻提出资源申请，但所有申请必须按照资源的编号顺序（升序）提出。这样做就能保证系统不出现死锁。
方法：利用银行家算法避免死锁。

[死锁参考](https://blog.csdn.net/yanxiaolx/article/details/51944048)

* 线程安全    

	一段线程安全的代码（对象），可以同时被多个线程或并发的任务调度，不会产生问题，非线程安全的只能按次序被访问。所有Mutable对象都是非线程安全的，所有Immutable对象都是线程安全的，使用Mutable对象，一定要用同步锁来同步访问（@synchronized）。   
	  
	互斥锁：能够防止多线程抢夺造成的数据安全问题，但是需要消耗大量的资源    
原子属性（atomic）加锁    

	atomic: 原子属性，为setter方法加锁，将属性以atomic的形式来声明，该属性变量就能支持互斥锁了。 
	   
	nonatomic: 非原子属性，不会为setter方法加锁，声明为该属性的变量，客户端应尽量避免多线程争夺同一资源。    
	
> ##  几种多线程技术比较

* pthread   
优点: 跨平台，可移植性强   
缺点: 程序员管理生命周期，使用难度大。一般不用

* NSThread (抽象层次：低)     
优点：轻量级，简单易用，可以直接操作线程对象   
缺点: 需要自己管理线程的生命周期，线程同步。线程同步对数据的加锁会有一定的系统开销。

* Cocoa NSOperation (抽象层次：中)      
优点：不需要关心线程管理，数据同步的事情，可以把精力放在学要执行的操作上。基于GCD，是对GCD 的封装，比GCD更加面向对象    
缺点: NSOperation是个抽象类，使用它必须使用它的子类，可以实现它或者使用它定义好的两个子类NSInvocationOperation、NSBlockOperation.

* GCD 全称Grand Center Dispatch (抽象层次：高)      
优点：是 Apple 开发的一个多核编程的解决方法，简单易用，效率高，速度快，基于C语言，更底层更高效，并且不是Cocoa框架的一部分，自动管理线程生命周期（创建线程、调度任务、销毁线程）。    
缺点: 使用GCD的场景如果很复杂，就有非常大的可能遇到死锁问题。   

GCD抽象层次最高，使用也简单，因此，苹果也推荐使用GCD  

* 为什么要用 GCD 呢？
	- GCD 可用于多核的并行运算   
	- GCD 会自动利用更多的 CPU 内核（比如双核、四核）   
	- GCD 会自动管理线程的生命周期（创建线程、调度任务、销毁线程）   
	- 程序员只需要告诉 GCD 想要执行什么任务，不需要编写任何线程管理代码  

	
> ## GCD

1.主线程串行队列（main queue）：提交至Main queue的任务会在主线程中执行，Main queue 可以通过```DispatchQueue.main```来获取,主队列一定伴随主线程，但主线程不一定伴随主队列。    
2.全局并发队列（Global queue）：全局并发队列由整个进程共享，有Qos优先级别。Global queue 可以通过调用```DispatchQueue.global()```函数来获取（可以设置优先级）    
3.自定义队列(Custom queue): 可以为串行，也可以为并发。Custom queue 可以通过```DispatchQueue(label: String)和DispatchQueue(label: String, attributes: DispatchQueue.Attributes.concurrent)```来获取；   
4.队列组 (Group queue)：将多线程进行分组，最大的好处是可获知所有线程的完成情况。
Group queue 可以通过调用dispatch_group_create()来创建，通过dispatch_group_notify 可以直接监听组里所有线程完成情况。 

* 主线程串行队列(The main queue)

	1.主线程串行队列同步执行任务，在主线程运行时，会产生死锁
	
	```
	DispatchQueue.main.sync {
		print("死锁了不执行")
	}
	```
	
	2.主线程串行队列异步执行任务，在主线程运行，不会产生死锁
	
	```
	DispatchQueue.main.async {
		print("执行了")
	}
	```
	
	3.安全异步主线程主队列
	
	```
	import Foundation

	extension DispatchQueue {
		fileprivate static var currentQueueLabel: String? {
		let cString = __dispatch_queue_get_label(nil)
		return String(cString: cString)
    	}
    	// "com.apple.main-thread"
    	fileprivate static var isMainQueue: Bool {
		return currentQueueLabel == self.main.label
    	}
	}    
	
	func ddyMainAsyncSafe(_ execute: @escaping () -> Void) {
		DispatchQueue.isMainQueue ? execute() : DispatchQueue.main.async(execute: execute)
	}
	
	// 调用
	ddyMainAsyncSafe {
		print("主线程主队列刷新UI")
	}	
	```
	
	附：RxSwift中判断主线程主队列方式
	
	```
	extension DispatchQueue {
		private static var token: DispatchSpecificKey<()> = {
			let key = DispatchSpecificKey<()>()
			DispatchQueue.main.setSpecific(key: key, value: ())
			return key
		}()

		static var isMain: Bool {
			return DispatchQueue.getSpecific(key: token) != nil
		}
	}
	```

	注意：主线程串行队列由系统默认生成，无法调用conQueue.resume()和Queue.suspend()来控制执行继续或中断。

* 全局并发队列(Global queue)
	
	耗时操作(如网络请求，IO，数据库读写等)在子线程中处理，然后通知主线程更新界面
	
	1.全局并发队列同步执行任务，在主线程执行会导致页面卡顿。
	
	```
	print("同步执行任务 1")
	DispatchQueue.global().sync {
		print("同步执行任务 2")
	}
	print("同步执行任务 3")
	```
	2 全局并发队列异步执行任务，会开启新的子线程去执行任务，页面不会卡顿。
	
	```
	print("异步执行任务 1")
	DispatchQueue.global().async {
		print("异步执行任务 2")
	}
	print("异步执行任务 3")	
	```
	
	3 多个全局并发队列，异步执行任务。
	
	```
	print("全局并发队列 异步执行任务 1")
	DispatchQueue.global().async {
		print("全局并发队列 异步执行任务 2")
	}
	DispatchQueue.global().async {
		print("全局并发队列 异步执行任务 3")
	}
	print("全局并发队列 异步执行任务 4")
	```
	
	异步线程的执行顺序是不确定的，几乎同步开始执行，2和3 顺序不确定   
	全局并发队列由系统默认生成，无法调用conQueue.resume()和Queue.suspend()来控制执行继续或中断。
	
	附 
	```
	// Swift3开始使用了DispatchWorkItem类将任务封装成为对象，由对象进行任务。
	let item = DispatchWorkItem {
		// do task
	}
	DispatchQueue.global().async(execute: item)

	// 也可以使用DispatchWorkItem实例对象的perform方法执行任务
	let workItem = DispatchWorkItem {
		// do task
	}
	DispatchQueue.global().async {
		workItem.perform()
	}
	```
	
* 自定义队列 (Custom queue)

	1 自定义串行队列同步执行任务(依次执行)
	
	```
	let serialQueue = DispatchQueue(label: "com.ddy.serialQueue")
	print("自定义串行队列同步执行任务 1")
	serialQueue.async {
		print("自定义串行队列同步执行任务 2")
	}
	serialQueue.async {
		print("自定义串行队列同步执行任务 3")
	}
	print("自定义串行队列同步执行任务 4")
	```
	
	2 自定义串行队列同步任务 嵌套 该自定义串行队列同步任务，产生死锁
	
	```
	let serialQueue = DispatchQueue(label: "com.ddy.serialQueue")
	serialQueue.sync {
		print("执行了1")
		serialQueue.sync {
			print("死锁了不执行")
		}
	}
	print("没执行")
	```
	
	3 自定义串行队列同步任务嵌套并发队列同步任务 不死锁
	
	```
	let serialQueue = DispatchQueue(label: "com.ddy.serialQueue")
	let globalQueue = DispatchQueue.global()
	let concurrentQ = DispatchQueue(label: "concurrentQueue1", attributes: .concurrent)
	serialQueue.sync {
		print("执行了1")
		globalQueue.sync {
			print("执行了2")
		}
		concurrentQ.sync {
			print("执行了3")
		}
	}
	print("执行4")
	```
	
	4 自定义串行队列同步执行任务 嵌套 另一个自定义串行队列同步任务 不死锁
	
	```
	let serialQueue1 = DispatchQueue(label: "com.ddy.serialQueue1")
	let serialQueue2 = DispatchQueue(label: "com.ddy.serialQueue2")
	serialQueue1.sync {
		print("执行了1")
		serialQueue2.sync {
			print("执行了2")
		}
	}
	print("执行了3")
	```
	
	5 自定义串行队列异步执行任务 嵌套 该自定义串行队列同步任务，产生死锁
	
	```
	let serialQueue = DispatchQueue(label: "com.ddy.serialQueue")
	serialQueue.async {
		print("执行了1")
		serialQueue.sync {
			print("死锁了不执行")
		}
	}
	print("执行了3")
	```
	总结：遇到嵌套串行队列同步任务要小心 可能 会死锁
	
	
	6 自定义并发队列执行同步任务(顺序执行)
	
	```
	let concurrentQ = DispatchQueue(label: "concurrentQueue1", attributes: .concurrent)
	print("执行了 1")
	concurrentQ.sync {
		print("执行了 2")
	}
	concurrentQ.sync {
		print("执行了 3")
	}
	print("执行了 4")
	```
	
	7 自定义并发队列同步任务 嵌套 该自定义并发队列同步任务 (顺序执行 不会死锁)
	
	```
	let concurrentQ = DispatchQueue(label: "concurrentQueue1", attributes: .concurrent)
		print("执行了 1")
		concurrentQ.sync {
			print("执行了 2")
			concurrentQ.sync {
				print("执行了 3")
			}
		}
	print("执行了 4")
	```
	
	8 自定义并发队列执行异步任务（2，3不确定顺序）
	
	```
	let concurrentQ = DispatchQueue(label: "concurrentQueue1", attributes: .concurrent)
	print("执行了 1")
	concurrentQ.async {
		print("执行了 2")
	}
	concurrentQ.async {
		print("执行了 3")
	}
	print("执行了 4")
	```
	
	队列便利构造器
	
	- label: 队列的唯一标识符(用于调试)
	- qos: 队列的优先级(.userInteractive .userInitiated .default .utility .background .unspecified)
		- .userInteractive：用户交互相关，为了好的用户体验，任务需要立马执行。使用该优先级用于UI更新，事件处理和小工作量任务，在主线程执行。
		- .userInitiated：优先级等同于DISPATCH_QUEUE_PRIORITY_HIGH,需要立刻的结果
		- .default：默认优先级,优先级等同于DISPATCH_QUEUE_PRIORITY_DEFAULT，建议大多数情况下使用默认优先级
		- .utility：优先级等同于DISPATCH_QUEUE_PRIORITY_LOW，可以执行很长时间，再通知用户结果。比如：下载一个大文件，网络，计算
		- .background：最低优先级，等同于DISPATCH_QUEUE_PRIORITY_BACKGROUND. 用户不可见，比如：在后台存储大量数据
		- .unspecified：未定义
	- attributes: 队列属性(attributes是一个结构体并遵守OptionSet协议，所以传入的参数可以为[.option1, .option2])
		- .concurrent并发队列，
		- .initiallyInactive表明队列需要手动开启。不填写时默认队列串行、自动执行
	- autoreleaseFrequency:自动释放频率，有些列队会在执行完任务之后自动释放，有些是不会自动释放的，需要手动释放。官方文档是说当设为.workItem时，所有异步任务提交的代码块会被封装成独立的任务，在同步执行的队列则不受影响。
	- target:目标队列
	
	```
	// 队列的便利构造函数(便利构造器)
	public convenience init(label: String, qos: DispatchQoS = .unspecified, attributes: DispatchQueue.Attributes = [], autoreleaseFrequency: DispatchQueue.AutoreleaseFrequency = .inherit, target: DispatchQueue? = nil)
	// 优先级顺序:userInteractive> userInitiated> default> utility> background> unspecified
	
	let label = "com.ddy.concurrentQueue"
	let qos = DispatchQoS.default
	let attributes = DispatchQueue.Attributes.concurrent
	let autoreleaseFrequnecy = DispatchQueue.AutoreleaseFrequency.never
	let queue = DispatchQueue(label: label, qos: qos, attributes: attributes, autoreleaseFrequency: autoreleaseFrequnecy, target: nil)
	```
	
	等待任务结束
	
	```
	let concurrentQ = DispatchQueue(label: "concurrentQueue1", attributes: .concurrent)
	// 可以在初始化的时候指定更多的参数
	let workItem = DispatchWorkItem(qos: .default, flags: .barrier) {
		sleep(5)
		print("done")
	}
	concurrentQ.async(execute: workItem)
	print("before waiting")
	workItem.wait()
	print("after waiting")
	// before waiting
	// done
  	// after waiting
	```

* 队列组(Group queue)   

	场景1: A、B、C 三个任务并发异步执行，都执行完才执行D任务(三个网络请求和都请求完毕才刷新UI)   
	
	```
	// 并发队列
	let concurrentQ = DispatchQueue(label: "com.ddy.concurrentQueue", attributes: .concurrent)
	// 创建组
	let group = DispatchGroup()
	// 网络请求1
	group.enter()
	concurrentQ.async {
		DDYRequest.request(["test":"1"], { (success: Bool) in
			group.leave()
		})
	}
	// 网络请求2
	group.enter()
	concurrentQ.async {
		DDYRequest.request(["test":"2"], { (success: Bool) in
			group.leave()
		})
	}
	// 调度组里的任务都执行完毕执行
	group.notify(queue: concurrentQ) {
		DispatchQueue.global().async {
			// 处理数据
			DispatchQueue.main.async {
				// 刷新UI
			}
		}
	}
	```

	场景2:A执行完才执行B(第一次请求网络拿到ID去再次请求网络拿具体数据,最后刷新UI)

	```
	// 并发队列
	let concurrentQ = DispatchQueue(label: "com.ddy.concurrentQueue", attributes: .concurrent)
	// 创建组
	let group = DispatchGroup()
	// 网络请求1
	group.enter()
	concurrentQ.async {
		DDYRequest.request(["test":"1"], { (success: Bool) in
			print("离开 1")
			group.leave()
	    })
	}
	group.wait()
	// 网络请求2
	group.enter()
	concurrentQ.async {
		DDYRequest.request(["test":"2"], { (success: Bool) in
			print("离开 2")
			group.leave()
	    })
	}
	// 调度组里的任务都执行完毕执行
	group.notify(queue: concurrentQ) {
		DispatchQueue.global().async {
			print("处理数据")
			DispatchQueue.main.async {
				print("刷新UI")
			}
		}
	}
	```
	
	group.wait(timeout: DispatchTime(uptimeNanoseconds: 10*NSEC_PER_SEC)) 来表示等待与超时
	
> ## GCD一些常用函数

* asyncAfter 延迟添加调用

	asyncAfter并不是在指定时间后执行任务处理，而是在指定时间后把任务追加到queue里面。因此会有少许延迟。 

	```
	DispatchQueue.global().asyncAfter(deadline: DispatchTime.now()+2.0) {
		print("2秒后执行的")
	}
	// let delay = DispatchTime.now() + Double(Int64(3 * 1000 * 1000000)) / Double(NSEC_PER_SEC)
	// let delay = DispatchTime.now() + DispatchTimeInterval.seconds(10)
	let delay = DispatchTime.now() + 10
	DispatchQueue.main.asyncAfter(deadline: delay) {
		print("10秒后执行的")
	} 
	// 注意:我们不能直接取消我们已经提交到 asyncAfter 里的任务代码。
	// 如需取消正在等待执行的Block操作，可先将这个Block封装到DispatchWorkItem对象中，然后对其发送cancle，来取消一个正在等待执行的block
	// 将要执行的操作封装到DispatchWorkItem中
	let task = DispatchWorkItem { print("3秒后执行被取消") }
	// 延时2秒执行
	DispatchQueue.main.asyncAfter(deadline: DispatchTime.now() + 3.0, execute: task)
	// 取消任务
	task.cancel()
	```  
* 循环执行concurrentPerform(OC快速迭代 apply)     

	OC中GCD的dispatch_apply()，而Swift中用concurrentPerform()
	
	```
	let concurrentQ = DispatchQueue(label: "com.ddy.concurrentQueue", attributes:.concurrent)
	let array = ["1", "3", "5", "7", "9"];
	concurrentQ.async {
		DispatchQueue.concurrentPerform(iterations: array.count) { (index) in
			print("\(array[index])")
		}
	}
	// 1 9 3 5 7  可以利用多核的优势，所以无序
	// 简化 迭代五次
	// DispatchQueue.concurrentPerform(iterations: 5) {
   	// 		print("\($0)")
 	//	}
	```

* 信号量 semaphore

	创建信号量对象，调用signal方法发送信号，信号加1，调用wait方法等待，信号减1.用信号量实现刚刚的多个请求功能。
	
	```
	// DispatchSemaphore(value: )：用于创建信号量，可以指定初始化信号量计数值，这里我们默认1.
	// semaphore.wait()：会判断信号量。如果是0，则等待，如果非0，则往下执行
	// semaphore.signal()：代表运行结束，信号量加1，有等待的任务这个时候才会继续执行。
	let queue = DispatchQueue.global()
   let group = DispatchGroup()
   let semaphore = DispatchSemaphore(value: 0)
 
   queue.async(group: group) {
       DispatchQueue.main.asyncAfter(deadline: .now() + 0.6, execute: {
           semaphore.signal()
           print("Task one finished")
       })
       semaphore.wait()
   }
   queue.async(group: group) {
       DispatchQueue.main.asyncAfter(deadline: .now() + 1.5, execute: {
           semaphore.signal()
           print("Task two finished")
       })
       semaphore.wait()
   }
   queue.async(group: group) {
       print("Task three finished")
   }
 
   group.notify(queue: queue) {
       print("All task has finished")
   }
	```
	
* 栅栏操作 barrier

	GCD里的Barrier和NSOperationQueue的dependency比较接近    
	只能用在自定义并行队列，且只保证任务中代码执行到，如果任务中存在异步则不保证执行完

	```
	let concurrentQ = DispatchQueue(label: "com.ddy.barrier", attributes: .concurrent)
	concurrentQ.async {
		print("task 0-0")
		DDYRequest.request(2, { (success: Bool) in
			print("task 0-1")
		})
		print("task 0-3")
	}
	concurrentQ.async {
		print("task 1-0")
		DDYRequest.request(2, { (success: Bool) in
			print("task 1-1")
		})
		print("task 1-3")
	}
	concurrentQ.async(flags: .barrier) {
		print("task 2-0")
		DDYRequest.request(2, { (success: Bool) in
			print("task 2-1")
		})
	}
	concurrentQ.async {
		print("task 3-0")
		DDYRequest.request(2, { (success: Bool) in
			print("task 3-1")
		})
	}
	```

	```
	let concurrentQ = DispatchQueue(label: "com.ddy.barrier", attributes: .concurrent)
	concurrentQ.async {
		print("task 0-0")
		DDYRequest.request(2, { (success: Bool) in
			print("task 0-1")
		})
		print("task 0-3")
	}
	concurrentQ.async {
		print("task 1-0")
		DDYRequest.request(2, { (success: Bool) in
			print("task 1-1")
		})
		print("task 1-3")
	}
	let writeTask = DispatchWorkItem(flags: .barrier) {
		print("task 2-0")
		DDYRequest.request(2, { (success: Bool) in
			print("task 2-1")
		})
	}
	concurrentQ.async(execute: writeTask)
	concurrentQ.async {
		print("task 3-0")
		DDYRequest.request(2, { (success: Bool) in
			print("task 3-1")
		})
	}
	```
	
	执行顺序分析    
	先排除各个任务中异步延迟操作(该操作已经执行到，但不保证执行完回调)    
	0-0         
	(0-3 1-0 1-3) or (1-0 1-3 0-3) or (1-0 0-3 1-3)    
	2-0   
	然后分析异步延迟的回调   
	(0-1 1-1 3-1) or (0-1 3-1 1-1) or (1-1 0-1 3-1) or (1-1 3-1 0-1) or (3-1 0-1 1-1) or (3-1 1-1 0-1)    
	2-1    
	
* DispatchSource实现GCD定时器

	Timer的无奈:
	Timer的创建与撤销必须在同一个线程操作,在多线程环境下使用不便.
	使用时必须保证有一个活跃的runloop,然而主线程的runloop是默认开启的,子线程的runloop却是默认不开启的,当在子线程中使用Timer的时候还需要先激活runloop,否则Timer是不会起效的.
	内存泄漏问题. 在控制器中使用Timer的时候需要控制器对Timer进行强引用,然而Timer还会对控制器进行强引用,造成循环引用最终控制器无法释放导致内存泄漏.

	```
	private class func testGCDTimer() {
		// 倒计时总次数
		var timeCount = 20
		// 自定义并发队列
		let concurrentQ = DispatchQueue(label: "com.ddy.timer", attributes: .concurrent)
		// 在自定义队列的定时器
		let timer = DispatchSource.makeTimerSource(flags: [], queue: concurrentQ)
		// 设置立即开始 0.5秒循环一次
		timer.schedule(deadline: .now(), repeating: 0.5)
		// 触发回调事件
		timer.setEventHandler {
			timeCount = timeCount - 1
			if timeCount <= 0 {
				timer.cancel()
			}
			DispatchQueue.main.async {
				print("主线程更新UI \(timeCount)")
			}
		}
		// cancel事件回调
		timer.setCancelHandler {
			DispatchQueue.main.async {
				print("已结束I \(timeCount)")
			}
		}
		// 启动定时器
		timer.resume()
	}
	```

	
[参考 Swift4 - GCD的使用](https://blog.csdn.net/longshihua/article/details/79756676)
[参考 Swift4.0 - GCD](https://www.jianshu.com/p/96032a032c7c)
[参考 多线程之GCD](https://blog.csdn.net/longshihua/article/details/50523051)
[参考 从使用场景了解GCD新API](https://www.jianshu.com/p/fc78dab5736f)