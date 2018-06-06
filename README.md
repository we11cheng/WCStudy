## class-dump工具的使用

### class-dump可以将Mach-O文件中的Objective-C运行时的声明信息导出，即编写OC代码时的.h文件。class-dump只能导出未加密的app头文件，class-dump是对"otool -ov"信息的翻译.以一种我们熟悉的易读的方式呈现。
#### otool简单使用
- ```cd ../xx00.app``` 进入APP包文件目录
- ```otool -L 依赖库名称``` 依赖库的查询
- ```otool -l WeChat.app/WeChat | grep -B 2 crypt```是否加壳
