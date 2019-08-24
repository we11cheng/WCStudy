### AFN DELETE PUT 后台接受不到参数解决办法。
#### 问题起源：AFN现在成了网络请求必用的第三方框架，AFN目前支持GET/POST/PUT/DELETE/HEAD请求.其中，GET与POST请求是使用的最多的请求。但是最近跟后台的交互中，应用了大量的PUT与DELETE请求，这让我有了一些全新的体验。PUT请求与POST基本一致，参数的传递也是传递字典即可。而DELETE请求的过程中，让我遇到了一个很大的坑。

      AFN做网络请求时，如果是用DELETE做请求时，AFN规定，要将请求参数放在URL中传递。 在类 AFURLRequestSerialization中，有这样一句设置，

self.HTTPMethodsEncodingParametersInURI = [NSSet setWithObjects:@"GET", @"HEAD", @"DELETE",nil];
      其中，HTTPMethodsEncodingParametersInURI 的意思很明显，就是指在URI中传递参数的方法有：GET/HEAD/DELETE。

      但是，有时后台会要求将参数放在请求体中传递。所以，这时候就需要重新设置一下HTTPMethodsEncodingParametersInURI。

      首先实例化AFHTTPSessionManager，然后在HTTPMethodsEncodingParametersInURI之中，将DELETE去掉就可以了。 代码如下：

  1.实例化manager,BASEURL是自己设置的
  
  ```
AFHTTPSessionManager *manager = [[AFHTTPSessionManager alloc] initWithBaseURL:[NSURL URLWithString:BASEURL]];

manager.requestSerializer.HTTPMethodsEncodingParametersInURI = [NSSet setWithObjects:@"GET", @"HEAD", nil];
 
