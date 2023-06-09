---
title: "golang"
subtitle: ""
description: ""
date: 2023-05-20T13:05:00+08:00
draft: true
authors: [索隆不喝酒]

tags: []
series: [card]
categories: [灵感、文献笔记（非永久笔记）]
series_weight: 1
seriesNavigation: true
featuredImage: ""
featuredImagePreview: ""
---
<!--more-->

我想让你充当 Golang 开发工程师面试官，你可能会想要对面试者进行全面的评估，接下来我会给你一些关键词，使用「」符号包含，比如「select」，它可能是一个语法，或者一个关键词，你需要针对这个内容，提出尽可能多的相关的问题，达到全面考核面试者对于这个知识点的掌握程度

## 针对 select 语法可能提到的问题

当面试者提到关键词「select」时，以下是一些可能的问题，可以用来评估他们对该知识点的掌握程度：

1. 请解释一下在 Go 中的「select」语句的作用是什么？

2. 「select」语句用于什么类型的操作？

3. 「select」语句与「switch」语句有何区别？

4. 在使用「select」语句时，可以选择多少个 case？

5. 「select」语句如何处理多个可执行的 case？

6. 当多个 case 同时满足条件时，「select」语句如何选择执行哪个 case？

7. 你能否解释一下「select」语句的阻塞行为是怎样的？

8. 「select」语句中的 default case 的作用是什么？

9. 是否可以在「select」语句中使用超时机制？如果可以，请举例说明如何实现。

10. 在使用「select」语句时，如何处理不同 case 的返回值？

11. 「select」语句与协程（Goroutine）之间的关系是什么？

12. 是否可以在「select」语句中使用带缓冲的通道？

13. 你可以举一个实际的示例，说明在什么情况下使用「select」语句会很有用？

14. 「select」语句有没有一些常见的使用陷阱需要注意？

15. 在使用「select」语句时，有没有一些最佳实践或建议可以分享？

通过这些问题，您可以评估面试者对于 Golang 中「select」语句的理解程度、语法的熟悉程度，以及对于其使用场景和注意事项的了解。记得根据面试者的回答进行深入追问，以进一步评估他们的知识深度和实际经验。

下面是对于前面提到的15个问题的简要答案：

1. 「select」语句在Go中用于在多个通道操作中选择执行。
2. 「select」语句用于非阻塞地执行一系列通道操作，例如发送数据到通道或从通道接收数据。
3. 「select」语句与「switch」语句不同，它用于选择可执行的通道操作，而不是值的比较。
4. 「select」语句可以选择任意数量的 case。
5. 当多个 case 同时满足条件时，「select」语句会随机选择一个可执行的 case。
6. 「select」语句会根据 case 的顺序进行选择，先出现的 case 优先被选择。
7. 当没有任何一个 case 可执行时，「select」语句会阻塞当前的 Goroutine。
8. default case 会在没有其他 case 可执行时立即执行，用于避免「select」语句的阻塞。
9. 可以使用「time.After」函数结合「select」语句实现超时机制，例如：`select { case <-time.After(time.Second): fmt.Println("Timeout") }`。
10. 「select」语句无法直接获取 case 的返回值，可以使用辅助变量或通道来传递结果。
11. 「select」语句可以用于在不同的协程之间进行通信和同步操作。
12. 是的，可以在「select」语句中使用带缓冲的通道，它们的发送和接收操作不会阻塞。
13. 一个常见的使用场景是同时监听多个通道，以便在任意通道有数据可用时立即进行处理。
14. 「select」语句可能导致 Goroutine 的泄漏，需要确保在执行完「select」后，所有相关资源都被正确释放。
15. 最佳实践包括为每个 case 添加合适的超时机制、使用 default case 来处理错误情况，并仔细处理可能的竞态条件。

请注意，以上答案提供的是基本概念和常见情况下的解释，并不涵盖所有细节和特殊情况。在实际面试中，可以根据面试者的回答深入探讨相关主题，以便更好地评估他们的知识和能力。

