### iOS [UIKeyboardLayoutStar release]: message sent to deallocated

### 情景描述：使用MethodSwizzle 实现对数组、字典 等系统方法的安全校验。显然能达到预期效果，但实际发现当键盘显示的情况下  home app 进入后台，再单击app  图标 切换回前台时 发生crash :

```
[UIKeyboardLayoutStar release]: message sent to deallocated instance
```

### 多方查询，得到解决方案，使用MethodSwizzle修改了NSMutableArray的objectAtIndex:等方法的分类添加非ARC支持，并改写代码实现。
### 示例如下
```
-(id)securityObjectsAtIndexes:(NSUInterger)index {
    @autoreleasepool {
        if (index < self.count) {
            return [self securityObjectsAtIndexes:index];
        }else {
            return nil;
        }
    }
}

```
### 真实代码，可以看到两处添加了`@autoreleasepool {}`
```
#import "NSArray+Gwc_safe.h"
#import "NSObject+Gwc_exchageImp.h"

@implementation NSArray (Gwc_safe)

+ (void)load {
    //只执行一次这个方法
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        
        //替换 objectAtIndex
        NSString *tmpStr = @"objectAtIndex:";
        NSString *tmpFirstStr = @"safe_ZeroObjectAtIndex:";
        NSString *tmpThreeStr = @"safe_objectAtIndex:";
        NSString *tmpSecondStr = @"safe_singleObjectAtIndex:";
        
        [self exchangeImplementationWithClassStr:@"__NSArray0" originalMethodStr:tmpStr newMethodStr:tmpFirstStr];
        
        [self exchangeImplementationWithClassStr:@"__NSSingleObjectArrayI" originalMethodStr:tmpStr newMethodStr:tmpSecondStr];
        
        [self exchangeImplementationWithClassStr:@"__NSArrayI" originalMethodStr:tmpStr newMethodStr:tmpThreeStr];
    });
}

#pragma mark --- implement method

/**
 取出NSArray 第index个 值 对应 __NSArrayI
 
 @param index 索引 index
 @return 返回值
 */
- (id)safe_objectAtIndex:(NSUInteger)index {
    if (index >= self.count){
        return nil;
    }
    return [self safe_objectAtIndex:index];
}


/**
 取出NSArray 第index个 值 对应   __NSSingleObjectArrayI
 
 @param index 索引 index
 @return 返回值
 */
- (id)safe_singleObjectAtIndex:(NSUInteger)index {
    @autoreleasepool {
        if (index >= self.count){
            return nil;
        }
        return [self safe_singleObjectAtIndex:index];
    }
    
}

/**
 取出NSArray 第index个 值 对应 __NSArray0
 
 @param index 索引 index
 @return 返回值
 */
- (id)safe_ZeroObjectAtIndex:(NSUInteger)index {
    @autoreleasepool {
        if (index >= self.count){
            return nil;
        }
        return [self safe_ZeroObjectAtIndex:index];
    }
    
}
```
### 让项目同时支持ARC和非ARC
##### 可以在Build Phases中的Compile Sources中加入编译标记-fno-objc-arc，既可以给ARC 项目添加MRC标记（-fno-objc-arc），也可以给MRC项目添加ARC标记（-fobjc-arc）。如图所示
![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190403131418.jpg)
