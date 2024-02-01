# css 动画

## 1. transition
我们可以为类名添加transition属性，来实现动画效果，在类名添加或者删除时，会有一个过渡效果。
```css
    .inside{
        width: 10vh;
        height: 4.2vh;
        border: 0.4vh solid rgb(67, 65, 65);
        align-content: center;
        background-color: white;
        /*添加transition属性，实现过渡效果*/
        transition:
            background-color 0.5s;
    }
    /*实现一个inside的active类，当添加active类时，会有一个过渡效果*/
    .inside.active{
        background-color: #95d602;
    }
```
## 2. js实现动画
使用js进行类名的添加和删除
```js
    const container = document.querySelector(".inside");
    container.classList.add("active");
    container.classList.remove("active");
```
## 3.一个丝滑的开关
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="dyrmZoM" data-user="hwq1215" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/hwq1215/pen/dyrmZoM">
  light Toggle</a> by hwq (<a href="https://codepen.io/hwq1215">@hwq1215</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>


