## JS是什么
JavaScript是一门语言。
- single-threaded 单线程
- non-blocking 非阻塞
- asynchronous 异步
- concurrent 并存、同时发生

JavaScript拥有
- call stack
- event loop
- callback queue
- APIs
- 等等

## v8

- call stack
- heap
- no DOM，No setTimeout, No XMLHttpRequest

## Call Stack

- one thread == one call stack == on thing at a time

## 同步的JavaScript
js在同一时间只能运行一条语句。

```html
<script>
    console.log(1)
    console.log(2)
    console.log(3)
    console.log(4)
</script>
```
## JavaScript处理异步的机制
```html
<script>
    console.log(1)
    console.log(2)
    setTimeout(function(){
        console.log('我这个函数被调用了，我是一个回调')
    },0)
    console.log(3)
    console.log(4)
</script>
```

## HTTP Request

- `readyState` 属性存留 XMLHttpRequest 的生命周期状态。
    - 0: 请求未初始化
    - 1: 服务器连接已建立
    - 2: 请求已接收
    - 3: 正在处理请求
    - 4: 请求已完成且响应已就绪
- `onreadystatechange` 属性定义当 readyState 发生变化时执行的函数。
- `status` 属性和 `statusText` 属性存有 XMLHttpRequest 对象的结果状态。
    - 200: "OK"
    - 403: "Forbidden"
    - 404: "Page not found"

```html
<script>
    const request = new XMLHttpRequest()
    request.addEventListener('readystatechange',() =>{
        console.log(request,request.readyState)
        if(request.readyState == 4){
            console.log(request.responseText)
        }
    })
    request.open('GET','https://jsonplaceholder.typicode.com/todos')
    request.send()
</script>
```

>A readystatechange occurs several times in the life of a request as it progresses to different phases, but a load event only occurs when the request has successfully completed.If you're not interested in detecting intermediate states or errors, then onload might be a good choice.


## status code


```html
<script>
    const request = new XMLHttpRequest()
    request.addEventListener('readystatechange',() =>{
        //console.log(request,request.readyState)
        if(request.readyState == 4 && request.status == 200){//not enough
            console.log(request,request.responseText)
        } else if(request.readyState == 4){
            console.log('请求数据失败')
        }
    })
    request.open('GET','https://jsonplaceholder.typicode.com/todoss')//error
    request.send()
</script>
```
## callback function

如果想复用，我们可以封装一个函数：

```html
<script>
const getData = (callback) => { //1封装：为复用 3.传入回调函数
    const request = new XMLHttpRequest()
    request.addEventListener('readystatechange', () => {
        //console.log(request,request.readyState)
        if (request.readyState == 4 && request.status == 200) {//not enough
            //console.log(request, request.responseText) //2.只是打印，如果传入一个回调函数处理会更好
            callback(undefined,JSON.parse(request.responseText)) // 4. 添加回调函数  7.传入实参
        } else if (request.readyState == 4) {
            //console.log('请求数据失败')
            callback('请求数据失败',undefined) //4. 添加回调函数 5. 无法处理成功失败。7.传入实参
        }
    })
    request.open('GET', 'https://jsonplaceholder.typicode.com/todos')//error
    request.send()
}

console.log(1)
console.log(2)
getData( (err,data) => { //6. 处理成功失败：传入两个参数
    console.log('我是回调函数') //1. 封装
    //console.log(err,data) //
    if(err){ //8. 针对成功和失败做点事情
        console.log(err)
    } else {
        console.log(data)
    }
} )
console.log(3)
console.log(4)
</script>

```

## Using JSON Data

创建自己的json文件：1.json
```json
[
    {"name": "zhangsan", "age": 18},
    {"name": "xiaoming", "age": 28},
    {"name": "xiaohong", "age": 38}
]

```



```html
<script>
const getData = (callback) => { //1封装：为复用 3.传入回调函数
    const request = new XMLHttpRequest()
    request.addEventListener('readystatechange', () => {
        //console.log(request,request.readyState)
        if (request.readyState == 4 && request.status == 200) {//not enough
            //console.log(request, request.responseText) //2.只是打印，如果传入一个回调函数处理会更好
            callback(undefined,JSON.parse(request.responseText)) // 4. 添加回调函数  7.传入实参
        } else if (request.readyState == 4) {
            //console.log('请求数据失败')
            callback('请求数据失败',undefined) //4. 添加回调函数 5. 无法处理成功失败。7.传入实参
        }
    })
    //request.open('GET', 'https://jsonplaceholder.typicode.com/todos')//error
    request.open('GET', 'data/1.json') //10--自定义json
    request.send()
}

console.log(1) //999999999
console.log(2)
getData( (err,data) => { //6. 处理成功失败：传入两个参数
    console.log('我是回调函数') //1. 封装
    //console.log(err,data) //
    if(err){ //8. 针对成功和失败做点事情
        console.log(err)
    } else {
        console.log(data)
    }
} )
console.log(3)
console.log(4)
</script>
```

