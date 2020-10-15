# NSURLConnection (废弃)
- 同步请求
```
+ (NSData *)sendSynchronousRequest:(NSURLRequest *)request
                 returningResponse:(NSURLResponse **)response
                             error:(NSError **)error;
```

- 异步请求
```
+ (void)sendAsynchronousRequest:(NSURLRequest*) request
                          queue:(NSOperationQueue*) queue
              completionHandler:(void (^)(NSURLResponse* response, NSData* data, NSError* connectionError)) handler;
```

- 异步请求（委托）
```
    - (id)initWithRequest:(NSURLRequest *)request delegate:(id)delegate;

    - (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response
    - (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data
    - (void)connectionDidFinishLoading:(NSURLConnection *)connection
    - (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error
```
- NSURLConnection 异步方法默认还是在主线程执行，并且跑在Default Mode, UI滚动会影响其执行和回调
- NSURLConnection 异步线程执行
```
    NSRunLoop* runLoop = [NSRunLoop currentRunLoop];
    [runLoop addPort:[NSMachPort port] forMode:NSDefaultRunLoopMode]; // 添加 inputSource，让 runloop 保持 alive
    [self.connection scheduleInRunLoop:runLoop forMode:NSDefaultRunLoopMode];   
    [self.connection start];
    [runLoop run];
    - (void)connectionDidFinishLoading:(NSURLConnection *)connection
    {
        CFRunLoopStop(CFRunLoopGetCurrent());
    }
    - (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error
    {
        CFRunLoopStop(CFRunLoopGetCurrent());
    }
 
    //
    NSURLConnection *connection = [[NSURLConnection alloc] initWithRequest:aURLRequest delegate:self startImmediately:NO];
    [connection setDelegateQueue:[[NSOperationQueue alloc] init]];
    [connection start];

```