## select 的 case 判断条件必须是 io 操作吗？

不，`select` 语句中的 `case` 判断条件并不限于只能是 I/O 操作。`case` 判断条件可以是任何可执行的表达式。

在 `select` 语句中，每个 `case` 后面跟随的表达式通常是一个 I/O 操作，比如通道的读写操作、文件的读写操作等。这是因为 `select` 语句主要用于处理并发的 I/O 操作，通过监听多个通道的可读/可写状态，从而实现非阻塞的 I/O 处理。

然而，并不局限于 I/O 操作。`case` 后面的表达式可以是任何可以求值的表达式，包括变量、函数调用、算术运算等。只要表达式的类型和可执行性满足 `select` 语句的语法要求，都可以作为 `case` 判断条件。

例如，以下是一些合法的 `select` 语句示例，其中 `case` 判断条件并不涉及 I/O 操作：

```go
select {
case x := <-ch:
    // 处理通道的读操作
case y := <-anotherCh:
    // 处理另一个通道的读操作
case z = someFunc():
    // 调用函数获取结果
case <-time.After(time.Second):
    // 等待 1 秒钟
default:
    // 默认情况
}
```

因此，`select` 语句中的 `case` 判断条件可以根据具体的需求选择不同的表达式类型，不一定限定为 I/O 操作。


## defer recover 的使用语法

```go
func something() {
    defer func() {
        if err := recover(); err != nil {
            // 捕获并处理 panic 引发的错误
        }
    }()

    // 可能发生 panic 的代码
}
```

## goroutine 相关问题

1. 什么是 goroutine？

    相比线程更加轻量，他是运行再用户态，由 go 定义的一个抽象概念 processor「扩展」来调度，上下文切换耗时更短，资源消耗更少

2. goroutine 如何实现并发？并行？

    并发是交替的，并行是一起的

    单个 p 上看，是并发的，多个 p 上看是并行的

3. grt 如何调度的？解释一下 pmg 模型

    Goroutine 的调度是由 Go 语言运行时系统中的调度器（scheduler）负责的。调度器负责决定将哪些 goroutine 调度到可用的逻辑处理器（P）上执行，并在不同的 goroutine 之间进行切换。

4. goroutine 的生命周期？


    Goroutine 的生命周期是从创建到退出的完整过程，涵盖了它的创建、执行和结束。下面是 goroutine 的典型生命周期：

    1. 创建：Goroutine 通过使用关键字 `go` 启动一个函数的执行来创建。例如，`go foo()` 将会创建一个新的 goroutine 并开始执行 `foo()` 函数。创建 goroutine 的成本非常低，可以同时创建大量的 goroutine。

    2. 就绪：一旦 goroutine 被创建，它进入就绪状态。在就绪状态下，该 goroutine 等待调度器将其放置在可用的逻辑处理器上执行。多个就绪状态的 goroutine 可能会等待执行的机会。

    3. 运行：当调度器选择一个逻辑处理器，并将就绪状态的 goroutine 分配给它时，该 goroutine 进入运行状态。在运行状态下，goroutine 开始执行其函数体内的代码，并消耗 CPU 时间。一旦它占用了逻辑处理器，它会一直执行，直到完成、阻塞或被调度器暂停。

    4. 阻塞：如果 goroutine 在执行过程中发生了阻塞操作（如等待通道操作、互斥锁等待、睡眠等），它会被标记为阻塞状态。在阻塞状态下，goroutine 不再消耗 CPU 时间，并等待满足阻塞操作的条件。一旦条件满足，它将重新进入就绪状态，等待调度器重新分配。

    5. 死亡：当 goroutine 完成其任务、函数返回或遇到错误时，它将进入死亡状态。在死亡状态下，该 goroutine 不再执行，并且它占用的资源会被回收。死亡的 goroutine 无法再次进入就绪或运行状态。

    需要注意的是，这些生命周期状态并不是严格的状态机，而是描述 goroutine 的不同状态和行为。调度器根据需要在这些状态之间进行转换，以实现并发执行和资源管理。

