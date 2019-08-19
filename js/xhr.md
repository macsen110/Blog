一. Ajax 请求中携带cookie解决
----

> 同源就是指URL中protocol协议、host域名、port端口这三个部分相同。简单来说，同源策略就是浏览器出于网站安全性的考虑，限制不同源之间的资源相互访问的一种政策.有些请求是不受到跨域限制。例如：WebSocket，script、img、iframe、video、audio标签的src属性等




前端进行数据请求中有: 普通ajax 请求, jsonp跨域请求, cors ajax跨域请求, fetch请求等

普通ajax请求和jsonp跨域请求默认都是携带cookie的，而fetch和cors ajax跨域请求默认不携带cookie

fetch 请求添加 

    credentials: 'include',


cors ajax 跨域请求时,需要添加(包括fetch)

    xhrFiles {
        withCredentials: true
    },
    crossDomain: true


跨域请求并携带cookie后端处理

res.header("Access-Control-Allow-Origin", "http://dev.macsen318.com:9000");
res.header("Access-Control-Allow-Credentials", true);
res.header("Access-Control-Allow-Headers", "Origin,No-Cache, X-Requested-With, If-Modified-Since, Pragma, Last-Modified, Cache-Control, Expires, Content-Type, X-E4M-With, userId, token");
res.header("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE"); //
(后端对于客户端cookie的读取操作还要注意是否是同域(根据客户端cookie存储域名))
或者

nginx 上添加

add_header 'Access-Control-Allow-Origin' "$http_origin" always;
add_header 'Access-Control-Allow-Credentials' 'true' always;
add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Requested-With' always;

或者nginx上面
针对请求的接口路径来反向代理  proxy_pass http://localhsot:3000;会带有客户端存储的cookie


本质上 Access-Control-Allow-Origin * 代表跨域允许,但是要通过客户端传cookie到后端,
是需要有一个指定的地址和端口号




jsonp

> jsonp是跨域领域中历史非常传统的一种方法。如果你还记得第一部分中我们提到过的内容，一些跨域请求是不会受到同源政策的限制的。其中，script标签就是一个。

>

* 在script标签中我们可以引用其他服务上的脚本，最常见的场景就是CDN。因此，有人想到，当有跨域请求到来时，如果我们可以把客户端需要的数据写到javascript脚本文件中并返回给客户端，那么客户端就可以拿到这些数据并使用了。具体是怎样一个流程呢？

* 首先，在myweb端，我们可以预先定义一个处理函数，叫它callback；
* 然后，在myweb端，我们动态创建一个script标签，并将该标签的src属性指向跨域的接口，并将callback函数名作为请求的参数；
* 跨域的thirdparty端接受到该请求后，返回一个javascript脚本文件，用callback函数包裹住数据；
* 这时候，前端收到响应数据会自动执行该脚本，这样便会自动执行预先定义的callback函数。

such as 

```
// myweb 部分
// 1. 创建回调函数callback
function myCallback(res) {
    alert(JSON.stringify(res, null , 2));
}
document.getElementById('btn-4').addEventListener('click', function() {
    // 2. 动态创建script标签，并设置src属性，注意参数cb=myCallback
    var script = document.createElement('script');
    script.src = 'http://127.0.0.1:3000/info/jsonp?cb=myCallback';
    document.getElementsByTagName('head')[0].appendChild(script);
});

```
```
// thirdparty
router.get('/jsonp', (req, res, next) => {
    var str = JSON.stringify(data);
    // 3. 创建script脚本内容，用`callback`函数包裹住数据
    // 形式：callback(data)
    var script = `${req.query.cb}(${str})`;
    res.send(script);
});
// 4. 前端收到响应数据会自动执行该脚本

```


二. Ajax 以及表单提交(浏览器提交支持的数据格式)
Content-type: 是浏览器对请求体的格式的约定
---

 * x-www-form-urlencoded

    类似于传统的表单 post提交，
    数据格式为'user='+user+'&passwd='+pwd

    method 'post'

    如果是表单浏览器提交 在form enctype属性设置为 ‘application/x-www-form-urlencoded’


如果是AJAX， Content-type 应该设置为 'application/x-www-form-urlencoded'


 * json

    只能是 AJAX 异步提交 
    
    Content-type 应该设置为 'application/json'
    
    提交的数据格式是 JSON.stringify(data)

 >


 * form-data

原生表单提交 设置enctype='multipart/form-data'
AJAX 中不用设置,form-data格式会自动改变请求头部为 Content-type multipart/form-data

ps：一个表单有文件上传,最好使用form-data格式
简单使用js 获取的表单数据 formdata = new FormData(form)

* text/plain 纯文本

* text/xml XML 请求体为xml

三. responseType

XMLHttpRequest.responseType
an enumerated string value specifying the type of data contained in the response.

对应的jq 中ajax dataType属性 


四. cookie 和 session 区别
1. cookie和session的共同之处在于：cookie和session都是用来跟踪浏览器用户身份的会话方式。
2. cookie 和session的区别是：cookie数据保存在客户端，session数据保存在服务器端。


浏览器application 选项下, cookie 只展示当前域名和根域名的key-value值