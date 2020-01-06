> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.cnblogs.com/yangyxd/p/10411114.html

下面这段代码的输出是什么？
-------------

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

```
import 'dart:async';
main() {
  print('main #1 of 2');
  scheduleMicrotask(() => print('microtask #1 of 2'));
 
  new Future.delayed(new Duration(seconds:1),
                     () => print('future #1 (delayed)'));
  new Future(() => print('future #2 of 3'));
  new Future(() => print('future #3 of 3'));
 
  scheduleMicrotask(() => print('microtask #2 of 2'));
 
  print('main #2 of 2');
}

```

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

 答案在下面：

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

```
1 main #1 of 2
2 main #2 of 2
3 microtask #1 of 2
4 microtask #2 of 2
5 future #2 of 3
6 future #3 of 3
7 future #1 (delayed)

```

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

main 方法中的普通代码都是同步执行的，所以肯定是 main 打印先全部打印出来，等 main 方法结束后会开始检查 microtask 中是否有任务，若有则执行，执行完继续检查 microtask，直到 microtask 列队为空。所以接着打印的应该是 microtask 的打印。最后会去执行 event 队列（future）。由于有一个使用的 delay 方法，所以它的打印应该是在最后的。  
原文：https://blog.csdn.net/meiyulong518/article/details/81773365