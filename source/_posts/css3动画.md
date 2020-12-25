---
title: css3动画
date: 2020-12-25 16:24:14 
tags: css3
categories: Web
comment: true
---

# 3个重要属性

## transform 变形

> transform（变形）允许你旋转，缩放，倾斜或平移给定元素
>
拥有 `rotate` 旋转 `skew` 扭曲 `scale` 缩放 `translate` 移动 `matrix` 矩阵变形五大特效

```css
div {
    border: solid red;
    transform: translate(30px, 20px) rotate(20deg);
    width: 140px;
    height: 60px;
}
```

- 设置变换中心

```css
div {
    transform-origin: center bottom;
}
```

可选值有`left top center  right  bottom`

还可以直接写位置`50px 50px`

### scale 缩放

```css
 /*
  scale: 缩放
  scale(水平放缩比例，垂直放缩比例)；
  大于1：放大
  小于1：缩小
  如果只写一个值等比例缩放
 */
div {
    transform: scale(1);
    transform: scale(0.5);
    transform: scale(1, 1.5);
}
```

### translate 平移

```css
div {
    /* translate:(水平位移，垂直位移)；
       正值：向右向下
       负值：向左向上
       如果只写一个值 水平移动
       百分比 ：相对于自身移动
    */

    /* Single values */
    transform: translate(100px);
    transform: translate(50%);

    /* Two values */
    transform: translate(100px 200px);
    transform: translate(50% 105px);
}
```

### skew 倾斜

> 这个CSS属性定义了在2D平面上一个对象的歪斜变换
>

```css
div {
    /* skew(ax, ay) */
    /* ax 用于沿横坐标扭曲元素的角度 */
    transform: skew(10deg);
    /* ay 用于沿纵坐标扭曲元素的角度。如果未定义，则其默认值为0，导致纯水平倾斜 */
    transform: skew(10deg, 20deg);
}
```

### rotate X，Y,Z 旋转

> 图不就不贴了 随便在一个网址上F12改元素的样式就可以看出来效果
>

```css
div {
    /*   
    rotate
    直接旋转
     
    rotateX
    上下旋转
    
    rotateY
    左右旋转
    
    rotateZ
    旋转的
    */
}
```

rotateZ和rotate看起来效果一样

### translate X,Y,Z 平移

```css
body {
    /* 透视*/
    perspective: 600px;
}

.box:hover {
    /* translateZ必须配合透视来使用*/
    transform: translateZ(400px);
} 
```

translate和translateX看起来效果一样

## transition 过渡

> 拥有修改执行变换的属性，时长，速率和延迟时间的能力

