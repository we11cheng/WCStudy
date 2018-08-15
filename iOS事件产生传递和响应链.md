### iOS事件产生、传递和响应链
#### 简单介绍一下 iOS 开发中事件的产生、传递和响应链。
#### 事件产生：
#### 系统注册了一个 Source 1（基于 mach port）用来接收系统事件，其回调函数为 _IOHIDEventSystemClientQueueCallback() 。当一个硬件事件（比如触摸/锁屏/摇晃等）发生后，首先由 IOKit.framework 生成一个 IOHIDEvent 事件并由 SpringBoard 接收，SpringBoard 只接收按键（锁屏/静音等）、触摸、加速、传感器等几种 event ，随后用 mach port 转发给需要的 APP 进程。随后苹果注册的哪个 Source 1 就会触发回调，并调用 _UIApplicationHandleEventQueue() 进行应用内部的分发。
#### _UIApplicationHandleEventQueue() 会以先进先出的顺序把 IOHIDEvent 处理并包装成 UIEvent 进行处理分发，其中包括识别 UIGesture /处理屏幕旋转/发送给 UIWindow 等。通常事件比如 UIButton 点击、touchesBegan/Moved/End/Cancel 等都是在这个回调中完成的。
#### 触摸事件传递，大致是从父控件传递到子控件：
### UIApplication -> UIWindow -> UIView （or Gesture recognizer 这时会被当前 vc 截断） -> 寻找处理事件最合适的 view
#### 那么如何寻找处理事件最合适的 view ？步骤：

- 首先判断主窗口能否接收事件
- 触摸点是否在自己身上
- 若在，从后往前遍历自己的子控件，重复前两个步骤（能否接收事件？是否在控件上？）
- 直到找不到合适的子控件，那么自己就成为最合适的 view ，之后调用具体的如 touches 方法处理

#### 底层实现主要涉及两个方法：func hitTest(_ point: CGPoint, with event: UIEvent?) -> UIView? 和 func point(inside point: CGPoint, with event: UIEvent?) -> Bool
#### hitTest 方法会根据视图层级结构往上调用 pointInside 方法，确定能否接收事件。如果 pointInside 返回 true ，则继续调用子视图层级结构，直到在最远的视图找到点击的 point 。如果一个视图没有找到该 point ，则不会继续它往上的视图层级结构。

#### 事件响应，大致是从子控件传递到父控件：
#### 如果当前 view 是控制器的 view ，那么控制器就是上一个响应者，事件就传递给控制器；如果当前 view 不是控制器的 view ，那么父视图就是当前 view 的上一个响应者，事件就传递给它的父视图
#### 在视图层次结构的最顶级视图，如果也不能处理收到的事件或消息，则其将事件或消息传递给 window 对象进行处理
- 如果 window 对象也不处理，则其将事件或消息传递给 UIApplication 对象
- 如果 UIApplication 也不能处理该事件或消息，则将其丢弃

#### 在上述过程中，如果某个控件实现了 touchesXxx 方法，则这个事件将由该控件接管，如果调用 super 的 touchesXxx ，就会将事件顺着响应者链继续往上传递，接着会调用上一个响应者的 touchesXxx 方法。
#### 一般我们会选择使用 block 或者 delegate 或者 notification center 去做一些消息事件的传递，而现在我们也可以利用响应者链的关系来进行消息事件的传递。
