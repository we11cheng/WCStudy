### 浅谈 iOS swizzle
##### Method Swizzle的本质是在运行时交换方法实现（IMP），一般是在原有的方法中，插入自己的业务需求。
###### 每一个OC实例对象都保存有isa指针和实例变量，其中isa指针所属类，类维护一个运行时可接收的方法列表(MethodLists)；方法列表(MethodLists)中保存selector & IMP的映射关系。在运行时，通过selecter找到匹配的IMP，从而找到的具体的实现函数。
##### 使用swizzle时，我们应该注意哪些问题呢？
##### 如果 originalMethod 是其父类实现的，那么直接 method_exchangeImplementations 是把父类中的 originalMethod 给替换了，导致该父类以及其他子类调用的 originalMethod 也会被替换
##### 解决：
- 通过 class_addMethod 判断 method 是不是属于本类自己实现的？
- class_addMethod 返回 YES -> addMethod 成功，class中不存在 method，也就是存在父类中。addMethod之后，当前class也就存在method 了（覆盖了父类的方法）
class_addMethod 返回 NO  -> addMethod 失败，class中存在 method，说明当前方法属于当前class
- 判断之后，再执行 exchange

```
@implementation Model (Swizzle)
+ (void)load {
    Class class = [self class];
    SEL originalSelector = @selector(hhh);
    SEL swizzledSelector = @selector(new_hhh);
    Method originalMethod = class_getInstanceMethod(class, originalSelector);
    Method swizzledMethod = class_getInstanceMethod(class, swizzledSelector);
    // 添加 originalSelector->swizzle method 到 class
    BOOL success = class_addMethod(class, originalSelector, method_getImplementation(swizzledMethod), method_getTypeEncoding(swizzledMethod));
    if (success) { // 说明originalSelector在父类中
        class_replaceMethod(class, swizzledSelector, method_getImplementation(originalMethod), method_getTypeEncoding(originalMethod));
    } else { // 说明originalSelector在当前类中
        method_exchangeImplementations(originalMethod, swizzledMethod);
    }
}
@end

```
##### 问题二：方法的参数会被改变
#### 解决：C方法+ method_setImplementation 的方式
```
@interface CorrectSwizzleClass : NSObject
- (void) swizzleExample;
- (void) originalMethod;
@end
static IMP __original_Method_Imp;
void replaceImp(id self, SEL _cmd) {
    /*
     * 添加自己的逻辑：比如添加 log
     */
    ((int(*)(id,SEL))__original_Method_Imp)(self, _cmd);
}
@implementation CorrectSwizzleClass
- (void)swizzleExample {
    Method m = class_getInstanceMethod([self class],@selector(originalMethod));
    /// method_setImplementation：return The previous implementation of the method
    __original_Method_Imp = method_setImplementation(m,(IMP)replaceImp);
}
- (void)originalMethod {
    NSLog(@"方法名为 originalMethod，其 _cmd 的值为:%@",[NSString stringWithFormat:@"*** -%@", NSStringFromSelector(_cmd)]);
}
@end

- (void)correct {
    NSLog(@"####################  correct   #######################");
    CorrectSwizzleClass* example = [[CorrectSwizzleClass alloc] init];
    NSLog(@"## swizzle 之前，调用 originalMethod 的打印信息：");
    [example originalMethod];
    [example swizzleExample];
    NSLog(@"## swizzle 之后，调用 originalMethod 的打印信息：");
    [example originalMethod];
}
```
##### 问题三：如何做到对象级别的 swizzle？
```
类本身支持。可以标记一下，在执行方法时，判断是否存在标记来判断是否执行swizzle 之后的方法。可以参考：第三方库 DZNEmptyDataSet（统一空白页）

动态生成一个当前对象所属类的子类，并将当前对象与子类关联。这样的话，swizzle的都是其子类的方法，不会影响父类。可以参考：第三方库  Aspects
```
##### 聊一下Aspects
#### Aspects属于AOP编程的库，源码总数不超过1000行，对外就暴露了两个方法。 使用方式：可以hook 类方法、对象实例方法，还有三种执行位置：before、insert、after
```
/**
 *  事件拦截
 *  拦截UIViewController的viewDidLoad方法
 */
[UIViewController aspect_hookSelector:@selector(viewDidLoad) withOptions:AspectPositionAfter 
usingBlock:^(id<AspectInfo> aspectInfo)
 {
     /**
      *  添加我们要执行的代码，由于withOptions 是 AspectPositionAfter。
      *  所以每个控制器的 viewDidLoad 触发都会执行下面的方法
      */
     [self doSomethings];
 } error:NULL];
- (void)doSomethings {
    //TODO: 比如日志输出、统计代码
    NSLog(@"------");
}
```
#### 简单原理：

```
把待 hook 的 originalSelector 生成 aliasSelector
	1.把待 hook 的 originalSelector 添加前缀aspects_ -> aliasSelector -> 用 block & aliasSelector生成 aspectContainer
	2.通过 associated 把 aspectContainer 绑定到 self( 对象或class)，key为 aliasSelector

把 originalSelector 的 IMP 设置为 _objc_msgForward（会触发消息转发，不会查询方法列表了）

swizzle forwardInvocation
	1.在自定义的 forwardInvocation 中通过 associated & selector -> aliasSelector 获取 aspectContainer
	2.根据 before/insert/after 的规则执行 originalSelector & block

```

