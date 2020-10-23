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