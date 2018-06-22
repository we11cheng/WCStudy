### Method Swizzling实践
#### 下面我们就以替换 viewWillAppear 方法为例谈谈 Method Swizzling 的最佳实践，话不多说，直接上代码：

```
@interface UIViewController (MRCUMAnalytics)

@end

@implementation UIViewController (MRCUMAnalytics)

+ (void)load {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        Class class = [self class];

        SEL originalSelector = @selector(viewWillAppear:);
        SEL swizzledSelector = @selector(mrc_viewWillAppear:);

        Method originalMethod = class_getInstanceMethod(class, originalSelector);
        Method swizzledMethod = class_getInstanceMethod(class, swizzledSelector);

        BOOL success = class_addMethod(class, originalSelector, method_getImplementation(swizzledMethod), method_getTypeEncoding(swizzledMethod));
        if (success) {
            class_replaceMethod(class, swizzledSelector, method_getImplementation(originalMethod), method_getTypeEncoding(originalMethod));
        } else {
            method_exchangeImplementations(originalMethod, swizzledMethod);
        }
    });
}

#pragma mark - Method Swizzling

- (void)mrc_viewWillAppear:(BOOL)animated {
    [self mrc_viewWillAppear:animated];
    [MobClick beginLogPageView:NSStringFromClass([self class])];
}

@end
```
#### 补充
#### Question1：为什么是在 +load 方法中实现 Method Swizzling 的逻辑，而不是其他的什么方法，比如 +initialize 等。
- Answer1：+load 方法是在类被加载的时候调用的，而 +initialize 方法是在类或它的子类收到第一条消息之前被调用的，这里所指的消息包括实例方法和类方法的调用。也就是说 +initialize 方法是以懒加载的方式被调用的，如果程序一直没有给某个类或它的子类发送消息，那么这个类的 +initialize 方法是永远不会被调用的。此外 +load 方法还有一个非常重要的特性，那就是子类、父类和分类中的 +load 方法的实现是被区别对待的。换句话说在 Objective-C runtime 自动调用 +load 方法时，分类中的 +load 方法并不会对主类中的 +load 方法造成覆盖。综上所述，+load 方法是实现 Method Swizzling 逻辑的最佳“场所”。

#### Question2：为什么 Method Swizzling 的逻辑需要用 dispatch_once 来进行调度。
- Answer2：保证方法只执行一次。

#### Question3：什么需要调用 class_addMethod 方法，并且以它的结果为依据分别处理两种不同的情况。
- Answer3：我们使用 Method Swizzling 的目的通常都是为了给程序增加功能，而不是完全地替换某个功能，所以我们一般都需要在自定义的实现中调用原始的实现。所以这里就会有两种情况需要我们分别进行处理：

- 第 1 种情况：主类本身有实现需要替换的方法，也就是 class_addMethod 方法返回 NO 。这种情况的处理比较简单，直接交换两个方法的实现就可以了：

```
- (void)viewWillAppear:(BOOL)animated {
    /// 先调用原始实现，由于主类本身有实现该方法，所以这里实际调用的是主类的实现
    [self mrc_viewWillAppear:animated];
    /// 我们增加的功能
    [MobClick beginLogPageView:NSStringFromClass([self class])];
}

- (void)mrc_viewWillAppear:(BOOL)animated {
    /// 主类的实现
}
```

- 第 2 种情况：主类本身没有实现需要替换的方法，而是继承了父类的实现，即 class_addMethod 方法返回 YES 。这时使用 class_getInstanceMethod 函数获取到的 originalSelector 指向的就是父类的方法，我们再通过执行 class_replaceMethod(class, swizzledSelector, method_getImplementation(originalMethod), method_getTypeEncoding(originalMethod)); 将父类的实现替换到我们自定义的 mrc_viewWillAppear 方法中。这样就达到了在 mrc_viewWillAppear 方法的实现中调用父类实现的目的。

```
- (void)viewWillAppear:(BOOL)animated {
    /// 先调用原始实现，由于主类本身并没有实现该方法，所以这里实际调用的是父类的实现
    [self mrc_viewWillAppear:animated];
    /// 我们增加的功能
    [MobClick beginLogPageView:NSStringFromClass([self class])];
}

- (void)mrc_viewWillAppear:(BOOL)animated {
    /// 父类的实现
}
```
#### 参考链接 <http://nshipster.com/method-swizzling/>
