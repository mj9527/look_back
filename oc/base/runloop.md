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


