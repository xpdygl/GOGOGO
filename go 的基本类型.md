# Go 语言数据类型

在 Go 编程语言中，数据类型用于声明函数和变量。

数据类型的出现是为了把数据分成所需内存大小不同的数据，编程的时候需要用大数据的时候才需要申请大内存，就可以充分利用内存。

Go 语言按类别有以下几种数据类型：

| 序号 | 类型和描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **布尔型** 布尔型的值只可以是常量 true 或者 false。一个简单的例子：var b bool = true。 |
| 2    | **数字类型** 整型 int 和浮点型 float32、float64，Go 语言支持整型和浮点型数字，并且支持复数，其中位的运算采用补码。 |
| 3    | **字符串类型:** 字符串就是一串固定长度的字符连接起来的字符序列。Go 的字符串是由单个字节连接起来的。Go 语言的字符串的字节使用 UTF-8 编码标识 Unicode 文本。 |
| 4    | **派生类型:** 包括：(1) 指针类型（Pointer）(2) 数组类型(3) 结构化类型(struct)  (4) Channel 类型(5) 函数类型(6) 切片类型(7) 接口类型（interface）(8) Map 类型 |

## 

##  go 变量

go使用 var定义变量。

go的变量名不能使用数字开头，同时也不能使用系统自定义的关键字。

定义变量一般使用   :=

```go
a := 123
a := true
a := "hello"
```

go会自动识别我们定义的类型。

此外基本的定义变量的方法有

```go
var a int = 3
```





## go 常量

常量是运行时不会被修改的量

在变量前使用const就可以定义一个常量了

```go
const a :=123
const a :="123"
```

```go
package main

import "fmt"

func main() {
   const LENGTH int = 10
   const WIDTH int = 5  
   var area int
   const a, b, c = 1, false, "str" //多重赋值

   area = LENGTH * WIDTH
   fmt.Printf("面积为 : %d", area)
   println()
   println(a, b, c)  
}
```



##  Go语言的算术运算符&逻辑运算府&关系运算府与JAVA一致，此处不赘述





## 数组

## 数组定义 array

#### 初始化数组

以下演示了数组初始化：

```
var variable_name [SIZE] variable_type
var abc [3] int
// 定义了一个长度为3的，int类型的数组
```



我们也可以通过字面量在声明数组的同时快速初始化数组：

```
balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
```

如果数组长度不确定，可以使用 **...** 代替数组的长度，编译器会根据元素个数自行推断数组的长度：

```
var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
或
balance := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
```

## slice动态数组结构

slice 动态数组结构，通过[i,j]来获取,i是数组的开始位置(默认是0)，j是结束位置，但不包含j