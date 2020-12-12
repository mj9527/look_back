# lldb 常用的调试命令
- po: 打印对象，会调用对象的description方法
- expr: 动态执行表达式
- print: 打印命令
- bt: 答应调用堆栈
- br l : 断点列举

# 如何调试BAD_ACCESS
- 开启僵尸对象调试功能
![](image/zoombie.png "调试")

# 常用命令
1. 打印对象p, po
2. image lookup - a [adress], 查找栈对应的地址
3. bt 显示调用堆栈
4. frame select [n], 查看调用函数信息
5. frame variable, 查看当前栈的变量值
6. continue, step, next, frame

