### YYCache学习

![](https://github.com/we11cheng/WCImageHost/raw/master/1611c66be5bd7907.png)

从架构图上来看，该组件里面的成员并不多：

```
YYCache：提供了最外层的接口，调用了YYMemoryCache与YYDiskCache的相关方法。
YYMemoryCache：负责处理容量小，相对高速的内存缓存。线程安全，支持自动和手动清理缓存等功能。
_YYLinkedMap：YYMemoryCache使用的双向链表类。
_YYLinkedMapNode：是_YYLinkedMap使用的节点类。
YYDiskCache：负责处理容量大，相对低速的磁盘缓存。线程安全，支持异步操作，自动和手动清理缓存等功能。
YYKVStorage：YYDiskCache的底层实现类，用于管理磁盘缓存。
YYKVStorageItem：内置在YYKVStorage中，是YYKVStorage内部用于封装某个缓存的类。
```

#### 代码讲解
知道了YYCache的架构图与成员职责划分以后，现在结合代码开始正式讲解。
讲解分为下面6个部分：

- YYCache
- YYMemoryCache
- YYDiskCache
- 保证线程安全的不同方案
- 提高缓存性能的几个尝试

#### YYCache给用户提供所有最外层的缓存操作接口，而这些接口的内部内部实际上是调用了YYMemoryCache和YYDiskCache对象的相关方法。
我们来看一下YYCache的属性和接口：
YYCache的属性和接口

```
@interface YYCache : NSObject


@property (copy, readonly) NSString *name;//缓存名称
@property (strong, readonly) YYMemoryCache *memoryCache;//内存缓存
@property (strong, readonly) YYDiskCache *diskCache;//磁盘缓存

//是否包含某缓存，无回调
- (BOOL)containsObjectForKey:(NSString *)key;
//是否包含某缓存，有回调
- (void)containsObjectForKey:(NSString *)key withBlock:(nullable void(^)(NSString *key, BOOL contains))block;

//获取缓存对象，无回调
- (nullable id<NSCoding>)objectForKey:(NSString *)key;
//获取缓存对象，有回调
- (void)objectForKey:(NSString *)key withBlock:(nullable void(^)(NSString *key, id<NSCoding> object))block;

//写入缓存对象，无回调
- (void)setObject:(nullable id<NSCoding>)object forKey:(NSString *)key;
//写入缓存对象，有回调
- (void)setObject:(nullable id<NSCoding>)object forKey:(NSString *)key withBlock:(nullable void(^)(void))block;

//移除某缓存，无回调
- (void)removeObjectForKey:(NSString *)key;
//移除某缓存，有回调
- (void)removeObjectForKey:(NSString *)key withBlock:(nullable void(^)(NSString *key))block;

//移除所有缓存，无回调
- (void)removeAllObjects;
//移除所有缓存，有回调
- (void)removeAllObjectsWithBlock:(void(^)(void))block;
//移除所有缓存，有进度和完成的回调
- (void)removeAllObjectsWithProgressBlock:(nullable void(^)(int removedCount, int totalCount))progress
                                 endBlock:(nullable void(^)(BOOL error))end;

@end
```

#### 基本使用方法，举一个缓存用户姓名的例子来看一下YYCache的几个API：
```
    //需要缓存的对象
    NSString *userName = @"Jack";
   
   //需要缓存的对象在缓存里对应的键
    NSString *key = @"user_name";
    
    //创建一个YYCache实例:userInfoCache
    YYCache *userInfoCache = [YYCache cacheWithName:@"userInfo"];
    
    //存入键值对
    [userInfoCache setObject:userName forKey:key withBlock:^{
        NSLog(@"caching object succeed");
    }];
    
    //判断缓存是否存在
    [userInfoCache containsObjectForKey:key withBlock:^(NSString * _Nonnull key, BOOL contains) {
        if (contains){
            NSLog(@"object exists");
        }
    }];

    //根据key读取数据
    [userInfoCache objectForKey:key withBlock:^(NSString * _Nonnull key, id<NSCoding>  _Nonnull object) {
        NSLog(@"user name : %@",object);
    }];

    //根据key移除缓存
    [userInfoCache removeObjectForKey:key withBlock:^(NSString * _Nonnull key) {
        NSLog(@"remove user name %@",key);
    }];
    
    //移除所有缓存
    [userInfoCache removeAllObjectsWithBlock:^{
        NSLog(@"removing all cache succeed");
    }];

    //移除所有缓存带进度
    [userInfoCache removeAllObjectsWithProgressBlock:^(int removedCount, int totalCount) {
        NSLog(@"remove all cache objects: removedCount :%d  totalCount : %d",removedCount,totalCount);
    } endBlock:^(BOOL error) {
        if(!error){
            NSLog(@"remove all cache objects: succeed");
        }else{
            NSLog(@"remove all cache objects: failed");
        }
    }];
```