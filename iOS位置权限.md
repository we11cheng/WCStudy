iOS 定位权限获取

#### 在特定场景下我们需要判断用户是否允许应用获取定位权限

- 导入类库

`#import <CoreLocation/CLLocationManager.h>`

- 判断用户手机是否开启了定位服务

```
CLLocationManager的授权状态，此方法会返回当前授权状态：
[CLLocationManager authorizationStatus]

授权状态为枚举值：
kCLAuthorizationStatusNotDetermined                  //用户尚未对该应用程序作出选择
kCLAuthorizationStatusRestricted                     //应用程序的定位权限被限制 
kCLAuthorizationStatusAuthorizedAlways               //一直允许获取定位
kCLAuthorizationStatusAuthorizedWhenInUse            //在使用时允许获取定位
kCLAuthorizationStatusAuthorized                     //已废弃，相当于一直允许获取定位
kCLAuthorizationStatusDenied                         //拒绝获取定位
```
- 判断用户是否授权应用获取定位权限的完整代码

```
    if ([CLLocationManager locationServicesEnabled] && ([CLLocationManager authorizationStatus] == kCLAuthorizationStatusAuthorizedWhenInUse || [CLLocationManager authorizationStatus] == kCLAuthorizationStatusNotDetermined || [CLLocationManager authorizationStatus] == kCLAuthorizationStatusAuthorizedAlways)) {
        
        //定位功能可用
        
    } else if ([CLLocationManager authorizationStatus] ==kCLAuthorizationStatusDenied) {
        //定位不能用
        [UIAlertView showWithTitle:@"提示" message:@"无法获取您的位置,请检查定位服务是否开启" handler:^(UIAlertView *alertView, NSInteger buttonIndex) {
            //todo
        }];
    }
```