### iOS 横向展示瀑布流 排列不规则长度标题的UIButton

#### 核心代码
```
NSMutableArray *testArray = [NSMutableArray array];
    [testArray addObject:@"看家"];
    [testArray addObject:@"智能硬一"];
    [testArray addObject:@"哇建立技术开发是"];
    [testArray addObject:@"技术"];
    [testArray addObject:@"索朗多"];
    [testArray addObject:@"科技"];
    [testArray addObject:@"索朗多索朗多"];
    [testArray addObject:@"科技"];
    [testArray addObject:@"上课福建省地方"];
    [testArray addObject:@"科技"];
 
    CGFloat startX = 10;
    CGFloat startY = 100;
    CGFloat buttonHeight = 40;
    
    for(int i = 0; i < 10; i++)
    {
        UIButton *btn = [[UIButton alloc]init];
        
        btn.backgroundColor = [UIColor whiteColor];
        btn.layer.cornerRadius = 5;
        [btn setTitle:testArray[i] forState:UIControlStateNormal];
        [btn setTitleColor:[UIColor redColor] forState:UIControlStateNormal];
        [self.view addSubview:btn];
        
        CGSize titleSize = [testArray[i] sizeWithAttributes:@{NSFontAttributeName: [UIFont fontWithName:btn.titleLabel.font.fontName size:btn.titleLabel.font.pointSize]}];
        
        titleSize.height = 20;
        titleSize.width += 20;
    
        if(startX + titleSize.width > [UIScreen mainScreen].bounds.size.width){
            startX = 10;
            startY = startY + buttonHeight + 10;
        }
        btn.frame = CGRectMake(startX, startY, titleSize.width, buttonHeight);
        startX = CGRectGetMaxX(btn.frame) + 10;
    }
```