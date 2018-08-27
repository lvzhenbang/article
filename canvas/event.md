## canvas中的事件处理

在canvas的实践中，就像处理其它的dom对象一样，我们往往会用到鼠标或触摸事件，有时也会用到键盘或拖放事件来操作canvas。下面针对这些事件应用进行详解。

### 鼠标事件

mousedown,mouseup,mousemove,mouseout。

要实现canvas的鼠标事件，需要进行坐标转换，将相对于窗口的鼠标坐标，转换为相对于canvas的坐标。