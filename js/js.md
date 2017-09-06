## 写点js吧 

对象 
===
js中所有变量都是对象,除了null和underfined
---------------

  * 2.toString()会报错,操作符.会被当成浮点数的符号,改为(2).toString();
  * for in 访问一个对象,原型链上的所有属性都将被访问

函数,js的一等对象 
------------

这意味着可以把函数像其它值一样传递。 一个常见的用法是把匿名函数作为回调函数传递到异步函数中。

* 函数声明
    function foo() {}
    上面的方法会在执行前被 解析(hoisted)，因此它存在于当前上下文的任意一个地方， 即使在函数定义体的上面被调用也是对的.

* 函数赋值表达式
    var foo = function() {};

  这个例子把一个匿名的函数赋值给变量 foo。

  foo; // 'undefined'
  foo(); // 出错：TypeError
  var foo = function() {};

    由于 var 定义了一个声明语句，对变量 foo 的解析是在代码运行之前，因此 foo 变量在代码运行时已经被定义过了。

    但是由于赋值语句只在运行时执行，因此在相应代码执行之前， foo 的值缺省为 undefined。
    命名函数的赋值表达式

* 另外一个特殊的情况是将命名函数赋值给一个变量。

    var foo = function bar() {
        bar(); // 正常运行
    }
    bar(); // 出错：ReferenceError

  bar 函数声明外是不可见的，这是因为我们已经把函数赋值给了 foo； 然而在 bar 内部依然可见。这是由于 JavaScript 的 命名处理      所致， *函数名在函数内总是可见的*。
  
 
 ### this 的工作原理 ###

JavaScript 有一套完全不同于其它语言的对 this 的处理机制。 在五种不同的情况下 ，this 指向的各不相同。


1. 全局范围内 this;

        当在全部范围内使用 this，它将会指向全局对象。浏览器中运行的 JavaScript 脚本，这个全局对象是 window。

2. 函数调用

        foo();

        这里 this 也会指向全局对象。在严格模式下（strict mode），不存在全局变量。 这种情况下 this 将会是 undefined。
        
3. 方法调用

        test.foo(); 
        这个例子中，this 指向 test 对象。

4. 调用构造函数

        new foo(); 

        如果函数倾向于和 new 关键词一块使用，则我们称这个函数是 构造函数。 在函数内部，this 指向新创建的对象。
        显式的设置 this

        function foo(a, b, c) {}

        var bar = {};
        foo.apply(bar, [1, 2, 3]); // 数组将会被扩展，如下所示
        foo.call(bar, 1, 2, 3); // 传递到foo的参数是：a = 1, b = 2, c = 3

        当使用 Function.prototype 上的 call 或者 apply 方法时，函数内的 this 将会被 显式设置为函数调用的第一个参数。

        因此函数调用的规则在上例中已经不适用了，在foo 函数内 this 被设置成了 bar。

  





作用域与命名空间 
---

尽管 JavaScript 支持一对花括号创建的代码段，但是并不支持块级作用域； 而仅仅支持 函数作用域。

    function test() { // 一个作用域
        for(var i = 0; i < 10; i++) { // 不是一个作用域
            // count
        }
        console.log(i); // 10
    }


  
window.URL.createObjectURL与FileReader
---
<blink>Returns a DOMString containing a unique blob URL, that is a URL with blob: as its scheme, followed by an opaque string uniquely identifying the object in the browser.</blink>

The FileReader object lets web applications asynchronously read the contents of files (or raw data buffers) stored on the user's computer, using File or Blob objects to specify the file or data to read.

    备注: URL.createObjectURL 返回值跟应用所在协议有关,files//或者http比方说在本地环境下生成的图片格式不能用在http环境下,而用FileReader.readAsDataURL() 则没有这个问题

    另外,使用URL.createObjectURL以后最好记得使用URL.revokeObjectURL()释放内存


## import 与script便签引入js的区别 ##

> import 在js文件中会使得提升到顶级处理,有点类似于函数体的变量提升，就是
    
    window.a = 110;
    import obj from './obj.js';
    会优先执行下面的语句
    obj.js中声明的全局变量都不会暴露出来
    export 导出的变量或者是函数在其他文件中import到,都是只读的,不能覆盖,重置

> script标签中声明的全局变量会在下一个script js中被读取到


# Promise

new Promise中如果不显示指定reject方法, throw new Errow 会被catch 方法获取到


# js tips



