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

声明文档使用的字符编码，
* <meta charset="utf-8">
html5之前会这样写
* <meta http-equiv="Content-Type" content="text/html; charset=utf-8">

### lang属性

简体中文

* <html lang="zh-cmn-Hans">