!['''](http://qiniu.iplus-studio.top/Snipaste_2020-12-25_17-20-10.png)

```css
div {
    /* Apply to 1 property */
    /* property name | duration */
    transition: margin-right 4s;

    /* property name | duration | delay */
    transition: margin-right 4s 1s;

    /* property name | duration | timing function */
    transition: margin-right 4s ease-in-out;

    /* property name | duration | timing function | delay */
    transition: margin-right 4s ease-in-out 1s;

    /* Apply to 2 properties */
    transition: margin-right 4s, color 1s;

    /* Apply to all changed properties */
    transition: all 0.5s ease-out;

    /* Global values */
    transition: inherit;
    transition: initial;
    transition: unset;
}

```

- 指定transition效果的转速曲线

指定为一个或多个 CSS 属性的过渡效果，多个属性之间用逗号进行分隔。

`
transition-timing-function: linear | ease | ease-in | ease-out | ease-in-out | cubic-bezier(n, n, n, n);
`

值     | 描述
----   | ----
linear | 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。
ease   | 规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)）。
ease-in   | 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。
ease-out   | 规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。
ease-in-out   | 规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。
cubic-bezier(n,n,n,n)   | 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。

- 指定动画延迟时间

```css
div {
    transition-delay: 0.8s;
}
```

- 指定CSS属性的name，transition效果

```css
div {
    transition-property: width;
}
```

- transition效果需要指定多少秒或毫秒才能完成

```css
div {
    transition-duration: 1s;
}
```

## animation 重难点

> 若将Transform解释为动作，Transition解释为过渡，那么Animation则是连续的几个动作，即动画。Animation可以我们设定keyframes的值，让元素在一段时间内完成多个动作。

- 语法

`animation` :   name   duration timing-function delay iteration-count direction   fill-mode        play-state
`animation` : 动画名称  持续时间        运动曲线    延迟执行    执行次数      方向    动画结束后元素应该在的位置    动画状态    


```css
div {
    /* animation属性详解*/
    /* 动画名称*/
    animation-name: move;

    /* 一次动画持续时间  前两属性是必须，且顺序固定*/
    animation-duration: 3s;

    /* 动画执行次数  无数次：infinite*/
    animation-iteration-count: 1;

    /* 动画的方向： normal 正常 ， alternate： 反向*/
    animation-direction: alternate;

    /* 动画延迟执行*/
    animation-delay: 1s;

    /* 设置动画结束盒子盒子的状态
           forwards：保持动画结束后的状态  backwards：保持动画开始前的状态*/
    animation-fill-mode: forwards;

    /* 运动曲线  linear   ease-in-out  steps()*/
    /*animation-timing-function:ease-in;*/

    animation-timing-function: steps(8);

    /* 控制动画状态 paused 暂停  running　播放*/
    animation-play-state: paused;
}
```

值 | 描述
---|---
animation-name | 指定 @keyframes 动画的名称。
animation-duration | 指定动画完成一个周期所需要时间，单位秒（s）或毫秒（ms），默认是 0。
animation-timing-function | 指定动画计时函数，即动画的速度曲线，默认是 "ease"。
animation-delay | 指定动画延迟时间，即动画何时开始，默认是 0。
animation-iteration-count | 指定动画播放的次数，默认是 1。
animation-direction | 指定动画播放的方向。默认是 normal。
animation-fill-mode | 指定动画填充模式。默认是 none。
animation-play-state | 指定动画播放状态，正在运行或暂停。默认是 running

关于几个值，除了名字，动画时间，延时有严格顺序要求其它随意

一个例子：

```css
.box {
    width: 200px;
    height: 200px;
    margin: 100px;
    background-color: green;
    /* 调用*/
    /* infinite:无限次*/
    /* animation: 动画名称 持续时间  执行次数  是否反向  运动曲线 延迟执行*/
    /*animation: move 1s  alternate linear 3 ;*/
    animation: gun 4s;
}

/* 定义动画*/
@keyframes move {
    from {
        transform: translateX(0px) rotate(0deg);
    }
    to {
        transform: translateX(500px) rotate(555deg);
    }
}

/* 定义多组动画*/
@keyframes gun {
    0% {
        transform: translateX(0px) translateY(0px);
        background-color: green;
        border-radius: 0;
    }

    25% {
        transform: translateX(500px) translateY(0px);
    }

    50% {
        transform: translateX(500px) translateY(300px);
        border-radius: 50%;
    }


    75% {
        transform: translateX(0px) translateY(300px);
    }

    100% {
        transform: translateX(0px) translateY(0px);
        background-color: red;
        border-radius: 0;
    }
}
```

> animation-timing-function

常见值有：`linear、ease、ease-in、ease-out、ease-in-out`。

这些值其实都是 `cubic-bezier(n,n,n,n)` 的特例。它们被称为贝塞尔曲线。

一个在线调试贝塞尔曲线的网站：[cubic-bezier.com](cubic-bezier.com)。

> animation-fill-mode (重点)

除了默认值 `none` 外，还有另外 3 个值：

- `forwards`，表示，动画完成后，元素状态保持为最后一帧的状态。
- `backwards`，表示，有动画延迟时，动画开始前，元素状态保持为第一帧的状态。 
- `both`，表示上述二者效果都有。

这个贴个图理解一下

举个例子，div 从 100px 处移动到 200px 处的关键帧定义为：
```css
@keyframes move{
  0%{
    transform: translate(100px,0);
  }
  100%{
    transform: translate(200px,0);
  }
}
```

设置填充模式为 `forwards` 时，动画最后停留在 200px 处：

!['''](https://user-gold-cdn.xitu.io/2019/5/16/16ac0310ff7ba99a?imageslim)

设置动画延迟 1s 后执行，且填充模式为 `backwards` 时，可以看到动画在开始前是处于 100px 处，动画结束后回到 0px 处：

!['''](https://user-gold-cdn.xitu.io/2019/5/16/16ac035a53f40b08?imageslim)

最后设置填充模式为 `both` 的情形：

!['''](https://user-gold-cdn.xitu.io/2019/5/16/16ac0380ae444be8?imageslim)

动画结束后，保持动画最后一帧的状态，这个太有用了.

一个例子

```css
div{
    height: 10px;
    border: 1px solid;
    background: linear-gradient(#0ff,#0ff);
    background-repeat: no-repeat;
    background-size: 0;
    animation: move 2s linear forwards;
}

@keyframes move{
    100%{
        background-size: 100%;
    }
}
```

!['''](https://user-gold-cdn.xitu.io/2019/5/16/16ac0435e14da43a?imageslim)

> animation-delay
>
不为大家注意的是，延迟可以为负数。负延迟表示动画仿佛开始前就已经运行过了那么长时间。

拿上述进度条为例子，原动画用了 2s 是从 0% 加载到 100% 的。如果设置延迟为 -1s。这动画会从 50% 加载到 100%。仿佛已经运行了 1s 一样：

!['''](https://user-gold-cdn.xitu.io/2019/5/16/16ac04adc7f87ab6?imageslim)

> animation-play-state
> 
CSS 动画是可以暂停的。属性 animation-play-state 表示动画播放状态，默认值 `running` 表示播放， `paused` 表示暂停

!['''](https://user-gold-cdn.xitu.io/2019/5/16/16ac050679097040?imageslim)

> animation-iteration-count
> 
表示动画播放次数。它很好懂，只有一点要注意，无限播放时使用 infinite

> animation-direction
> 
它的意思说指定动画按照指定顺序来播放 @keyframes 定义的关键帧

- `normal` 默认值。
- `reverse` 表示动画反向播放。
- `alternate` 表示正向和反向交叉进行。
- `alternate-reverse` 表示反向和正向交叉进行。

 这个自己动手在浏览器改下样式更容易理解

最后附带一个代码 [css3动画](https://gitee.com/gdoudeng/css3-animation.git)

还有几个参考动画[15个CodePen上启发灵感的CSS动画案例](https://webdesign.tutsplus.com/zh-hans/articles/15-inspiring-examples-of-css-animation-on-codepen--cms-23937)

一个动画库[AnimateCss](https://animate.style/)

一个动画可视化编辑工具网址[AnimationKit](https://angrytools.com/css/animation/)
