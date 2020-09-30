#### Runtime 消息转发
##### 当一个方法找不到的时候，Runtime 提供了 消息动态解析、消息接受者重定向、消息重定向 等三步处理消息，具体流程如下图所示：

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200817183249.png)

##### 第一步:消息动态解析
```
// 类方法未找到时调起，可以在此添加类方法实现
+ (BOOL)resolveClassMethod:(SEL)sel;
// 对象方法未找到时调起，可以在此对象方法实现
+ (BOOL)resolveInstanceMethod:(SEL)sel;

/** 
 * class_addMethod    向具有给定名称和实现的类中添加新方法
 * @param cls         被添加方法的类
 * @param name        selector 方法名
 * @param imp         实现方法的函数指针
 * @param types imp   指向函数的返回值与参数类型
 * @return            如果添加方法成功返回 YES，否则返回 NO
 */
BOOL class_addMethod(Class cls, SEL name, IMP imp, 
                const char * _Nullable types);

```
##### 第二步:消息接受者重定向
```
// 重定向方法的消息接收者，返回一个类或实例对象
- (id)forwardingTargetForSelector:(SEL)aSelector;
```
###### 举个例子
```
#import "ViewController.h"
#include "objc/runtime.h"

@interface Person : NSObject

- (void)fun;

@end

@implementation Person

- (void)fun {
    NSLog(@"fun");
}

@end

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    // 执行 fun 方法
    [self performSelector:@selector(fun)];
}

+ (BOOL)resolveInstanceMethod:(SEL)sel {
    return YES; // 为了进行下一步 消息接受者重定向
}

// 消息接受者重定向
- (id)forwardingTargetForSelector:(SEL)aSelector {
    if (aSelector == @selector(fun)) {
        return [[Person alloc] init];
        // 返回 Person 对象，让 Person 对象接收这个消息
    }
    
    return [super forwardingTargetForSelector:aSelector];
}
```
###### 我们通过 forwardingTargetForSelector 可以修改消息的接收者，该方法返回参数是一个对象，如果这个对象是不是 nil，也不是 self，系统会将运行的消息转发给这个对象执行。否则，继续进行下一步：消息重定向流程。

##### 第三步:消息重定向

```
// 获取函数的参数和返回值类型，返回签名
- (NSMethodSignature *)methodSignatureForSelector:(SEL)aSelector;

// 消息重定向
- (void)forwardInvocation:(NSInvocation *)anInvocation；
```
####### 举个例子
```
#import "ViewController.h"
#include "objc/runtime.h"

@interface Person : NSObject

- (void)fun;

@end

@implementation Person

- (void)fun {
    NSLog(@"fun");
}

@end

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    // 执行 fun 函数
    [self performSelector:@selector(fun)];
}

+ (BOOL)resolveInstanceMethod:(SEL)sel {
    return YES; // 为了进行下一步 消息接受者重定向
}

- (id)forwardingTargetForSelector:(SEL)aSelector {
    return nil; // 为了进行下一步 消息重定向
}

// 获取函数的参数和返回值类型，返回签名
- (NSMethodSignature *)methodSignatureForSelector:(SEL)aSelector {
    if ([NSStringFromSelector(aSelector) isEqualToString:@"fun"]) {
        return [NSMethodSignature signatureWithObjCTypes:"v@:"];
    }
    
    return [super methodSignatureForSelector:aSelector];
}

// 消息重定向
- (void)forwardInvocation:(NSInvocation *)anInvocation {
    SEL sel = anInvocation.selector;   // 从 anInvocation 中获取消息
    
    Person *p = [[Person alloc] init];

    if([p respondsToSelector:sel]) {   // 判断 Person 对象方法是否可以响应 sel
        [anInvocation invokeWithTarget:p];  // 若可以响应，则将消息转发给其他对象处理
    } else {
        [self doesNotRecognizeSelector:sel];  // 若仍然无法响应，则报错：找不到响应方法
    }
}
@end
```

##### 总结:消息发送以及转发机制总结
```

调用 [receiver selector]; 后，进行的流程：
1.编译阶段：[receiver selector]; 方法被编译器转换为:
objc_msgSend(receiver，selector) （不带参数）
objc_msgSend(recevier，selector，org1，org2，…)（带参数）

2.运行时阶段：消息接受者 recever 寻找对应的 selector。
通过 recevier 的 isa 指针 找到 recevier 的 class（类）；
在 class（类） 的 method list（方法列表） 中找对应的 selector；
如果在 class（类） 中没有找到这个 selector，就继续在它的 superclass（父类）中寻找；
一旦找到对应的 selector，直接执行 recever 对应 selector 方法实现的 IMP（方法实现）。
若找不到对应的 selector，Runtime 系统进入消息转发机制。

3.运行时消息转发阶段：
动态解析：通过重写 +resolveInstanceMethod: 或者 +resolveClassMethod:方法，利用 class_addMethod方法添加其他函数实现；
消息接受者重定向：如果上一步添加其他函数实现，可在当前对象中利用 -forwardingTargetForSelector: 方法将消息的接受者转发给其他对象；
消息重定向：如果上一步没有返回值为 nil，则利用 -methodSignatureForSelector:方法获取函数的参数和返回值类型。
如果 -methodSignatureForSelector: 返回了一个 NSMethodSignature 对象（函数签名），Runtime 系统就会创建一个 NSInvocation 对象，并通过 -forwardInvocation: 消息通知当前对象，给予此次消息发送最后一次寻找 IMP 的机会。
如果 -methodSignatureForSelector: 返回 nil。则 Runtime 系统会发出 -doesNotRecognizeSelector: 消息，程序也就崩溃了。
```