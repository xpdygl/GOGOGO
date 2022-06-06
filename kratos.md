## 目录层级

### internal 

internal主要负责代码内部的逻辑

biz层面向业务逻辑

data层面向数据库

conf层内的东西与config.yaml的配置对应

### makefile

makefile是kratos中非常重要的一个文件,里面是一堆脚本，开发中随时会用到它，使用方法是，cd 到Makefile的层级，然后输入已经配置好的命令，kratos初始提供了三个命令

+ make api 根据proto文件生成api 
+ make config 根据proto文件生成aconfig
+ make generate

更改了proto文件后使用make系列的命令就可以生成新的后缀为.go的文件

这类要生成的文件不建议手动修改，开发中一般使用make命令去生成，真正要修改的是proto文件。

### wire

安装wire

```
go install github.com/google/wire/cmd/wire@latest
```

需要注意的是，使用wire时需要在有wire.go的地方才能使用它。

wire的作用是自动生成依赖注入。调整代码错误

修改Makefile时添加语句

```
#     wire
wire:
	cd  cmd/Order/  &&  wire
```

就可以在有Makefile的目录下使用make wire ，需要注意的是，修改Makefile文件时的缩进要用tab而不是用空格，否则就会报错。



### 依赖注入

整个项目的流程为

New APP( NewServer)

New Server (New Service)

New Service(New OrderUseCase)										service 层

New OrderUseCase(New OrderRepo)								 biz层

New Repo(New OrderRepo)

New OrderRepo(New Data)

New Data(DB)

New DB(gorm)

gorm ->config

我们需要做的是在ProvideoSet（New Foo...）给出这些new的参数

最后用wire依赖注入，生成了wire_gen.go

### config.yaml

这个文件下面保存着数据库的配置信息，要想使用到这些配置信息需要wire的帮助

```yaml
data:
  database:
    driver: mysql
    dsn: root:root@tcp(localhost:3306)/test?charset=utf8mb4&parseTime=True&loc=Local
```

以gorm为例

在data包下的data.go文件中连接数据库

```
func NewDB(c *conf.Data ) *gorm.DB {
	db, err := gorm.Open(mysql.Open(c.Database.Dsn), & gorm.Config{})
	if err!=nil{
		panic("数据库没连接成功")
	}
	return db
}
```

其余的一切正常，除了open那一步的数据库信息有改动

具体做法为

在DATA这返回一个db

```go
type DATA struct{
    db *gorm.DB
}
```

新建一个连接

```go
func NewDB(c *conf.Data ) *gorm.DB {
	db, err := gorm.Open(mysql.Open(c.Database.Dsn), & gorm.Config{})
	if err!=nil{
		panic("数据库没连接成功")
	}
	return db
}
```

在NewData这个函数添加一个入参db *gorm.DB

在conf包的conf.proto文件下修改文件，删去目前还用不到的Redis

```go
message Data {
  message Database {
    string dsn = 1;
  }
  Database database = 1;
}
```

在命令行输入make wire

wire 会自动注入代码

此时点开wire_gen.go文件就会发现wireAPP这个函数下的第一行多出了一个db

最后返回data.go文件，在open数据库的语句处就可以点出c.Database.Dsn从而使用到config.yaml的数据了



### vo po dto bo

#### vo

VO（View Object）视图对象:和视图打交道的，那么经历了视图的都归属于这个类，所以我们的输入输出类都是属于VO

#### po

PO（Persistent Object）永久对象： 这些对象对应着数据库的每一个字段名以下是我的数据库表（如下表），所以我entity类对应着数据库的每个列，称为PO

#### dto

那么DTO又是什么呢？先给大家翻译下

DTO(Data Transfer Object)**数据传输对象：**我们sql查询的时候是通过Id查询的，但是查询是可以查询出很多条信息的，但是我们给前端的数据只要某一部分，比如有4个属性，但是只要求输出3个。

#### bo

### 各层的数据之间的转换

各个层的数据之间有可能不相同，

在data层中，将业务处理层给我的模型进行一层转换，业务处理层给我的模型里的字段有可能和我数据库的字段不对应，所以我需要在data层中再新建一个模型。

将我认可的数据从do转成po。最后通过po操作数据库

将外面可能给的错误数据排除在外，保证给到我数据库的数据都是我认可的数据

从而实现本套代码的健壮性。