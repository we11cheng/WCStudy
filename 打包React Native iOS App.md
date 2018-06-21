### 打包React Native iOS App (不需要每次开启终端服务)

#### 第一步：导出js bundle包和图片资源
- 我们无法通过命令一步进行导出React Native iOS应用。我们需要将JS部分的代码和图片资源等打包导出，然后通过XCode将其添加到iOS项目中。
- 导出js bundle的命令，在React Native项目的根目录下执行：

```
react-native bundle --entry-file index.ios.js --platform ios --dev false --bundle-output release_ios/main.jsbundle --assets-dest release_ios/
```

- 通过上述命令，我们可以将JS部分的代码和图片资源等打包导出到release_ios目录下。

- 在执行打包命令之前，我们需要先确保在我们项目的根目录有release_ios文件夹，没有的话创建一个。

- 然后，修改AppDelegate.m文件，添加如下代码：

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    
  NSURL *jsCodeLocation;
 //jsCodeLocation = [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index.ios" fallbackResource:nil];
 jsCodeLocation = [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
 
...
  return YES;
}
```
####上述代码的作用是让React Native去使用我们刚才导入的jsbundle，这样以来我们就摆脱了对本地nodejs服务器的依赖。
