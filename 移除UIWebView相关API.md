### ITMS-90809: Deprecated API Usage UIWebView 解决办法

#### 解决办法
```
1：终端CD到你的工程中输入 grep -r UIWebView . （注意后面的点不要忘了，不然会报循环查找的错误）。
2：如果自己写的有UIWebView直接替换成WKWebView
3：如果用到旧版本的MJRefresh，WechatOpenSDK，AlipaySDK-iOS都需要更新到新的版本。因为这些都在.a文件中引用了UIWebView，即使你全局搜索不到UIWebView但是他任然是存在的，只能升级。
4：AFNetworking的旧版本也包含UIWebView但是他比较特殊，可以升级改正，也可以直接删除这两个文件和引用头（因为一般用不上）
```

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200928150508.png)


```
-(void)webView:(UIWebView *)webView didFailLoadWithError:(NSError *)error {
  
}
-(void)webViewDidStartLoad:(UIWebView *)webView {
    [self showIndicatorView];
}
-(void)webViewDidFinishLoad:(UIWebView *)webView {
    [self hideIndicatorView];
    self.errorLab.hidden = YES;
}
```