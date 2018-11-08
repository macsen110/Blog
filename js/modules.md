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

es6 modules
___

1. ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量
2. CommonJS 和 AMD 模块，都只能在运行时确定这些东西。
3. ES6 模块输出的是值的引用，输出接口动态绑定，而 CommonJS 输出的是值的拷贝
4. ES6 模块编译时执行，而 CommonJS 模块总是在运行时加载


```
// CommonJS模块
let { stat, exists, readFile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```

`上面代码的实质是整体加载fs模块（即加载fs的所有方法），生成一个对象（_fs），然后再从这个对象上面读取 3 个方法。这种加载称为“运行时加载”，因为只有运行时才能得到这个对象，导致完全没办法在编译时做“静态优化”。`

A. import

```
// ES6模块
import { stat, exists, readFile } from 'fs';
```

`上面代码的实质是从fs模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。`

```
import defaultExport from "module-name";
import * as name from "module-name";
import { export } from "module-name";
import { export as alias } from "module-name";
import { export1 , export2 } from "module-name";
import { foo , bar } from "module-name/path/to/specific/un-exported/file";
import { export1 , export2 as alias2 , [...] } from "module-name";
import defaultExport, { export [ , [...] ] } from "module-name";
import defaultExport, * as name from "module-name";
import "module-name";
var promise = import("module-name");
```

B. export
1. export default就是输出一个叫做default的变量或方法，然后系统允许你为它取任意名字,下面的写法是有效的。

```
// modules.js
function add(x, y) {
  return x * y;
}
export {add as default};
// 等同于
// export default add;

// app.js
import { default as foo } from 'modules';
// 等同于
// import foo from 'modules';
```

2. export default命令其实只是输出一个叫做default的变量，所以它后面不能跟变量声明语句。

```
// 正确
export var a = 1;

// 正确
var a = 1;
export default a;

// 错误
export default var a = 1;

// 正确
export default 42;

// 报错
export 42;
```

3. 常用写法

```
export { name1, name2, …, nameN };
export { variable1 as name1, variable2 as name2, …, nameN };
export let name1, name2, …, nameN; // also var, const
export let name1 = …, name2 = …, …, nameN; // also var, const
export function FunctionName(){...}
export class ClassName {...}

export default expression;
export default function (…) { … } // also class, function*
export default function name1(…) { … } // also class, function*
export { name1 as default, … };

export * from …;
export { name1, name2, …, nameN } from …;
export { import1 as name1, import2 as name2, …, nameN } from …;
export { default } from …;
```