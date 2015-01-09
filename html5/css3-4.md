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

	在下面这个示例中,使用:has和:not结合在一起选择不包含段落<p>的<li>元素：

		li:not(:has(p)) { padding-bottom: 1em; }
4. d






