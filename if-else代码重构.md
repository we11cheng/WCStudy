### if-else代码重构
为什么我们写的代码都是if-else？
程序员想必都经历过这样的场景：刚开始自己写的代码很简洁，逻辑清晰，函数精简，没有一个if-else，

可随着代码逻辑不断完善和业务的瞬息万变:比如需要对入参进行类型和值进行判断；这里要判断下对象是否为null；不同类型执行不同的流程。

落地到具体实现只能不停地加if-else来处理，渐渐地，代码变得越来越庞大，函数越来越长，文件行数也迅速突破上千行，维护难度也越来越大，到后期基本达到一种难以维护的状态。

虽然我们都很不情愿写出满屏if-else的代码，可逻辑上就是需要特殊判断，很绝望，可也没办法避免啊。

其实回头看看自己的代码，写if-else不外乎两种场景：异常逻辑处理和不同状态处理。

两者最主要的区别是：异常逻辑处理说明只能一个分支是正常流程，而不同状态处理都所有分支都是正常流程。

怎么理解？举个例子：

```
//举例一：异常逻辑处理例子
Object obj = getObj();
if (obj != null) {
    //do something
}else{
    //do something
}
```
```
//举例二：状态处理例子
Object obj = getObj();
if (obj.getType == 1) {
    //do something
}else if (obj.getType == 2) {
    //do something
}else{
    //do something
}
```
第一个例子`if (obj != null)`是异常处理，是代码健壮性判断，只有if里面才是正常的处理流程，`else`分支是出错处理流程；

而第二个例子不管type等于1，2还是其他情况，都属于业务的正常流程。对于这两种情况重构的方法也不一样。

代码if-else代码太多有什么缺点？
缺点相当明显了：

最大的问题是代码逻辑复杂，维护性差，极容易引发bug。
如果使用if-else，说明if分支和else分支的重视是同等的，但大多数情况并非如此，容易引起误解和理解困难。


是否有好的方法优化？如何重构？
方法肯定是有的。重构if-else时，心中无时无刻把握一个原则：

尽可能地维持正常流程代码在最外层。
意思是说，可以写if-else语句时一定要尽量保持主干代码是正常流程，避免嵌套过深。

实现的手段有：减少嵌套、移除临时变量、条件取反判断、合并条件表达式等。

下面举几个实例来讲解这些重构方法：

异常逻辑处理型重构方法实例一：
重构前：

```
double disablityAmount(){
    if(_seniority < 2) 
        return 0;
 
    if(_monthsDisabled > 12)
        return 0;
 
    if(_isPartTime)
        return 0;
 
    //do somethig
 
}
```
重构后：

```
double disablityAmount(){
    if(_seniority < 2 || _monthsDisabled > 12 || _isPartTime) 
        return 0;
 
    //do somethig
}
```

这里的重构手法叫合并条件表达式：如果有一系列条件测试都得到相同结果，将这些结果测试合并为一个条件表达式。

这个重构手法简单易懂，带来的效果也非常明显，能有效地较少if语句，减少代码量逻辑上也更加易懂。

异常逻辑处理型重构方法实例二：
重构前：

```
double getPayAmount(){
    double result;
    if(_isDead) {
        result = deadAmount();
    }else{
        if(_isSeparated){
            result = separatedAmount();
        }
        else{
            if(_isRetired){
                result = retiredAmount();
            else{
                result = normalPayAmount();
            }
        }
    }
    return result;
```
重构后：

```
double getPayAmount(){
    if(_isDead) 
        return deadAmount();
 
    if(_isSeparated)
        return separatedAmount();
 
    if(_isRetired)
        return retiredAmount();
 
    return normalPayAmount();
}
```
怎么样？比对两个版本，会发现重构后的版本逻辑清晰，简洁易懂。

和重构前到底有什么区别呢？

最大的区别是减少if-else嵌套。

可以看到，最初的版本if-else最深的嵌套有三层，看上去逻辑分支非常多，进到里面基本都要被绕晕。其实，仔细想想嵌套内的if-else和最外层并没有关联性的，完全可以提取最顶层。

改为平行关系，而非包含关系，if-else数量没有变化，但是逻辑清晰明了，一目了然。
另一个重构点是废除了`result`临时变量，直接return返回。好处也显而易见直接结束流程，缩短异常分支流程。原来的做法先赋值给result最后统一return，那么对于最后return的值到底是那个函数返回的结果不明确，增加了一层理解难度。



总结重构的要点：如果if-else嵌套没有关联性，直接提取到第一层，一定要避免逻辑嵌套太深。尽量减少临时变量改用return直接返回。

异常逻辑处理型重构方法实例三：
重构前：

```
public double getAdjustedCapital(){
    double result = 0.0;
    if(_capital > 0.0 ){
        if(_intRate > 0 && _duration >0){
            resutl = (_income / _duration) *ADJ_FACTOR;
        }
    }
    return result;
}
```
第一步，运用第一招:减少嵌套和移除临时变量：

```
public double getAdjustedCapital(){
    if(_capital <= 0.0 ){
        return 0.0;
    }
    if(_intRate > 0 && _duration >0){
        return (_income / _duration) *ADJ_FACTOR;
    }
    return 0.0;
}
```
这样重构后，还不够，因为主要的语句`(_income / _duration) *ADJ_FACTOR;`在if内部，并非在最外层，根据优化原则（尽可能地维持正常流程代码在最外层），可以再继续重构：

