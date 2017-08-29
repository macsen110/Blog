# viewport 终端的一些个基本概念

1. 设备的pixels和CSS的pixels
CSS的pixels开始收缩，导致1单位的设备的pixels上重叠了多个CSS的pixels
同理，放大浏览器时，相反的事情发生了，CSS的pixels开始扩大，导致1单位的CSS的pixels上重叠了多个设备的pixels

2. 屏幕尺寸 Screen size screen.width/height
screen.width 和 screen.height。这两个属性包含了用户屏幕的完整宽度高度。这些尺寸使用设备的pixels来定义，他们的值不会因为缩放而改变：他们是显示器的特征，而不是浏览器。

3. 浏览器尺寸 Window size window.innerWidth/Height
窗口的内部宽度使用CSS的pixels.你需要知道多少你自己定义的元素能塞入浏览器窗口，而这些数量会随着用户放大浏览器而减少（如图1-6）。所以当用户放大显示时，你能获取的浏览器窗口可用空间会减少，window.innerWidth/Height就是缩小的比例。

4. 视窗 viewport
viewport的功能在于控制你网站的最高块状（block）容器：<html>元素。
document. documentElement. clientWidth/Height只会给出viewport的尺寸，而不管<html>元素尺寸如何改变。
- window.innerWidth/Height包含滚动条
- document. documentElement. clientWidth/Height不包含

5. 事件坐标 Event coordinates pageX/Y, clientX/Y, screenX/Y
 pageX/Y：从<html>原点到事件触发点的CSS的 pixels

- clientX/Y：从viewport原点（浏览器窗口）到事件触发点的CSS的 pixels
- screenX/Y：从用户显示器窗口原点到事件触发点的设备 的 pixels。

6. 物理像素(physical pixel)
物理像素又被称为设备像素，他是显示设备中一个最微小的物理部件。每个像素可以根据操作系统设置自己的颜色和亮度。正是这些设备像素的微小距离欺骗了我们肉眼看到的图像效果。

7. 设备独立像素(density-independent pixel)
设备独立像素也称为密度无关像素，可以认为是计算机坐标系统中的一个点，这个点代表一个可以由程序使用的虚拟像素(比如说CSS像素)，然后由相关系统转换为物理像素。

8. CSS像素
CSS像素是一个抽像的单位，主要使用在浏览器上，用来精确度量Web页面上的内容。一般情况之下，CSS像素称为与设备无关的像素(device-independent pixel)，简称DIPs。

9. 屏幕密度
屏幕密度是指一个设备表面上存在的像素数量，它通常以每英寸有多少像素来计算(PPI)。

10. 设备像素比(device pixel ratio)
设备像素比简称为dpr，其定义了物理像素和设备独立像素的对应关系。它的值可以按下面的公式计算得到：

设备像素比 ＝ 物理像素 / 设备独立像素