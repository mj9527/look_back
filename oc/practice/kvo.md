# KVO
- 当一个对象的属性被注册观察者时，会生成一个中间类继承自此类，然后将类的isa指针指向新生成的子类，这样被观察的对象就变成这个中间类，
同时重新属性的setter方法，当新对象的属性发生变化时，会依次通知注册的观察者对象
- 键值观察通知依赖于NSObject 的两个方法: willChangeValueForKey: 和 didChangevlueForKey:；在一个被观察属性发生改变之前， willChangeValueForKey: 一定会被调用，这就 会记录旧的值。而当改变发生后，didChangeValueForKey: 会被调用，继而 observeValueForKey:ofObject:change:context: 也会被调用
- KVO 手工触发
```
@property (nonatomic, strong) NSDate *now;
- (void)viewDidLoad
{
    [super viewDidLoad];

    // “手动触发self.now的KVO”，必写。
    [self willChangeValueForKey:@"now"];

    // “手动触发self.now的KVO”，必写。
    [self didChangeValueForKey:@"now"];
}
```
- 使用例子
```
// KVO.h
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface ObjectA : NSObject
@property (nonatomic, assign) NSInteger valueA;
@end

@interface ObjectB : NSObject
@property(nonatomic, assign) NSInteger valueB;

-(void) observeValuForKeyPath:(NSString*)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey, id>*)change context:(void*)context;
@end

NS_ASSUME_NONNULL_END

// KVO.m
#import "KVO.h"
@implementation ObjectA

@end

@implementation ObjectB

-(void) observeValueForKeyPath:(NSString*)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey, id>*)change context:(void*)context {
    if (![object isKindOfClass:[ObjectA class]]) {
        return ;
    }
    
    if (![keyPath isEqualToString:@"valueA"]) {
        return ;
    }
    
    NSLog(@"ObjectA valueA changed %@", change);
}

@end

// test function
void TestKVO() {
    ObjectA* objA = [[ObjectA alloc] init];
    ObjectB* objB = [[ObjectB alloc] init];
    [objA addObserver:objB forKeyPath:@"valueA" options:NSKeyValueObservingOptionNew context:nil];
    objA.valueA = 10;
    //[objA removeObserver:objB forKeyPath:@"valueA"];
}
```