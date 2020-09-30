### iOS基础题
- 分类和扩展有什么区别？可以分别用来做什么？分类有哪些局限性？分类的结构体里面有哪些成员？

```
1.分类主要用来为某个类添加方法，属性，协议（我一般用来为系统的类扩展方法或者把某个复杂的类的按照功能拆到不同的文件里）
2.扩展主要用来为某个类添加原来没有的成员变量、属性、方法。注：方法只是声明（我一般用扩展来声明私有属性，或者把.h的只读属性重写成可读写的）

分类和扩展的区别：
1.分类是在运行时把分类信息合并到类信息中，而扩展是在编译时，就把信息合并到类中的
2.分类声明的属性，只会生成getter/setter方法的声明，不会自动生成成员变量和getter/setter方法的实现，而扩展会
3.分类不可用为类添加实例变量，而扩展可以，分类可以为类添加方法的实现，而扩展只能声明方法，而不能实现

分类的局限性：
1.无法为类添加实例变量，但可通过关联对象进行实现，注：关联对象中内存管理没有weak，用时需要注意野指针的问题，可通过其他办法来实现，具体可参考iOS weak 关键字漫谈：http://mrpeak.cn/blog/ios-weak/
2.分类的方法若和类中原本的实现重名，会覆盖原本方法的实现，注：并不是真正的覆盖
3.多个分类的方法重名，会调用最后编译的那个分类的实现

分类的结构体里有哪些成员
 struct category_t {
     const char *name; //名字
     classref_t cls; //类的引用
     struct method_list_t *instanceMethods;//实例方法列表
     struct method_list_t *classMethods;//类方法列表
     struct protocol_list_t *protocols;//协议列表
     struct property_list_t *instanceProperties;//实例属性列表
     // 此属性不一定真正的存在
     struct property_list_t *_classProperties;//类属性列表
 };
```
- 讲一下atomic的实现机制；为什么不能保证绝对的线程安全（最好可以结合场景来说）？

```
atomic的实现机制
atomic是property的修饰词之一，表示是原子性的，使用方式为@property(atomic)int age;,此时编译器会自动生成getter/setter方法，最终会调用objc_getProperty和objc_setProperty方法来进行存取属性。若此时属性用atomic修饰的话，在这两个方法内部使用os_unfair_lock来进行加锁，来保证读写的原子性。锁都在PropertyLocks中保存着（在iOS平台会初始化8个，mac平台64个），在用之前，会把锁都初始化好，在需要用到时，用对象的地址加上成员变量的偏移量为key，去PropertyLocks中去取。因此存取时用的是同一个锁，所以atomic能保证属性的存取时是线程安全的。注：由于锁是有限的，不用对象，不同属性的读取用的也可能是同一个锁
atomic为什么不能保证绝对的线程安全？
atomic在getter/setter方法中加锁，仅保证了存取时的线程安全，假设我们的属性是@property(atomic)NSMutableArray *array;可变的容器时,无法保证对容器的修改是线程安全的
在编译器自动生产的getter/setter方法，最终会调用objc_getProperty和objc_setProperty方法存取属性，在此方法内部保证了读写时的线程安全的，当我们重写getter/setter方法时，就只能依靠自己在getter/setter中保证线程安全
```
- 被weak修饰的对象在被释放的时候会发生什么？是如何实现的？知道sideTable么？里面的结构可以画出来么？

```
weak修饰的对象在被释放的时候会发生什么？
被weak修饰的对象在被释放的时候，会把weak指针自动置位nil
weak是如何实现的？

runTime会把对weak修饰的对象放到一个全局的哈希表中，用weak修饰的对象的内存地址为key，weak指针为值，在对象进行销毁时，用通过自身地址去哈希表中查找到所有指向此对象的weak指针，并把所有的weak指针置位nil
sideTable的结构

 struct SideTable {
     spinlock_t slock;//操作SideTable时用到的锁
     RefcountMap refcnts;//引用计数器的值
     weak_table_t weak_table;//存放weak指针的哈希表
 };
```
- 关联对象有什么应用，系统如何管理关联对象？其被释放的时候需要手动将所有的关联对象的指针置空么？

