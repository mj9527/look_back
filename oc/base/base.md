# NS_ASSUME_NONNULL_BEGIN 和 NS_ASSUME_NONNULL_END
宏之间的代码，所有简单指针对象被假定为nonnull

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