### Airtest 断言使用总结
#### airtest提供了assert_equal和assert_not_equal两个接口，来断言传入的两个值相等或者不相等。

跟上述的俩个接口一样，这俩个接口也支持Android、iOS和Windows平台。它们的参数如下：

first – 第一个值

second – 第二个值

msg – 断言的简短描述，它将被记录在报告中


```
# assert_equal("实际值", "预测值", "请填写测试点.")

# 获取控件的“text”属性值
value = poco("com.miui.calculator:id/btn_8_s").attr("text")
assert_equal(value, "8", "按钮值为8")
```

### 处理断言失败
#### 不论是airtest提供的断言接口，还是Airtest-selenium提供的断言接口，如果断言失败，都会引发AssertionError，从而导致脚本执行终止；如果不想脚本因为一个断言失败就终止，可以将断言用try语句包起来：

```
value = poco("com.miui.calculator:id/btn_8_s").attr("text")

try:
    assert_equal(value, "8", "按钮值为8")
except AssertionError:
    print("按钮值断言失败")

poco("com.miui.calculator:id/btn_9_s").click()
```
