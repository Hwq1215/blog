# TypeArray、ArrayBuffer、Blob、File、DataURL、canvas的相互转换
## Blob to ArrayBuffer
```js
var blob = new Blob([data], {type: ‘text/plain‘});
var reader = new FileReader();
reader.onload = function(e) {
    callback(e.target.result);
}; 
reader.readAsArrayBuffer(blob);
```

## ArrayBuffer to Blob
```js
var buffer = new ArrayBuffer(32);
var blob = new Blob([buffer]);       // 注意必须包裹[]
```


## canvas to dataURL
```js
var png = canvas.toDataURL(‘image/png‘);
var jpg = canvas.toDataURL(‘image/jpeg‘, 0.8);
```


## File、Blob对象转换为dataURL
- `File`对象也是一个`Blob`对象，二者的处理相同。
```js
function readBlobAsDataURL(blob, callback) {
    var a = new FileReader();
    a.onload = function(e) {callback(e.target.result);};
    a.readAsDataURL(blob);
}
//example:
readBlobAsDataURL(blob, function (dataurl){
    console.log(dataurl);
});
readBlobAsDataURL(file, function (dataurl){
    console.log(dataurl);
});
```
## dataURL转换为Blob对象
```js
function dataURLtoBlob(dataurl) {
    var arr = dataurl.split(‘,‘), mime = arr[0].match(/:(.*?);/)[1],
    bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
    while(n--){
        u8arr[n] = bstr.charCodeAt(n);
    }
    return new Blob([u8arr], {type:mime});
}
//test:
var blob = dataURLtoBlob(‘data:text/plain;base64,YWFhYWFhYQ==‘);
```
## dataURL图片数据绘制到canvas
- 先构造`Image`对象，src为`dataURL`，图片`onload`之后绘制到`canvas`
```js
var img = new Image();
img.onload = function(){
    canvas.drawImage(img);
};
img.src = dataurl;
```

## File,Blob的图片文件数据绘制到canvas
- 还是先转换成一个url，然后构造Image对象，src为`dataURL`，图片`onload`之后绘制到`canvas`
- 利用上面的 `readBlobAsDataURL()` 函数，由`File`,`Blob`对象得到`dataURL`格式的url，再参考 `dataURL`图片数据绘制到`canvas`
```js
readBlobAsDataURL(file, function (dataurl){
    var img = new Image();
    img.onload = function(){
        canvas.drawImage(img);
    };
    img.src = dataurl;
});
```

## ArrayBuffer转Blob
```js
var buffer = new ArrayBuffer(32);
var blob = new Blob([buffer]);       // 注意必须包裹[]
```

## 将Blob对象转换成String字符串，使用FileReader的readAsText方法
```js
//将字符串转换成 Blob对象
var blob = new Blob(['中文字符串'], {
    type: 'text/plain'
});
//将Blob 对象转换成字符串
var reader = new FileReader();
reader.readAsText(blob, 'utf-8');
reader.onload = function (e) {
    console.info(reader.result);
}
```

## 将Blob对象转换成ArrayBuffer，使用FileReader的 readAsArrayBuffer方法
```js
//将字符串转换成 Blob对象
var blob = new Blob(['中文字符串'], {
    type: 'text/plain'
});
//将Blob 对象转换成 ArrayBuffer
var reader = new FileReader();
reader.readAsArrayBuffer(blob);
reader.onload = function (e) {
    console.info(reader.result); //ArrayBuffer {}
    //经常会遇到的异常 Uncaught RangeError: byte length of Int16Array should be a multiple of 2
    //var buf = new int16array(reader.result);
    //console.info(buf);

    //将 ArrayBufferView  转换成Blob
    var buf = new Uint8Array(reader.result);
    console.info(buf); //[228, 184, 173, 230, 150, 135, 229, 173, 151, 231, 172, 166, 228, 184, 178]
    reader.readAsText(new Blob([buf]), 'utf-8');
    reader.onload = function () {
        console.info(reader.result); //中文字符串
    };

    //将 ArrayBufferView  转换成Blob
    var buf = new DataView(reader.result);
    console.info(buf); //DataView {}
    reader.readAsText(new Blob([buf]), 'utf-8');
    reader.onload = function () {
        console.info(reader.result); //中文字符串
    };
}
```