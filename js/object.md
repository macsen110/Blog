# 面向对象

面向对象编程是用抽象方式创建基于现实世界模型的一种编程模式。它使用先前建立的范例，包括模块化，多态和封装几种技术。今天，许多流行的编程语言（如Java，JavaScript，C＃，C+ +，Python，PHP，Ruby和Objective-C）都支持面向对象编程（OOP）。

面向对象编程可以看作是使用一系列对象相互协作的软件设计，相对于传统观念，一个程序只是一些函数的集合，或简单的计算机指令列表。 在OOP中，每个对象能够接收消息，处理数据和发送消息给其他对象。每个对象都可以被看作是一个拥有清晰角色或责任的独立小机器。

面向对象程序设计的目的是在编程中促进更好的灵活性和可维护性，在大型软件工程中广为流行。凭借其对模块化的重视，面向对象的代码开发更简单，更容易理解，相比非模块化编程方法 1, 它能更直接地分析, 编码和理解复杂的情况和过程。

每一个对象都有与之相关的的原型(prototype),类(class),和可扩展性(extensible attribute)

constructor
constructor 属性返回对创建此对象的函数的引用。
构造函数本身就是其类的公共标识

因此function B(){}

B === B.prototype.constructor

instanceof （A instanceof B ),此方法类似于isPrototypeOf(o)

instanceof 运算符希望左侧是一个对象,右侧操作数标识对象的类.

var d = new Date();
d instance Date或者Object;   //true

‘123’ instanceof String;//false，左侧应为对象,

原型链

Date.prototype的属性继承自Object.prototype,因此由new Date()创建的Date对象的属性同时继承自
Date.prototype和Object.prototype.这一系列的原型对象就是所谓的原型链.

类的静态方法(不能被子类继承,但可以影响)

function C () {

}
C.aaa ='somevalue'//此为静态方法

闭包是 JavaScript 一个非常重要的特性，这意味着当前作用域总是能够访问外部作用域中的变量。 因为 函数 是 JavaScript 中唯一拥有自身作用域的结构，因此闭包的创建依赖于函数。两个关键点, 内部函数和返回

Object.preventExtensions() 方法让一个对象变的不可扩展，也就是永远不能再添加新的属性。
需要注意的是不可扩展的对象的属性通常仍然可以被删除。 Object.preventExtensions 只能阻止一个对象不能再添加新的自身属性，仍然可以为该对象的原型添加属性。可以获取其原型新增属性,操作不可逆

Object.seal() 方法可以让一个对象密封，并返回被密封后的对象。密封对象是指那些不能添加新的属性，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性，但可能可以修改已有属性的值的对象。 属性不可配置的效果就是属性变的不可删除，以及一个数据属性不能被重新定义成为访问器属性，或者反之。但属性的值仍然可以修改。 不会影响从原型链上继承的属性

Object.freeze() 方法可以冻结一个对象。冻结对象是指那些不能添加新的属性，不能修改已有属性的值，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性的对象。也就是说，这个对象永远是不可变的。该方法返回被冻结的对象。 如果一个属性的值是个对象，则这个对象中的属性是可以修改的，除非它也是个冻结对象。

Object.create(obj) 创建一个对象继承自obj, Object.create(null) 创建一个纯净对象

> Object.assign(target, ...sources) 
used to copy the values of all enumerable own properties from one or more source objects to a target object. It will return the target object.