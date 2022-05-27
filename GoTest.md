### GoTest

go中提供了测试方法，据说很好用。

所有以`_test.go`为后缀名的源代码文件都是`go test`测试的一部分，不会被`go build`编译到最终的可执行文件中。

每个测试函数必须导入`testing`包，测试函数的基本格式（签名）如下：

```go
func TestName(t *testing.T){
    // ...
}
```





在`*_test.go`文件中有三种类型的函数，单元测试函数、基准测试函数和示例函数。

|   类型   |         格式          |              作用              |
| :------: | :-------------------: | :----------------------------: |
| 测试函数 |   函数名前缀为Test    | 测试程序的一些逻辑行为是否正确 |
| 基准函数 | 函数名前缀为Benchmark |         测试函数的性能         |
| 示例函数 |  函数名前缀为Example  |       为文档提供示例文档       |



测试函数的名字必须以`Test`开头，可选的后缀名必须以大写字母开头，举几个例子：

```go
func TestAdd(t *testing.T){ ... }
func TestSum(t *testing.T){ ... }
func TestLog(t *testing.T){ ... }
```



### test命令

接下来，我们在`base_demo`包中定义了一个`Split`函数，具体实现如下：

```go
// base_demo/split.go

package base_demo

import "strings"

// Split 把字符串s按照给定的分隔符sep进行分割返回字符串切片
func Split(s, sep string) (result []string) {
	i := strings.Index(s, sep)

	for i > -1 {
		result = append(result, s[:i])
		s = s[i+1:]
		i = strings.Index(s, sep)
	}
	result = append(result, s)
	return
}
```

在当前目录下，我们创建一个`split_test.go`的测试文件，并定义一个测试函数如下：

```go
// split/split_test.go

package split

import (
	"reflect"
	"testing"
)

func TestSplit(t *testing.T) { // 测试函数名必须以Test开头，必须接收一个*testing.T类型参数
	got := Split("a:b:c", ":")         // 程序输出的结果
	want := []string{"a", "b", "c"}    // 期望的结果
	if !reflect.DeepEqual(want, got) { // 因为slice不能比较直接，借助反射包中的方法比较
		t.Errorf("expected:%v, got:%v", want, got) // 测试失败输出错误提示
	}
}
```

#### go test

在命令行界面输入 `go test`就会运行测试用例 

#### go test -v

有多个测试用例时，使用此命令，可以打印详细信息

#### go test -run

在执行`go test`命令的时候可以添加`-run`参数，它对应一个正则表达式，只有函数名匹配上的测试函数才会被`go test`命令执行。

例如通过给`go test`添加`-run=Sep`参数来告诉它本次测试只运行`TestSplitWithComplexSep`这个测试用例：

#### go test -cover

#### 测试覆盖率

测试覆盖率是指代码被测试套件覆盖的百分比。通常我们使用的都是语句的覆盖率，也就是在测试中至少被运行一次的代码占总代码的比例。在公司内部一般会要求测试覆盖率达到80%左右。

Go提供内置功能来检查你的代码覆盖率，即使用`go test -cover`来查看测试覆盖率。



### 断言

**断言(assertion)**是一种在程序中的一阶逻辑(如：一个结果为真或假的逻辑判断式)，目的为了表示与验证软件开发者预期的结果——当程序执行到断言的位置时，对应的断言应该为真。若断言不为真时，程序会中止执行，并给出错误信息。

使用assert时需要先new出这个对象，

然后调用里面的方法

```go
func TestSomething(t *testing.T) {
  assert := assert.New(t)

  // assert equality 	比较相同
  assert.Equal(123, 123, "they should be equal")

  // assert inequality 	比较不同
  assert.NotEqual(123, 456, "they should not be equal")

  // assert for nil (good for errors) 	
  //比较是否为空对象，是空的就通过测试
  assert.Nil(object)

  // assert for not nil (good when you expect something)
  //比较是否为空对象，不是空的就通过测试
  if assert.NotNil(object) {

    // now we know that object isn't nil, we are safe to make
    // further assertions without causing any errors
    assert.Equal("Something", object.Value)
  }
}
```