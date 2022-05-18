## Go 的条件语句

if 之后的判断条件不用跟上（）

```
if 布尔表达式 {
   /* 在布尔表达式为 true 时执行 */
}
```

```go
   var a int = 10
   if a < 20 {
       /* 如果条件为 true 则执行以下语句 */
       fmt.Printf("a 小于 20\n" )
   }
```



## Go 的循环语句

go中只有一个循环语句，和JAVA 不同的是，for后面的条件不用跟上（）

eg：

```go
sum :=0
for i:=5;i<100;i++{
    sum+=i
}
```

GO 语言支持以下几种循环控制语句：

| 控制语句                                                     | 描述                                             |
| :----------------------------------------------------------- | :----------------------------------------------- |
| [break 语句](https://www.runoob.com/go/go-break-statement.html) | 经常用于中断当前 for 循环或跳出 switch 语句      |
| [continue 语句](https://www.runoob.com/go/go-continue-statement.html) | 跳过当前循环的剩余语句，然后继续进行下一轮循环。 |
| [goto 语句](https://www.runoob.com/go/go-goto-statement.html) | 将控制转移到被标记的语句。                       |

一般使用break比较多。



