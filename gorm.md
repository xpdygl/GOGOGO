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

### 快速启动   

 本例使用的数据库是mysql

(无需建立数据库，以下代码会自动生成一张表)

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

### 连接本地数据库

连接自己数据库时需要在代码中编写以下内容

```go
//         账号	密码                数据库端口		数据库名                                      数据库设置
	dsn:= "root:root@tcp(localhost:3306)/test?charset=utf8mb4&parseTime=True&loc=Local"

	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
//open数据库时会返回两个值，一个是db，一个是err，用来打印错误信息
	if err != nil {
		panic(err)
	}

```

## 创建数据库

使用AutoMigrate创建数据库时，需要注意引用的结构体内的字段名大写，数据库读不到小写的字段名

```go
type Order struct {
	gorm.Model
	OrderNo 	string
	UserName 	string
	Amount  	int64
	FileUrl 	string
	Status  	string
}
```



**不过AutoMigrate魔法值太大，开发中慎用。**



### 默认标签

可以通过标签 default 为字段定义默认值，如：

```go
type User struct {
  ID   int64
  Name string `gorm:"default:galeone"`
  Age  int64  `gorm:"default:18"`
}
```

插入记录到数据库时，默认值会被用于填充值为零值的字段



### 新增数据

```go
  db.Create(&Product{Code: "D42", Price: 100})
```

  

### 更新数据

```go
// 条件更新
//指定模型u ，取U的地址值，传进去，设置amount=12334的这一列，让这一列的name为hello
	db.Model(&u).Where("amount = ?", 12334).Update("name", "hello")
	fmt.Println(&u)							//打印

// UPDATE users SET name='hello' WHERE amount=12334;
```

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



### 删除数据

```go
删除一条记录
删除一条记录时，删除对象需要指定主键，否则会触发 批量 Delete，例如：

db.Delete(&u)
会把数据库中的数据库全部删了

//指定主键的话，就只会删除指定的行数
db.Delete(&User{}, 10)
// DELETE FROM users WHERE id = 10;
```



如果您的模型包含了一个 `gorm.deletedat` 字段（`gorm.Model` 已经包含了该字段)，它将自动获得软删除的能力！

拥有软删除能力的模型调用 `Delete` 时，记录不会从数据库中被真正删除。但 GORM 会将 `DeletedAt` 置为当前时间， 并且你不能再通过普通的查询方法找到该记录。
