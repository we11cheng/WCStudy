### QQ分享小记
### 腾讯开放平台登陆注册应用，需要审核。地址<https://connect.qq.com/>
### 解惑开始
- 1Q:注册应用的时候，需要填写URL schema，这个是干嘛用的。
- 1A: 设置这个URL schema是为了保证在分享完成或取消的时候自己的app 能被唤起,将申请的qq app id 转换为相应的 URL Schema得来。格式如下

![](https://gitee.com/he11oworld/picBed/raw/master/20201130113507.png)

```
"QQ" + 腾讯QQ互联应用appId 转换成十六进制 (不足8位前面补0). 例如：QQ05FA957C
附：将十进制的app id 转换为十六进制的方法：
方法①：在命令行中输入以下命令：
echo 'ibase=10;obase=16;您的腾讯QQ互联应用Id'|bc
方法②：在线十进制转化十六<https://tool.oschina.net/hexconvert/>
```

- 2Q:如何让配置SSO白名单。
- 2A:如果你的应用使用了如SSO授权登录或跳转到第三方分享功能，在iOS9/10下就需要增加一个可跳转的白名单，即LSApplicationQueriesSchemes，否则将在SDK判断是否跳转时用到的canOpenURL时返回NO，进而只进行webview授权或授权/分享失败。 在项目中的info.plist中加入应用白名单，右键info.plist选择source code打开(plist具体设置在Build Setting -> Packaging -> Info.plist File可获取plist路径) 

```
<key>LSApplicationQueriesSchemes</key>
<array>
    <!-- 微信 URL Scheme 白名单-->
    <string>wechat</string>
    <string>weixin</string>

    <!-- 新浪微博 URL Scheme 白名单-->
    <string>sinaweibohd</string>
    <string>sinaweibo</string>
    <string>sinaweibosso</string>
    <string>weibosdk</string>
    <string>weibosdk2.5</string>

    <!-- QQ、Qzone URL Scheme 白名单-->
    <string>mqqapi</string>
    <string>mqq</string>
    <string>mqqOpensdkSSoLogin</string>
    <string>mqqconnect</string>
    <string>mqqopensdkdataline</string>
    <string>mqqopensdkgrouptribeshare</string>
    <string>mqqopensdkfriend</string>
    <string>mqqopensdkapi</string>
    <string>mqqopensdkapiV2</string>
    <string>mqqopensdkapiV3</string>
    <string>mqqopensdkapiV4</string>
    <string>mqzoneopensdk</string>
    <string>wtloginmqq</string>
    <string>wtloginmqq2</string>
    <string>mqqwpa</string>
    <string>mqzone</string>
    <string>mqzonev2</string>
    <string>mqzoneshare</string>
    <string>wtloginqzone</string>
    <string>mqzonewx</string>
    <string>mqzoneopensdkapiV2</string>
    <string>mqzoneopensdkapi19</string>
    <string>mqzoneopensdkapi</string>
    <string>mqqbrowser</string>
    <string>mttbrowser</string>
    <string>tim</string>
    <string>timapi</string>
    <string>timopensdkfriend</string>
    <string>timwpa</string>
    <string>timgamebindinggroup</string>
    <string>timapiwallet</string>
    <string>timOpensdkSSoLogin</string>
    <string>wtlogintim</string>
    <string>timopensdkgrouptribeshare</string>
    <string>timopensdkapiV4</string>
    <string>timgamebindinggroup</string>
    <string>timopensdkdataline</string>
    <string>wtlogintimV1</string>
    <string>timapiV1</string>

    <!-- 支付宝 URL Scheme 白名单-->
    <string>alipay</string>
    <string>alipayshare</string>

    <!-- 钉钉 URL Scheme 白名单-->
      <string>dingtalk</string>
      <string>dingtalk-open</string>

    <!--Linkedin URL Scheme 白名单-->
    <string>linkedin</string>
    <string>linkedin-sdk2</string>
    <string>linkedin-sdk</string>

    <!-- 点点虫 URL Scheme 白名单-->
    <string>laiwangsso</string>

    <!-- 易信 URL Scheme 白名单-->
    <string>yixin</string>
    <string>yixinopenapi</string>

    <!-- instagram URL Scheme 白名单-->
    <string>instagram</string>

    <!-- whatsapp URL Scheme 白名单-->
    <string>whatsapp</string>

    <!-- line URL Scheme 白名单-->
    <string>line</string>

    <!-- Facebook URL Scheme 白名单-->
    <string>fbapi</string>
    <string>fb-messenger-api</string>
    <string>fb-messenger-share-api</string>
    <string>fbauth2</string>
    <string>fbshareextension</string>

    <!-- Kakao URL Scheme 白名单-->  
    <!-- 注：以下第一个参数需替换为自己的kakao appkey--> 
    <!-- 格式为 kakao + "kakao appkey"-->    
    <string>kakaofa63a0b2356e923f3edd6512d531f546</string>
    <string>kakaokompassauth</string>
    <string>storykompassauth</string>
    <string>kakaolink</string>
    <string>kakaotalk-4.5.0</string>
    <string>kakaostory-2.9.0</string>

   <!-- pinterest URL Scheme 白名单-->  
    <string>pinterestsdk.v1</string>

   <!-- Tumblr URL Scheme 白名单-->  
    <string>tumblr</string>

   <!-- 印象笔记 -->
    <string>evernote</string>
    <string>en</string>
    <string>enx</string>
    <string>evernotecid</string>
    <string>evernotemsg</string>

   <!-- 有道云笔记-->
    <string>youdaonote</string>
    <string>ynotedictfav</string>
    <string>com.youdao.note.todayViewNote</string>
    <string>ynotesharesdk</string>

   <!-- Google+-->
    <string>gplus</string>

   <!-- Pocket-->
    <string>pocket</string>
    <string>readitlater</string>
    <string>pocket-oauth-v1</string>
    <string>fb131450656879143</string>
    <string>en-readitlater-5776</string>
    <string>com.ideashower.ReadItLaterPro3</string>
    <string>com.ideashower.ReadItLaterPro</string>
    <string>com.ideashower.ReadItLaterProAlpha</string>
    <string>com.ideashower.ReadItLaterProEnterprise</string>

   <!-- VKontakte-->
    <string>vk</string>
    <string>vk-share</string>
    <string>vkauthorize</string>

   <!-- Twitter-->
    <string>twitter</string>
    <string>twitterauth</string>
</array>
```

- 3Q:如何配置URL Scheme
- 3A:如图

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20191223141758.png)

微信直接填AppID

![](https://gitee.com/he11oworld/picBed/raw/master/20201216181029.png)

- 4Q:如何处理分享回调。
- 4A:如下代码

```
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey, id> *)options {
	BOOL result = [[UMSocialManager defaultManager] handleOpenURL:url];
	if (!result) {
			// 其他如支付等SDK的回调
	}
	return result;
}
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
