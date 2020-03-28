---
uuid: 5af8db2d-f6c8-ae8c-c397-5d833fd3ec13
title: 'css3动画'
date: 2020-03-21 19:09:05
tags: css效果
category: 学习整理的前端
---
tarnsfrom可以用来设置元素的形状改变、主要有几种：

### transfrom设置元素的形状改变
tarnsfrom可以用来设置元素的形状改变、主要有几种：
rotata（旋转（单位deg））、
scale（缩放）、
skew（扭曲）、
teanslate（移动）、
marix（矩形变阵）

使用方法
.类名称{
transfrom：rotate（60deg） scale（.2） skew（） translate（X，Y） matrix（）；
}

### transfrom-origin基点设置

所有的变形都是基于基点，基点默认为元素的中心点。用法：transform-origin: (x, y)，其中 x 和 y 的值可以是百分比、rem 或者是 px 等等，也可以用表示位置的单词来表示例如：x 可以用left、center、right；y 可以用top、center、bottom。

使用方法
.transform-class {
    transform-origin: (left, bottom);
}

### rotate 旋转
用法：rotate(<angle>)；表示通过指定的角度对元素进行旋转变形，如果是正数则顺时针旋转，如果是负数则逆时针旋转，例如：

.transform-rotate {
    transform: rotate(30deg);
}

### scale 缩放
它有三种用法：scale(<number>[, <number>])、scaleX(<number>)和scaleY(<number>)；分别代表水平和垂直方向同时缩放、水平方向的缩放以及垂直方向的缩放，入参代表水平或者垂直方向的缩放比例。缩放比例如果大于1则放大，反之则缩小，如果等于1代表原始大小。

.transform-scale {
    transform: scale(2,1.5);
}

.transform-scaleX {
    transform: scaleX(2);
}

.transform-scaleY {
    transform: scaleY(1.5);
}

### translate 移动
移动也分三种情况：translate(<translation-value>[, <translation-value>])、translateX(<translation-value>)和translateY(<translation-value>)；分别代表水平和垂直的移动、水平方向的移动以及垂直方向同时移动，移动单位是 CSS 中的长度单位：px、rem等;

.transform-translate {
    transform: translate(400px, 20px);
}

.transform-translateX {
    transform: translateX(300px);
}

.transform-translateY {
    transform: translateY(20px);
}
### skew 扭曲
扭曲同样也有三种情况，skew(<angle>[, <angle>])、skewX(<angle>)和skewY(<angle>)；同样也是水平和垂直方向同时扭曲、水平方向的扭曲以及垂直方向的扭曲，单位为角度。

.transform-skew {
    transform: skew(30deg, 10deg);
}

.transform-skewX {
    transform: skewX(30deg);
}

.transform-skewY {
    transform: skewY(10deg);
}

### transition一种状态变平滑过渡到另外一种状态
transition是用来设置样式的属性值是如何从从一种状态变平滑过渡到另外一种状态，它有四个属性：

transition-property（变换的属性，即那种形式的变换：大小、位置、扭曲等）；
transition-duration（变换延续的时间）；
transition-timing-function（变换的速率）
transition-delay（变换的延时）

.transition-class {
    transition ： [<'transition-property'> || <'transition-duration'> || <'transition-timing-function'> || <'transition-delay'> [, [<'transition-property'> || <'transition-duration'> || <'transition-timing-function'> || <'transition-delay'>]]*;
}

##  transition-property平滑过渡的效果
它是用来设置哪些属性的改变会有这种平滑过渡的效果，主要有以下值：

none；
all；
元素属性名：
color；
length；
visibility；
…
.transition-property {
    transition-property ： none | all | [ <IDENT> ] [ ',' <IDENT> ]*;
}

### transition-duration转换过程的持续时间
它是用来设置转换过程的持续时间，单位是s或者ms，默认值为0；

.transition-duration {
    transition-duration ： <time> [, <time>]* ;
}

### transition-timing-function渡效果的速率
它是来设置过渡效果的速率，它有6种形式的速率：

ease：逐渐变慢（默认），等同于贝塞尔曲线(0.25, 0.1, 0.25, 1.0)；
linear：匀速，等同于贝塞尔曲线(0.0, 0.0, 1.0, 1.0)；
ease-in：加速，等同于贝塞尔曲线(0.42, 0, 1.0, 1.0)；
ease-out：减速，等同于贝塞尔曲线(0, 0, 0.58, 1.0)；
ease-in-out：先加速后减速，等同于贝塞尔曲线(0.42, 0, 0.58, 1.0)；
cubic-bezier：自定义贝塞尔曲线。
.transition-timing {
    transition-timing-function ： ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>) [, ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>)]*;
}

