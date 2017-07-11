## Amd, Cmd, CommonJs, Umd ##

模块的基本意义
___
1.定义封装的模块。

2.定义新模块对其他模块的依赖。

3.可对其他模块的引入支持。

CommonJs
___

>CommonJS是服务器端模块的规范，Node.js采用了这个规范。一个单独的文件就是一个模块。加载模块使用require方法，该方法读取一个文件并执行，最后返回文件内部的exports对象。

CommonJS 加载模块是同步的，所以只有加载完成才能执行后面的操作。像Node.js主要用于服务器的编程，加载的模块文件一般都已经存在本地硬盘，所以加载起来比较快，不用考虑异步加载的方式，所以CommonJS规范比较适用。但如果是浏览器环境，要从服务器加载模块，这是就必须采用异步模式。所以就有了 AMD CMD 解决方案。

AMD和RequireJS
___

AMD是"Asynchronous Module Definition"的缩写，意思就是"异步模块定义".

模块加载require([module], callback)

CMD和SeaJS
___

> CMD是SeaJS 在推广过程中对模块定义的规范化产出

对于依赖的模块AMD是提前执行，CMD是延迟执行。不过RequireJS从2.0开始，也改成可以延迟执行（根据写法不同，处理方式不通过）。
CMD推崇依赖就近，AMD推崇依赖前置。

    //AMD
    define(['./a','./b'], function (a, b) {
    
        //依赖一开始就写好
        a.test();
        b.test();
    });
 
    //CMD
    define(function (requie, exports, module) {
        
        //依赖可以就近书写
        var a = require('./a');
        a.test();
        
        ...
        //软依赖
        if (status) {
        
            var b = requie('./b');
            b.test();
        }
    });

UMD
___
> UMD是AMD和CommonJS的糅合

UMD先判断是否支持Node.js的模块（exports）是否存在，存在则使用Node.js模块模式。在判断是否支持AMD（define是否存在），存在则使用AMD方式加载模块。

    (function (window, factory) {
        if (typeof exports === 'object') {
        
            module.exports = factory();
        } else if (typeof define === 'function' && define.amd) {
        
            define(factory);
        } else {
        
            window.eventUtil = factory();
        }
    })(this, function () {
        //module ...
    });