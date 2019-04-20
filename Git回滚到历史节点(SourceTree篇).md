### Git回滚到历史节点(SourceTree篇)

- 点击历史节点，重置到历史节点 


![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190420122126.png)


- 选择硬合并


![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190420122336.png)


- 点击当前节点，重置到当前节点


![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190420122521.png)


- 选择软合并


![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190420122625.png)


- 提交


![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190420122724.png)

### 概括一下

```
两个节点 当前节点(最新节点) 与 历史节点 
1 点击历史节点，重置到历史节点，选择硬合并； 
2 点击当前节点，重置到当前节点，选择软合并； 
3 提交；
```
### 合并的几个概念
```
1.回退到暂存区 软合并
2.回退到未暂存区
3.直接把提交的文件reset （最好不要用）硬合并
```
