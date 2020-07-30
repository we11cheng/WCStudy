### Python获取当前年月日
#### 首先导入import datetime

```
a = datetime.datetime.now().year
b = datetime.datetime.now().month
c = datetime.datetime.now().day
print(a)
print(b)
print(c)
```
#### 打印结果
```
2020
7
21
```

### 获取当前日期是星期几

#### 首先导入`import datetime`

```
dayOfWeek = datetime.datetime.now().weekday()
print(dayOfWeek)
 
dayOfWeek = datetime.datetime.today().weekday()
print(dayOfWeek)
```
注意：datetime类的weekday()方法可以获得datetime是星期几，注意weekday()返回的是0~6，是星期一到星期日,返回的int类型。