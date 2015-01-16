##写点js吧

###对象
js中所有变量都是对象,除了null和underfined

  * 2.toString()会报错,操作符.会被当成浮点数的符号,改为(2).toString();
  * for in 访问一个对象,原型链上的所有属性都将被访问

###函数,js的一等对象,

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
  
 
 ###this 的工作原理

JavaScript 有一套完全不同于其它语言的对 this 的处理机制。 在五种不同的情况下 ，this 指向的各不相同。


1. 全局范围内
                this;

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

  



  

  


