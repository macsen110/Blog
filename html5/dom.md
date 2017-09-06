# DOM 知识点
1. offsetTop 值不随滚动变化(跟父级有无定位有关),无定位时相对于文档
2.  The MouseEvent.offsetX read-only property provides the offset in the X coordinate of the mouse pointer between that event and the padding edge of the target node.offsetX 与offsetY 是代表事件触发点在目标元素上的位置坐标
3. pageX, pageY 是相对于文档.
4. screenX, screenY 是相对于浏览器屏幕对象
5. clientX, clientY 是相对于可见工作区对象
6. clientTop,一般代表边框的宽度
7. 当元素没有设置高度属性和边框时,有没有滚动条 clienHeight与scrollHeight,offsetHeight一致
8. 当元素有设置边框时,在chrome下  scrollWidth 不包含边框
9. getBoundingClientRect().top 随着滚动元素到屏幕距离变化
10. 文档结构和遍历
     10.1 作为节点树的文档
>

     Document对象,Element对象以及text对象都表示为node对象
     parentNode，childnodes ,firstChild, lastChild,nextSibling,previousSibling,nodeType,nodeValue,nodeName

     注意,获取某些节点上时有空白和文本的节点可以通过以下函数来筛选

     //判断筛选的元素级是否全是空格(is entirely whitespace.)
     function isAllWs( nod ) {
          return !(/[^\t\n\r ]/.test(nod.textContent));
     }
     //判断是否可忽略,包括注释节点和空格
     function isIgnorable( nod ) {
          return ( nod.nodeType == 8) || // A comment node
               ( (nod.nodeType == 3) && isAllWs(nod) ); // a text node, all ws
     }
     //check if the previous sibling node is an element node
     function nodeBefore( sib ) {
          while ((sib = sib.previousSibling)) {
               if (!isIgnorable(sib)) return sib;
          }
          return null;
     }

     //判断node next

     function nodeNext( sib ) {
          console.log(sib = sib.nextSibling)
          while (sib = sib.nextSibling) {

               if (!isIgnorable(sib)) return sib;
          }
          return null;
     }

     3.2作为元素树的文档
     主要针对操作元素,忽略元素节点间的空白,Text 和 Comment节点,其实也是节点,也有
nodeType,nodeValue,nodeName
     FirstElementChild,lastElementChild,nextElementSibling,previousElementSibling,childElementCount
     addEventListerner 第三个参数只指定了程序执行的阶段,true表示捕获阶段,false 表示冒泡阶段,stopPropagagation()才是阻止冒泡

* 页面重绘

window.requestAnimationFrame()这个方法是用来在页面重绘之前，通知浏览器调用一个指定的函数，以满足开发者操作动画的需求。这个方法接受一个函数为参，该函数会在重绘前调用。

```
// 模拟DOMContentLoaded
document.onreadystatechange = function () {
  if (document.readyState == "interactive") {
    initApplication();
  }
}

// 模拟 load事件
document.onreadystatechange = function () {
if (document.readyState == "complete") {
  initApplication();
}}

```

## 高性能 DOM

### 简化HTML 结构

* 使用 HTML5 语义化标签，且标签使用规范，减少浏览器的判断时间。 
* 代码要结构化、语义化。 
* HTML(页面结构)、CSS（样式表）、JS(脚本) 文件分离，各司其职。 
* 把 script 标签移到 HTML 文件末尾，因为 JS 会阻塞后面的页面的显示。 减少不必要的嵌套，尽量扁平化，因为当浏览器编译器遇到一个标签时就开始寻找它的结束标签，直到它匹配上才能显示它的内容，所以当嵌套很多时打开页面就会特别慢。 
* 避免使用 br 标签分行，可以使用 block 元素或 CSS 显示属性来代替,使用 CSS 来调整边距。 
* 避免使用 hr 标签来添加水平线，可使用 CSS 的 border-bottom 来代替。 
* 使用 DIV + CSS 替代 Tables 来布局。 (但有些结构明显使用table高效的就使用table)
* 可以多使用 Flex Box。 除去无用的标签和空标签。

### 浏览器工作原理

