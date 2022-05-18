## Go语言函数

函数是基本的代码块，用于执行一个任务

函数定义格式如下

```go
func function_name( [parameter list] ) [return_types] {
   函数体
}

func 函数名（参数列表）[返回值]{
    函数体
}
ps 有些功能不需要返回值，这种情况下 return_types 不是必须的。
```



## 函数调用

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int = 200
   var ret int

   /* 调用函数并返回最大值 */
   ret = max(a, b)

   fmt.Printf( "最大值是 : %d\n", ret )
}

/* 函数返回两个数的最大值 */
func max(num1, num2 int) int {
   /* 定义局部变量 */
   var result int

   if (num1 > num2) {
      result = num1
   } else {
      result = num2
   }
   return result
}
```





## 函数返回多个值

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
   return y, x
}

func main() {
   a, b := swap("Google", "Runoob")
   fmt.Println(a, b)
}
```



## 函数的参数

参数的传递有两种方式

+ 一种是值的传递
  + 在a函数将值传递给b函数时，这个值在b函数改变但并不会影响a

+ 一种是引用传递
  + 在a函数将地址值传递给b函数时，这个值在b函数改变会影响a，因为地址值改变了

变量在读取内存时读的是地址，b将地址值改变之后，a再去读取数据，a读到的就是改变之后的地址了