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