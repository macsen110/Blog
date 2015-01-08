Html 头部标签(参考baidu前端的文章)
=================
HTML head 头部分的标签、元素有很多，涉及到浏览器对网页的渲染，SEO 等等，而各个浏览器内核以及各个国内浏览器厂商都有些自己的标签元素,这就造成了很多差异性。移动互联网时代，head 头部结构，移动端的 meta 元素，显得更为重要。了解每个标签的意义，写出满足自己需求的 head 头标签，是本文的目的。本篇以一丝的文章为基础，进行扩展总结介绍常用的 head 中各个标签、元素的意义以及使用场景。

###DOCTYPE

DOCTYPE(Document Type)，该声明位于文档中最前面的位置，处于 html 标签之前，此标签告知浏览器文档使用哪种 HTML 或者 XHTML 规范。

DTD(Document Type Definition) 声明以 <!DOCTYPE> 开始，不区分大小写，前面没有任何内容，如果有其他内容(空格除外)会使浏览器在 IE 下开启怪异模式(quirks mode)渲染网页。公共 DTD，名称格式为注册//组织//类型 标签//语言,注册指组织是否由国际标准化组织(ISO)注册，+表示是，-表示不是。组织即组织名称，如：W3C。类型一般是 DTD。标签是指定公开文本描述，即对所引用的公开文本的唯一描述性名称，后面可附带版本号。最后语言是 DTD 语言的 ISO 639 语言标识符，如：EN 表示英文，ZH 表示中文。XHTML 1.0 可声明三种 DTD 类型。分别表示严格版本，过渡版本，以及基于框架的 HTML 文档。
* HTML 4.01 strict

  ><!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

* HTML 4.01 Transitional

  ><!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

* HTML 4.01 Frameset

  ><!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">

* 最新 HTML5 推出更加简洁的书写，它向前向后兼容，推荐使用。

  ><!doctype html>

在 HTML中 doctype 有两个主要目的。

1. 对文档进行有效性验证.
	它告诉用户代理和校验器这个文档是按照什么 DTD 写的。这个动作是被动的，每次页面加载时，浏览器并不会下载 DTD 并检查合法性，只有当手动校验页面时才启用。

2. 决定浏览器的呈现模式
	对于实际操作，通知浏览器读取文档时用哪种解析算法。如果没有写，则浏览器则根据自身的规则对代码进行解析，可能会严重影响 html 排版布局。浏览器有三种方式解析 HTML 文档。
	
    + 非怪异（标准）模式
    + 怪异模式
    + 部分怪异（近乎标准）模式 关于IE浏览器的文档模式，浏览器模式，严格模式，怪异模式，DOCTYPE 标签，可详细阅读模式？标准！的内容。

### charset
声明文档使用的字符编码


	<meta charset="utf-8">
html5之前会这样写

	<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
### lang属性
简体中文

	<html lang="zh-cmn-Hans">

繁体中文

	<html lang="zh-cmn-Hant">

### 优先使用 IE 最新版本和 Chrome

	<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />

###360 使用Google Chrome Frame
	<meta name="renderer" content="webkit">
360 浏览器就会在读取到这个标签后，立即切换对应的极速核。 另外为了保险起见再加入

	<meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1">
	
这样写可以达到的效果是如果安装了 Google Chrome Frame，则使用 GCF 来渲染页面，如果没有安装 GCF，则使用最高版本的 IE 内核进行渲染。

###百度禁止转码
	
通过百度手机打开网页时，百度可能会对你的网页进行转码，脱下你的衣服，往你的身上贴狗皮膏药的广告，为此可在 head 内添加

	<meta http-equiv="Cache-Control" content="no-siteapp" />
	
###SEO 优化部分
+ 页面标题

	<title>标签(head 头部必须)
	

		<title>your title</title>
+ 页面关键词 keywords
	

		<meta name="keywords" content="your keywords">
