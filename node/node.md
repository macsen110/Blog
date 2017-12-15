# node开发注意点

1. 本地开发监听file change, 使用nodemon 模块,可减少内存开销
* node 两个模块是针对二进制流的处理 `Buffer` && `TypedArray`

2. module.exports 与 export.prop 不要同时应用到一个js文件中

使用pm2后更新process.env.NODE_EDV
```
$ pm2 stop all && pm2 kill && rm -R ~/.pm2 && sudo rm -R /root/.pm2
$ pm2 startup
```