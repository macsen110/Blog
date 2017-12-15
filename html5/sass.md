# sass笔记
1. 转义变量为字符串 #{}

```
$name: foo;
$attr: border;
p.#{$name} {
  #{$attr}-color: blue;
}
```
transforms

```
p.foo {
  border-color: blue; }
```

2. 变量默认值： !default
你可以在变量尚未赋值前，通过在值的末尾处添加 !default 标记来为其指定。 也就是说，如果该变量已经被赋值， 就不会再次赋值， 但是，如果还没有被赋值，就会被指定一个值。null会被当做没有值

3. 
