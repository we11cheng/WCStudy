## Objective-C 对象模型
### 概念
#### OC是一门面向对象的编程语言，每一个对象都是一个类的实例，在OC中，每一个对象都有一个名为 isa 的指针，指向该对象的类。每一个类描述了一系列它的实例的特点，包括成员变量的列表、成员函数的列表等。每一个对象都可以接受到消息，而对象能够接受到的消息列表保存在它所对应的类中。

#### 为什么结构体内还有个isa指针，它是干什么的？
###### 因为类也是一个对象，那它也必须是另一个类的实列，这个类就是元类 (metaclass)。元类保存了类方法的列表。当一个类方法被调用时，元类会首先查找它本身是否有该类方法的实现，如果没有，则该元类会向它的父类查找该方法，直到一直找到继承链的头。
#### 附上度娘和Google上搜索OC对象模型

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200811172740.png)

#### 消息传递是如何工作的？
```
1. 检查接受对象是否为nil，如果是nil，调用nil处理程序。
2. 检查缓存中是不是已经有方法实现了，有的话，直接调用。
3. 比较请求的选择器和类中定义的选择器，如果找到了，调用方法实现。
4. 比较请求的选择器和父类中定义的选择器，然后是父类的父类，以此类推，如果找到了选择器，调用方法实现。
5. 调用resolveInstanceMethod:(或resolveClassMethod)。如果它返回YES，那么重新开始。这一次对象会响应这个选择器，一般是因为它已经调用过class_addMethod。
6. 调用forwardingTargetForSelector:，如果返回非nil，那就把消息发送到返回的对象上，这里不要返回self，否则会形成死循环的。
7. 调用methodSignatureForSelector:，如果返回非nil，创建一个NSInvocation并传给forwardInvocation:。
8. 调用doesNotRecognizeSelector:，默认的实现是抛出异常。
```
#### 可变与不可变问题？
#### 因为对象在内存中的排布可以看成一个结构体，该结构体的大小并不能动态变化。所以无法在运行时动态给对象增加成员变量。

#### 相对的，对象的方法定义都保存在类的可变区域中。我们可以看到方法的定义列表是一个名为 methodLists的指针的指针。通过修改该指针指向的指针的值，就可以实现动态地为某一个类增加成员方法。

###### 当然这也是Category实现的原理。同时也说明了为什么Category只可为对象增加成员方法，却不能增加成员变量。需要特别说明一下，通过objc_setAssociatedObject 和 objc_getAssociatedObject方法可以变相地给对象增加成员变量，但由于实现机制不一样，所以并不是真正改变了对象的内存结构。
#### 除了对象的方法可以动态修改，因为 isa 本身也只是一个指针，所以我们也可以在运行时动态地修改 isa 指针的值，达到替换对象整个行为的目的。不过该应用场景较少（KVO的实现就用了isa混合）。
#### 方法混写(Method Swizzling)与ISA混写(Isa Swizzling)
##### Objective-C中，混写（Swizzling）是指透明地把一个东西换成另一个，我们可以利用Objective-C中的运行时来实现混写。我们先看下方法混写，Objective-C提供了以下API来动态替换类方法或实例方法的实现：

```
class_replaceMethod替换类方法的定义。
method_exchangeImplementations交换两个方法的实现。
method_setImplementation设置一个方法的实现。
```

#### 三者的区别：
```
class_replaceMethod，当需要替换的方法有可能不存在时，可以考虑使用该方法。
method_exchangeImplementations，当需要交换两个方法的实现时使用。
method_setImplementation是最简单的用法，当仅仅需要为一个方法设置其实现方式时使用。

```
#### 系统中提供的KVO使用到了isa混写，具体是怎么实现的呢？
###### 当你观察一个对象时，一个新的类会被自动创建，这个新类继承自该对象的原本的类，并且重写了被观察属性setter方法。重写setter方法会负责在调用原setter方法之前和之后，通知所有观察对象：值的更改。最后通过isa混写，把这个对象的isa指针指向这个新创建的子类，对象就神奇的变成了新创建的子类的实例。