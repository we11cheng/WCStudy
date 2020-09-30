### iOS加载HTML中特殊字符串
#### 接口返回

```
{
        "id" : 19,
        "answer" : "&lt;h1&gt;wefwafwa&lt;\/h1&gt;&lt;div&gt;wfewfwefwefw&lt;\/div&gt;&lt;div&gt;&lt;img src=&quot;http:\/\/codemasterrunbayunbucket.oss-cn-hangzhou.aliyuncs.com\/img\/202009201447000160346.png&quot; alt=&quot;&quot; \/&gt;&lt;br \/&gt;&lt;\/div&gt;",
        "question_title" : "11111",
        "type" : 6,
        "update_time" : 1600584423
      }
```

##### 这些不是标准的HTML字符串 所以我们要先转成带<>的HTML字符串 然后在进行加载

```
-(NSString *)filterHTML:(NSString *)html
{
    NSScanner * scanner = [NSScanner scannerWithString:html];
    NSString * text = nil;
    while([scanner isAtEnd]==NO)
    {
        //找到标签的起始位置
        [scanner scanUpToString:@"&" intoString:nil];
        //找到标签的结束位置
        [scanner scanUpToString:@";" intoString:&text];
        //替换字符
        html = [html stringByReplacingOccurrencesOfString:[NSString stringWithFormat:@"%@>",text] withString:@""];
    }
    //    NSString * regEx = @"<([^>]*)>";
    //    html = [html stringByReplacingOccurrencesOfString:regEx withString:@""];
    return html;
}
//将HTML字符串转化为NSAttributedString富文本字符串
- (NSAttributedString *)attributedStringWithHTMLString:(NSString *)htmlString
{
    NSDictionary *options = @{ NSDocumentTypeDocumentAttribute : NSHTMLTextDocumentType,
                               NSCharacterEncodingDocumentAttribute :@(NSUTF8StringEncoding) };
    
    NSData *data = [htmlString dataUsingEncoding:NSUTF8StringEncoding];
    
    return [[NSAttributedString alloc] initWithData:data options:options documentAttributes:nil error:nil];
}
//将 &lt 等类似的字符转化为HTML中的“<”等
- (NSString *)htmlEntityDecode:(NSString *)string
{
    string = [string stringByReplacingOccurrencesOfString:@"&quot;" withString:@"\""];
    string = [string stringByReplacingOccurrencesOfString:@"&apos;" withString:@"'"];
    string = [string stringByReplacingOccurrencesOfString:@"&lt;" withString:@"<"];
    string = [string stringByReplacingOccurrencesOfString:@"&gt;" withString:@">"];
    string = [string stringByReplacingOccurrencesOfString:@"&amp;" withString:@"&"]; // Do this last so that, e.g. @"&amp;lt;" goes to @"&lt;" not @"<"
    
    return string;
}
```
#### 使用办法
```
NSString *str = [[responseObject valueForKey:@"info"] valueForKey:@"content"];
NSLog(@"%@", str);
str = [self htmlEntityDecode:str];
NSAttributedString *attributedStr = [self attributedStringWithHTMLString:str];
contentLable.attributedText = attributedStr;
[self.webView loadHTMLString:str baseURL:nil];
```

### 后续 iOS 让HTML网页内容和图片自适应UIWebView的宽度

```

	NSString *jScript = @"var meta = document.createElement('meta'); meta.setAttribute('name', 'viewport'); meta.setAttribute('content', 'width=device-width'); document.getElementsByTagName('head')[0].appendChild(meta);";
    WKUserScript *wkUScript = [[WKUserScript alloc] initWithSource:jScript injectionTime:WKUserScriptInjectionTimeAtDocumentEnd forMainFrameOnly:YES];
    WKUserContentController *wkUController = [[WKUserContentController alloc] init];
    [wkUController addUserScript:wkUScript];
    WKWebViewConfiguration *wkWebConfig = [[WKWebViewConfiguration alloc] init];
    wkWebConfig.userContentController = wkUController;

    // 创建设置对象
    WKPreferences *preference = [[WKPreferences alloc]init];
    // 设置字体大小(最小的字体大小)
    preference.minimumFontSize = 15;
    // 设置偏好设置对象
    wkWebConfig.preferences = preference;
       
       
    WKWebView *wkbView = [[WKWebView alloc] initWithFrame:CGRectZero configuration:wkWebConfig];
       wkbView.scrollView.alwaysBounceVertical = NO;
    [self.view addSubview:wkbView];

	NSString *htmlString = [NSString stringWithFormat:@"<html> \n"
    "<head> \n"
    "<style type=\"text/css\"> \n"
    "body {font-size:15px;}\n"
    "</style> \n"
    "</head> \n"
    "<body>"
    "<script type='text/javascript'>"
    "window.onload = function(){\n"
    "var $img = document.getElementsByTagName('img');\n"
    "for(var p in  $img){\n"
    " $img[p].style.width = '100%%';\n"
    "$img[p].style.height ='auto'\n"
    "}\n"
    "}"
    "</script>%@"
    "</body>"
    "</html>", [self htmlEntityDecode:self.baseM.answer]];
    [wkbView loadHTMLString:htmlString baseURL:nil];
```