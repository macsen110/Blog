## web开发中碰到的一些东西 ##

一. input 标签上传图片会造成浏览器卡顿,主要原因是 accpet = "image/*" 所致,

当chrome 监听到有图片上传来时会连接到chrome来监听判断, 本质上上因为翻墙所导致的

改成 accept="image/gif,image/png,image/jpeg,image/jpg/,image/bmg"
___