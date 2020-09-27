# 静态库和动态库的制作


# 多线程编程

# 多线程同步机制

# 定时器
- NSTimer
缺点：存在延迟
```
-(void) startTimer {
    NSTimer* t = [NSTimer timerWithTimeInterval:1 target:self selector:@selector(onTimer) userInfo:nil repeats:YES];
    [[NSRunLoop currentRunLoop] addTimer:t forMode:NSRunLoopCommonModes];
}

-(void) onTimer{
    static int count = 0;
    NSLog(@"ontimer %d", count++);
}

-(void) startTimer1 {
    [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(onTimerWithInfo:) userInfo:@"mj" repeats:YES]; 
}

-(void)onTimerWithInfo:(NSTimer*)t {
    static int count = 0;
    NSLog(@"onTimerWithInfo %@-%d", t.userInfo, count++);
}
```

- CADisplayLink

1. 创建方法
    displayLink = [CADisplayLink displayLinkWithTarget:self selector:@selector(handleDisplayLink:)];
    [displayLink addToRunLoop:[NSRunLoop currentRunLoop] forMode:NSDefaultRunLoopMode];

2. 停止方法
    [displayLink invalidate];
    displayLink = nil;
    当把CADisplayLink对象add到runloop中后，selector就能被周期性调用，类似于重复的NSTimer被启动了；执行invalidate操作时，CADisplayLink对象就会从runloop中移除，selector调用也随即停止，类似于NSTimer的invalidate方法。

3. 特点：
    屏幕刷新时调用：CADisplayLink是一个能让我们以和屏幕刷新率同步的频率将特定的内容画到屏幕上的定时器类。CADisplayLink以特定模式注册到runloop后，每当屏幕显示内容刷新结束的时候，runloop就会向CADisplayLink指定的target发送一次指定的selector消息， CADisplayLink类对应的selector就会被调用一次。所以通常情况下，按照iOS设备屏幕的刷新率60次/秒=
    延迟：iOS设备的屏幕刷新频率是固定的，CADisplayLink在正常情况下会在每次刷新结束都被调用，精确度相当高。但如果调用的方法比较耗时，超过了屏幕刷新周期，就会导致跳过若干次回调调用机会。
             如果CPU过于繁忙，无法保证屏幕60次/秒的刷新率，就会导致跳过若干次调用回调方法的机会，跳过次数取决CPU的忙碌程度。
    使用场景：从原理上可以看出，CADisplayLink适合做界面的不停重绘，比如视频播放的时候需要不停地获取下一帧用于界面渲染。

- GCD
```
 NSTimeInterval period = 1.0; //设置时间间隔
    dispatch_queue_t queue = dispatch_get_main_queue();
    dispatch_source_t _timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0, queue);
    dispatch_source_set_timer(_timer, dispatch_walltime(NULL, 0), period * NSEC_PER_SEC, 0); //每秒执行
    dispatch_source_set_event_handler(_timer, ^{
        //在这里执行事件
        static int count = 0;
        NSLog(@"gcd %d", count++);
    });
    
    dispatch_resume(_timer);
```

# 网络
- TCP
- UDP
- HTTP

# 文件


