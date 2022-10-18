# 实现一个个性化的站内搜索
> 个性化，意味着少用定制化的api，如百度，必应，Google的搜索引擎直接指向站内搜索的方法我是放弃的。

# 首先下载lunr.js
> 官网[https://lunrjs.com](https://lunrjs.com) 
#### 但是官网不支持中文的匹配搜索
> 我们采用国内老哥修改的[https://github.com/codepiano/lunr.js](https://github.com/codepiano/lunr.js)

## 基础用法
- 引入一个json数组，这个需要我们提前编写
```js
var documents = [{
  "name": "Lunr",
  "text": "Like Solr, but much smaller, and not as bright."
}, {
  "name": "React",
  "text": "A JavaScript library for building user interfaces."
}, {
  "name": "Lodash",
  "text": "A modern JavaScript utility library delivering modularity, performance & extras."
}]
```

- 将document元素放入
```js
var idx = lunr(function () {
  this.ref('name')
  this.field('text')

  documents.forEach(function (doc) {
    this.add(doc)
  }, this)
})
```

- 利用lunr对象的search方法获得搜索对象数组
```js
idx.search("bright")
```

  


