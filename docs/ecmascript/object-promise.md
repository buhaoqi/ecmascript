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