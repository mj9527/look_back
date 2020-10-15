# main 函数里面的autorelease pool 是非必须的
- UIApplicationMain永远不会返回，autorelease pool 不会进入释放阶段
- UIApplication 会创建main run loop, runloop会自动包含autorelease pool

