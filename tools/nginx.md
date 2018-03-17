# Nginx (rewrite 指令)
* rewrite 
> If the specified regular expression matches a request URI, URI is changed as specified in the replacement string. The rewrite directives are executed sequentially in order of their appearance in the configuration file. It is possible to terminate further processing of the directives using flags. If a replacement string starts with “http://”, “https://”, or “$scheme”, the processing stops and the redirect is returned to a client.
>
rewrite 规则 定向路径 重写类型;

```
last ：相当于Apache里德(L)标记，表示完成rewrite，浏览器地址栏URL地址不变
break；本条规则匹配完成后，终止匹配，不再匹配后面的规则，浏览器地址栏URL地址不变
redirect：返回302临时重定向，浏览器地址会显示跳转后的URL地址
permanent：返回301永久重定向，浏览器地址栏会显示跳转后的URL地址
```

如果直接写一个路径，则匹配该路径下的

```
~ 表示执行一个正则匹配，区分大小写

~* 表示执行一个正则匹配，不区分大小写

^~ 表示普通字符匹配。使用前缀匹配。如果匹配成功，则不再匹配其他location。

= 进行普通字符精确匹配。也就是完全匹配。

!~表示区分大小写并把匹配结果取反；同理，!~*表示不区分大小写并把最终匹配结果取反
```
优先级
```
等号类型（=）的优先级最高。一旦匹配成功，则不再查找其他匹配项。

^~类型表达式。一旦匹配成功，则不再查找其他匹配项。

正则表达式类型（~ ~*）的优先级次之。如果有多个location的正则能匹配的话，则使用正则表达式最长的那个。

常规字符串匹配类型。按前缀匹配。
```

## if 指令与全局变量

if判断指令
语法为 if(condition) {...}，对给定的条件 condition 进行判断。如果为真，大括号内的 rewrite 指令将被执行，if 条件(conditon)可以是如下任何内容：
当表达式只是一个变量时，如果值为空或任何以 0 开头的字符串都会当做 false
直接比较变量和内容时，使用 = 或 !=
~ 正则表达式匹配，~* 不区分大小写的匹配，!~ 区分大小写的不匹配
-f 和 !-f 用来判断是否存在文件
-d 和 !-d 用来判断是否存在目录
-e 和 !-e 用来判断是否存在文件或目录
-x 和 !-x 用来判断文件是否可执行

```
# 如果UA包含"MSIE"，rewrite 请求到 /msid/ 目录下
if ($http_user_agent ~ MSIE) {
    rewrite ^(.*)$ /msie/$1 break;
} 
# 如果 cookie 匹配正则，设置变量 $id 等于正则引用部分
if ($http_cookie ~* "id=([^;]+)(?:;|$)") {
    set $id $1;
}
# 如果提交方法为 POST，则返回状态 405（Method not allowed）。return 不能返回 301, 302
if ($request_method = POST) {
    return 405;
}
# 限速，$slow 可以通过 set 指令设置
if ($slow) {
    limit_rate 10k;
} 
# 如果请求的文件名不存在，则反向代理到 localhost。这里的 break 也是停止 rewrite 检查
if (!-f $request_filename){
    break;
    proxy_pass  http://127.0.0.1; 
} 
# 如果 query string 中包含"post=140"，永久重定向到 example.com
if ($args ~ post=140){
    rewrite ^ http://example.com/ permanent;
}
# 防盗链
location ~* \.(gif|jpg|png|swf|flv)$ {
    valid_referers none blocked www.jefflei.com www.leizhenfang.com;
    if ($invalid_referer) {
        return 404;
    }
}
```

全局变量(Embedded Variables)
> The ngx_http_core_module module supports embedded variables with names matching the Apache Server variables. First of all, these are variables representing client request header fields, such as $http_user_agent, $http_cookie, and so on. Also there are other variables:
>
* $arg_name
argument name in the request line

* $args
arguments in the request line

* $binary_remote_addr
client address in a binary form, value’s length is always 4 bytes for IPv4 addresses or 16 bytes for IPv6 addresses

* $body_bytes_sent
number of bytes sent to a client, not counting the response header; this variable is compatible with the “%B” parameter of the mod_log_config Apache module

* $bytes_sent
number of bytes sent to a client (1.3.8, 1.2.5)

