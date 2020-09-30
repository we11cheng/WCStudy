### iOS xib文件中添加ScrollView约束
#### 刚开始用ScrollVIew的时候，先是在xib中试验的，添加好子布局后无论如何都没法滑动。后来经过诸多尝试终于解决，也正好记录一下自己解决的过程。

- 第1步：添加ScrollView

- 第2步：给ScrollView设置上、下、左、右的约束

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190824113814.png)

- 给ScrollView添加一个ContentView,设置它的上下左右约束，宽度同父布局相等（宽度也可以不相等），高度暂时先不设定，因为后期要用这个特性让其高度自适应内容。

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190824114718.png)

- 因为高度没有确定所以会报错，ContentView加一个固定大小(intrinsic size) ，这个约束报错就没有了。当当程序运行时ContentViw的 size 会根据你的约束重新改变,intrinsic size并不会影响你的约束。

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190824114821.png)

- 给ContentView添加子View，用以将父View撑开，从而可以滑动。

### Demo地址 <https://github.com/we11cheng/WCXIBScrollerView>
