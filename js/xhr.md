一. Ajax 请求中携带cookie解决
----

前端进行数据请求中有: 普通ajax 请求, jsonp跨域请求, cors ajax跨域请求, fetch请求等

普通ajax请求和jsonp跨域请求默认都是携带cookie的，而fetch和cors ajax跨域请求默认不携带cookie

fetch 请求添加 

    credentials: 'include',


cors ajax 跨域请求时,需要添加(包括fetch)

    xhrFiles {
        withCredentials: true
    },
    crossDomain: true


后端处理

res.header("Access-Control-Allow-Origin", "http://10.6.26.38:9001");
res.header("Access-Control-Allow-Credentials", true);

或者

nginx 上添加

            add_header Access-Control-Allow-Origin http://10.6.26.38:9002;
            add_header Access-Control-Allow-Credentials true;

本质上 Access-Control-Allow-Origin * 代表跨域允许,但是要通过客户端传cookie到后端,
是需要有一个指定的地址和端口号

二. Ajax 以及表单提交(浏览器提交支持的数据格式)
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

 表单提交 设置enctype='multipart/form-data'
AJAX 中不用设置,form-data格式会自动改变请求头部为 Content-type multipart/form-data

ps：一个表单有文件上传,最好使用form-data格式
简单使用js 获取的表单数据 formdata = new FormData(form)

* text/plain 纯文本

* text/xml XML

请求题为xml


