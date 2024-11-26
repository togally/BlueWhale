+++
title = 'Go语言基础结构和语法'
series = ['速通GO']
tags = ["go"]
date = 2024-11-19T10:28:23+08:00
draft = false
+++

## 基础结构以及执行方式
### 基础结构
```go
// 包名
package main

// 引入包
import "fmt"

// main 函数是每一个可执行程序所必须包含的，一般来说都是在启动后第一个执行的函数（如果有 init() 函数则会先执行该函数
// 注意 { 不能单独放在一行，否则在运行时会产生错误
func main() {
   /* 这是我的第一个简单的程序 */
   fmt.Println("Hello, World!")
}
```

### 编译运行
假设有一个hello.go文件
1. 使用go run来运行
```go
$ go run hello.go
Hello, World!
```
2. 使用go build来运行
```go
$ go build hello.go
$ ls
hello    hello.go
$ ./hello
Hello, World!
```

go build：编译并生成可执行文件，适用于发布和生产环境。

go run：编译并立即运行代码，适用于开发和测试阶段。

根据你的需求选择合适的命令，如果你只是在开发过程中测试代码，go run 会非常方便；

如果你需要生成一个可分发的应用程序，go build 则是合适的选择。

## 基础语法
### 行分隔符
一行代表一个语句结束,不需要特别用;指出。
如果多个语句在同一行需要使用;来区分

### 注释
```
// 单行注释
/*
 Author by jasperWei
 我是多行注释
 */

```

### 标识符
标识符主要用来命名类、实体、方法等。可以由数字、字母、下划线组成，但是第一个不能是数字

### 字符串连接
Go 语言的字符串连接可以通过 + 实现：

### 关键字
|     |     |     |     |     |
|------------|------------|------------|------------|------------|
| break      | default    | func       | interface  | select     |
| case       | defer      | go         | map        | struct     |
| chan       | else       | goto       | package    | switch     |
| const      | fallthrough| if         | range      | type       |
| continue   | for        | import     | return     | var        |

#### 预定义标识符
|    |     |   |    |     |       |         |        |      |
|-------|--------|------|-------|--------|----------|------------|-----------|---------|
| append| bool   | byte | cap   | close  | complex  | complex64  | complex128| uint16  |
| copy  | false  | float32| float64| imag   | int      | int8       | int16     | uint32  |
| int32 | int64  | iota | len   | make   | new      | nil        | panic     | uint64  |
| print | println| real | recover| string | true     | uint       | uint8     | uintptr |

### 空格
1. 关键字和表达式之间要使用空格。
```Go
if x > 0 {
    // do something
}
```
2. 在函数调用时，函数名和左边等号之间要使用空格，参数之间也要使用空格。
```Go
result := add(2, 3)
```

### 格式化字符串
fmt.Sprintf 或 fmt.Printf 可以格式化字符串并赋值给新串
- Sprintf 根据格式化参数生成格式化的字符串并返回该字符串。
- Printf 根据格式化参数生成格式化的字符串并写入标准输出
```Go
package main

import (
    "fmt"
)

func main() {
   // %d 表示整型数字，%s 表示字符串
    var stockcode=123
    var enddate="2020-12-31"
    var url="Code=%d&endDate=%s"
    var target_url=fmt.Sprintf(url,stockcode,enddate)
    // Code=123&endDate=2020-12-31
    fmt.Println(target_url)
    //Code=123&endDate=2020-12-31
    fmt.Printf(url,stockcode,enddate)
}
```