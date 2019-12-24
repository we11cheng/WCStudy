### Universal Links配置小记
#### Universal Links（通用链接）是iOS9.0出的新技术。如果我们的应用支持通用链接，那么就可以通过https链接来打开APP（手机中已经安装此APP），或者跳转到https链接（手机中没有安装此APP）看起来很厉害的样子。
#### 配置步骤要求条件
```
1.有一个注册的域名。
2.支持https请求，并且CA证书是有效的，这个需要与后端同事进行确认。
3.可上传一个json文件到web服务器
4.APP版本至少为iOS9及以上
5.Xcode版本为7以上
```
### 1.苹果开发者后台配置Associated Domains，如图

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20191223145007.png)

### 2.创建apple-app-site-association文件
首先创建一个apple-app-site-association文件(注意是没有后缀的)，其内容是json格式，官方示例如下
```
{
    "applinks": {
        "apps": [],
        "details": [
            {
                "appID": "TEAMIDSHSAUX.com.test.bundle",
                "paths": [ "*" ]
            }
        ]
    }
}
```
#### 相关注释
#### appID：由TeamID.BundleID组成。TeamID可在开发者中心查看，BundleID可在Xcode中查看。
#### paths：设定一个App的路径支持列表，只有这些指定的路径链接才会被App所处理。（paths是大小写敏感，*是通配符表示任意路径，一般填写这个就可以）
#### apple-app-site-association文件保存的位置
#### 根目录下。`https://test.com/apple-app-site-association`
#### .well-known文件夹下(推荐，苹果在iOS9.3更改了通用链接的请求文件的位置，但是仍然支持上面的路径)。在根目录新建.well-known文件夹(不要忘记前面的.)。`https://test.com/.well-known/apple-app-site-association`
#### 检查
- 使用浏览器打开我们上传的文件路径，应该可以直接看到刚刚上传的json文件，或者是会自动下载到电脑。
- 苹果也提供了一个官方网页供我们开发者来验证https://test.com/apple-app-site-association是否有效。验证地址：<https://search.developer.apple.com/appsearch-validation-tool/>

### Xcode配置
- 首先是将Associated Domains打开，并填写我们的域名，前缀是`applinks`。如果你的域名是test.com，则填上`applinks:test.com`。

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20191223150010.png)

- 配置后会发现项目中多了一个APPNAME.entitlements文件。

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20191223150127.png)

- 重新编译后，同时开发者中心的Associated Domains也会变成启用状态。

#### AppDelegate回调(友盟为例子)
```
//Universal Links 配置
- (BOOL)application:(UIApplication *)application continueUserActivity:(NSUserActivity *)userActivity restorationHandler:(void (^)(NSArray<id<UIUserActivityRestoring>> * _Nullable))restorationHandler {
    if ([userActivity.activityType isEqualToString:NSUserActivityTypeBrowsingWeb]) {
			NSURL *url = userActivity.webpageURL;
			BOOL result = [[UMSocialManager defaultManager] handleOpenURL:url];
			if (!result) {
					// 其他如支付等SDK的回调
			}
			return result;
 
    }
    return YES;
}
```
