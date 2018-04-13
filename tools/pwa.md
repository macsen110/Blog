PWA知识点整理
一. serviceWorker

1. register(scriptURL, options)

    A. option.scope 
    > A USVString representing a URL that defines a service worker's registration scope; that is, what range of URLs a service worker can control.

  score 不设定值得话,service worker 作用范围是 `scriptURL` 所在的目录

  ```
  if ('serviceWorker' in navigator) {
    window.addEventListener('load', function () {
        navigator.serviceWorker.register('/sw.js', {scope: '/'})
            .then(function (registration) {

                // 注册成功
                console.log('ServiceWorker registration successful with scope: ', registration.scope);
            })
            .catch(function (err) {

                // 注册失败:(
                console.log('ServiceWorker registration failed: ', err);
            });
    });
}
  ```
在chrome://inspect/#service-workers中查看pwa是否注册成功


  A. 不是 HTTPS 环境，不是 localhost 或 127.0.0.1。

  B. Service Worker 文件的地址没有写对，需要相对于 origin。

  C. Service Worker 文件在不同的 origin 下而不是你的 App 的，这是不被允许的。

2. install 
> install 事件我们会绑定在 Service Worker 文件中，在 Service Worker 安装成功后，install 事件被触发。
>install 事件一般是被用来填充你的浏览器的离线缓存能力。为了达成这个目的，我们使用了 Service Worker 新的标志性的存储 cache API — 一个 Service Worker 上的全局对象，它使我们可以存储网络响应发来的资源，并且根据它们的请求来生成key。这个 API 和浏览器的标准的缓存工作原理很相似，但是是只对应你的站点的域的。它会一直持久存在，直到你告诉它不再存储，你拥有全部的控制权
>localStorage 的用法和 Service Worker cache 的用法很相似，但是由于 localStorage 是同步的用法，所以不允许在 Service Worker 中使用。 IndexedDB 也可以在 Service Worker 内做数据存储。

```
// 监听 service worker 的 install 事件
this.addEventListener('install', function (event) {
    // 如果监听到了 service worker 已经安装成功的话，就会调用 event.waitUntil 回调函数
    event.waitUntil(
        // 安装成功后操作 CacheStorage 缓存，使用之前需要先通过 caches.open() 打开对应缓存空间。
        caches.open('my-test-cache-v1').then(function (cache) {
            // 通过 cache 缓存对象的 addAll 方法添加 precache 缓存
            return cache.addAll([
                '/',
                '/index.html',
                '/main.css',
                '/main.js',
                '/image.jpg'
            ]);
        })
    );
});
```