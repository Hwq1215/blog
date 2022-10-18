# jquery简介
> jQuery 库可以通过一行简单的标记被添加到网页中。

## jquery特点
jQuery 是一个 JavaScript 函数库。
jQuery 是一个轻量级的"写的少，做的多"的 JavaScript 库。
jQuery 库包含以下功能：
- HTML 元素选取
- HTML 元素操作
- CSS 操作
- HTML 事件函数
- JavaScript 特效和动画
- HTML DOM 遍历和修改
- AJAX

## 访问元素
#### js访问dom方法
```js
var keyName = document.getElementById("keyName-inp").value() //value()可以被替换成text()等标签内部存在的属性
```
#### jquery的方法
```js
var keyName = $("#keyName-inp").value();
```

## ajax
> 常见的使用[https://www.cnblogs.com/wtmb/p/14351648.html]

#### 参数介绍
- url 
    请求的url
- type
    请求的类型
- contentType
    请求数据的类型
- data
    请求的数据
- success：
    请求成功后的操作