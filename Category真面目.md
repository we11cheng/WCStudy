### Category真面目
### 我们知道，所有的OC类和对象，在runtime层都是用struct表示的，category也不例外，在runtime层，category用结构体category_t（在objc-runtime-new.h中可以找到此定义），它包含了
- 类的名字（name）
- 类（cls）
- category中所有给类添加的实例方法的列表（instanceMethods）
- category中所有添加的类方法的列表（classMethods）
- category实现的所有协议的列表（protocols）
- category中添加的所有属性（instanceProperties）

```
typedef struct category_t {
    const char *name;
    classref_t cls;
    struct method_list_t *instanceMethods;
    struct method_list_t *classMethods;
    struct protocol_list_t *protocols;
    struct property_list_t *instanceProperties;
} category_t;
```
从category的定义也可以看出category的可为（可以添加实例方法，类方法，甚至可以实现协议，添加属性）和不可为（无法添加实例变量）。

### 问答环节
- Q1在类的+load方法调用的时候，我们可以调用category中声明的方法么？
- Q2这么些个+load方法，调用顺序是咋样的呢？
- A1可以调用，因为附加category到类的工作会先于+load方法的执行
- A2load的执行顺序是先类，后category，而category的+load执行顺序是根据编译顺序决定的，取决于Compile Source 文件先后顺序。

### 触类旁通-category和方法覆盖
鉴于上面几节我们已经把原理都讲了，这一节只有一个问题:
怎么调用到原来类中被category覆盖掉的方法？
对于这个问题，我们已经知道category其实并不是完全替换掉原来类的同名方法，只是category在方法列表的前面而已，所以我们只要顺着方法列表找到最后一个对应名字的方法，就可以调用原来类的方法：

```
Class currentClass = [MyClass class];
MyClass *my = [[MyClass alloc] init];

if (currentClass) {
    unsigned int methodCount;
    Method *methodList = class_copyMethodList(currentClass, &methodCount);
    IMP lastImp = NULL;
    SEL lastSel = NULL;
    for (NSInteger i = 0; i < methodCount; i++) {
        Method method = methodList[i];
        NSString *methodName = [NSString stringWithCString:sel_getName(method_getName(method)) 
                                        encoding:NSUTF8StringEncoding];
        if ([@"printName" isEqualToString:methodName]) {
            lastImp = method_getImplementation(method);
            lastSel = method_getName(method);
        }
    }
    typedef void (*fn)(id,SEL);

    if (lastImp != NULL) {
        fn f = (fn)lastImp;
        f(my,lastSel);
    }
    free(methodList);
}
```
### 更上一层-category和关联对象
如上所见，我们知道在category里面是无法为category添加实例变量的。但是我们很多时候需要在category中添加和对象关联的值，这个时候可以求助关联对象来实现。

```
MyClass+Category1.h:

#import "MyClass.h"

@interface MyClass (Category1)

@property(nonatomic,copy) NSString *name;

@end
MyClass+Category1.m:

#import "MyClass+Category1.h"
#import <objc/runtime.h>

@implementation MyClass (Category1)

+ (void)load
{
    NSLog(@"%@",@"load in Category1");
}

- (void)setName:(NSString *)name
{
    objc_setAssociatedObject(self,
                             "name",
                             name,
                             OBJC_ASSOCIATION_COPY);
}

- (NSString*)name
{
    NSString *nameObject = objc_getAssociatedObject(self, "name");
    return nameObject;
}

@end
```
关联的对象生命周期跟主对象生命周期一致。