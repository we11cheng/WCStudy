## GCD常用操作
### 1.group : 当所有任务都执行完成之后，才执行```dispatch_group_notify```中的任务 dosthNotify。

```
- (void)gcdGroup{
    dispatch_group_t group =  dispatch_group_create();
    
    dispatch_group_async(group, dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        // dosth1;
    });
    dispatch_group_async(group, dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        // dosth2;
    });
    dispatch_group_notify(group, dispatch_get_main_queue(), ^{
        // dosthNotify;
    });
}
```
### 2.barrier : 栅栏方法,当栅栏前一组操作执行完之后，才能开始执行后一组方法。

```
- (void)barrier {
    dispatch_queue_t queue = dispatch_queue_create("net.bujige.testQueue", DISPATCH_QUEUE_CONCURRENT);

    dispatch_async(queue, ^{
        // dosth1;
    });
    dispatch_async(queue, ^{
        // dosth2;
    });
    dispatch_barrier_async(queue, ^{
        // doBarrier;
    });
    dispatch_async(queue, ^{
        // dosth4;
    });
    dispatch_async(queue, ^{
        // dosth5;
    });
}

```

### 3.信号量 : dispatch_semaphore 保持线程同步，将异步执行任务转换为同步执行任务。

```
  dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
  dispatch_semaphore_t semaphore = dispatch_semaphore_create(0);
  dispatch_async(queue, ^{
      // dosth1
      // 使信号量+1并返回
      dispatch_semaphore_signal(semaphore);
  });
  // 若信号的信号量为0，则会阻塞当前线程，直到信号量大于0或者经过输入的时间值；若信号量大于0，则会使信号量减1并返回，程序继续住下执行
  dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER);
  // dosth2 ,只有当dosth1执行完,信号量+1之后,才会执行这里

```

