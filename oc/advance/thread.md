# Runloop
- Runloop 实际是一个对象，管理需要其处理的事件和消息，并提供一个处理事件的循环函数
- Runloop 的创建发生在第一次获取时，Runloop的销毁发生在线程结束时， 只能在线程内部获取其Runloop
- Runloop 由若干个mode组成，每个mode包含若干个Source/Timer/Observer item
- 一个item可以加入多个mode, 如果一个mode一个item都没有，Runloop会直接退出
- 每当 RunLoop 的内容发生变化时，RunLoop 都会自动将 _commonModeItems 里的 Source/Observer/Timer 同步到具有 "Common" 标记的所有Mode里。
- kCFRunLoopDefaultMode(mode name) 和 UITrackingRunLoopMode（mode name） 都被标记位mode属性
- Runloop 实现的功能
  1. AutoreleasePool, 通过Observer实现
  2. 事件响应和手势识别
  3. 界面更新， 通过Observer实现
  4. PerformSelecter 会在线程内部创建一个timer

- Runloop 一个时刻只能监听一个mode, 只有计时器关联的mode处于监听状态，计时器才会开始执行任务
- 线程需要启动Runloop的场景
  1. 使用NSport或者自定义输入源与其它线程通信
  2. 使用计时器 
  3. 在一个Cocoa应用中使用performSelector相关方法

# GCD
- 任务
  1. 同步执行：等待任务完成之后返回，只能在当前线程执行，不具备开启新线程的能力
  2. 异步执行：无需等待任务的完成，具备开启新线程的能力
  3. 任务的创建
  ```
    // 同步执行任务创建方法
    dispatch_sync(queue, ^{
        // 这里放同步执行任务代码
    });
    // 异步执行任务创建方法
    dispatch_async(queue, ^{
        // 这里放异步执行任务代码
    });
  ```
- 队列
  1. 串行队列：串行执行任务
  2. 并发队列：让多个任务并发执行
  3. 队列的获取：dispatch_get_main_queue(), dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
  4. 队列的创建：dispatch_queue_create("net.bujige.testQueue", DISPATCH_QUEUE_CONCURRENT);

- 任务和队列的组合
  1. 同步执行 + 并发队列 : 当前线程
  2. 异步执行 + 并发队列 : 开启多个线程
  3. 同步执行 + 串行队列 : 当前线程
  4. 异步执行 + 串行队列 : 开启新线程
  5. 同步执行 + 主队列 : 主线程
  6. 异步执行 + 主队列 : 主线程
  当前线程为主线程
  ![](image/compose.png "组合")

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