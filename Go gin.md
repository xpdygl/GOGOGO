### gin

quick Start

首先下载包

```
go get -u github.com/gin-gonic/gin
```

然后在编写以下代码，点击运行之后在浏览器输入localhost:4010即可访问

```go
import   "github.com/gin-gonic/gin"

func main() {

	//创建一个默认的路由引擎
	r:=gin.Default()
	//GET请求方式 /hello请求路径
    //GET后面有两个参数，一个是路径，一个是前端发送过来的请求
    // gin.Context，封装了request和response
	r.GET("/hello", func(c *gin.Context) {
		c.JSON(200,gin.H{
			"message":"Hello World!",
		})
	})
	//启动HTTP服务，默认在8080启动，也可以自己配置端口号
    //这行代码的作用是一直发布这个程序服务
	r.Run(":4010")
}
```

  gin.Context，封装了request和response

我们可以从中拿到我们想要的数据。



## gin接收JSON数据

场景为前端发送正确格式的JSON数据过来时，后端原封不动的将数据存入数据库

```go
// 添加数据
func (orderHandler OrderHandler) AddNewOrder(c *gin.Context) {
   var order model.DemoOrder

   c.BindJSON(&order)
   fmt.Println("JSON数据")
   log.Print(&order)

   err := orderHandler.orderService.AddNewOrder(&order)
   if err != nil {
      ApiHelpers.RespondJSON(c, 404,"添加数据失败",nil)
   }
}
```

在gin框架中因为有*gin.Context的原因

直接 c.BindJSON（&对象）

就可以成功接收，然后进行后续操作。