### kVO原理
#### KVO 是基于 runtime 运行时来实现的，当你观察了某个对象的属性，内部会生成一个该对象所属类的子类，然后重写被观察属性的setter方法，当然在重写的方法中会调用父类的setter方法从而不会影响框架使用者的逻辑，之后会将该对象的isa指针指向新创建的这个类，最后会重写-(Class)class;方法，让使用者通过[obj class]查看当前对象所属类的时候会返回其父类，达到移花接木的目的。

### 好了，原理不难，下面通过一小段代码测试一下：

```
    NSLog(@"class-withOutKVO: %@ \n", object_getClass(_obj));
    NSLog(@"setterAdress-withOutKVO: %p \n", [_obj methodForSelector:@selector(setAName:)])
    [_obj addObserver:self forKeyPath:@"aName" options:NSKeyValueObservingOptionOld|NSKeyValueObservingOptionNew context:(__bridge void *)(self)];
    NSLog(@"class-addKVO: %@ \n", object_getClass(_obj));
    NSLog(@"setterAdress-addKVO: %p \n", [_obj methodForSelector:@selector(setAName:)])
    [_obj removeObserver:self forKeyPath:@"aName"];
    NSLog(@"class-removeKVO: %@", object_getClass(_obj));
    NSLog(@"setterAdress-removeKVO: %p \n", [_obj methodForSelector:@selector(setAName:)])
    
```
#### 打印如下

```
class-withOutKVO: TestObj
setterAdress-withOutKVO: 0x10e819030
class-addKVO: NSKVONotifying_TestObj
setterAdress-addKVO: 0x10f050efe
class-removeKVO: TestObj
setterAdress-removeKVO: 0x10e819030
```
#### 看到了么，我们使用object_getClass ()方法成功躲开了 KVO 的障眼法，发现添加观察过后，_obj的类变成了NSKVONotifying_TestObj，在移除观察过后，_obj的类又变回TestObj。同时，我们还观察了setAName:方法的地址，发现同样是有变化，同样验证了重写setter方法的逻辑。