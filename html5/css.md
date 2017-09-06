# css 基础通用部分 #
1.css 优先级
多重样式（Multiple Styles）：如果外部样式、内部样式和内联样式同时应用于同一个元素，就是使多重样式的情况。
一般情况下，优先级如下：
（外部样式）External style sheet <（内部样式）Internal style sheet <（内联样式）Inline style
有个例外的情况，就是如果外部样式放在内部样式的后面，则外部样式将覆盖内部样式。

2.选择器的权值
解释：
1.  内联样式表的权值最高 1000；
2.  ID 选择器的权值为 100
3.  Class 类选择器的权值为 10
4.  HTML 标签选择器的权值为 1

CSS 优先级法则：

A  选择器都有一个权值，权值越大越优先；

B  当权值相等时，后出现的样式表设置要优于先出现的样式表设置；

C  创作者的规则高于浏览者：即网页编写者设置的CSS 样式的优先权高于浏览器所设置的样式；

D  继承的CSS 样式不如后来指定的CSS 样式；

E  在同一组属性设置中标有“!important”规则的优先级最大

	所以本质上是权值的比较, 
	内部样式(<style>标签) 和外部样式本质上都是相同的权值,
	<span class="class-a class-b"></span>class-a, class-b的优先执行是两者在css中出现的先后,后出现的覆盖前出现的


# css 技巧实用篇 #

## css3,css4的一些个使用情况

1. select元素去掉默认的三角ff下面暂不支持,使用父元素的overflow属性隐藏子元素多余三角.

	>-webkit-appearance: none;appearance: none;
2. 否定伪类:not

	否定伪类选择器其实在CSS3选择器中就出现了，只不过CSS4选择器对否定伪类选择器升级了。在CSS3中，你可以通过否定伪类选择器不去选中你不需要用到的CSS样式的元素。比如说，你想除了.intro的段落之外文本都不加粗，你就可以这样使用伪类选择器。

		p:not(.intro) { font-weight: normal; }

	在CSS4选择器中，可以传入一个逗号(,)分隔的列表选择器：

		p:not(.intro, blockquote) { font-weight: normal; }

