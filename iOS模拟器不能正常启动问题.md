### iOS模拟器不能正常启动问题<Unable to boot device because it cannot be located on disk.>

#### 事情背景：电脑做了一下磁盘清理，导致Xcode编译的时候模拟器无法正常启动。
#### 解决办法：在终端中输入 `xcrun simctl erase all` 完成该命令即可。