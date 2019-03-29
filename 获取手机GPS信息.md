### 获取手机GPS信息

- 导入 `#import <MapKit/MapKit.h>`

- 引入 

```
@property (strong, nonatomic) CLLocationManager *locationManager;
```

- 初始化

```
- (CLLocationManager *)locationManager {
    if (_locationManager == nil) {
        _locationManager = [[CLLocationManager alloc] init];
    }
    return _locationManager;
}
```

- 使用

```
	#define IS_OS_8_OR_LATER ([[[UIDevice currentDevice] systemVersion] floatValue] >= 8.0)
	
    self.locationManager.delegate = self;
    if(IS_OS_8_OR_LATER) {
        [self.locationManager requestWhenInUseAuthorization];
        [self.locationManager requestAlwaysAuthorization];
    } else {
    [self.locationManager startUpdatingLocation];
    }
```

- 位置回调(前提遵守代理)

```
- (void)locationManager:(CLLocationManager *)manager didChangeAuthorizationStatus:(CLAuthorizationStatus)status {
    switch (status) {
        case kCLAuthorizationStatusDenied: {
            if ([self.locationManager respondsToSelector:@selector(requestWhenInUseAuthorization)]) {
                [self.locationManager requestWhenInUseAuthorization];
            }
        }
            break;
            
        default:
            break;
    }
}

- (void)locationManager:(CLLocationManager *)manager didUpdateLocations:(NSArray<CLLocation *> *)locations {
    CLLocation *newLocation = locations[0];
        if (newLocation) {
            //得到GPS位置
            @{@"lat":[NSNumber numberWithDouble:newLocation.coordinate.latitude],@"lng":[NSNumber numberWithDouble:newLocation.coordinate.longitude]};
        	//gps反解析
        	CLGeocoder *geocoder = [[CLGeocoder alloc] init];
        	[geocoder reverseGeocodeLocation:newLocation completionHandler:^(NSArray<CLPlacemark *> * _Nullable placemarks, NSError * _Nullable error) {
        for (CLPlacemark *place in placemarks) {
            NSLog(@"位置名:%@",place.name);
            NSLog(@"街道:%@",place.thoroughfare);
            NSLog(@"子街道:%@",place.subThoroughfare);
            NSLog(@"市%@",place.locality);
            NSLog(@"区:%@",place.subLocality);
            NSLog(@"国家:%@",place.country);
        }
    }];
}
```