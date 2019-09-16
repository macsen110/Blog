## web 开发实战技巧 ##

一. input 标签上传图片会造成浏览器卡顿,主要原因是 accpet = "image/*" 所致,

当chrome 监听到有图片上传来时会连接到chrome来监听判断, 本质上上因为翻墙所导致的

改成 accept="image/gif,image/png,image/jpeg,image/jpg/,image/bmg"

二. input ios下中文输入, 以及chrome上输入不同点,由非直接输入引起(input 输入中文)

1. 本质上都是监听input事件,
2. chrome 中文输入时compositionend 会在input之后触发, 
3. 安卓下面某些手机input不区分中英文,内容进去后都会触发
>


    var inputElement = document.querySelector('input');
    var inputLock = false;
    function do(inputElement) {
        var regex = /[^1-9a-zA-Z]/g;
        inputElement.value = inputElement.value.replace(regex, '');
    }
    inputElement.addEventListener('compositionstart', function() {
      inputLock = true;
    })
    inputElement.addEventListener('compositionend', function(event) {
      inputLock = false;
      do(event.target);
    })
    
    
    inputElement.addEventListener('input', function(event) {
      if (!inputLock) {
        do(event.target);
        event.preventDefault()
      }
    });


>三. retina屏幕下的 1px问题

1. ios8下面使用 0.5px

2. 使用border-image

3. 使用viewport + rem,类似于阿里flex库(详见yao-m-ui下的flexable)

4. background: -webkit-gradient(linear, left top, left bottom, color-stop(.5, transparent), color-stop(.5, #c8c7cc), to(#c8c7cc)) left bottom repeat-x;

5. 使用伪类, 设置 scale

四. Mutation Observer可以实时监听target dom变化, 有回调函数,场景例如
1.启动页fullpage

五. 检测手机横屏竖屏

六. tips: 

1. HTML5还允许通过正则表达式自定义输入的字符,例如: `<input type="text" pattern="[0-9]*">`
2. aria-hidden="true" 图标的可访问性,能够通过被辅助设备识别,帮助残障人士更好的访问网站。
3. form获取action,默认会带有url全拼

七. web worker 和 service worker
Web Worker 是脱离在主线程之外的，将一些复杂的耗时的活交给它干，
完成后通过 postMessage 方法告诉主线程，而主线程通过 onMessage 方法得到 Web Worker 的结果反馈。
但 Web Worker 是临时的，Service Worker 在 Web Worker 的基础上加上了持久离线缓存能力。

八. function androidInputBugFix(){
        // .container 设置了 overflow 属性, 导致 Android 手机下输入框获取焦点时, 输入法挡住输入框的 bug
        // 相关 issue: https://github.com/weui/weui/issues/15
        // 解决方法:
        // 0. .container 去掉 overflow 属性, 但此 demo 下会引发别的问题
        // 1. 参考 http://stackoverflow.com/questions/23757345/android-does-not-correctly-scroll-on-input-focus-if-not-body-element
        //    Android 手机下, input 或 textarea 元素聚焦时, 主动滚一把
        if (/Android/gi.test(navigator.userAgent)) {
            window.addEventListener('resize', function () {
                if (document.activeElement.tagName == 'INPUT' || document.activeElement.tagName == 'TEXTAREA') {
                    window.setTimeout(function () {
                        document.activeElement.scrollIntoViewIfNeeded();
                    }, 0);
                }
            })
        }
    }

  九. base64, Buffer
  1. Buffer 类是作为 Node.js API 的一部分引入的，用于在 TCP 流、文件系统操作、以及其他上下文中与八位字节流进行交互, Buffer 类的实例类似于从 0 到 255 之间的整数数组,
  Buffer 实例也是 Uint8Array 实例
  2. base64 为字符编码
    2.0  Node.js 当前支持的字符编码有：
    2.1  'ascii' - 仅适用于 7 位 ASCII 数据。此编码速度很快，如果设置则会剥离高位。
    2.2  'utf8' - 多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8。
    2.3  'utf16le' - 2 或 4 个字节，小端序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）。
    2.4  'ucs2' - 'utf16le' 的别名。
    2.5  'base64' - Base64 编码。当从字符串创建 Buffer 时，此编码也会正确地接受 RFC 4648 第 5 节中指定的 “URL 和文件名安全字母”。
    2.6  'latin1' - 一种将 Buffer 编码成单字节编码字符串的方法（由 RFC 1345 中的 IANA 定义，第 63 页，作为 Latin-1 的补充块和 C0/C1 控制码）。
    2.7  'binary' - 'latin1' 的别名。
    2.8  'hex' - 将每个字节编码成两个十六进制的字符。
