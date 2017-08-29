# Vuejs 解读

* 指令一般通过v-on:来绑定事件监听器,或者是指令缩写@

* v-bind 动态的绑定一个或多个特性,或一个组件prop到表达式缩写为:,
比如绑定class style src

* vm.$on(todo) vm.emit在本组件内部有效,事件不能传递,父组件可以通过属性传递到子组件 such as:
<child @todo="parentFun"></child>

## 一些规范性的东西
* 指令对应的函数处理一般都是方法里面,不要绑定到props里面

* props 对应的属性不可更改 赋值,有需求的话使用data替代，props 默认初始话的时候都会执行一次, such as 

```
props: {fun: function () {console.log(0)}} //will print 0
```
