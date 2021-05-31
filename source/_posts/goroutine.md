---
title: goroutine
---

这篇文章记录了我对 `goroutine` 的一些理解。

## goroutine

Go 语言使用 goroutine  处理并发。每个 Go 程序都有一个最基础的 goroutine，main 函数执行在这个 goroutine 上。同时，可以在代码中用 `go` 关键字开启一个新的 goroutine。

Go 程序生命周期中，最长的 goroutine 是 main 函数所在的 goroutine。这个 goroutine 一旦结束，整个程序的所有 goroutine 都会结束，不论它们有没有完成任务。

## Channel

Go 语言中，不同的 goroutine 之间可以通过 Channel 通信。

每个 Channel 都有两个状态：`buffered` 和 `unbuffered` ，定义 Channel 时可以指定初始状态。

改变 Channel 状态的运算符是 `<-`。这个运算符左边的 Channel 会被设为 `unbuffered` 状态，右边的 Channel 会被设为 `buffered` 状态。

### 当一个 goroutine 想要设置一个 Channel 的状态时，如果：

- 这个 Channel 的目标状态与当前状态不同：

  设置会立马生效，Channel 的状态立刻改变。

- 这个 Channel 的目标状态与当前状态相同：

  这个设置不会立刻生效。当前 goroutine 会等待这个 Channel 变为与当前不同的状态，然后再改变这个 Channel 的状态。

- 这个 Channel 目标状态与当前状态相同，且没有其他 goroutine 正在运行：

  Go Runtime 会报错，因为当前 goroutine 永远不可能等到 Channel 到达自己想要的状态。整个程序已经锁死在这里。

使用 Channel 的这个特性，可以确保不同 goroutine 之内代码的执行顺序受控制。