* $connection
connection serial number (1.3.8, 1.2.5)

* $connection_requests
current number of requests made through a connection (1.3.8, 1.2.5)

* $content_length
“Content-Length” request header field

* $content_type
“Content-Type” request header field

* $cookie_name
the name cookie

* $document_root
root or alias directive’s value for the current request

* $document_uri
same as $uri

* $host
in this order of precedence: host name from the request line, or host name from the “Host” request header field, or the server name matching a request

* $hostname
host name
* $http_name
arbitrary request header field; the last part of a variable name is the field name converted to lower case with dashes replaced by underscores
* $https
“on” if connection operates in SSL mode, or an empty string otherwise
* $is_args
“?” if a request line has arguments, or an empty string otherwise

* $limit_rate
setting this variable enables response rate limiting; see limit_rate

* $msec
current time in seconds with the milliseconds resolution (1.3.9, 1.2.6)

* $nginx_version
nginx version

* $pid
PID of the worker process

* $pipe
“p” if request was pipelined, “.” otherwise (1.3.12, 1.2.7)
* $proxy_protocol_addr
client address from the PROXY protocol header, or an empty string otherwise (1.5.12)
The PROXY protocol must be previously enabled by setting the proxy_protocol parameter in the listen directive.

* $proxy_protocol_port
client port from the PROXY protocol header, or an empty string otherwise (1.11.0)
The PROXY protocol must be previously enabled by setting the proxy_protocol parameter in the listen directive.

* $query_string
same as $args

* $realpath_root
an absolute pathname corresponding to the root or alias directive’s value for the current request, with all symbolic links resolved to real paths

* $remote_addr
client address

* $remote_port
client port

* $remote_user
user name supplied with the Basic authentication

* $request
full original request line

* $request_body
request body
The variable’s value is made available in locations processed by the proxy_pass, fastcgi_pass, uwsgi_pass, and scgi_pass directives when the request body was read to a memory buffer.


* $request_body_file
name of a temporary file with the request body
At the end of processing, the file needs to be removed. To always write the request body to a file, client_body_in_file_only needs to be enabled. When the name of a temporary file is passed in a proxied request or in a request to a FastCGI/uwsgi/SCGI server, passing the request body should be disabled by the proxy_pass_request_body off, fastcgi_pass_request_body off, uwsgi_pass_request_body off, or scgi_pass_request_body off directives, respectively.

* $request_completion
“OK” if a request has completed, or an empty string otherwise

* $request_filename
file path for the current request, based on the root or alias directives, and the request URI

* $request_id
unique request identifier generated from 16 random bytes, in hexadecimal (1.11.0)

* $request_length
request length (including request line, header, and request body) (1.3.12, 1.2.7)

* $request_method
request method, usually “GET” or “POST”

* $request_time
request processing time in seconds with a milliseconds resolution (1.3.9, 1.2.6); time elapsed since the first bytes were read from the client

* $request_uri
full original request URI (with arguments)

* $scheme
request scheme, “http” or “https”

* $sent_http_name
arbitrary response header field; the last part of a variable name is the field name converted to lower case with dashes replaced by underscores

* $sent_trailer_name
arbitrary field sent at the end of the response (1.13.2); the last part of a variable name is the field name converted to lower case with dashes replaced by underscores

* $server_addr
an address of the server which accepted a request
Computing a value of this variable usually requires one system call. To avoid a system call, the listen directives must specify addresses and use the bind parameter.

* $server_name
name of the server which accepted a request

* $server_port
port of the server which accepted a request

* $server_protocol
request protocol, usually “HTTP/1.0”, “HTTP/1.1”, or “HTTP/2.0”

* $status
response status (1.3.2, 1.2.2)

* $tcpinfo_rtt, $tcpinfo_rttvar, $tcpinfo_snd_cwnd, $tcpinfo_rcv_space
information about the client TCP connection; available on systems that support the TCP_INFO socket option

* $time_iso8601
local time in the ISO 8601 standard format (1.3.12, 1.2.7)

* $time_local
local time in the Common Log Format (1.3.12, 1.2.7)
$uri
current URI in request, normalized

