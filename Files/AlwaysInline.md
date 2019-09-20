__attribute__((always_inline)) 的意思是强制内联，所有加了__attribute__((always_inline)) 的函数再被调用时不会被编译成函数调用而是直接扩展到调用函数体内

比如定义了函数

```
__attribute__((always_inline)) void a()

void b()｛   a(); ｝

```

b 调用 a 函数的汇编代码不会是跳转到a执行，而是 a 函数的代码直接在 b 内成为 b 的一部分

#define __inline __attribute__((always_inline)) 的意思就是用
__inline 代替__attribute__((always_inline))
内声明a的时候可以直接写成__inline void a() 这样比较方便因为__attribute__((always_inline)) 字多。