>
![浏览器工作流程图](http://i.imgur.com/Y0xF7Qr.png)
>

1. 首先用户在地址栏输入域名，如 fsux.me，DNS（域名解析系统）服务器根据输入的域名查找对应 IP，然后向该 IP 地址发起请求。 
2. 其次浏览器获得并解析服务器的返回内容 (HTTP response)。 
3. 浏览器加载 HTML 文件及文件内包含的外部引用文件（CSS、JS）及图片，多媒体等资源。 
4. 根据请求回来的 HTML 文件开始从上到下解析 HTML 文档生成 DOM 节点树（DOM tree），也叫内容树（content tree），此时解析过程中碰见了外部引用的 CSS 文件，去服务器请求回 CSS 文件，构建 CSSOM (CSS Object Model)树，加载解析样式生成 CSSOM 树。 
5. 此时继续解析 HTML，又碰见了外部引用的 JS 文件，去服务器请求回 JS 文件，加载并执行 JS 代码（包括内联代码或外联 JS 文件）。 
6. 此时在解析 HTML 过程中发现一个标签内引用了一张关联图片，去服务器请求回这张图片，浏览器解析器不会等待图片下载完，而是继续渲染后面的代码。 
7. 此时 HTML 代码和 CSS 代码已经形成 DOM 树和 CSSOM 树,并生成渲染树(render tree)，渲染树按顺序展示在屏幕上的一系列矩形，这些矩形带有字体，颜色和尺寸等视觉属性。 
8. 布局（layout）：根据渲染树将节点树的每一个节点布局在屏幕上的正确位置。 9.绘制（painting）：遍历渲染树绘制所有节点，为每一个节点适用对应的样式，这一过程是通过 UI 后端模块完成。

> 重绘 当前元素的颜色样式(背景颜色、字体颜色等)发生改变的时候，我们只需要把改变的元素重新的渲染一下即可，重绘主要改变外观风格（改个颜色，换个皮肤），不改变布局，不影响其他的dom，所以重绘对浏览器的性能影响较小，一般不做优化，但是能避免最好。 
>

> 回流 指浏览器为了重新渲染部分或者全部的文档而重新计算文档中元素的位置和几何构造的过程。 因为回流可能导致整个 DOM 树的重新构造，所以是性能的一大杀手，一个元素的回流导致了其所有子元素以及 DOM 中紧随其后的祖先元素的随后的回流。 

触发回流的主要有以下操作： 

* 调整窗口大小 
* 改变字体 增加或者移除样式表。
* 内容变化，比如用户在 input、textarea、下拉框中输入或选择文字 激活 CSS 伪类，比如 :hover (IE 中为兄弟结点伪类的激活) 操作 class 属性 
* JS 操作 DOM 
* 计算 offsetWidth 和 offsetHeight 属性 
* 设置 style 属性的值 
* fixed 定位的元素,在拖动滚动条的时候会一直回流

避免重绘和回流

* 动态改变样式，则使用 className。

* 使用 DocumentFragment 进行缓存操作,引发一次回流和重绘。（兼容 IE9+ 及主流浏览器） 例如创建一个文档碎片，这里相当于一个容器，把动态创建的元素先放到容器中,最后再一起添加到页面中，这样只引发一次回流。

```
// Create the fragment 
var fragment = document.createDocumentFragment();
 //add DOM to fragment 
 for(var i = 0; i < 10; i++) { var spanNode = document.createElement("span"); spanNode.innerHTML = "number:" + i; fragment.appendChild(spanNode); } 
 //add this DOM to body 
 document.body.appendChild(spanNode);

```

*  使用 cloneNode (true or false) 和 replaceChild 技术，引发一次回流和重绘。 如果需要对一个元素进行复杂的操作（删减、添加子节点），那么我们应当先将元素从页面中移除，然后再对其进行操作，或者将其复制一个，在内存中进行操作后再替换原来的节点。 
```
//cloneNode 克隆节点 
var Box = document.getElementById("box"); 
var Li = document.createElement("li"); 
Li.innerHTML = "hello"; 
for(var i= 0; i<100; i++){ var CreateLi = Li.cloneNode(true); Box.appendChild(CreateLi); } 
```

* 不要对元素进行 JS 动画流操作，尽量使用 CSS 动画属性，以减少回流的 Render Tree 的规模。 
```
//不提倡一下做法 
$(".divOne").animate({top:100}); 
$(".DivTwo").animate({bottom:100});
```


