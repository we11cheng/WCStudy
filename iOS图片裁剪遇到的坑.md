### iOS图片裁剪遇到的坑

### 问题原因:
### @2x，表示图片的scale是2，但是裁剪的时候，使用UIImage size获取到的其实是图片@2x处理后的大小，也就是除以2之后的大小，所以导致了裁剪的区域错误。

### 解决方案:
### 解决的方案如代码所见，对需要裁剪的高度和宽度都乘以scale就可以了。

```
/**
 *  截取部分图像
 *
 *  @param rect 裁剪的区域
 *  @param image 要裁剪的image
 *  return 根据rect裁剪完的image
 *
 */
- (UIImage *)captureWithRect:(CGRect)rect Image:(UIImage *)image {

	CGImageRef subImageRef = CGImageCreateWithImageInRect(image.CGImage,
                                                          CGRectMake(rect.origin.x,
                                                                     rect.origin.y,
                                                                     rect.size.width * image.scale,
                                                                     rect.size.height * image.scale));
    CGRect smallBounds = CGRectMake(0, 0, CGImageGetWidth(subImageRef), CGImageGetHeight(subImageRef));
    
    //UIGraphicsBeginImageContext(smallBounds.size);
    UIGraphicsBeginImageContextWithOptions(smallBounds.size, NO, [UIScreen mainScreen].scale);
    CGContextRef context = UIGraphicsGetCurrentContext();
    CGContextDrawImage(context, smallBounds, subImageRef);
    UIImage* smallImage = [UIImage imageWithCGImage:subImageRef];
    UIGraphicsEndImageContext();
    
    return smallImage;
    
}
```