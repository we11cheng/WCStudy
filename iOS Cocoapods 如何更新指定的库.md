### iOS Cocoapods 如何更新指定的库

#### 更新指定第三方库

`
pod update 库名
`

#### 把Podfile内全部的库更新重新安装

`
pod install
`

#### 只安装新添加的库，已更新的库忽略

`
pod install --verbose --no-repo-update
`

#### 只更新指定的库，其它库忽略

`
pod update 库名 --verbose --no-repo-update
`
