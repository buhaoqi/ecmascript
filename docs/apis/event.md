## DOM事件基础
事件是什么
事件类型
事件目标
事件处理程序
事件对象
事件传播
## DOM事件深入
设置JS对象属性为事件处理程序

设置HTML标签属性为事件处理程序
## load
加载整个页面时触发 load 事件，包括所有依赖资源，例如样式表和图像。

```html
<script>
window.addEventListener('load', (event) => {
  console.log('page is fully loaded');
});
<script>
```

```html
<script>
window.onload = (event) => {
  console.log('page is fully loaded');
};
<script>
```


## DOMContentLoaded
DOMContentLoaded在页面 DOM 加载后立即触发，无需等待资源完成加载。

## scroll

```html

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>瀑布流</title>
    <style>
        .container {
            position: relative;
            width: 500px;
            margin: 0 auto;
        }

        .wf-item {
            position: absolute;
        }

        .wf-item .wf-img {
            width: 100%;
            height: 100%;
        }
    </style>
</head>

<body>
    <div class="container"></div>
    <script>
            const url = `https://api.unsplash.com/photos/random?count=20&client_id=LMC4yPkAL3R5HyXt52SB-JlOx0rTZj0s3DM1kW6Uw-0`
            const oContainer = document.querySelector('.container')
            const column = 3
            const gap = 10
            let heightArr = [0, 0, 0]
            let on = true
            //fetch data
            function fetchData(api, callback) {
                const xhr = new XMLHttpRequest()
                let data = null
                xhr.open('GET', api)
                xhr.onload = function () {
                    if (this.status == 200) {
                        data = JSON.parse(this.responseText)
                        console.log(data)
                        callback(data)
                    }
                }
                xhr.send()
            }
            fetchData(url, loadImages)
            //load data
            function loadImages(data) {
                for (let i = 0; i < data.length; i++) {
                    let img = document.createElement('img')
                    img.className = 'wf-img lazy'
                    img.src = data[i].urls.regular
                    let div = document.createElement('div')
                    div.className = 'wf-item'
                    div.style.width = (oContainer.offsetWidth - gap * (column - 1)) / column + 'px'
                    div.style.height = (parseFloat(div.style.width) / data[i].width) * data[i].height + 'px'
                    minHeightColumn = getMinHeightColumn(heightArr)
                    div.style.top = heightArr[minHeightColumn] + 'px'
                    div.style.left = minHeightColumn * (parseFloat(div.style.width) + gap) + 'px'
                    heightArr[minHeightColumn] += parseInt(div.style.height)
                    div.appendChild(img)
                    oContainer.appendChild(div)
                }
                on = true
            }

            function getMinHeightColumn(arr) {
                return arr.indexOf(Math.min.apply(null, arr))
            }
            window.onscroll = function () {
                if (document.documentElement.scrollHeight - Math.abs(document.documentElement.scrollTop) <= document.documentElement.clientHeight
                ) {
                    if (on) {
                        on = false
                        console.log('可以加载了')
                        fetchData(url, loadImages)
                    }
                }
            }
    </script>

</body>

</html>
```