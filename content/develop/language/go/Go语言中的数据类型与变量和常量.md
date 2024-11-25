+++
title = 'Go语言中的数据类型与变量和常量'
series = ['开发']
tags = ["go"]
date = 2024-11-20T09:59:27+08:00
+++
## 数据类型
### 总览
<pre class="mermaid">
mindmap
数据类型
    布尔型
    数字类型
    字符串类型
    派生类型
</pre>
### 派生类型
- 指针类型（Pointer）
- 数组类型
- 结构化类型 (struct)
- Channel 类型
- 函数类型
- 切片类型
- 接口类型（interface）
- Map 类型

### 数值类型
#### 整型
| 序号 | 类型      | 描述                                 |
|------|-----------|--------------------------------------|
| 1    | uint8    | 无符号 8 位整型 (0 到 255)          |
| 2    | uint16   | 无符号 16 位整型 (0 到 65535)       |
| 3    | uint32   | 无符号 32 位整型 (0 到 4294967295)  |
| 4    | uint64   | 无符号 64 位整型 (0 到 18446744073709551615) |
| 5    | int8     | 有符号 8 位整型 (-128 到 127)       |
| 6    | int16    | 有符号 16 位整型 (-32768 到 32767)  |
| 7    | int32    | 有符号 32 位整型 (-2147483648 到 2147483647) |
| 8    | int64    | 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807) |

#### 浮点型
| 序号 | 类型      | 描述                                  |
|------|-----------|---------------------------------------|
| 1    | float32  | IEEE-754 32 位浮点型数                |
| 2    | float64  | IEEE-754 64 位浮点型数                |
| 3    | complex64 | 32 位实数和虚数                       |
| 4    | complex128| 64 位实数和虚数                       |

#### 其他数字类型
| 序号 | 类型      | 描述                                  |
|------|-----------|---------------------------------------|
| 1    | byte     | 类似 uint8                            |
| 2    | rune     | 类似 int32                            |
| 3    | uint     | 32 或 64 位                          |
| 4    | int      | 与 uint 一样大小                      |
| 5    | uintptr  | 无符号整型，用于存放一个指针         |

## 变量
### 变量的声明方式
- 指定变量名和类型
```go
// 如果没有初始化，则变量默认为零值。
var v_name v_type

package main
import "fmt"
func main() {

    // 声明一个变量并初始化
    var a = "RUNOOB"
    fmt.Println(a)

    // 没有初始化就为零值
    var b int
    fmt.Println(b)

    // bool 零值为 false
    var c bool
    fmt.Println(c)
}
- 数值类型（包括complex64/128）为 0

- 布尔类型为 false

- 字符串为 ""（空字符串）

- 以下几种类型为 nil：
var a *int
var a []int
var a map[string] int
var a chan int
var a func(string) int
var a error // error 是接口
```

- “=”方式
```
// 根据值自行判定变量类型。
var v_name = value
```
- “:=”方式
```
v_name := value

intVal := 1 相等于：
var intVal int
intVal =1
```

### 多变量声明方式
```
//类型相同多个变量, 非全局变量
var vname1, vname2, vname3 type
vname1, vname2, vname3 = v1, v2, v3

var vname1, vname2, vname3 = v1, v2, v3 // 和 python 很像,不需要显示声明类型，自动推断

vname1, vname2, vname3 := v1, v2, v3 // 出现在 := 左侧的变量不应该是已经被声明过的，否则会导致编译错误


// 这种因式分解关键字的写法一般用于声明全局变量
var (
    vname1 v_type1
    vname2 v_type2
)

// 省略变量赋值会延续上一个指定的值
const (
        a = "ha"   //ha
        b          //ha
        c          //ha
)
```

## 常量
常量是一个简单值的标识符，在程序运行时，不会被修改的量。

常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。
- const 常量
```
const identifier [type] = value
- 显式类型定义： const b string = "abc"
- 隐式类型定义： const b = "abc"


多个变量可以简写
const c_name1, c_name2 = value1, value2
```

- const常量的枚举应用
```
package main

import "unsafe"
const (
    a = "abc"
    b = len(a)
    c = unsafe.Sizeof(a)
)

func main(){
    println(a, b, c)
}
```
- iota 常量
iota常量是一种特殊的常量，可理解为 const 语句块中的行索引
```go
package main

import "fmt"

func main() {
    const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
}
```