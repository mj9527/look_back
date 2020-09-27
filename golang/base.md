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



