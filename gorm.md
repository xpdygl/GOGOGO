## GORM个人笔记

前几天学习这个orm框架遇到了点困难。不过在同事和自己的努力下目前初步会使用GORM了

使用GORM首先要import依赖，需要注意的是GORM有两个版本，这两个版本是个各自不同的

2.0版本的依赖如下

```go
import (
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
)
```

快速启动   mysql(无需建立数据库，以下代码会自动生成一张表)

```go
package main

import (
  "gorm.io/gorm"
  "gorm.io/driver/mysql"
)

type Product struct { //定义模型，	使本模型与数据库中的字段对应
  gorm.Model				//struct可以	“继承”，gorm.Model是官方给的模型，里面包含ID 主键，创建时间，删除时间，更新时间
  Code  string				 	//定义code 	类型为string
  Price uint						//定义price	 类型为uint
}


//以下这个函数为快速启动用到的连接数据库的函数，若想连接自己的数据库需要进行修改
func main() {
  db, err := gorm.Open(mysql.Open("test.db"), &gorm.Config{})
  if err != nil {
    panic("failed to connect database")
  }

	 // 迁移 schema					//这段代码是将模型转化为数据库的表
  db.AutoMigrate(&Product{})

  	// Create增			
    //往数据库增加一条数据，使用db这个对象增加一条数据，数据由我们给product赋值决定

  db.Create(&Product{Code: "D42", Price: 100})
    


  	// Read查 			
	//定义一个实例，等会儿用这个实例操作数据库
  var product Product
    
   //查询数据库，First为查询数据库第一条数据
  db.First(&product, 1) // 根据整形主键查找
    
    //打印刚刚查询出的结果
    fmt.Prinln("&Product")
    
   // 获取第一条记录（主键升序）
  db.First(&product, "code = ?", "D42") // 查找 code 字段值为 D42 的记录
    
   //可以后面可以跟上想要查找的主键参数，本例为查找主键id=2的第一条数据
  db.First(&Product{Code: "D42", Price: 100},2)

    //db.Last() 为查询最后一条数据
    
    
  // Update - 将 product 的 price 更新为 200
  db.Model(&product).Update("Price", 200)
  // Update - 更新多个字段
  db.Model(&product).Updates(Product{Price: 200, Code: "F42"}) // 仅更新非零值字段
  db.Model(&product).Updates(map[string]interface{}{"Price": 200, "Code": "F42"})

  // Delete - 删除 product
  db.Delete(&product, 1)
}
```



可以通过标签 default 为字段定义默认值，如：

```go
type User struct {
  ID   int64
  Name string `gorm:"default:galeone"`
  Age  int64  `gorm:"default:18"`
}
```


插入记录到数据库时，默认值 会被用于 填充值为 零值 的字段

