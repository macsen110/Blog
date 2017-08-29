# javascript 设计模式与开发实践
1.单例模式
(保证一个类仅有一个实例,并提供一个访问它的全局访问点)
用一个变量标识当前是否为某个类创建过对象,如果是,下次获取该类实例时,直接返回之前创建的对象，
使用单例模式设计页面中的单个弹框
```
var createLoginLayer = (function () {
     var div;
     return function () {
          if (!div) {
               div = document.createElement('div');
               div.innerHTML = '我是登陆浮窗';
               div.style.display = "noen";
               document.body.appendChild(div)
          }
          return div
     }
}())
```

2.策略模式
(定义一系列的算法,把它们一个个封装起来,并且使它们可以相互替换,目的是将算法的使用和算法的实现分离开来)
```
var strategies = {
     "S": function (salary) {
          return  salary * 4
     },
     "A": function (salary) {
          return salary * 3
     },
     "B": function (salary) {
          return salary *2
     }
}
var calculateBonus = function () {level, salary} {
     return strategies[level](salary)
}
```
3.代理模式
为一个对象提供一个代用品或占位符,以便控制对它的访问
单一职责原则是指一个类(通常也包括对象和函数)，应该仅有一个引起它变化的原因
```
var myImage = (function () {
     var imgNode = document.createElement('img');
     document.body.appendChild(imgNode);
     return {
          setSrc: function (src) {
               imgNode.src = src
          }
     }
}())

var proxyImage = (function () {
     var img = new Image;
     img.onload = function () {
          myImage.setSrc(this.src)
     }
     return {
          setSrc: function (src) {
               myImage.setSrc('../loading.gif');
               img.src = src
          }
     }
} ())
```

proxyImage.setSrc('http://imgcache.qq.com/some/file/some.jpg')

4.迭代器模式
提供一种方法顺序访问一个聚合对象中的各个元素,而又不需要暴漏该对象的内部表示

5.发布-订阅模式
又叫观察者模式,它定义对象间的一种一对多的依赖关系,当一个对象的状态发生改变时,所有依赖它的对象都将得到通知,在javascript中我们一般使用事件模型来替代传统的发布-订阅模式

```
var event = {
     clientList: [],
     listen: function (key, fn) {
          if (!this.clientList[key]) {
               this.clientList[key] = [];
          }
          this.clientList[key].push(fn)  //订阅的消息添加进缓存列表
     },
     trigger: function () {
          var key = Array.prototype.shift.call(arguments),
               fns = this.clientList[key];
          if(!fns || fns.length === 0) {   //如果没有绑定对应的消息
               return false
          }
          for (var i = 0, fn ; fn = fns[i++]) {
               fn.apply(this, arguments)  // arguments 是trriger时带上的参数
          }
     },
     remove: function (key, fn) {
          var fns = this.clientList[key];
          if (!fns) { //如果key对应的消息没有被人订阅,则直接返回
               return false
          }
          if (!fn) {  //如果没有传入具体的回调函数,表示需要取消key对应消息的所有订阅
               fns && (fns.length = 0)
          }else {
               for (var l = fns.length -1; l >= 0; l--) {
                    var _fn = fns[l];
                    if(_fn === fn) {
                         fns.splice(l, 1)
                    }
               }
          }

     }
 }

```
网站登录案例,各模块监听登录成功后的响应事件

6.命令模式

7.组合模式
应用场景:表示对象的部分--整体层次结构
客户希望统一对待树中的所有对象

8.模板方法模式
模板方法模式是一种严重依赖抽象类的设计模式。在java中,类分为两种,一种为具体类,另一种为抽象类,具体类可以被实例化,抽象类不能被实例化，如果编写了一个抽象类,那么这个抽象类一定用来被某些具体类继承的,基于继承的一种模式,抽离具体类的公共方法或者属性写成抽象类

9.享元模式
用于性能优化的模式

10. 职责链模式
使多个对象都有机会处理请求,从而避免请求的发送者和接收者之间的耦合关系,类似于方法链

11.中介者模式
使各个对象之间得以解耦,以中介者和对象之间得以解耦,以中介者和对象之间的一对多关系取代了对象之间的网状多对多关系.各个对象只需关注自身功能的实现,对象之间的交互关系交给中介者对象来实现和维护

12.状态模式