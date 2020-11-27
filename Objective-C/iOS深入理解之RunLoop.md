### iOS深入理解之RunLoop

#### Runloop的概念

一个线程一次只能执行一个任务，执行完成后就会退出，如果我们需要一个机制让线程随时处理事件但不退出，这就是Runloop，事件来的时候来进行处理事件

1. 找到事件，
2. 处理事件
3. 找下一个事件
4. 处理事件
5. 直到没有事件，
6. 判断是否为退出
7. 不退出继续循环事件

#### Runloop与线程的关系

线程和Runloop是一一对应的关系 ，主线程默认开启Runloop，而子线程，没有默认开启子Runloop

NSThread 只是 pthread_t 的封装

Runloop 没有创建方法只有两个获取方法 CFRunLoopGetMain（）和 CFRunLoopGetCurrent

传入一个线程得到一个Runloop



#### Runloop对外接口

CFRunLoopRef
CFRunLoopModeRef 模式
CFRunLoopSourceRef 来源 分为 source0 和source1 

- Source0 只包含了一个回调（函数指针），它并不能主动触发事件。使用时，你需要先调用 CFRunLoopSourceSignal(source)，将这个 Source 标记为待处理，然后手动调用 CFRunLoopWakeUp(runloop) 来唤醒 RunLoop，让其处理这个事件
- Source1 包含了一个 mach_port 和一个回调（函数指针），被用于通过内核和其他线程相互发送消息。这种 Source 能主动唤醒 RunLoop 的线程

CFRunLoopTimerRef 时间 

- 是基于时间的触发器，它和 NSTimer 是toll-free bridged 的，可以混用。其包含一个时间长度和一个回调（函数指针）。当其加入到 RunLoop 时，RunLoop会注册对应的时间点，当时间点到时，RunLoop会被唤醒以执行那个回调

CFRunLoopObserverRef 监听

- 是观察者，每个 Observer 都包含了一个回调（函数指针），当 RunLoop 的状态发生变化时，观察者就能通过回调接受到这个变化

Runloop中有多个Mode ，每个mode内都有 ，一个Source Set timer list 监听List



#### RunLoop处理流程 

1. 即将进入RunLoop                                 - 通知
2. 通知RunLoop ， 即将处理Timer         - 通知 
3. 即将处理Source0                                  - 通知 
4. 处理Source0                                         - Source0 
5. 如果有Source1 处理source1 - 跳9步  - Source1 
6. 通知线程即将休眠                                 - 通知
7. 休眠等待唤醒                                         - 唤醒的几种方式(Source0 ， Timer ， 外部手动唤醒)
8. 线程刚被唤醒                                         - 通知
9. 处理唤醒时收到的消息 之后调回2       - Timer ， Source 1(点击事件等)
10. 即将推出Runloop                                 - 通知  

一条 Mach 消息实际上就是一个二进制数据包 (BLOB)，其头部定义了当前端口 local_port 和目标端口 remote_port body中包含Mach的消息

`RunLoop 的核心`就是一个 mach_msg() (见上面代码的第7步)，RunLoop 调用这个函数去接收消息，如果没有别人发送 port 消息过来，内核会将线程置于等待状态。例如你在模拟器里跑起一个 iOS 的 App，然后在 App 静止时点击暂停，你会看到主线程调用栈是停留在 mach_msg_trap() 这个地方

就是一个收到消息就处理消息没有消息就休息。通过port端的通信来进行，收到port_msg就处理

Runloop 和AutoRealeasePool关系 

在即将进入Runloop的时候 创建自动释放池 _objc_autoreleasePoolPush()

在即将休眠的时候   _objc_autoreleasePoolPop() 然后   _objc_autoreleasePoolPush()

在Runloop即将推出的时候 _objc_autoreleasePoolPop()

#### Runloop的Mode

#### Runloop的底层实现

#### Apple用Runloop实现的功能

#### Runloop实际使用用例



1. 自己理解Runloop 什么是runLoop
2. runloop有什么作用 
3. 用Runloop可以实现哪些功能
4. Runloop如何实现 
5. 怎么优化runloop