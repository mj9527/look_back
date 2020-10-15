```
# Foundation和Core Foundation的区别
- Foundation 使用OC实现，Core Foundation 使用C实现
- Foundation 和Core Foundation 对象间的转换: 俗称桥接
- Foundation 类以NS开头, Core Foundation以CF开头

## OC(Foundation)strOC ->C(Core Foundation) strC
- __bridge 没有转移对象所有权，依然由OC管理
- __bridge_retianed 转移对象所有权，strC 必须自己手动释放内存

## C(Core Foundation) strC -> OC(Foudation) strOC
- __bridge 没有转移对象所有权，依然由C管理
- __bridge_transfer 转移对象所有权，由OC管理

# NS_ASSUME_NONNULL_BEGIN 和 NS_ASSUME_NONNULL_END
宏之间的代码，所有简单指针对象被假定为nonnull

# +(void)load
- load 方法在main函数之前调用
- 类被import时初始加载时调用，顺序依次是父类优于子类，子类优于分类
- 主要用来进行method swizzle
- 子类没有load，则不会调用父类

# +(void)initialize
- initialize 方法在main函数之后调用
- 第一次调用类方法时加载，属于懒加载
- 主要用来初始化全局变量和静态变量
- 子类没有initialize 方法也会调用父类的方法，走的是发送消息的流程

# SEL 就是C语言的指针函数

# NSNotification
- 单例 + 观察者模式
```
     // object : nil 接收所有发送者事件
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(onNotify:) name:@"stop" object:nil];
    
    -(void) onNotify:(NSNotification*)notification {
        NSLog(@"recv notify %@", notification);
    }

    // 发送消息
    [[NSNotificationCenter defaultCenter] postNotificationName:@"stop" object:self userInfo:nil];
```