```
关联对象有什么应用？:一般用于在分类中给类添加实例变量
系统如何管理关联对象？
首先系统中有一个全局AssociationsManager,里面有个AssociationsHashMap哈希表，哈希表中的key是对象的内存地址，value是ObjectAssociationMap,也是一个哈希表，其中key是我们设置关联对象所设置的key，value是ObjcAssociation,里面存放着关联对象设置的值和内存管理的策略。
void objc_setAssociatedObject(id object, const void * key,id value, objc_AssociationPolicy policy)为例，首先会通过AssociationsManager获取AssociationsHashMap，然后以object的内存地址为key，从AssociationsHashMap中取出ObjectAssociationMap，若没有，则新创建一个ObjectAssociationMap，然后通过key获取旧值，以及通过key和policy生成新值ObjcAssociation(policy, new_value)，把新值存放到ObjectAssociationMap中，若新值不为nil，并且内存管理策略为retain，则会对新值进行一次retain，若新值为nil，则会删除旧值，若旧值不为空并且内存管理的策略是retain，则对旧值进行一次release
其被释放的时候需要手动将所有的关联对象的指针置空么？
注：对这个问题我的理解是：当对象被释放时，需要手动移除该对象所设置的关联对象吗？ 不需要，因为在对象的dealloc中，若发现对象有关联对象时，会调用_object_remove_assocations方法来移除所有的关联对象，并根据内存策略，来判断是否需要对关联对象的值进行release
```
- KVO的底层实现？如何取消系统默认的KVO并手动触发（给KVO的触发设定条件：改变的值符合某个条件时再触发KVO）？

```
当某个类的属性被观察时，系统会在运行时动态的创建一个该类的子类。并且把改对象的isa指向这个子类

当使用KVC赋值的时候,在NSObject里的setValue:forKey:方法里,若父类不存在setName:或这_setName:这些方法,会调用_NSSetValueAndNotifyForKeyInIvar这个函数，这个函数里同样也会调用willChangeValueForKey:和didChangevlueForKey:,若存在则调用
举例：取消Person类age属性的默认KVO，设置age大于18时，手动触发KVO

 	+ (BOOL)automaticallyNotifiesObserversForKey:(NSString *)key {
 	    if ([key isEqualToString:@"age"]) {
 	        return NO;
 	    }
 	    return [super automaticallyNotifiesObserversForKey:key];
 	}
 	
 	- (void)setAge:(NSInteger)age {
 	    if (age > 18 ) {
 	        [self willChangeValueForKey:@"age"];
 	        _age = age;
 	        [self didChangeValueForKey:@"age"];
 	    }else {
 	        _age = age;
 	    }
 	}
```
- Autoreleasepool所使用的数据结构是什么？AutoreleasePoolPage结构体了解么？

```
Autoreleasepool是由多个AutoreleasePoolPage以双向链表的形式连接起来的，

Autoreleasepool的基本原理：在每个自动释放池创建的时候，会在当前的AutoreleasePoolPage中设置一个标记位，在此期间，当有对象调用autorelsease时，会把对象添加到AutoreleasePoolPage中，若当前页添加满了，会初始化一个新页，然后用双向量表链接起来，并把新初始化的这一页设置为hotPage,当自动释放池pop时，从最下面依次往上pop，调用每个对象的release方法，直到遇到标志位。 AutoreleasePoolPage结构如下

 class AutoreleasePoolPage {
     magic_t const magic;
     id *next;//下一个存放autorelease对象的地址
     pthread_t const thread; //AutoreleasePoolPage 所在的线程
     AutoreleasePoolPage * const parent;//父节点
     AutoreleasePoolPage *child;//子节点
     uint32_t const depth;//深度,也可以理解为当前page在链表中的位置
     uint32_t hiwat;
 }
```

