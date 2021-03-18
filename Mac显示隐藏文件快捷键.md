# Mac显示隐藏文件快捷键

## 第一种:
在 macOS Sierra及以上(Mojave)，我们可以使用快捷键 ⌘⇧.(Command + Shift + .) 来快速（在 Finder 中）显示和隐藏隐藏文件了。

## 第二种:
在终端使用:

显示隐藏文件
```
defaults write com.apple.finder AppleShowAllFiles -bool true
```

不显示隐藏文件
```
defaults write com.apple.finder AppleShowAllFiles -bool false
```
## 最后需要重启Finder:

重启Finder：窗口左上角的苹果标志-->强制退出-->Finder-->重新启动

