 1. chrome中 console 的方法会数组或者对象进行延迟的计算, 类似于异步
 It really is a case of lazy evaluation.
 本质原因是页面还未完全加载,console 还未准备好
 解决方案时是针对数组或对象 使用JSON解析
 console.log(JSON.parse(JSON.stringify(options)));