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

