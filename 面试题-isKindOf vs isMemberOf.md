#### 面试题-isKindOf vs isMemberOf

```
+ (Class)class {
    return self;// 返回的是本身
}

- (Class)class {
    return object_getClass(self); //返回的是元类 metal class
}
复制代码+ (BOOL)isKindOfClass:(Class)cls {
    for (Class tcls = object_getClass((id)self); tcls; tcls = tcls->superclass) {
        if (tcls == cls) return YES;
    }
    return NO;
}

- (BOOL)isKindOfClass:(Class)cls {
    for (Class tcls = [self class]; tcls; tcls = tcls->superclass) {
        if (tcls == cls) return YES;
    }
    return NO;
}

```
##### 分析
###### +isKindOfClass作为类方法调用的时候，object_getClass传入的参数是类，返回的是元类，多层级遍历之后根元类的父类就是根类，所以isKindOfClass我们只需要判断当前调用的元类、父类和比较类是否相同即可。
###### -isKindOfClass作为实例方法调用的时候，由于此时self是一个实例对象，[self class]，则返回的是当前实例对象的类，所以isKindOfClass需要判断当前调用的类、其父类和比较类是否相同即可。

```
+ (BOOL)isMemberOfClass:(Class)cls {
    return object_getClass((id)self) == cls;
}

- (BOOL)isMemberOfClass:(Class)cls {
    return [self class] == cls;
}

```
##### 分析：
###### +isMemberOfClass作为类方法调用的时候，object_getClass返回的是调用者的元类，所以只需要判断调用者的元类和比较类是否相等即可。
###### -isMemberOfClass作为实例方法调用的时候，就是比较调用实例的类是否和比较类相同。


相关验证

```
BOOL re1 = [(id)[NSObject class] isKindOfClass:[NSObject class]];       // 1
BOOL re2 = [(id)[NSObject class] isMemberOfClass:[NSObject class]];     // 0
BOOL re3 = [(id)[TPerson class] isKindOfClass:[TPerson class]];         // 0
BOOL re4 = [(id)[TPerson class] isMemberOfClass:[TPerson class]];       // 0
NSLog(@" re1 :%hhd\n re2 :%hhd\n re3 :%hhd\n re4 :%hhd\n",re1,re2,re3,re4);

BOOL re5 = [(id)[NSObject alloc] isKindOfClass:[NSObject class]];       // 1
BOOL re6 = [(id)[NSObject alloc] isMemberOfClass:[NSObject class]];     // 1
BOOL re7 = [(id)[TPerson alloc] isKindOfClass:[TPerson class]];         // 1
BOOL re8 = [(id)[TPerson alloc] isMemberOfClass:[TPerson class]];       // 1
NSLog(@" re5 :%hhd\n re6 :%hhd\n re7 :%hhd\n re8 :%hhd\n",re5,re6,re7,re8);
```