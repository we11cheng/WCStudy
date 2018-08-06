## 简述 `KVO` 的实现机制。 

可以看我之前写的博客 - [KVC-KVO](https://github.com/liberalisman/KVC-KVO)

实际上是采用了 isa——swilling 的方法。

- 利用 `Runtime API` 动态生成一个子类，并且让 `instance` 对象的 isa 指向这个全新的子类。
- 当修改 instance 对象的属性时，会调用 Foundation 的`_NSSetXXXValueAndNotify` 函数

    ```objc
    willChangeValueForKey:
    父类原来的setter
    didChangeValueForKey:
    ```

- 内部会触发监听器（Oberser）的监听方法`observeValueForKeyPath:ofObject:change:context:`。

## `KVO` 在使用过程中有哪些注意点？有没有使用过其他优秀的 `KVO` 三方替代框架？

##### 注意点

* 1.在继承关系中，如果父类也绑定了一些 `KVO`，子类在自己的路径中没有找到对应的路径是应该去父类找的，所以要显示调用 `super`。

* 2.父类和子类有可能对同一个属性进行观察，我们知道如果对同一个属性的观察者移除两次会造成崩溃，所以我们每个类应该有唯一的 `Context` 进行区分。

###### 替代品 ：FaceBook - [KVOController](https://github.com/facebook/KVOController)

## 6.如何做到 `KVO` 手动通知？

```objc
显式的调用 didChangeValueForKey:
```

如果想实现手动通知，我们需要借助一个额外的方法

```objc
+ (BOOL)automaticallyNotifiesObserversForKey:(NSString *)key
```

这个方法默认返回`YES`,用来标记 `Key` 指定的属性是否支持 `KVO`，如果返回值为 `NO`，则需要我们手动更新。

## 在什么情况下会触发 `KVO`? 

###### 1.使用了KVC

使用了 `KVC`，如果有访问器方法，则运行时会在访问器方法中调用 `will/didChangeValueForKey:` 方法；
没用访问器方法，运行时会在 `setValue:forKey` 方法中调用 `will/didChangeValueForKey:`方法。

###### 2.有访问器方法

运行时会重写访问器方法调用 `will/didChangeValueForKey:` 方法。
因此，直接调用访问器方法改变属性值时，`KVO` 也能监听到。

###### 3.直接调用

显式调用 `will/didChangeValueForKey:` 方法。

## 具体使用

### 有时我们会有理由不想用 KeyValueObserver 辅助类。创建另一个对象会有额外的性能开销。如果我们观察很多个键的话，这个开销可能会变得明显。
#### 如果我们在实现一个类的时候把它自己注册为观察者的话：

```
- (void)addObserver:(NSObject *)anObserver
         forKeyPath:(NSString *)keyPath
            options:(NSKeyValueObservingOptions)options
            context:(void *)context
```
### 非常重要的点是我们要传入一个这个类唯一的 context。我们推荐把以下代码```static int const PrivateKVOContext;```写在这个类 .m 文件的顶端，然后我们像这样调用 API 并传入 PrivateKVOContext 的指针：

```
[otherObject addObserver:self forKeyPath:@"someKey" options:someOptions context:&PrivateKVOContext];
```
然后我们这样写 -observeValueForKeyPath:... 的方法：

```
- (void)observeValueForKeyPath:(NSString *)keyPath
                      ofObject:(id)object
                        change:(NSDictionary *)change
                       context:(void *)context
{
    if (context == &PrivateKVOContext) {
        // 这里写相关的观察代码
    } else {
        [super observeValueForKeyPath:keyPath ofObject:object change:change context:context];
    }
}
```
这将确保我们写的子类都是正确的。如此一来，子类和父类都能安全的观察同样的键值而不会冲突。否则我们将会碰到难以 debug 的奇怪行为。




