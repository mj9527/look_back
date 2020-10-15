# Block
- block 是一种OC对象，可以用于赋值，当参数传递，当用于函数参数时, block 应该放在参数列表的最后一个
- block 可以捕获外部作用域的变量，但默认无法修改，需要修改的变量使用__block修饰
- block 需要维持其作用域捕获的变量，所以属性定义为copy
- 语法
```
    // 变量定义
    int (^addFunc)(int, int) = ^int(int a, int b) {
        return (a+b);
    };

    // 属性定义
    @property(nonatomic, copy) int(^addP)(int, int);

    // 类型定义
    typedef int(^addType)(int, int);

    // 函数参数
    -(void) useBlockParam:(int(^)(int, int)) func{
        NSLog(@"use block param %d", func(1,1));
    }
```
