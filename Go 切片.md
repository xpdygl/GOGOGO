## Go 切片

```go
s :=[] int {1,2,3 } 	
//初始化切片 s为切片名称
//[]可以留空
//int为切片内存放数据的类型
//{1,2,3 } 为切片内的数据

var slice1 []type = make([]type, len)
也可以简写为
slice := make([]type, len)
```

切片有许多方法，常见的有

+ len 切片的长度
+ cap 切片的最大长度
+ append 给切片追加内容
+ copy  从源slice的src中复制元素到目标dst，并返回复制的元素的个数。



```go
	numbers := []int{0,1,2,3,4,5,6,7,8}
	fmt.Println(numbers)
	fmt.Println(append(numbers, 9))
```

输出结果如下

```
[0 1 2 3 4 5 6 7 8]
[0 1 2 3 4 5 6 7 8 9]
```



## 切片截取

可以通过设置下限及上限来设置截取切片 ，实例如下：

[ i , j ]

+ i 代表开始截取的位置
+ j 代表截取结束的位置，不包含 j,如果 j = 4 ,那么就截取到切片索引的3为止。

```go
numbers := []int{0,1,2,3,4,5,6,7,8}
fmt.Println("numbers[1:4] ==", numbers[1:4])
```

输出结果如下

```
numbers[1:4] == [1 2 3]
```



## 切片复制

```go
copy(复制接受者，复制提供者)

eg:
/* 创建切片 */
	numbers := []int{0,1,2,3,4,5,6,7,8}
	numbersPlus := make([]int, len(numbers), (cap(numbers))*2)

	copy(numbersPlus,numbers)
```

这样做的话numbersPlus就会有numbers的全部内容啦。