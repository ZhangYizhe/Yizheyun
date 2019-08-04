---
title: ObjC GCD串行队列
date: 2019-08-04 14:30:14
tags: objc
---



 通过设置串行队列，可以安排任务依次进行，便于管理。



```objective-c
// 任务组
dispatch_group_t group = dispatch_group_create();
        
// 任务
dispatch_queue_t queue = dispatch_queue_create("myqueue",DISPATCH_QUEUE_SERIAL);
        
// 信号量
dispatch_semaphore_t semaphore = dispatch_semaphore_create(1);

dispatch_group_enter(group); // 进入群组
dispatch_group_async(group, queue, ^{
    dispatch_group_leave(group); // 离开群组
    dispatch_semaphore_signal(semaphore); // 注销信号量
});

// 任务全部完成
dispatch_group_notify(group, queue, ^{
    NSLog(@"====test 完成全部任务");
});

```