## Callback Hell

```html
<script>
const getData = (resource, callback) => { //1.传入资源
    const request = new XMLHttpRequest()
    request.addEventListener('readystatechange', () => {
        if (request.readyState == 4 && request.status == 200) {
            callback(undefined, JSON.parse(request.responseText))
        } else if (request.readyState == 4) {
            callback('请求数据失败', undefined)
        }
    })
    request.open('GET', resource) //2.替换参数
    request.send()
}

getData('data/1.json', (err, data) => { //3. 添加实参
    console.log(data)
    getData('data/2.json', (err, data) => { //3. 添加实参
        console.log(data)
        getData('data/3.json', (err, data) => { //3. 添加实参
            console.log(data)
        })
    })
})
</script>
```

## promise

### Promise是什么

- Promise是一个构造函数返回的对象。
- Promise对象存储着一个异步操作的悬而未决的结果。
- Promise对象可以通过`then()`方法绑定处理Promise结果的回调函数。（不需要把回调函数当做参数传入函数）
- 在Promise的结果未确定之前，`then()`方法不会被调用。


### then()的用法

- 定义：`promise.then()` 方法：用于绑定处理Promise结果的回调函数。
- 参数：promise.then(successCallback,failureCallback)
- 返回值：promise

我们可以把回调绑定到返回的 Promise 上，形成一个 Promise 链：

```javascript
doSomething().then(function(result) {
  return doSomethingElse(result);
})
.then(function(newResult) {
  return doThirdThing(newResult);
})
.then(function(finalResult) {
  console.log('Got the final result: ' + finalResult);
})
.catch(failureCallback);
```




示例：问题：
```html
<script>
let a = 1
setTimeout(function(){
    a = 10
},2000)
console.log(a)
</script>
```

示例：promise处理器函数的参数
```html
<script>
    const p1 = new Promise((resolve,reject) => { //参数：处理器函数
        // 做一些异步操作
        setTimeout(function(){
            a = 10
            // resolve() //手动设置 修改状态
            reject() //失败状态
        },2000)
    })
    //////////////promise.then()方法
    p1.then(() => { //then()方法：设置回调函数 继续执行的任务。Promise.then() 有两个参数，一个是成功时的回调，另一个是失败时的回调。两者都是可选的，因此您可以为成功或失败添加回调。
        console.log(a)
    }, () => {
        console.log('失败了')
    })
</script>

```

## AJAX & Promise

```html
<script>
const getData = (resource) => { //1.传入资源
    return new Promise((resolve,reject) => {
        const request = new XMLHttpRequest()
        request.addEventListener('readystatechange', () => {
            if (request.readyState == 4 && request.status == 200) {
                const data = JSON.parse(request.responseText)
                resolve(data)
            } else if (request.readyState == 4) {
                reject('请求数据失败')
            }
        })
        request.open('GET', resource) //2.替换参数
        request.send()
    })
}
getData('data/1.json').then(data => {
    console.log(data)
},(err) => {
    console.log('promise 请求失败')
})

getData('data/2.json').then(data => {
    console.log(data)
},(err) => {
    console.log('promise 请求失败')
})

getData('data/3.json').then(data => {
    console.log(data)
},(err) => {
    console.log('promise 请求失败')
})

</script>

```

##  promise.catch()

`catch()` 方法返回一个Promise，并且处理拒绝的情况。

- 它的行为与调用`Promise.prototype.then(undefined, onRejected) `相同。 
- 事实上, `calling obj.catch(onRejected)` 内部 `calls obj.then(undefined, onRejected))`.

## Chainning Promise

```html
<script>
const getData = (resource) => { //1.传入资源
    return new Promise((resolve,reject) => {
        const request = new XMLHttpRequest()
        request.addEventListener('readystatechange', () => {
            if (request.readyState == 4 && request.status == 200) {
                const data = JSON.parse(request.responseText)
                resolve(data)
            } else if (request.readyState == 4) {
                reject('请求数据失败')
            }
        })
        request.open('GET', resource) //2.替换参数
        request.send()
    })
}
 getData('data/1.json').then(data => {
    return getData('data/2.json')
}).then( data => {
    console.log(data)
    return getData('data/3.json')
}).then( data => {
    console.log(data)
}).catch((err) => { //一遇到异常抛出，浏览器就会顺着 Promise 链寻找下一个 onRejected 失败回调函数或者由 .catch() 指定的回调函数
    console.log(err)
})

// getData('data/2.json').then(data => {
//     console.log(data)
// },(err) => {
//     console.log('promise 请求失败')
// })

// getData('data/3.json').then(data => {
//     console.log(data)
// },(err) => {
//     console.log('promise 请求失败')
// })
</script>

```

