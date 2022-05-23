## Go 结构体

go的结构体类似于JAVA的对象

### 结构体的定义

```go
type Book struct {    	/* 声明一个结构体 */
    bookId int
    name string
    price int
}

func main() {
   var Book1 Books        /* 声明 Book1 为 Books 类型 */
   var Book2 Books        /* 声明 Book2 为 Books 类型 */
    
   Book1.bookId = 8       /* 给Book1对像中的bookId赋值*/
    fmt.Pringln(Book1.bookId)   //取值
}
```

结构体定义在函数的外面

Book1 声明为Book类型