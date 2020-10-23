[GO 指南](https://tour.go-zh.org/welcome/1)

# 基本语法
- 包
1. go 程序由包组成，程序从main包开始运行
2. 包名和导入路径的最后一个元素一致
3. 包通过import 导入
4. 导出名字以大写字母开头

- 函数
1. 命名返回值只在短函数中使用
```
    func FuncExample(x int, y int)(int, error);
```

- 变量定义
```
   // var identfifier type
   i := 5
```

- 基本数据类型
1. 数据类型和C语言一致
2. go 不同类型的赋值需要显示转换，T(v)
3. 常量可以是字符，字符串，布尔值和数值 
```
   cont PI = 3.14
```

- for 循环
```
  sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}

  // while
  for sum < 100 {

  }

  // 无限循环
  for {

  }

  pow := make([]int, 5)
  for i, v := range pow {
    fmt.println(i, v)
  }
```

- if 语句
```
    i:=0
    if i < 10 {

    }

    if i:=0; i <10 {

    }
```

- switch 语句
1. 每个case 语句默认是break, fallthrough 允许分支继续执行
2. switch 和switch true 一样
3. switch 运行的数据类型有整形，字符串

- defer 
1. defer 函数以栈的顺序执行

- 指针
1. 指针保持了值的内存地址

- 结构体
1. 结构体字段用点号访问
2. 前缀& 返回结构体指针
```
  type Point struct {
    x int
    y int
  }

  p := &Point {
    x: 10,  // 需用逗号分隔
    y: 20,
  }
  // c++

  struct MyPoint {
    int x;
    int y;
    MyPoint(int x1, int y1) {
        x = x1;
        y = y1;
    }
  };

  // 顺序初始化
  MyPoint pt = {10, 10};

  // 构造函数初始化 
  MyPoint pt1(20, 20);
```

- 数组
1. [3]bool{true, true, false}

- 切片
1. []bool{true, true, false}
2. 切片下界的默认值为0, 上界为该切片的长度
3. 切片的长度len(s): 切片包含的元素个数
4. 切片的容量cap(s): 从它的第一个元素开始数，到其底层数组元素的个数
5. 切片的默认值是nil
6. 切片的创建 make([]int, 5)

- map
1. 默认值为nil
2. 删除 delete(m, key)
3. 判断键是否存在elem, ok := m[key]

- 方法





# 编译
go build -o [输出文件名]

## 指定平台编译 GOOS=linux
1. mac: darwin
2. linux: linux
3. windows: windows

## 指定处理器 GOARCH=amd64
1. 386
2. amd64
3. arm

编译liunx运行的可执行文件
GOOS=linux GOARCH=amd64 go build -o [输出文件名]

# 调试
  在fmt.Printf 中使用下面的说明符来打印有关变量的相关信息
- %+v 打印包括字段在内的实例的完整信息
- %#v 打印包括字段和限定类型名称在内的实例的完整信息
- %T 打印某个类型的完整说明