### 贝塞尔曲线
##  transition-delay渡动画开始执行的时间
它是来设置过渡动画开始执行的时间，单位是s或者ms，默认值为0；

.transition-delay {
    transition-delay ： <time> [, <time>]*;
}

##  transition它是transition-property、transition-duration、transition-timing-function、transition-delay的简写：

.transition {
    transition ：<property> <duration> <timing function> <delay>;
}

##  animationflash 中的逐帧动画
animation比较类似于 flash 中的逐帧动画，逐帧动画就像电影的播放一样，表现非常细腻并且有非常大的灵活性。然而transition只是指定了开始和结束态，整个动画的过程也是由特定的函数控制。学习过 flash 的同学知道，这种逐帧动画是由关键帧组成，很多个关键帧连续的播放就组成了动画，在 CSS3 中是由属性keyframes来完成逐帧动画的。

##  @keyframes
@keyframes animationName {
    from {
        properties: value;
    }
    percentage {
        properties: value;
    }
    to {
        properties: value;
    }
}
//or
@keyframes animationName {
    0% {
        properties: value;
    }
    percentage {
        properties: value;
    }
    100% {
        properties: value;
    }
}

percentage：为百分比值，可以添加多个百分比值；
properties：样式属性名称，例如：color、left、width等等。

##  animation设置动画的名称
它是用来设置动画的名称，可以同时赋值多个动画名称用空格隔开：

none：不改变默认行为。    
forwards ：当动画完成后，保持最后一个属性值（在最后一个关键帧中定义）。    
backwards：在 animation-delay 所指定的一段时间内，在动画显示之前，应用开始属性值（在第一个关键帧中定义）。    
both：向前和向后填充模式都被应用。  

.animation {
    animation:name| none | IDENT[,none | IDENT]*;
    例子:animation: name .5s forwards;
}

##  animation-duration设置动画的持续时间
它是用来设置动画的持续时间，单位为s，默认值为0：

.animation {
    animation-duration: <time>[,<time>]*;
}

##  animation-timing-function和transition-timing-function类似
和transition-timing-function类似：

.animation {
    animation-timing-function:ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>) [, ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>)]*;
}

##  animation-delay设置动画的开始时间
它是来设置动画的开始时间，单位是s或者ms，默认值为0：

.animation {
    animation-delay: <time>[,<time>]*;
}

##  animation-iteration-count动画循环的次数
它是来设置动画循环的次数，默认为1，infinite为无限次数的循环：

.animation {
    animation-iteration-count:infinite | <number> [, infinite | <number>]*;
}

##  animation-direction动画播放的方向
它是来设置动画播放的方向，默认值为normal表示向前播放，alternate代表动画播放在第偶数次向前播放，第奇数次向反方向播放：

.animation {
    animation-direction: normal | alternate [, normal | alternate]*;
}

##  animation-play-state控制动画的播放状态
它主要是来控制动画的播放状态：running代表播放，而paused代表停止播放，running为默认值：

.animation {
    animation-play-state:running | paused [, running | paused]*;
}

##  animation简写
它是animation-name、animation-duration、animation-timing-function、animation-delay、animation-iteration-count、animation-direction的简写：

.animation {
animation:[<animation-name> || <animation-duration> || <animation-timing-function> || <animation-delay> || <animation-iteration-count> || <animation-direction>] [, [<animation-name> || <animation-duration> || <animation-timing-function> || <animation-delay> || <animation-iteration-count> || <animation-direction>] ]*;
}
### 总结一下
>关于 CSS3 的动画的三个属性transform、transition、animation我们都介绍完了，让我们回顾一下。transform我们可以理解为元素的几何变形，它是有规律可寻的，这种变形并不会产生动画效果仅仅是原有形状的改变；transition和animation它们很像 flash 中的补间动画和逐帧动画；transition是从一个状态变化到另外一种状态，当变化有了平滑的效果后就产生了动画，它是一个公式化的变化，在比较规则的动画效果中我们可以使用，例如：旋转的风车、行驶的汽车、颜色的渐变等等；animation的动画效果更加灵活，可以实现像影片一样的复杂无规则的动画