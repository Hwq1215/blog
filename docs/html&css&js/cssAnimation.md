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
## 3.开关
完整的html代码如下
```html preview
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>开灯控件</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh; /* 设置 body 高度为视口高度，以确保垂直居中 */
            margin: 0;
        }   
        .main-container{
            text-align: center; 
        }
        .inside{
            width: 10vh;
            height: 4.2vh;
            border: 0.4vh solid rgb(67, 65, 65);
            border-radius: 2.5vh;
            display: flex;
            justify-content: space-between;
            align-items: center;
            z-index: 0;
            align-content: center;
            background-color: white;
            transition:
                background-color 0.5s;
        }
        .inside.active{
            background-color: #95d602;
        }
        .circle{
            margin-left: 0.2vh;
            width: 3.0vh;
            height: 3.0vh;
            border: 0.5vh solid rgb(67, 65, 65);
            border-radius: 3vh;
            background-color: #b4b4b4;
            transition:
                background-color 0.5s,
                margin-left 0.3s;
        }

        .circle.active{
            background-color: #e1ffc3;
            margin-left: 6vh;
        }

    </style>
</head>
<body>
    <div class="main-container">
        <div class="inside">
            <div class="circle">

            </div>
        </div>
    </div>
    <script>
        const container = document.querySelector(".inside");
        const circle = document.querySelector(".circle");
        
        //
        function toggleChange(){
            container.classList.toggle('active');
            circle.classList.toggle("active");
        }

        container.addEventListener("mousedown",()=>{
            toggleChange();
        })
    </script>
    
</body>
</html>
```