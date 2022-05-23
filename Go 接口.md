## Go 接口

在go中，实现了接口的所有方法就是实现了该接口，无需在该类型上添加声明。

```go

type  Person struct {
	name string
	age int
}
//定义一个接口
type People interface {
	ReturnName() string
}

func (p Person) ReturnName() string  {
	return p.name
}

//斐波那契递归实现   函数
//func fibonaqi(n int) int {
//	if n <2 {
//		return n
//	}
//	return fibonaqi(n - 2) +fibonaqi( n -1 )
//}


func main() {

	cbs := Person{name: "羊驼",age: 18}
	var what People
	what = cbs
	name:=what.ReturnName()
	fmt.Printf(name)
```

