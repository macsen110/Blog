# module
导出一个模块标准写法
module.exports = Object;

以下两种写法均是module.exports 的简写形式
exports.aa = something
module.exports.aa = something

但是简写方式均不能和module.exports = Somethig 标准化输出方式在一个文件中

会以 module.exports = Something这种形式为主

与浏览器中 es6模块不一样 
export 和 export default 可以共存


