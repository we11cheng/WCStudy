### iOS实现ScrollView中子控件(Button,自定义View)的触摸事件响应

### 既要能实现button点击效果，又要实现画板画画无延迟，那么解决办法来啦~
### 1.首先要设置scrollView的两个属性
    self.scrollView.canCancelContentTouches = YES;
    self.scrollView.delaysContentTouches = NO;

#### delaysContentTouches表示scrollView的子控件响应触摸事件是否有延迟,NO表示立即响应,YES表示延迟响应；canCancelContentTouches与delaysContentTouches相反，假如你设置canCancelContentTouches为YES，那么当你在UIScrollView上面放置任何子视图的时候,在子视图上移动手指的时候，UIScrollView会给子视图发送touchCancel的消息,既不响应触摸事件。而如果该属性设置为NO，ScrollView本身不 处理这个消息，全部交给子视图处理。
### 2.接着创建一个scrollView的分类，实现两个与上面属性配套的方法
### .h
```
#import <UIKit/UIKit.h>

NS_ASSUME_NONNULL_BEGIN

@interface WCScrollView : UIScrollView

@end

NS_ASSUME_NONNULL_END
```
### .m
```
#import "WCScrollView.h"

@implementation WCScrollView

/*
// Only override drawRect: if you perform custom drawing.
// An empty implementation adversely affects performance during animation.
- (void)drawRect:(CGRect)rect {
    // Drawing code
}
*/
    
- (BOOL)touchesShouldCancelInContentView:(UIView *)view {
    if ([view isKindOfClass:[UIButton class]]) {
        return YES;
    }
    return [super touchesShouldCancelInContentView:view];
}
    
- (BOOL)touchesShouldBegin:(NSSet *)touches withEvent:(UIEvent *)event inContentView:(UIView *)view {
    if ([view isKindOfClass:[UIButton class]]) {
        return YES;
    }
	if ([view isKindOfClass:[UIResponder class]]) {
		return YES;
	}
    return NO;
    }
@end
```