* 单元运算符 + 也可以把数字字符串转换成数值：+ ‘222’ //222
* num + ''可以转换成字符串
* !! 可以换布尔值
* [Variables//类数组]可以转换为数组
* replace 参数理解
* div.innertext=div.innertext,去除html标签，

```
var a = {n:1}
var b =a
a.x = a = {n:2}
a.x //undefined;
b.x //{n:2}

```

* document.currentScript;获取当前执行的script标签元素,

也可以用
```
var arrScripts = document.getElementsByTagName('script');
var strScriptTagId = arrScripts[arrScripts.length - 1].dataSet.id;
```
* js. splite还有第二个参数，limiter,限制分割数组的长度


js文件本身的编码问题,如果不是utf-8的话,会有影响中文返回值,特别是nodejs http response里面,所以特别注意

项目文件里面编码都要统一设置,必须设置

方法内部的函数（Functions inside a method）
每个函数都有一个特殊变量this。如果你在方法内部嵌入函数是很不方便的，因为你不能从函数中访问方法的this。
```
var jane = {
    name: 'Jane',
    friends: [ 'Tarzan', 'Cheeta' ],
    logHiToFriends: function () {
        'use strict';
        this.friends.forEach(function (friend) {
            // 这里的“this”是undefined
            console.log(this.name+' says hi to '+friend);
        });
    }
}
调用 logHiToFriends 会产生错误：

> jane.logHiToFriends()
TypeError: Cannot read property 'name' of undefined
```

有两种方法修复这问题。

### 将this存储在不同的变量。
```
logHiToFriends: function () {
    'use strict';
    var that = this;
    this.friends.forEach(function (friend) {
        console.log(that.name+' says hi to '+friend);
    });
}

```
### forEach的第二个参数允许提供this值。

```
logHiToFriends: function () {
    'use strict';
    this.friends.forEach(function (friend) {
        console.log(this.name+' says hi to '+friend);
    }, this);
}
```

遗漏点
object.keys(obj),返回一个数组，包含指定对象的所有自有可遍历（own enumerable）属性的名称。

Ajax 同步发生  window.open() 会发生,异步不会

文档中插入纯文本的标准方法:  Node.textContent

```
0 === false
false

0 == false
true

'' == false
true

1 == true
true

1 === true
false

2 == true
false

NaN === NaN
false

NaN == NaN
false

undefined === undefined
true

null === null
true

null === undefined
false

null == undefined
true

var de = new Date()
de instanceof Date  //true
de instanceof Object //true
typeof de  //"object"

var fun = function() {}
fun instanceof Function //true
fun instanceof Object    //true
typeof fun  //"function"

funtion E() {}
var e = new E()
e instanceof Object //true
e instanceof Function //false
E instanceof Function //true
E instanceof Object //true
Object.isPrototypeOf(e) //false
Object.prototype.isPrototypeOf(e) //true
Object.prototype.isPrototypeOf(E) //true
function EE() {}
EE.prototype = new E()
E.isPrototypeOf(EE) //false
E.prototype.isPrototypeOf(EE) //false
var ee = new EE()
E.prototype.isPrototypeOf(ee) //true
E.isPrototypeOf(ee) //false

isPrototype 是当前元素往下的原型链查找

function A() {}
function A() {this.aa = 'sd'}
A.hasOwnProperty('aa’) //false
var aa = new A()
aa.hasOwnProperty('aa’) //true
A.aaa = 'type'
A.hasOwnProperty('aaa’) //true

```


一行代码学js  (微博)

```
[].forEach.call($ $(""),function(a){
a.style.outline="1px solid #"+(~~(Math.random()*(1<<24))).toString(16)
})
```

正则
```
reg.text(str)          return boolen
reg.exec(str)         return array    (只是返回一个数组，只有一个元素,跟全局无关)注意其lastIndex属性
str.match(reg)      return array
str.search()
str.split()
str.replace()
```
类数组转换为数组
Array.prototype.slice.call()
原型继承时,基类里面有参数转化为自身属性时,可以在子类的构造函数里面使用call方法将基类里面的参数属性引入过来,也可以直接new的时候把参数转换为实参引入,但两者均可以继承,只是未定义
```
1.
function Parent (arg) {this.arg = arg}
Parent.prototype = {
     constructor: Parent,
     ccc: 'ccc'
}

function Child () {
     Parent.call(this,'test')
}

Child.prototype.constructor = Child;
var children = new Child();
children.arg === 'test' // true
children.ccc === 'ccc'  //false


2.
function Parent (arg) {this.arg = arg}
Parent.prototype = {
     constructor: Parent,
     ccc: 'ccc'
}
function Child () {}
Child.prototype = new Parent(’test');
Child.prototype.constructor = Child;
var children = new Child();
children.arg === ’test’ // true
children.ccc === 'ccc' //true
```
累加,累减其实是对变量的赋值操作
i++返回的是自增之前的值，++i返回的则是自增后的值。
如：
var i = 1;
var a = i++;  //a = 1;  此时i为2，但赋给a的是1
var b = ++i;  //b = 3

requireJs  -------->  AMD
seajs  -----------> CMD

switch用的是===进行判断

js 获取元素的样式

```
window.getComputedStyle(document.getElementById('test'),null).getPropertyValue('display'));
```
```
var obj = {
dom: {
a:1,
b: this
}
}
//这里的this指代window

var obj = {
dom: {
a:1,
b:function () { this}
}
}

//这里的this指代{a:1,b: function() {this}} 对象
```

1. 每次调用then都会返回一个新创建的promise对象
2. promise中尽量使用链式调用,
3. js 三元操作符里面不能使用return