---
layout:     post
title:      Metux和WaitGroup包的使用
subtitle:   
date:       2020-12-08
author:     北天
header-img: 
catalog: true
tags:
    - 共享内存
---
# *Mutex和WaitGroup*

## 1 sync.Mutex 

用于实现全局共享，将Mutex属性加入struct中，从而保证x中的val字段一定是被保护的

```go
type Mutex struct {
    m  sync.Mutex
    var val int
}

x := new (Mutex)

func testFunction() {
    x.Lock
    x.val ++
    x.Unlock
}
```

##2 sync.WaitGroup

WaitGroup 对象内部有一个计数器，最初从0开始，它有三个方法：Add(), Done(), Wait() 用来控制计数器的数量。Add(n) 把计数器设置为n ，Done() 每次把计数器-1 ，wait() 会阻塞代码的运行，直到计数器地值减为0。

```go
func main() {
    wg := sync.WaitGroup{}
    wg.Add(100)
    for i := 0; i < 100; i++ {
        go func(i int) {
            fmt.Println(i)
            wg.Done()
        }(i)
    }
    wg.Wait()
}
```
## 注意事项
    1). 计数器不能为负值:
        不能使用Add() 给wg 设置一个负值，否则代码将会报错;
    2). WaitGroup对象不是一个引用类型:
        WaitGroup对象不是一个引用类型，在通过函数传值的时候需要使用地址;

```go
func main() {
    wg := sync.WaitGroup{}
    wg.Add(100)
    for i := 0; i < 100; i++ {
        go f(i, &wg)
    }
    wg.Wait()
}
ol
// 一定要通过指针传值，不然进程会进入死锁状态
func f(i int, wg *sync.WaitGroup) { 
    fmt.Println(i)
    wg.Done()
}
```

