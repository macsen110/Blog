#css3,css4的一些个使用情况

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

##CSS混合模式

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

##calc()函数

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

##CSS变量
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

##CSS EXCLUSIONS

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

##CSS SHAPES

除了Exclusions规范可以让文本围绕一个矩形四周之外，CSS的Shapes给我们带来了更强大的文本排列功能，他不但可以围绕矩形，还可以根据非矩形排列文本，比如文本围绕一曲线排列。

CSS的Shapes规范定义了一个新属性shape-outside。这个属性可以运用在一个浮动元素上。在下面的示例中，给浮动图片应用shape-outside，让文本围绕一个圆排列。

	.shape {
	  width: 300px;
	  float: left;
	  shape-outside: circle(50%);
	}

CSS Shapes允许我们的文本按一个球形状排列。

Shapes规范标准一得到了Chrome和Safari浏览器的支持，这也意味着在iOS的设备中可以这样使用。Shapes规范二中允许形状文本内部元素使用shape-inside属性。

##CSS网格布局

由于这个浏览器的兼容性问题不在此多做探讨









