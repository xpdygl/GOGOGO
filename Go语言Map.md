## Go语言Map

go语言中的map是一种无序的键值对的集合，map使用key来检索数据，key指向map的值。





### 基本语法

var map 变量名  map[keytype]valuetype
通常 key 为 int 、string

**注意：声明是不会分配内存的，初始化需要 make ，分配内存后才能赋值和使用。**

```go
map1 := make(map[int]float32)
    map1[1] = 1.0
    map1[2] = 2.0
    map1[3] = 3.0
    map1[4] = 4.0
```

map 声明举例

var a map[string]string
var a map[string]int
var a map[int]string
var a map[string]map[string]string





### 遍历map

```go
for key, value := range map1 {
   fmt.Printf( key, value)
}

//value可以省略 , 直接不写就行
for key := range map1{
    fmt.Prinltn(key)
}

//key可以省略
for _,value:= range map1{    //用_来省略
		fmt.Println(value)
	}
```



### 删除map

delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key。

```go
	countryCapitalMap := map[string]string{"France": "Paris" , "Italy": "Rome", "Japan": "Tokyo", "India": "New delhi"}

	/*删除元素*/ delete(countryCapitalMap, "Japan")
					//  指定map          , 制定key
```