* $args: #这个变量等于请求行中的参数，同$query_string
* $content_length: 请求头中的Content-length字段。
* $content_type: 请求头中的Content-Type字段。
* $document_root: 当前请求在root指令中指定的值。
* $host: 请求主机头字段，否则为服务器名称。
* $http_user_agent: 客户端agent信息
* $http_cookie: 客户端cookie信息
* $limit_rate: 这个变量可以限制连接速率。
* $request_method: 客户端请求的动作，通常为GET或POST。
* $remote_addr: 客户端的IP地址。
* $remote_port: 客户端的端口。
* $remote_user: 已经经过Auth Basic Module验证的用户名。
* $request_filename: 当前请求的文件路径，由root或alias指令与URI请求生成。
* $scheme: HTTP方法（如http，https）。
* $server_protocol: 请求使用的协议，通常是HTTP/1.0或HTTP/1.1。
* $server_addr: 服务器地址，在完成一次系统调用后可以确定这个值。
* $server_name: 服务器名称。
* $server_port: 请求到达服务器的端口号。
* $request_uri: 包含请求参数的原始URI，不包含主机名，如：”/foo/bar.php?arg=baz”。
* $uri: 不带请求参数的当前URI，$uri不包含主机名，如”/foo/bar.html”。
* $document_uri: 与$uri相同。

匹配规则

* `.` 匹配除换行符以外的任意字符
* `?` 重复0次或1次
* `+` 重复1次或更多次
* `\*` 重复0次或更多次
* `\d` 匹配数字
* `^` 匹配字符串的开始
* `$` 匹配字符串的介绍
* `{n}` 重复n次
* `{n,}` 重复n次或更多次
* `[c]` 匹配单个字符c
* `[a-z]` 匹配a-z小写字母的任意一个

小括号 () 之间匹配的内容，可以在后面通过 $1 来引用，$2 表示的是前面第二个 () 里的内容。正则里面容易让人困惑的是 \ 转义特殊字符。
such as: 
```
rewrite ^/(.*)([^/])$ http://$host/$1$2/ permanent;
```

## Rewrite 的综合使用

### Rewrite可以实现一级域名或多级域名的跳转。示例如下
```
## 示例1
...
server {
    listen 80;
    server_name lyblog.net;
    rewrite ^/ http://www.lyblog.net/;             ## 域名跳转
}
 
## 示例2
server {
    listen 80;
    server_name lyblog.net www.lyblog.net;
    if ($host ~ myweb\.net){
        rewrite ^(.*) http://www.lyblog.org$1 permanent;          ## 多域名跳转
    }
}
 
 
## 示例3
server {
    listen 80;
    server_name demo1.lyblog.net demo2.lyblog.net;
    if ($http_host ~* ^(.*)\.lyblog\.net$){
        rewrite ^(.*) http://demo.lyblog.net$1;                     ## 三级域名的跳转
    }
}
```

##域名镜象

镜像网站是指将一个完全相同的网站分别放置到几个服务器上，并分别使用独立的URL，其中一个服务器上的网站叫主站，其他的都为镜像网站。镜像站就可以看作是主站的一个副本。可以在主站存在问题时，作备份服务器使用。另外，也可以提高不同地区网站的响应速度。镜像网站可以响应网站流量负载，解决网络带宽封锁等问题。

Nginx中的Rewrite功能可以轻松实现域名镜像的跳转。实现原理很简单，也就是把不同镜像URL重写到指定的URL就可以了。以下是示例配置：
```
server {
    ...
    listen 80;
    server_name google.lyblog.net;
    rewrite ^(.*) http://www.google.com$1 last;
}
server {
    ...
    listen 81;
    server_name bings.lyblog.net;
    rewrite ^(.*) http://bings.cn$1 last;
}
```
当然，我们也可以为某一个目录下镜像，实现如下：
```
server {
    listen 80;
    server_name cdn.lyblog.net;
    location ^~ /source {
        ...
        rewrite ^/source(.*) http://cdn.google.com/websrc2$1 last;
    }
}
 
server {
    listen 81;
    server_name cdn1.lyblog.net;
    rewrite ^(.*) http://cdn.baidu.com/
 
    location ^~ /source2 {
        ...
        rewrite ^/source2(.*) http://cdn.baidu.com/websrc2$1 last;
    }
}
```

### 目录前自动添加"/"

如果网站设定了默认资源文件，那么客户端访问时可以不加具体的资源文件名。如，你访问：http://www.lyblog.net时，直接就可以访问到"/index.html"文件。

