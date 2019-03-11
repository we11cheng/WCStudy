### 成功编译，简单记录一下编译的时候遇到的问题。xcode版本9.2

### Q1:Swift Compiler Error: No such module "libxml2"
### A1: 
```
Please use the latest Xcode(9.3) for latest Fuzi(2.0.2).

For Xcode 9.2 please use Fuzi 2.0.1
```
### 参考地址:<https://github.com/cezheng/Fuzi/issues/85>
---
### Q2:Value of type '[UIView?]' has no member 'compactMap'
### A2: replace `compactMap`为`flatMap `，参看地址地址<https://stackoverflow.com/questions/48731723/how-to-fix-value-of-type-receiptinfo-aka-has-no-member-compactmap>
---
### Q3:Crash : Realm Open Failed,APPdelegate部分`let realmURL = realmDirectoryPath.appendingPathComponent("default.realm")`
### A3:

```
		guard let documentsDirectory = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first else {
            fatalError("Could not access documents directory when setting up Realm")
        }
        let realmDirectoryPath = documentsDirectory.appendingPathComponent("Realm")
        
        do {
            // Disable file protection to allow Realm to be used during Background App Refresh.
            // See https://realm.io/docs/swift/latest/#using-realm-with-background-app-refresh
            try FileManager.default.createDirectory(atPath: realmDirectoryPath.path, withIntermediateDirectories: true, attributes: [.protectionKey: FileProtectionType.none])
        } catch {
            fatalError("Error creating directory: \(error.localizedDescription)")
        }
        let realmURL = realmDirectoryPath.appendingPathComponent("default.realm")
```
### A3参考链接<https://github.com/realm/realm-cocoa/issues/5938>
---
### 百度网盘备份一份：链接:<https://pan.baidu.com/s/1l0RbXKeq2CSv_q8HrO-LZw> 提取码: `ruwj` 复制这段内容后打开百度网盘手机App，操作更方便哦
---
![](https://github.com/we11cheng/WCImageHost/raw/master/WechatIMG1.jpeg)