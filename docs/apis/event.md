



## UiEvent

### load
- 定义：文档全部内容完全加载后触发 load 事件；
- 文档全部内容包括：文字、文档依赖的外部资源，如：样式表、图像、视频等。
- 语法格式

```html
<element onload="myScript">
<script>
object.onload = function(){myScript};
</script>

<script>
object.addEventListener("load", myScript);
</script>

```

### DOMContentLoaded
- 定义：DOMContentLoaded在页面 DOM 加载后立即触发，无需等待资源完成加载。
- 区别：window load事件应该只用于检测一个完全加载的页面。在更多时候，推荐使用DOMContentLoaded event。

```html
<h1>load event</h1>
<p><img src="https://source.unsplash.com/random/1"></p>
<p><img src="https://source.unsplash.com/random/2"></p>
<p><img src="https://source.unsplash.com/random/3"></p>
<p><img src="https://source.unsplash.com/random/4"></p>
</p><img src="https://source.unsplash.com/random/5"></p>
<p><img src="https://source.unsplash.com/random/6"></p>
<p><img src="https://source.unsplash.com/random/7"></p>
<p><img src="https://source.unsplash.com/random/8"></p>
<p><img src="https://source.unsplash.com/random/9"></p>
<p><img src="https://source.unsplash.com/random/10"></p>
<p><img src="https://source.unsplash.com/random/11"></p>
<script>
    document.addEventListener('DOMContentLoaded',function(){
        alert('DOM已加载完毕')
    })
    window.addEventListener('load',function(){
        alert('图片、css文件已加载完毕')
    })
</script>
```


### scroll
- scroll: 当元素的滚动条被滚动时触发。
- 语法格式

```html
<element onscroll="myScript">
<script>
object.onscroll = function(){myScript};
</script>

<script>
object.addEventListener("scroll", myScript);
</script>

```

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

### resize
- resize: 当浏览器窗口尺寸改变时触发。