如果访问一个二级目录，如：http://www.lyblog.net/category/index.html；则如果直接输入http://www.lyblog.net/category可能无法访问，必须在后面加上斜线http://www.lyblog.net/category/。像这种情况，也不可能去要求用户这么输入，可以通过Rewrite功能为末尾没有斜杠"/"：

```
server {
    ...
    listen 81;
    server_name www.lyblog.net;
    location ^~ /bbs {
        ...
        if (-d $request_filename){
            rewrite ^/(.*)([^/])$ http://$host/$1$2/ permanent;
        }
    }
}
```

### 目录合并

搜索引擎优化是一种利用搜索引擎的索引规则来提高网站排名。其中目录也是增强SEO的一种手段。如一个网站的路径如下：

```
/server/12/34/56/78/9.html
```
如果用户访问这个资源，则URL也必须写成http://www.lyblog.net/server/12/34/56/78/9.html；这非常不利于SEO，并且对用户的输入也存在难度。对于以上URL，可以改写成http://www.lyblog.net/server/12-34-56-78-9.html，具体配置如下：

```
server {
    ...
    listen 80;
    server_name www.lyblog.net;
    location ^~ /server {
        ...
        rewrite ^/server-([0-9]+)-([0-9]+)-([0-9]+)-([0-9]+)-([0-9]+)\.html$ /server/$1/$2/$3/$4/$5.html last;
        break;
    }
 
}
```

防盗链

> 盗链是一种损害原始网站合法利益，给原服务器造成额外负担。首先了解一下防盗链的原理。
客户端向服务器请求资源时，为了减少网络带宽，提高响应速度，服务器一般不会一次把所有资源完整的传给客户端。如，请求一个网页，首先传回网页文本文件，当服务器解析网页时，再开始下载资源文件，如图片，样式表，可执行脚本。如果这些资源文件不是放在该服务器上，而在其他服务器上。这就构成了盗链。
要防止盗链，需要了解HTTP协议中的请求头部的Referer头域和采用URL的格式表示访问当前网页或者文件的源地址。通过头域的值，可以检测到访问目标资源的源地址。这样，如果检测到Referer头域中的值并不是自己站点内的URL，就采取阻止措施，实现防盗链的目的。但是，Referer头域的值是可以更改的，因此该方法不能够完全阻止所有盗链。
Nginx配置中有一个指令 valid_referers，用来获取 Referer 头域中的值，并且根据该值的情况给$invalid_referer变量赋值。如果 Referer 头域中没有符合 valid_referers 指令配置的值，则$invalid_referer 变量将会被赋值为1。valid_referers 指令的语法结构为:
>

```
valid_referers none | blocked | server_names | string ...;
```
* none，检测Referer头域不存在的情况
* blocked，检测Referer头域的值被防火墙或代理服务器删除或伪装的情况。这种情况下，该头域的值不以"http://"或者"https://"开头
* server_names，设置一个或多个URL，检测Referer头域的值是否是这些URL中的某个。在Nginx 0.5.33以后支持使用通配符"*"

有了valid_referers指令和$invalid_referer变量，配合Rewrite功能就可以实现防盗链。有两种实现方案：1、根据资源文件类型；2、根据请求的目录
```
server {
    ...
    listen 80;
    server_name www.lyblog.net;
    location ~* ^.+\.(gif|jpg|png|swf|flv|rar|zip)$ {
        ...
        valid_referers none blocked server_names *.lyblog.net;
        if ($invalid_referer){
            rewrite ^/ http://www.lyblog.net/images/default.jpg;
        }
    }
 
}
```
下面是根据目录实现防盗链的配置：
```
server {
    ...
    listen 80;
    server_name www.lyblog.net;
    location /file/ {
        ...
        root /server/file/;
        valid_referers none blocked server_names *.myblog.net;
 
        if($invalid_referer){
            rewrite ^/ http://www.lyblog.net/images/default.jpg;
        }
    }
 
}
```

openssl s_client -connect 127.0.0.1:443 -no_ticket -servername bit.dazhifund.com
测试ssl 中的问题, 
1.环境中是否安装了openssl
2.openssl 版本 ssl_protocols TLSv1 TLSv1.1 TLSv1.2;