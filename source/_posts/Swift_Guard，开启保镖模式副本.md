title: 'Swift_Closure，闭包'
categories: Swift
date: 2015-11-27 09:23:33
tags: [iOS, Swift, 闭包, Closure, Block]
---

**闭包**（*Closure*）与OC中的Block语法并无太大区别，只是名字与写法变了而已，要想使用起来也并不复杂。

### 1. 基本语法

- 闭包作为变量

```
var sumClosure: ((Int, Int) -> Int)! = nil
```
<!--more-->
- 给闭包变量赋值

```
// 方式1：带有参数名，参数类型，返回值类型
sumClosure = { (a: Int, b: Int) -> Int in
    return a + b
}

// 方式2：省略参数类型
sumClosure = { (a, b) -> Int in
    return a + b
}

// 方式3：省略返回值类型
sumClosure = { (a, b) in
    return a + b
}

// 方式4：省略参数的小括号
sumClosure = { a, b in
    return a + b
}

// 方式5：省略参数名
sumClosure = {
    return $0 + $1
}

// 方式6：省略return关键字
sumClosure = {
    $0 + $1
}
```

- 调用

```
// 就像函数一样使用，该传参传参，该接收结果就接收结果
let res = sumClosure(10, 20)
print(res)
```

### 2. 给闭包易名

- 易名

```
// 给无参无返回值的闭包起别名
typealias ClosureName1 = () -> Void

// 给有两个字符串参数和无返回值的闭包起别名
typealias ClosureName2 = (String, String) -> Void

// 给有两个浮点型参数和一个浮点型返回值的闭包起别名
typealias ClosureName3 = (a: Float, b: Float) -> Float

// 给有两个整型参数和两个整形返回值的闭包起别名（返回值是元组）
typealias ClosureName4 = (Int, Int) -> (Int, Int)
```

- 通过易名使得闭包变量简化

```
// 使用别名，声明闭包变量
let closure2: ClosureName4 = { (a: Int, b: Int) -> (Int, Int) in
    return (a + b, a - b)
}

// 闭包的使用
let res2 = closure2(10, 20)
print("sum: \(res2.0)  diff: \(res2.1)")
```


### 3. 闭包作为函数参数

- 带有闭包参数的方法定义

```
func function1(closure: ClosureName1) {
    print("closure before")

    // 调用闭包
    closure()

    print("closure after")
}
```

- 使用

```
function1 { () -> Void in
    print("~~~~~~~~~~~~")
    print("closure body")
    print("~~~~~~~~~~~~")
}
```