```
public double getAdjustedCapital(){
    if(_capital <= 0.0 ){
        return 0.0;
    }
    if(_intRate <= 0 || _duration <= 0){
        return 0.0;
    }
 
    return (_income / _duration) *ADJ_FACTOR;
}
```
这才是好的代码风格，逻辑清晰，一目了然，没有if-else嵌套难以理解的流程。

这里用到的重构方法是：将条件反转使异常情况先退出，让正常流程维持在主干流程。

异常逻辑处理型重构方法实例四：
重构前：
```
   /* 查找年龄大于18岁且为男性的学生列表 */
    public ArrayList<Student> getStudents(int uid){
        ArrayList<Student> result = new ArrayList<Student>();
        Student stu = getStudentByUid(uid);
        if (stu != null) {
            Teacher teacher = stu.getTeacher();
            if(teacher != null){
                ArrayList<Student> students = teacher.getStudents();
                if(students != null){
                    for(Student student : students){
                        if(student.getAge() > = 18 && student.getGender() == MALE){
                            result.add(student);
                        }
                    }
                }else {
                    logger.error("获取学生列表失败");
                }
            }else {
                logger.error("获取老师信息失败");
            }
        } else {
            logger.error("获取学生信息失败");
        }
        return result;
    }
```
典型的"箭头型"代码，最大的问题是嵌套过深，解决方法是异常条件先退出，保持主干流程是核心流程：

重构后：

```
   /* 查找年龄大于18岁且为男性的学生列表 */
    public ArrayList<Student> getStudents(int uid){
        ArrayList<Student> result = new ArrayList<Student>();
        Student stu = getStudentByUid(uid);
        if (stu == null) {
            logger.error("获取学生信息失败");
            return result;
        }
 
        Teacher teacher = stu.getTeacher();
        if(teacher == null){
            logger.error("获取老师信息失败");
            return result;
        }
 
        ArrayList<Student> students = teacher.getStudents();
        if(students == null){
            logger.error("获取学生列表失败");
            return result;
        }
 
        for(Student student : students){
            if(student.getAge() > 18 && student.getGender() == MALE){
                result.add(student);
            }
        }
        return result;
    }
```
状态处理型重构方法实例一
重构前：

```
double getPayAmount(){
    Object obj = getObj();
    double money = 0;
    if (obj.getType == 1) {
        ObjectA objA = obj.getObjectA();
        money = objA.getMoney()*obj.getNormalMoneryA();
    }
    else if (obj.getType == 2) {
        ObjectB objB = obj.getObjectB();
        money = objB.getMoney()*obj.getNormalMoneryB()+1000;
    }
}
```
重构后：

```
double getPayAmount(){
    Object obj = getObj();
    if (obj.getType == 1) {
        return getType1Money(obj);
    }
    else if (obj.getType == 2) {
        return getType2Money(obj);
    }
}
 
double getType1Money(Object obj){
    ObjectA objA = obj.getObjectA();
    return objA.getMoney()*obj.getNormalMoneryA();
}
 
double getType2Money(Object obj){
    ObjectB objB = obj.getObjectB();
    return objB.getMoney()*obj.getNormalMoneryB()+1000;
}
```
这里使用的重构方法是：把if-else内的代码都封装成一个公共函数。函数的好处是屏蔽内部实现，缩短if-else分支的代码。代码结构和逻辑上清晰，能一下看出来每一个条件内做的功能。

状态处理型重构方法实例二
针对状态处理的代码，一种优雅的做法是用多态取代条件表达式(《重构》推荐做法)。

你手上有个条件表达式，它根据对象类型的不同而选择不同的行为。将这个表达式的每个分支放进一个子类内的覆写函数中，然后将原始函数声明为抽象函数。
重构前：

```
double getSpeed(){
    switch(_type){
        case EUROPEAN:
            return getBaseSpeed();
        case AFRICAN:
            return getBaseSpeed()-getLoadFactor()*_numberOfCoconuts;
        case NORWEGIAN_BLUE:
            return (_isNailed)?0:getBaseSpeed(_voltage);
    }
}
```
重构后：

```
class Bird{
    abstract double getSpeed();
}
 
class European extends Bird{
    double getSpeed(){
        return getBaseSpeed();
    }
}
 
class African extends Bird{
    double getSpeed(){
        return getBaseSpeed()-getLoadFactor()*_numberOfCoconuts;
    }
}
 
class NorwegianBlue extends Bird{
    double getSpeed(){
        return (_isNailed)?0:getBaseSpeed(_voltage);
    }
}
```
可以看到，使用多态后直接没有了if-else，但使用多态对原来代码修改过大，需要一番功夫才行。最好在设计之初就使用多态方式。

总结
if-else代码是每一个程序员最容易写出的代码，同时也是最容易被写烂的代码，稍不注意，就产生一堆难以维护和逻辑混乱的代码。

针对条件型代码重构把握一个原则：

尽可能地维持正常流程代码在最外层，保持主干流程是正常核心流程。
为维持这个原则：合并条件表达式可以有效地减少if语句数目；减少嵌套能减少深层次逻辑；

异常条件先退出自然而然主干流程就是正常流程。

针对状态处理型重构方法有两种：一种是把不同状态的操作封装成函数，简短if-else内代码行数；另一种是利用面向对象多态特性直接干掉了条件判断。