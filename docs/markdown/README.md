# Markdown
> 是一款轻量级的标记语言，可以把我们从繁杂的文档写作工作中解脱出来，已经成为世界流行的文本格式。本文将记录我在markdown使用过程中的心得总结，希望能对大家有所帮助，在这之中如果有错误和不足，希望能得到指正。

# 一、标题
#### 说明
`#`表示一级标题，`##`表示二级标题，以此类推,最高可以达到六级标题，**行首生效，记得空格**。

#### 示例
- 代码
```md
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题 
```
-效果
# 一级标题 <!--{docsify-ignore}-->
## 二级标题 <!--{docsify-ignore}--> 
### 三级标题 <!--{docsify-ignore}-->
#### 四级标题 <!--{docsify-ignore}-->
##### 五级标题 <!--{docsify-ignore}--> 
###### 六级标题 <!--{docsify-ignore}-->
- 效果这里结束
---
# 二、引用
#### 说明
 `>`用于引用，`>>`可以嵌套两层引用，以此类推，我们可以继续嵌套下去, 一般我就用一层，为了好看，**行首生效**。
#### 示例
- 代码
```md
> a 
>> b
>>> c
>>>> d
```
-效果
> a 
>> b
>>> c
>>>> d
- 效果这里结束
---

# 三、分割线
#### 说明
 `---` ， `***` **单独一行**，用于表示分割线
#### 示例
- 代码

```md
---
***
```
- 效果
---
***
- 效果这里结束

---
# 四、斜体，粗体，删除线
#### 说明
1. `*斜体内容*`表示*斜体内容* 

2. `**粗体内容**`表示**粗体内容**

3. `~~删除线内容~~`表示~~删除线内容~~

#### 示例
- 代码

```md
*斜体内容*

**粗体内容**

~~删除线内容~~
```
- 效果
*斜体内容*

**粗体内容**

~~删除线内容~~
- 效果这里结束
---
# 五、链接和图片插入
#### 说明
1. `[文字内容](要链接的url)`点击文字内容链接到需要链接的url
2. `![图片内容]( 图片相对地址 或 图片url )` 显示图片
#### 示例
- 代码

```md

[回到首页](/)

[我的github](https://github.com/Hwq1215)

![fivecoins](../imgs/icon.png)     <!--表示相对路径-->

![github-icon](https://github.com/fluidicon.png)
```
- 效果

[回到首页](/)

[我的github](https://github.com/Hwq1215)

![fivecoins](../imgs/icon.png)     <!--表示相对路径-->

![github-icon](https://github.com/fluidicon.png)
- 效果这里结束
---

# 六、表格
#### 说明
1. `|标题一|标题二|标题三|` 写在**第一行**作为标题;

2. `|----|`用于写在**第二行**分割标题和内容 , 默认这个列 , `|:---|`表示这个列左对齐，`|---:|`表示这个列右对齐;

3. `|元素1|元素2|元素3|`填入元素，**每一行都要分行写**

#### 示例
- 代码
```md
|编号|水果种类|售价（元）|
|----|---:|:---|
|1|苹果|5元|
|2|梨子|4元|
|3|葡萄|9元|
```

- 效果
|编号|水果种类|售价（元）|
|----|---:|:---|
|1|苹果|5元|
|2|梨子|4元|
|3|葡萄|9元|
- 效果这里结束

---

# 七、无须列表和有序序列
#### 说明
1. `*`,`-`均可表示无序列表

2. `1.`,`2.`,`···.` 可表示有序列表

3. **均行首生效，需要空格**

#### 示例
- 代码

```md
* 五个
- 硬币
1. 分享
2. 成长
3. 超越
```
> * 效果
* 五个
- 硬币
1. 分享
2. 成长
3. 超越

> * 效果这里结束

---

# 八、代码块
**说明**
1. ` ``` `表示代码块，**行首生效，记得空格**
2. 单个 ` 可以用于行内


