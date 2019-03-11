### iTunes将你的iOS设备备份到外置磁盘。

- 退出 iTunes（如果你已经打开了的话），并连接你的外置磁盘。

- 打开 Finder，依次点击「前往」-「前往文件夹」，在弹出窗口中输入如下路径（iTunes 的默认备份路径）：`~/Library/Application Support/MobileSync/Backup`

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190119122700.png)

- 将 Backup 文件夹中的备份文件（如有）拷贝到外置磁盘中。完成后，即可删除 Backup 文件夹或将其重命名为其他名字，例如 Backup old（一定要执行删除或重命名操作，这很重要！）

- 在可移动磁盘创建文件夹，如下图
![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190119123015.png)

- 建立软连接 `ln -s /Volumes/TOSHIBA\ EXT/iPersistenceBackup ~/Library/Application Support/MobileSync/Backup`

- 重新打开`~/Library/Application Support/MobileSync/Backup`

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190119123350.png)

