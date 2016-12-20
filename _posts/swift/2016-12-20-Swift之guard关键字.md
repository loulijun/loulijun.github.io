---
layout: post
title: swift之guard关键字
tags: swift guard
date: 2016-12-20 16:10:24.000000000 +09:00
---

### 前言
看了很多关于guard关键字的文章，但是还是不能够很好的理解它，而且网上的很多文章示例在我亲自测试的情况下大部分都编译不了，可能是姿势不对，也有些是写法有误，不过这反而让我更困惑，所以在swift3下我梳理了下这部分的内容，打算对guard关键字一探究竟

### guard关键字是什么

guard关键字在官方文档中的在Early Exit章节，具体解释如下：

>“A guard statement, like an if statement, executes statements depending on the Boolean value of an expression. You use a guard statement to require that a condition must be true in order for the code after the guard statement to be executed. Unlike an if statement, a guard statement always has an else clause—the code inside the else clause is executed if the condition is not true.”

摘录来自: Apple Inc. “The Swift Programming Language (Swift 3.0.1)”。 iBooks. 

翻译为中文就是：

> guard语句类似于if语句，是否执行语句也是基于Boolean表达式，使用guard关键字需要条件为true才会让guard后面的语句执行。与if语句不通，guard语句一直需要有一个else语句块，else语句块是条件不满足时需要执行的代码

### guard与if的区别

其实guard可以理解为对if的一种优化，可以让代码写的更优雅，如官方稳定的标题**Early Exit**，而这也是guard关键字最关键的作用，让检查在最开始的时候完成，而不是通过一大坨判断然后将需要执行的语句放到if语句块内

另外使用guard有很多好处：

* 而guard关键字为什么一定要有个else呢？其实就是想让你检查你期望的条件，而不是通过一堆判空检查你不希望的条件，这样更加接近自然语言，更易理解
* 如果检查通过，会自动将optional变量解包，以便后面的语句使用
* 可以尽早**Early Exit**将bad cases过滤掉，而不希望变成通过一堆层层的if判断这种金字塔式的写法，这样代码看起来也更加优雅

啰嗦了这么多，让我们用一个🌰来解释下guard与if的不同吧

```
import Foundation

var hi: String? = "hello world"

func testGuard(say: String?) {
    guard let sayHi = say else {
        print("sayHi is nil")
        return
    }
    
    print(sayHi)
}

func testIf(say: String?) {
    if let sayHi = say {
        print(sayHi)
    }
}

testGuard(say: hi)
testIf(say: hi)

```

**注**：如果guard的代码块中不写break或return，代码会无法编译，报错：'guard' body may not fail through, consider using 'return' or 'break' to exit the scope

当然也有人持不同观点，认为guard重构后的代码反而更加难懂了。我个人的观点是看使用场景吧，很多场景（如复杂的逻辑条件判断）不一定适合使用guard，当然对于大多简单的条件判断我是会使用guard的

**参考资料及优秀文章**

* [Swift guard statement](http://ericcerney.com/swift-guard-statement/)
* [关于guard的另一种观点](http://swift.gg/2016/01/14/another-take-on-guard/)
* [使用guard的正确姿势](http://swift.gg/2016/02/14/swift-guard-radix/)