3. 关系伪类:has

	这个选择器通过一个参数从一个列表选择器中匹配到相对应的元素。有一个最易说明的示例，在这个例子中任何一个包含<img>的<a>链接都会加上一个黑色的边框：

		a:has( > img ) { border: 1px solid #000; }

	在下面这个示例中,使用:has和:not结合在一起选择不包含段落p的li元素：

		li:not(:has(p)) { padding-bottom: 1em; }
4. 匹配任何伪类:matches

	这个伪类意味着，可以将规则应用到一个组合选择器中，如：

		p:matches(.alert, .error, .warn) { color: red; }

	带有只要元素带<p>带有.alert、.error和.warn中任何一个类名，文本颜色都将会是red。

	你可以在[css4-selectors.com]('css4-selectors.com')网站上测试你的浏览器是否支持这些CSS4选择器。这也是一个资源网站，你可以在这个站上找到将要添加的选择器。

## CSS混合模式

如果你熟悉在Photoshop中使用图层混合模式，那么你可能会对CSS的混合模式和规范感兴趣。该规范可以将混合模式的背景适用于任何HTML元素。

在下面的CSS中我们一个运用了背景图像的盒子。通过添加一个背景颜色background-color，然后设置background-blend-mode的值为hue和multiply，可以看到神奇的效果。

	.box {
	  background-image: url(balloons.jpg);
	}
	.box2 {
	  background-color: red;
	  background-blend-mode: hue;
	}
	.box3 {
	  background-color: blue;
	  background-blend-mode: multiply;
	}

## calc()函数

calc()函数是CSS3中值和单位模块中的一部分。可以使用其做一些数学相关的函数计算。

使用calc()可以很容易将背景图定位在一个元素的右下角。我们来看一个示例，将一个元素定位在左上角30px的位置很容易，你会使用：

	.box {
	  background-image: url(check.png);
	  background-position: 30px 30px;
	}

然而，当你不知道容器具体有多大的情况之下，要让背景图定位在右下角30px位置处，你就很难做到。那么calc()函数可以从100%中减去30px的宽度或高度来实现：

	.box {
	  background-image: url(check.png);
	  background-position: calc(100% - 30px) calc(100% - 30px);
	}

calc()在现代浏览器中得到很好的支持，Can I Use的报告，在IE9浏览器中background-positon使用calc()来计算，会直接导致浏览器崩溃。

## CSS变量
在Sass这样的CSS预处理器语言中，有一个强大的特性就是可以定义变量。这是一个非常简单而又能帮我们节约很多时间。可以在设计中给颜色、字体等定义亦是。如果我们要调整字体或颜色，只需要在一个地方修改这些值就行。

CSS的变量模块中对CSS的变量进行了描述，将会把这个功能引入到CSS中。

	:root {
	  --color-main: #333333;
	  --color-alert: #ffecef;
	}
	.error {
	  color: var(--color-alert);
	}

遗憾的是到目前为止，只有Firefox浏览器支持CSS变量。

## CSS EXCLUSIONS

大家都熟悉CSS的浮动。举个例子，有张图像浮动了，图像周边的文本流会围绕图像，但这是相当有限的。尽管可以将图像左浮动，文本流也停留在浮动图像的右侧和底部，却没有办法能让图像四周都有文本围绕。

Exclusions可以让文本四周围绕一个对象。它本身并不是一种新的定位方法，所以可以和其他方法结合使用。在下面的示例中，在绝对定位的元素中使用了一个wrap-flow属性，并且取值为both。那么文本就围绕在这个绝对定位元素的四周。

	.main {
	  position:relative;
	}
	.exclusion {
	  position: absolute;
	    top: 14em;
	    left: 14em;
	    width: 320px;
	    wrap-flow: both;
	}

到目前为止也仅在IE10+上支持wrap-flow:both，而且需要在其前面添加前缀-ms。注意,EXCLUSIONS的效果看上去有点类似于CSS Shapes的规范。网上也有一些信息将他们做为对比性的介绍。

## CSS SHAPES

除了Exclusions规范可以让文本围绕一个矩形四周之外，CSS的Shapes给我们带来了更强大的文本排列功能，他不但可以围绕矩形，还可以根据非矩形排列文本，比如文本围绕一曲线排列。

CSS的Shapes规范定义了一个新属性shape-outside。这个属性可以运用在一个浮动元素上。在下面的示例中，给浮动图片应用shape-outside，让文本围绕一个圆排列。

	.shape {
	  width: 300px;
	  float: left;
	  shape-outside: circle(50%);
	}

CSS Shapes允许我们的文本按一个球形状排列。

Shapes规范标准一得到了Chrome和Safari浏览器的支持，这也意味着在iOS的设备中可以这样使用。Shapes规范二中允许形状文本内部元素使用shape-inside属性。

## CSS网格布局

由于这个浏览器的兼容性问题不在此多做探讨

# More

1. -webkit-backface-visibility:hidden  指定元素背面是否可见,可用于旋转的属性
2.  p~ul    选择前面有 `<p>` 元素的每个 `<ul>` 元素。(同一层级,中间可以有其他元素间隔)
3.  div+p    选择紧接在 `<div>` 元素之后的所有 `<p>` 元素。(同一层级,中间不能有其他元素的间隔)
4. box或者flex 布局中,子元素的order顺序很重要,相同order的话会按照模板中的html顺序来渲染( box-ordinal-group或者order)
5. 父元素应用column-count3份， 指其块状元素平均分割为三分,其子元素的宽度要设置成100%，block, column-grap不支持百分比,
column-rule 属性设置列之间的宽度、样式和颜色规则,-webkit-column-span:设置元素横跨所有的列,column-width代表每列的宽度，
如有指定就不用
6. translateZ()函数仅让元素在Z轴进行位移，当其值越大时，元素离观看者越近，视觉上元素放大，反之元素缩小。translateZ()函数在实际使用中等同于translate3d(0,0,tz)。
7. 当元素不存在width属性或者（width：auto）的时候，负margin会增加元素的宽度，
8. 负margin会改变浮动元素的显示位置，即使元素写在DOM的后面，也能让它显示在最前面
9. margin为负值的常见布局应用,左右固定，中间自适应（双飞翼）
10. @import影响css文件的加载速度
11. 避免使用复杂的选择器，层级越少越好
12. 结构和样式分离,容器和内容分离
13. 堆叠上下文

堆叠与浮动

对于浮动的块元素来说，堆叠顺序变得有些不同。浮动块元素被放置于非定位块元素与定位块元素之间：

- 根元素的背景与边框

- 位于普通流中的后代块元素按照它们在 HTML 中出现的顺序堆叠

- 浮动块元素

- 常规流中的后代行内元素

- 后代中的定位元素按照它们在 HTML 中出现的顺序堆叠

以下情况产生一个新的层叠上下文:

- 当一个元素是文档的根元素时，在完整的html文档里，跟元素是html标签；

- 当元素拥有一个值为非static的position且拥有一个值为非auto的z-index样式时；

- 当元素拥有一个值小于1的opacity样式时；

- 当元素拥有一个值不为none且有效的transform样式时

元素的层叠关系不仅与它在堆叠上下文中所属的层叠级别有关，还与它所在的堆叠上下文的顺序有关

CSS3时代，flex子项也支持z-index，使得我们面对的情况比以前要负复杂。然而，好的是，规则都是一样的，对于z-index的使用和表现也是如此






css动画参数翻译

transition

transition 里面时间参数,第一个为
transition-duration,

第二个为transition-delay

order不能变

delay的真正意义在于，它指定了动画发生的顺序，使得多个不同的transition可以连在一起，形成复杂效果。

transition: <property> <duration> <timing-function> <delay>;

（1）linear：匀速

（2）ease-in：加速

（3）ease-out：减速

（4）cubic-bezier函数：自定义速度模式(贝塞尔曲线)

animation

- animation-name: as specified
- animation-duration: (custom time)
- animation-timing-function: (ease, linear,step-start, steps,cubic-bezier)

- animation-delay: (custom time)
- animation-direction: as specified
- animation-iteration-count: (custom number,infinite)
- animation-fill-mode: as specified
- animation-play-state: as specified

Formal syntax: `<single-animation-name> || <time> || <timing-function> || <time> || <single-animation-iteration-count> || <single-animation-direction> || <single-animation-fill-mode> || <single-animation-play-state>`

animation-direction:

normal
>The animation should play forward each cycle. In other words, each time the animation cycles, the animation will reset to the beginning state and start over again. This is the default animation direction setting.

alternate
>The animation should reverse direction each cycle. When playing in reverse, the animation steps are performed backward. In addition, timing functions are also reversed; for example, an ease-in animation is replaced with an ease-out animation when played in reverse. The count to determine if it is an even or an odd iteration starts at one.(0-100)

reverse
> The animation plays backward each cycle. Each time the animation cycles, the animation resets to the end state and start over again.(100-0)
alternate-reverse
> The animation plays backward on the first play-through, then forward on the next, then continues to alternate. The count to determinate if it is an even or an odd iteration starts at one.

animation-fill-mode:
> The animation-fill-mode CSS property specifies how a CSS animation should apply styles to its target before and after it is executing.
none
The animation will not apply any styles to the target before or after it is executing.

forwards
> The target will retain the computed values set by the last keyframe encountered during execution. The last keyframe encountered depends on the value of animation-direction and animation-iteration-count:

backwards
>The animation will apply the values defined in the first relevant keyframe as soon as it is applied to the target, and retain this during the animation-delay period. The first relevant keyframe depends of the value of animation-direction:
both
The animation will follow the rules for both forwards and backwards, thus extending the animation properties in both directions.

infinite关键字，可以让动画无限次播放。也可以指定动画具体播放的次数，比如3次。

steps:n  表明动画过渡需要n步

cubic-bezier

css 伪类和伪元素

伪类和伪元素的根本区别在于：它们是否创造了新的元素(抽象)。从我们模仿其意义的角度来看，如果需要添加新元素加以标识的，就是伪元素，反之，如果只需要在既有元素上添加类别的，就是伪类。

优先级区分:除了否定伪类的特殊规定外，分开各自作为真正的类和元素计算。

元素的BFC特性与自适应布局

块状元素的流体

块状元素随着margin, padding,

 border的出现，其可用宽度自动跟着减小，

形成了自适应效果。就像放在容器中的水流一样，内容区域会随着margin, padding,
border的出现自动填满剩余空间，这就是块状元素的流体特性。

display:box,display:flex两者都是表示弹性布局,display:box  2009提出的用法，flex是它的增强版

目前某些移动端的浏览器因为操作系统只支持display:-webkit-box;并且还不支持多行显示

低版本浏览器下必须指定flex item 的display:block
在flex box中-webkit-box版本下,给其子item添加{width: 1%},可解决容器溢出的情况

css3 中currentColor 类似一个变量值为当前字体颜色(可用在hover当前元素的状态切换等)

问题:
低版本下如何能强制不让其伸缩

小tip:CSS计数器+伪类实现数值动态计算与呈现

“使用CSS(Unicode字符)让inline水平元素换行”
grunt svg sprite

绝对定位元素 margin:0垂直居中

box-shadow
none | `[inset? && [ <offset-x> <offset-y> <blur-radius>? <spread-radius>? <color>? ] ]`

大部分的回流将导致页面的重新渲染。
伪元素 :before, :after 设置属性值的时候必不可少content属性

csss 可继承属性 ，不可继承属性

css 技巧:

选中连续的几个元素
```
ol li:nth-child(n+7):nth-child(-n+14) {
  background: lightpink;
}
/** Or Safari Way **/
ol li:nth-child(-n+14):nth-child(n+7) {
  background: lightpink;
}

outline-offset属性，在这个属性中，你可以设置默认线框的距离
input {
    outline-offset: 4px ;
}
```

css 中的数量查询

1. :only-child和:only-of-type都可以选择只有一个子元素。

2. not 配合查询,取反:not(only-child)

3. :nth-last-child(n)选择器，可以从后面开始遍历n个参数。也就是倒数第n个

4. 多层伪类选择器，在原选择器基础上添加:first-child来做数量的过滤:nth-last-child(6):first-child

5. :nth-last-child(6):first-child可以选择到第一个，而:nth-last-child(6):first-child ~ li可以选择到第2~6个li。如此一来，将这两个选择器组合在一起，就可以选择只有6个li的列表

6. 和:nth-last-child()有一个刚好相反的选择器:nth-child()，其也可以添加相应的参数，比如n或者n + [整数]。例如:nth-child(n + 6)会选择列表中第六个li后所有li(包括第6个)，nth-last-child(n + 6)。:nth-last-child(n + 6)将会从列表的中的倒数第六个li开始计算，直到没有匹配的li停止（都是递增）

7. 数小于或等于N
:nth-last-child(-n + 6)从每个列表中倒数第一个一直选择倒数第六个li。

8. 与nth-of-type 比较，重在理解type这个词,比较限制筛选的类型
比如p:nth-child(2) {}，p: nth-of-type(2) 前者只会对父标签的第二个子元素且子元素是p,
后者会对第二个p标签起作用,不管第二个p标签在何处

三.   background-size 可以设置多个背景图,值以‘,’区分
eg: background-size: 10px auto, 20px auto

四.当给元素设定transform时会产生层叠,而其相邻元素仍会按照之前正常文档流显示,不会受到变形元素的影响

五.max-width: less than or equal
min-width greater than or equal

