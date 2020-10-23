# main 函数里面的autorelease pool 是非必须的
- UIApplicationMain永远不会返回，autorelease pool 不会进入释放阶段
- UIApplication 会创建main run loop, runloop会自动包含autorelease pool

# ARC模式下无需手工把属性重置为nil
- 系统会自动在dealloc的时候把所有变量释放，没必要手工把变量重置为nil(release)
- 对于局部变量，由Autorelease Pool管理，会自动释放