+ 页面描述内容 description


		<meta name="description" content="your description">
+ 定义网页作者 author

	
		<meta name="author" content="author,email address">

+ 定义网页搜索引擎索引方式，robotterms 是一组使用英文逗号「,」分割的值，通常有如下几种取值：none，noindex，nofollow，all，index和follow。

	
		<meta name="robots" content="index,follow">
	

###viewport

viewport 可以让布局在移动浏览器上显示的更好。 通常会写

	<meta name="viewport" content="width=device-width, initial-scale=1.0">

width=device-width 会导致 iPhone 5 添加到主屏后以 WebApp 全屏模式打开页面时出现黑边(http://bigc.at/ios-webapp-viewport-meta.orz)

content 参数：

1. width viewport 宽度(数值/device-width)
2. height viewport 高度(数值/device-height)
3. initial-scale 初始缩放比例
4. maximum-scale 最大缩放比例
5. minimum-scale 最小缩放比例
6. user-scalable 是否允许用户缩放(yes/no)
7. minimal-ui iOS 7.1 beta2中新增属性，可以在页面加载时最小化上下状态栏。这是一个布尔值，可以直接这样写：

	<meta name="viewport" content="width=device-width, initial-scale=1, minimal-ui">
	
而如果你的网站不是响应式的，请不要使用 initial-scale 或者禁用缩放。

	<meta name="viewport" content="width=device-width,user-scalable=yes">

相关链接：[非响应式设计的viewport](http://www.qianduan.net/non-responsive-design-viewport.html)

适配 iPhone 6 和 iPhone 6plus 则需要写：

	<meta name="viewport" content="width=375">
	<meta name="viewport" content="width=414">

大部分 4.7~5 寸的安卓设备的 viewport 宽设为 360px，iPhone 6 上却是 375px，大部分 5.5 寸安卓机器（比如说三星 Note）的 viewport 宽为 400，iPhone 6 plus 上是 414px。

###ios 设备

添加到主屏后的标题（iOS 6 新增）

	<meta name="apple-mobile-web-app-title" content="标题"> <!-- 添加到主屏后的标题（iOS 6 新增） -->

是否启用 WebApp 全屏模式

	<meta name="apple-mobile-web-app-capable" content="yes" /> <!-- 是否启用 WebApp 全屏模式 -->

设置状态栏的背景颜色

	<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" /> <!-- 设置状态栏的背景颜色，只有在 `"apple-mobile-web-app-capable" content="yes"` 时生效 -->

只有在 "apple-mobile-web-app-capable" content="yes" 时生效

content 参数：

    * default 默认值。
    * black 状态栏背景是黑色。
    * black-translucent 状态栏背景是黑色半透明。 如果设置为 default 或 black,网页内容从状态栏底部开始。 如果设置为 black-translucent ,网页内容充满整个屏幕，顶部会被状态栏遮挡。

禁止数字识自动别为电话号码

	<meta name="format-detection" content="telephone=no" /> <!-- 禁止数字识自动别为电话号码 -->

##iOS 图标

rel 参数： apple-touch-icon 图片自动处理成圆角和高光等效果。 apple-touch-icon-precomposed 禁止系统自动添加效果，直接显示设计原图。 iPhone 和 iTouch，默认 57x57 像素，必须有

	<link rel="apple-touch-icon-precomposed" href="/apple-touch-icon-57x57-precomposed.png" /> <!-- iPhone 和 iTouch，默认 57x57 像素，必须有 -->

iPad，72x72 像素，可以没有，但推荐有
	
	<link rel="apple-touch-icon-precomposed" sizes="72x72" href="/apple-touch-icon-72x72-precomposed.png" /> <!-- iPad，72x72 像素，可以没有，但推荐有 -->

Retina iPhone 和 Retina iTouch，114x114 像素，可以没有，但推荐有

	<link rel="apple-touch-icon-precomposed" sizes="114x114" href="/apple-touch-icon-114x114-precomposed.png" /> <!-- Retina iPhone 和 Retina iTouch，114x114 像素，可以没有，但推荐有 -->


Retina iPad，144x144 像素，可以没有，但推荐有

	<link rel="apple-touch-icon-precomposed" sizes="144x144" href="/apple-touch-icon-144x144-precomposed.png" /> <!-- Retina iPad，144x144 像素，可以没有，但推荐有 -->

IOS 图标大小在iPhone 6 plus上是180×180，iPhone 6 是120x120。 适配iPhone 6 plus，则需要在中加上这段

	<link rel="apple-touch-icon-precomposed" sizes="180x180" href="retinahd_icon.png">
	
###iOS 启动画面

官方文档：[https://developer.apple.com/library/ios/qa/qa1686/_index.html](https://developer.apple.com/library/ios/qa/qa1686/_index.html)


iPad 的启动画面是不包括状态栏区域的。

iPad 竖屏 768 x 1004（标准分辨率）

	<link rel="apple-touch-startup-image" sizes="768x1004" href="/splash-screen-768x1004.png" /> <!-- iPad 竖屏 768 x 1004（标准分辨率） -->

iPad 竖屏 1536x2008（Retina）

	<link rel="apple-touch-startup-image" sizes="1536x2008" href="/splash-screen-1536x2008.png" /> <!-- iPad 竖屏 1536x2008（Retina） -->

iPad 横屏 1024x748（标准分辨率）

	<link rel="apple-touch-startup-image" sizes="1024x748" href="/Default-Portrait-1024x748.png" /> <!-- iPad 横屏 1024x748（标准分辨率） -->

iPad 横屏 2048x1496（Retina）

	<link rel="apple-touch-startup-image" sizes="2048x1496" href="/splash-screen-2048x1496.png" /> <!-- iPad 横屏 2048x1496（Retina） -->

iPhone 和 iPod touch 的启动画面是包含状态栏区域的。

iPhone/iPod Touch 竖屏 320x480 (标准分辨率)

	<link rel="apple-touch-startup-image" href="/splash-screen-320x480.png" /> <!-- iPhone/iPod Touch 竖屏 320x480 (标准分辨率) -->

iPhone/iPod Touch 竖屏 640x960 (Retina)

	<link rel="apple-touch-startup-image" sizes="640x960" href="/splash-screen-640x960.png" /> <!-- iPhone/iPod Touch 竖屏 640x960 (Retina) -->

iPhone 5/iPod Touch 5 竖屏 640x1136 (Retina)

	<link rel="apple-touch-startup-image" sizes="640x1136" href="/splash-screen-640x1136.png" /> <!-- iPhone 5/iPod Touch 5 竖屏 640x1136 (Retina) -->

添加智能 App 广告条 Smart App Banner（iOS 6+ Safari）

	<meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL"> <!-- 添加智能 App 广告条 Smart App Banner（iOS 6+ Safari） -->

iPhone 6对应的图片大小是750×1294，iPhone 6 Plus 对应的是1242×2148 。

	<link rel="apple-touch-startup-image" href="launch6.png" media="(device-width: 375px)">

	<link rel="apple-touch-startup-image" href="launch6plus.png" media="(device-width: 414px)">


###Windows 8

Windows 8 磁贴颜色

	<meta name="msapplication-TileColor" content="#000"/> <!-- Windows 8 磁贴颜色 -->

Windows 8 磁贴图标

	<meta name="msapplication-TileImage" content="icon.png"/> <!-- Windows 8 磁贴图标 -->

rss订阅

	<link rel="alternate" type="application/rss+xml" title="RSS" href="/rss.xml" /> <!-- 添加 RSS 订阅 -->

favicon icon

	<link rel="shortcut icon" type="image/ico" href="/favicon.ico" /> <!-- 添加 favicon icon -->




