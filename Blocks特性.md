### Blocks 变量截获局部变量值特性
```
// 使用 Blocks 截获局部变量值
- (void)useBlockInterceptLocalVariables {
    int a = 10, b = 20;

    void (^myLocalBlock)(void) = ^{
        printf("a = %d, b = %d\n",a, b);
    };

    myLocalBlock();    // 打印结果：a = 10, b = 20

    a = 20;
    b = 30;

    myLocalBlock();    // 打印结果：a = 10, b = 20
}
```
###### 明明在第一次调用 myLocalBlock(); 之后已经重新给变量 a、变量 b 赋值了，为什么第二次调用 myLocalBlock(); 的时候，使用的还是之前对应变量的值？
###### 因为 Block 语法的表达式使用的是它之前声明的局部变量 a、变量 b。Blocks 中，Block 表达式截获所使用的局部变量的值，保存了该变量的瞬时值。所以在第二次执行 Block 表达式时，即使已经改变了局部变量 a 和 b 的值，也不会影响 Block 表达式在执行时所保存的局部变量的瞬时值。
###### 这就是 Blocks 变量截获局部变量值的特性。

```

// 使用 __block 说明符修饰，更改局部变量值
- (void)useBlockQualifierChangeLocalVariables {
    __block int a = 10, b = 20;
    void (^myLocalBlock)(void) = ^{
        a = 20;
        b = 30;
        
        printf("a = %d, b = %d\n",a, b);  // 打印结果：a = 20, b = 30
    };
    
    myLocalBlock();
}
```
###### 使用 __block 说明符修饰之后，我们在 Block表达式中，成功的修改了局部变量值。

```
/* —————— retainCycleBlcok.m —————— */   
#import <Foundation/Foundation.h>
#import "Person.h"
int main() {
    Person *person = [[Person alloc] init];
    person.blk = ^{
        NSLog(@"%@",person);
    };

    return 0;
}


/* —————— Person.h —————— */ 
#import <Foundation/Foundation.h>

typedef void(^myBlock)(void);

@interface Person : NSObject
@property (nonatomic, copy) myBlock blk;
@end


/* —————— Person.m —————— */ 
#import "Person.h"

@implementation Person    

@end
```
###### 上面 retainCycleBlcok.m 中 main() 函数的代码会导致一个问题：person 持有成员变量 myBlock blk，而 blk 也同时持有成员变量 person，两者互相引用，永远无法释放。就造成了循环引用问题。通过 __weak 修饰符来消除循环引,如下
```
int main() {
    Person *person = [[Person alloc] init];
    __weak typeof(person) weakPerson = person;

    person.blk = ^{
        NSLog(@"%@",weakPerson);
    };

    return 0;
}
```
###### 通过 __block 引用的 blockPerson，是通过指针的方式来访问 person，而没有对 person 进行强引用，所以不会造成循环引用。