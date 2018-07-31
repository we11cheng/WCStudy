## VC多层push后回到指定页面的方法
#### 使用场景：场景如下： RootVC -- > A -- > B -- > C，然后现在要求C直接pop回到A。
#### 方法：写一个UIViewController的catrgory，有的同学可能会怀疑B会不会内存泄露，可以在B中打印dealloc。
#### 代码如下
```
- (void)backToController:(NSString *)ctrolClassName animated:(BOOL)animated {
    if (self.navigationController) {
        NSArray *controllers = self.navigationController.viewControllers;
        NSArray *result = [controllers filteredArrayUsingPredicate:[NSPredicate predicateWithBlock:^BOOL(id  _Nullable evaluatedObject, NSDictionary<NSString *,id> * _Nullable bindings) {
            return [evaluatedObject isKindOfClass:NSClassFromString(ctrolClassName)];
        }]];
        if (result.count > 0) {
            [self.navigationController popToViewController:result[0] animated:animated];
        }
    }
}

- (void)pop {
    [self backToController:@"A_ViewController" animated:YES];
}
```