#html中head的一些汇总(参考与百度前端中的汇总)
DOCTYPE

DOCTYPE(Document Type)，该声明位于文档中最前面的位置，处于 html 标签之前，此标签告知浏览器文档使用哪种 HTML 或者 XHTML 规范。

DTD(Document Type Definition) 声明以 <!DOCTYPE> 开始，不区分大小写，前面没有任何内容，如果有其他内容(空格除外)会使浏览器在 IE 下开启怪异模式(quirks mode)渲染网页。公共 DTD，名称格式为注册//组织//类型 标签//语言,注册指组织是否由国际标准化组织(ISO)注册，+表示是，-表示不是。组织即组织名称，如：W3C。类型一般是 DTD。标签是指定公开文本描述，即对所引用的公开文本的唯一描述性名称，后面可附带版本号。最后语言是 DTD 语言的 ISO 639 语言标识符，如：EN 表示英文，ZH 表示中文。XHTML 1.0 可声明三种 DTD 类型。分别表示严格版本，过渡版本，以及基于框架的 HTML 文档。

    HTML 4.01 strict

    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

    HTML 4.01 Transitional

    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

    HTML 4.01 Frameset

    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">

    最新 HTML5 推出更加简洁的书写，它向前向后兼容，推荐使用。

    <!doctype html>

在 HTML中 doctype 有两个主要目的。

    对文档进行有效性验证。

它告诉用户代理和校验器这个文档是按照什么 DTD 写的。这个动作是被动的，每次页面加载时，浏览器并不会下载 DTD 并检查合法性，只有当手动校验页面时才启用。

    决定浏览器的呈现模式

    对于实际操作，通知浏览器读取文档时用哪种解析算法。如果没有写，则浏览器则根据自身的规则对代码进行解析，可能会严重影响 html 排版布局。浏览器有三种方式解析 HTML 文档。
        非怪异（标准）模式
        怪异模式
        部分怪异（近乎标准）模式 关于IE浏览器的文档模式，浏览器模式，严格模式，怪异模式，DOCTYPE 标签，可详细阅读模式？标准！的内容。

charset

声明文档使用的字符编码，

<meta charset="utf-8">

html5 之前网页中会这样写：

<meta http-equiv="Content-Type" content="text/html; charset=utf-8">

这两个是等效的，具体可移步阅读：<meta charset='utf-8'> vs <meta http-equiv='Content-Type'>，所以建议使用较短的，易于记忆。
lang属性

简体中文

<html lang="zh-cmn-Hans">

繁体中文

<html lang="zh-cmn-Hant">

为什么 lang="zh-cmn-Hans" 而不是我们通常写的 lang="zh-CN" 呢，请移步阅读: 页头部的声明应该是用 lang="zh" 还是 lang="zh-cn"。
优先使用 IE 最新版本和 Chrome

<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />

360 使用Google Chrome Frame

<meta name="renderer" content="webkit">

360 浏览器就会在读取到这个标签后，立即切换对应的极速核。 另外为了保险起见再加入

<meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1">

这样写可以达到的效果是如果安装了 Google Chrome Frame，则使用 GCF 来渲染页面，如果没有安装 GCF，则使用最高版本的 IE 内核进行渲染。

相关链接：浏览器内核控制 Meta 标签说明文档
百度禁止转码

通过百度手机打开网页时，百度可能会对你的网页进行转码，脱下你的衣服，往你的身上贴狗皮膏药的广告，为此可在 head 内添加

<meta http-equiv="Cache-Control" content="no-siteapp" />

相关链接：SiteApp 转码声明
SEO 优化部分

    页面标题<title>标签(head 头部必须)

    <title>your title</title>

    页面关键词 keywords

    <meta name="keywords" content="your keywords">

    页面描述内容 description

    <meta name="description" content="your description">

    定义网页作者 author

    <meta name="author" content="author,email address">

    定义网页搜索引擎索引方式，robotterms 是一组使用英文逗号「,」分割的值，通常有如下几种取值：none，noindex，nofollow，all，index和follow。

    <meta name="robots" content="index,follow">

相关链接：WEB1038 - 标记包含无效的值
