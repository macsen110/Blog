# webpack
1. CommonsChunkPlugin
```
最好要在入口entry处
多个CommonChunkPlugin, 一般要和manifest配合
   // new webpack.optimize.CommonsChunkPlugin({
    //   name: 'vendor',
    //   minChunks: Infinity
    // }),

    // new webpack.optimize.CommonsChunkPlugin({
    //   name: 'ui',
    //   children: true,
    //   minChunks: function(module){
    //     return module.context && module.context.indexOf("node_modules") !== -1;
    //   },
    //   //filename: "my-single-lib-chunk.js",
    // }),
    // new webpack.optimize.CommonsChunkPlugin({
    //   name: "manifest",
    //   //minChunks: Infinity
    // }),

也可以写成

   // new webpack.optimize.CommonsChunkPlugin({
    //   names: ['vendor', 'tools', ''],
    //   minChunks: Infinity
    // }),


```