## fetch API
Fetch API 提供了一个 JavaScript 接口，用于访问和操纵 HTTP 管道的一些具体部分，例如请求和响应。它还提供了一个全局 fetch() 方法，该方法提供了一种简单，合理的方式来跨网络异步获取资源。

```html
<script>
fetch('data/1.json')//fetch()必须接受一个参数——资源的路径。无论请求成功与否，它都返回一个 Promise 对象
.then((response)=>{ //resolve 对应请求的 Response。response对象:请求的响应对象。当接收到一个代表错误的 HTTP 状态码时，从 fetch() 返回的 Promise 不会被标记为 reject，仅当网络故障时或请求被阻止时，才会标记为 reject。
    if(response.status !== 200){//如果响应的 HTTP 状态码在 200 - 299 的范围内，则resolve 返回值的 ok 属性为 true
        console.log('错误代码:' + response.status)
        return
    } 
    response.json().then( data => {
        console.log(data)
    })//
})
.catch((error)=>{ //
    console.log(error)
})
</script>
```

## Async & Await 

### Async是什么

示例1：基本用途-改成异步函数

```html
<script>
async function fn() {
    let a = 1
        setTimeout(() => {
            a = 100
        }, 2000)
    return a
}
console.log(fn())
</script>
```

### Await是什么

- 是一个关键字
- 使用方法：放在任何异步的，基于 promise 的函数之前
- 作用：它会暂停代码在该行上，直到 promise 完成，然后返回结果值。

示例2：基本用途-拿值
```html
<script>
async function fn() {
    let a = 1
    let result = await new Promise((resolve, reject) => {
        setTimeout(() => {
            a = 100
            resolve(a)
        }, 200)
    })
    console.log(result)//若没有await,拿不到100
}
fn()
</script>
```
示例3：fetch
```html
<script>
const getData = async () => {
    const response = await fetch('data/1.json')
    const data = await response.json()
    // console.log(response)
    // console.log(data)
    return data
}
const result = getData()
// console.log(result)
getData().then( data => console.log(data))
</script>
```

项目： 天气app
index.html
```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container my-5 mx-auto">
        <h1 class="text-muted text-center my-4">天气APP</h1>  
        <form class="my-4 text-center text-muted">
            <label for="city">输入要查询的城市名称</label>
            <input type="text" name="city" class="form-control p-2">
        </form> 
        
        <div class="card shadow-lg rounded">
            <img src="https://via.placeholder.com/400x300" class="time card-img-top">
            <div class="icon bg-light mx-auto text-center">
              <!-- icon here later -->
            </div>
            <div class="text-muted text-uppercase text-center details">
              <h5 class="my-3">城市</h5>
              <div class="my-3">天气</div>
              <div class="display-4 my-4">
                <span>temp</span>
                <span>&deg;C</span>
              </div>
            </div>
          </div>
    </div>

    <script src="scripts/weather.js"></script>
    <script src="scripts/app.js"></script>
    
</body>
</html>
```

weather.js
```javascript
// interacting with weather api
const key = 't3ADIAEBpaD3YwI2oRpPjVbA7RvOOFZj'

//get weather information
const getWeather = async (id) => {
    const base = 'http://dataservice.accuweather.com/currentconditions/v1/'
    const query = `${id}?apikey=${key}`
    const reponse = await fetch(base + query)
    const data = await reponse.json()
    // console.log(data)
    return data[0]
}

//get city information
const getCity = async (city) => {
    const base = 'http://dataservice.accuweather.com/locations/v1/cities/search'
    const query = `?apikey=${key}&q=${city}`
    const reponse = await fetch(base + query)
    const data = await reponse.json()
    // console.log(data[0])
    return data[0]

} 

getCity('shanghai') //第一版
    .then( data => console.log(data))
    .catch( error => console.log(error))

// getCity('shanghai')//第二版
//     .then( data => {
//         return getWeather(data.key)
//     }).then( data => {
//         console.log(data)
//     }).catch( error => console.log(error))

getWeather('106577')
```