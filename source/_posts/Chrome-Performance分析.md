---
title: Chrome Performance分析 
excerpt: 使用DevTools中的Performance来进行性能分析 
date: 2020-12-26 16:26:11 
tags: 性能优化
---

# 介绍

使用DevTools中的Performance来进行性能分析

# Performance

首先在新的无痕窗口打开网页（这样是为了避免你装的插件导致的影响），打开Chrome DevTools 切换到 Performance 下可以看到以下画面

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_16-33-33.png)

1. 左上角有个录制按钮，我们可以通过点击它来开始页面分析
2. 点击最右边的设置的小齿轮图标,如果是移动端项目,打开 CPU 节流开关,根据电脑性能选择相应的(用于模拟手机的性能)
3. 打开截图 Screenshots 记录过程中每一帧的截图

# 记录性能

1. 点击Record。DevTools会在页面运行时抓取性能相关的各种参数

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_16-41-00.png)

2. 点击Stop。DevTools停止记录，处理数据，展示结果。

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_16-43-01.png)

3. 展现数据

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_16-44-28.png)

# 分析（重点）

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_16-51-24.png)

## 桢率

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_16-55-27.png)

注意看FPS表，如果你看到了一条红色的条条在FPS上方，这证明你的帧速率掉得太低了，已经影响到了用户体验了。总的来说，绿色的条条越高，你的帧速率就越高。

## CPU

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_16-57-20.png)

在CPU图表中的各种颜色与Summary面板里的颜色是相互对应的，Summary面板就在Performance面板的下方。CPU图表中的各种颜色代表着在这个时间段内，CPU在各种处理上所花费的时间。如果你看到了某个处理占用了大量的时间，那么这可能就是一个可以找到性能瓶颈的线索。

## 截图

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_17-00-18.png)

在FPS，CPU，NET任意表上移动鼠标，你将看到那个事件点的页面快照，这叫scrubbing，可以用于手动计算动画的进展.

## Frames

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_17-03-15.png)

鼠标移上去可以读取到当时的帧率。

# 定位瓶颈

1. 先看summary页。在没选中任何事件的时候，该页展示的是活动期间的饼图。当前页面耗时最多是Scripting。Performance是一门减少工作的艺术，所以你的目标就是减少Scripting工作。

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_17-08-41.png)

2. 展开Main图表，Devtools展示了主线程运行状况。X轴代表着时间。每个长条代表着一个event。长条越长就代表这个event花费的时间越长。Y轴代表了调用栈（call
   stack）。在栈里，上面的event调用了下面的event。

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_17-12-28.png)

- 在性能报告中，有很多的数据。可以通过双击，拖动等等动作来放大缩小报告范围，从各种时间段来观察分析报告。

- 在事件长条的右上角出，如果出现了红色小三角，说明这个事件是存在问题的，需要特别注意。

3. 双击这个带有红色小三角的的事件。在Summary面板会看到详细信息。注意reveal这个链接，双击它会让高亮触发这个事件的event。如果点击了throttle.js?597f:83 这个链接，就会跳转到对应的代码处。

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_17-16-05.png)

4. 在wrapper事件下，你会看到一串紫色的事件。如果他们都很宽，这表明好像每一个都应该有个红色三角形。点击随便一个紫色的事件。你将在Summary页下看到关于他的一系列信息。

5. 在Summary Tab下，点击下的throttle.js?597f:83。你会看到是哪句代码导致警告。

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_17-17-47.png)

好，哪行代码都看到了，你可以去优化了。

# 一些事件

## Loading事件

事件 | 描述
---- | ----
Parse HTML | 浏览器执行HTML解析
Finish Loading | 网络请求完毕事件
Receive Data | 请求的响应数据到达事件，如果响应数据很大（拆包），可能会多次触发该事件
Receive Response | 响应头报文到达时触发
Send Request | 发送网络请求时触发

## Scripting事件

事件 | 描述
---- | ----
Animation Frame Fired | 一个定义好的动画帧发生并开始回调处理时触发
Cancel Animation Frame | 取消一个动画帧时触发
GC Event | 垃圾回收时触发
DOMContentLoaded     | 当页面中的DOM内容加载并解析完毕时触发
Evaluate Script | A script was evaluated.
Event | js事件
Function Call | 只有当浏览器进入到js引擎中时触发
Install Timer | 创建计时器（调用setTimeout()和setInterval()）时触发
Request Animation Frame | A requestAnimationFrame() call scheduled a new frame
Remove Timer | 当清除一个计时器时触发
Time | 调用console.time()触发
Time End | 调用console.timeEnd()触发
Timer Fired | 定时器激活回调后触发
XHR Ready State Change | 当一个异步请求为就绪状态后触发
XHR Load | 当一个异步请求完成加载后触发

## Rendering事件

事件 | 描述
---- | ----
Invalidate layout     | 当DOM更改导致页面布局失效时触发
Layout | 页面布局计算执行时触发
Recalculate style     | Chrome重新计算元素样式时触发
DOMContentLoaded     | 当页面中的DOM内容加载并解析完毕时触发
Scroll | 内嵌的视窗滚动时触发

## Painting事件

事件 | 描述
---- | ----
Composite Layers     | Chrome的渲染引擎完成图片层合并时触发
Image Decode | 一个图片资源完成解码后触发
Image Resize         | 一个图片被修改尺寸后触发
Paint         | 合并后的层被绘制到对应显示区域后触发

# 跟Summary Tab 同级的几个tab

## Bottom-Up Tab

![](http://qiniu.iplus-studio.top/16522d5a72ccd1e8.jpg)

在 Timeline 中选取一段时间,然后点击 Bottom-Up得到上图,图片中展示浏览器执行的各个操作说占用的时间

## Call-tree Tab

![](http://qiniu.iplus-studio.top/16522d60f31f4962.jpg)

同理点击Call Tree 得到上图: 表示浏览器的基本操作(事件执行,绘制...)所占用的时间

## Event log Tab

![](http://qiniu.iplus-studio.top/16522d5b3b6a26fb.jpg)

同理点击 Event Log得到上图: 可以按照选中时间内事件发生的顺序来查看事件执行所占用的时间.

# 开启FPS meter

还有一个利器，就是FPS meter了，它提供了一种实时的FPS评估。

1. Command+Shift+p（Mac）， Control+Shift+p（windows） 开启命令菜单
2. 输入Rendering，选择Show Rendering
3. 在Rendering页，开启FPS Meter。一个新的覆盖窗口将会在你视口的右上角出现。

![](http://qiniu.iplus-studio.top/Snipaste_2020-12-26_17-06-48.png)
