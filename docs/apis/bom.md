

# BOM
客户端JavaScript的存在使得静态的HTML文档编程了交互式的web应用。脚本化HTML页面内容是JavaScript的核心目标。

## BOM是什么
- BOM是Browser Object Model的缩写，翻译为“浏览器对象模型”。
- 浏览器对象模型为浏览器提供了运行JavaScript的上下文。
- BOM允许JS和浏览器对话。ECMAScript的一种扩展、与浏览器交互的一种方式方法。
- BOM把浏览器看成是一个对象模型。在BOM眼中：
    - window: 窗口是就window对象
    - screen: 显示窗口的屏幕就是screen对象
    - location: 窗口的地址栏就是location对象
    - histroy: 窗口的历史记录就是history对象
    - navigation: 窗口的导航面板就是navigation对象
    - document: 窗口中的文档就是document对象
- BOM尚未被标准化。


![BOM](images/bom.png)

### BOM和JavaScript的关系
请思考如何在web浏览器中呈现web页面？

- 文档：就是呈现静态信息的页面；
- 应用：加入JavaScrpt的文档就是应用。因为客户端JavaScript的存在使得静态的HTML文档编程了交互式的web应用。

### BOM和DOM对比

|noname|BOM|DOM|
|-|-|-|
|含义|浏览器对象模型|文档对象模型|
|对象|把文档内容看成对象|把浏览器看成对象|
|核心|核心对象是Window|核心对象是Document|
|用途|与窗口交互|与文档交互|
|标准化|未标准化|W3C|

## window对象
window对象表示浏览器的一个窗口，它有双重角色：

- window对象是JavaScript访问浏览器窗口的一个接口。
    - window.document
    - window.location
    - window.history
    - window.navigation
    - window.screen
- window对象也是全局对象。它处于作用域链的顶部，定义在全局作用域中的变量和函数都会自动成为window对象的属性和方法。

示例1：遍历window对象
```html
<body>
    <ol></ol>
    <script>
        let str = ''
        let oOl = document.querySelector('ol')
        for(attr in window){
            str += `<li>${attr}: ${window[attr]}</li>`
        }
        oOl.innerHTML = str //FF: 225个属性  Chrom:221个属性
    </script>
</body>

```
示例2：全局作用域下的变量和函数自动成为window对象的属性和方法
```javascript
var n1 = 10
let n2 = 20
const n3 = 30
function fn() {
    console.log('hello world')
}
console.log(window.n1) // 10
console.log(window.n2) // undefined
console.log(window.n3)  // undefined
```

### 关于窗口
- 一个浏览器窗口可能包含多个标签页。
- 每个标签页都是独立的“浏览上下文”，都有独立的window对象。和其他窗口没有任何联系。
- 由于同源策略的限制，导致窗口之间无法直接交互

### 三种弹框
- `Window.alert()` - 弹出警示框(一条消息+按钮)
- `Window.confirm(['确认信息'])` - 弹出确认框。返回值: true/false
- `Window.prompt('提示信息',['默认输入文本'])` - 显示提示对话框。返回值：用户输入的字符串

示例3：记住三种方法的含义、参数、返回值
```html
<button>警示框</button> <button>确认框</button> <button>提示对话框</button>
    <p class="box"></p>
    <script>
        const aBtn = document.querySelectorAll('button')
        const oBox = document.querySelector('.box')
        aBtn[0].onclick = function(){
            alert('夜深了，快睡觉吧！')
        }
        aBtn[1].onclick = function(){
            const result = confirm('确认删除吗?')
            if(result){
                oBox.innerHTML = '文件已经删除'
            } else {
                oBox.innerHTML = '文件未删除'
            }
        }
        aBtn[2].onclick = function(){
            const result = prompt('请输入大名','在这里输入')
            if(result != null){
                oBox.innerHTML = `你好！${result}`
            } 
        }
    </script>
```
### 查询视口尺寸
- `window.innerHeight`: 返回窗口的内部/视口高度，包括水平滚动条的高度。
- `window.innerWidth`: 返回窗口的内部/视口宽度，包括水平滚动条的高度。


- 视口尺寸(无单位) = 可视内容尺寸 + 内边距
- Element.clientWidth: 返回元素的可视内容宽度
- Element.clientHeight: 返回元素的可视宽度
- document.documentElement.clientWidth
- document.documentElement.clientHeight

### 查询文档滚动距离
- window.pageYOffset: 判断滚动条的滚动距离（IE8不能用）
- window.pageXOffset: 判断滚动条的滚动距离（IE8不能用）
- `Window.scrollX` - Read only. Returns the number of pixels that the document has already been scrolled horizontally.
- `Window.scrollY` - Read only. Returns the number of pixels that the document has already been scrolled vertically.

### 获取当前窗口
- `Window.window` - Read only. Returns a reference to the current window.
### 查询/设置窗口名字
- `Window.name` - Gets/sets the name of the window.
### 打开一个窗口
- `window.open()` - open a new window
### 关闭一个窗口
- `window.close()` - close the current window
### 查询窗口是否全屏
- `Window.fullScreen` - This property indicates whether the window is displayed in full screen or not.

### 打开控制台
- `Window.console` - Read only. Returns a reference to the console object which provides access to the browser's debugging console.
### 查询像素比
- `Window.devicePixelRatio` - Read only. Returns the ratio between physical pixels and device independent pixels in the current display.
### 获取本地存储对象
- `Window.localStorage` - Read only. Returns a reference to the local storage object used to store data that may only be accessed by the origin that created it.
### 获取当前事件对象
Window.event  Read only
Returns the current event, which is the event currently being handled by the JavaScript code's context, or undefined if no event is currently being handled. The Event object passed directly to event handlers should be used instead whenever possible.



### 设置窗口聚焦
Window.focus()
Sets focus on the current window.
### 设置窗口blur
Window.blur()
Sets focus away from the window.

### 设置文档滚动位置
- Window.scroll()
Scrolls the window to a particular place in the document.
- Window.scrollTo()
Scrolls to a particular set of coordinates in the document.

### 查询计算样式
Window.getComputedStyle()
Gets computed style for the specified element. Computed style indicates the computed values of all CSS properties of the element.

### 动画
- Window.requestAnimationFrame()
Tells the browser that an animation is in progress, requesting that the browser schedule a repaint of the window for the next animation frame.
- Window.cancelAnimationFrame() 
Enables you to cancel a callback previously scheduled with Window.requestAnimationFrame.

## location对象
- location接口表示当前窗口相关的URL地址。
- window.loaction:查询或设置当前window对象相关的location接口
- document.location: 查询或设置当前document对象相关的location接口

### 关于URL
- `https://example.org:8080/foo/bar?q=baz#bang`
- `url`: 统一资源定位符
- `https:`: 协议
- `example.org:8080`: 主机
- `example.org`: 主机名
- `8080`: 端口号
- `/foo/bar`: 路径
- `?q=baz`: 查询
- `#bang`: hash

### 查询url地址
-  `Location.href`: 返回与文档相关联的url地址(字符串类型）。

### 查询协议
- `Location.protocol`: 是一个包含 URL 协议方案的 USVString，包括最后的 ':'

### 查询主机
- `Location.host`:是一个包含主机的 USVString，即主机名、':' 和 URL 的端口

### 查询主机名
- `Location.hostname` - 是一个包含 URL 域的 USVString
### 查询端口号
- `Location.port` - 是一个包含 URL 端口号的 USVString
### 查询路径
- `Location.pathname` - 一个包含初始 '/' 后跟 URL 路径的 USVString，不包括查询字符串或片段
### 查询查询字符串
- `Location.search` - USVString 是否包含“？”后跟 URL 的参数或“查询字符串”。现代浏览器提供 URLSearchParams 和 URL.searchParams 以便于从查询字符串中解析出参数
### 查询hash
- `Location.hash`- 是一个包含“#”的 USVString，后跟 URL 的片段标识符
### 替换URL
- `Location.assign()`- 根据参数中提供的 URL 加载资源
### 重载URL
- `Location.reload()`- 重新加载当前 URL，如刷新按钮
### 替换URL
- `Location.replace()` - 用提供的 URL 替换当前资源（重定向到提供的 URL）。与 assign() 方法和设置 href 属性的区别在于，使用 replace() 后当前页面不会保存在 session History 中

## document对象
- `Window.document` - Read only. Returns a reference to the document that the window contains.

## history对象
`history`接口允许操作浏览器的的历史记录。
### 获取历史记录对象
- `window.history` - Returns a reference to the history object.
### 查询历史记录对象的长度
- `history.length` -  Read only. 返回一个表示会话历史中元素数量的整数，包括当前加载的页面。例如，对于在新选项卡中加载的页面，此属性返回 1
### 后退一页
- `history.back()` - 此异步方法转到会话历史记录中的上一页，与用户单击浏览器的后退按钮时的操作相同。等价于 history.go(-1)
### 前进一页
- `history.forward()` - 这个异步方法转到会话历史中的下一页，与用户单击浏览器的前进按钮时的操作相同；这相当于 history.go(1)
### 跳转到指定页面
- `history.go()` - 从会话历史记录中异步加载页面，由其与当前页面的相对位置标识，例如 -1 表示上一页或 1 表示下一页。如果您指定一个超出范围的值(例如，当会话历史记录中没有以前访问过的页面时指定 -1)这种方法静默没有效果。调用不带参数或值为 0 的 go() 会重新加载当前页面。 Internet Explorer 允许您指定字符串而不是整数，以转到历史列表中的特定 URL。


## navigator对象
`Navigator`接口表示用户代理的状态和身份。它允许脚本查询它并注册自己以进行某些活动。可以使用只读的 window.navigator 属性检索 Navigator 对象。

- `Navigator.connection` Read only 
Provides a NetworkInformation object containing information about the network connection of a device.

`Navigator.cookieEnabled` Read only
Returns false if setting a cookie will be ignored and true otherwise.

`Navigator.credentials` Read only
Returns the CredentialsContainer interface which exposes methods to request credentials and notify the user agent when interesting events occur such as successful sign in or sign out.

`Navigator.deviceMemory` Read only 
Returns the amount of device memory in gigabytes. This value is an approximation given by rounding to the nearest power of 2 and dividing that number by 1024.

`Navigator.geolocation` Read only
Returns a Geolocation object allowing accessing the location of the device.

`Navigator.hid` Read only
Returns an HID object providing methods for connecting to HID devices, listing attached HID devices, and event handlers for connected HID devices.

`Navigator.hardwareConcurrency` Read only
Returns the number of logical processor cores available.

`Navigator.keyboard` Read only 
Returns a Keyboard object which provides access to functions that retrieve keyboard layout maps and toggle capturing of key presses from the physical keyboard.

`Navigator.language` Read only
Returns a DOMString representing the preferred language of the user, usually the language of the browser UI. The null value is returned when this is unknown.

`Navigator.languages` Read only 
Returns an array of DOMString representing the languages known to the user, by order of preference.

`Navigator.locks` Read only 
Returns a LockManager object that provides methods for requesting a new Lock object and querying for an existing Lock object.

`Navigator.maxTouchPoints` Read only
Returns the maximum number of simultaneous touch contact points are supported by the current device.

`Navigator.mediaCapabilities` Read only 
Returns a MediaCapabilities object that can expose information about the decoding and encoding capabilities for a given format and output capabilities.

`Navigator.mediaDevices` Read only
Returns a reference to a MediaDevices object which can then be used to get information about available media devices (MediaDevices.enumerateDevices()), find out what constrainable properties are supported for media on the user's computer and user agent (MediaDevices.getSupportedConstraints()), and to request access to media using MediaDevices.getUserMedia().

`Navigator.mediaSession` Read only 
Returns MediaSession object which can be used to provide metadata that can be used by the browser to present information about the currently-playing media to the user, such as in a global media controls UI.

`Navigator.onLine Read` only
Returns a boolean value indicating whether the browser is working online.

`Navigator.permissions` Read only 
Returns a Permissions object that can be used to query and update permission status of APIs covered by the Permissions API.

`Navigator.presentation` Read only 
Returns a reference to the Presentation API.

`Navigator.serial` Read only
Returns a Serial object, which represents the entry point into the Web Serial API to enable the control of serial ports.

`Navigator.serviceWorker` Read only
Returns a ServiceWorkerContainer object, which provides access to registration, removal, upgrade, and communication with the ServiceWorker objects for the associated document.

`Navigator.storage` Read only
Returns the singleton StorageManager object used for managing persistence permissions and estimating available storage on a site-by-site/app-by-app basis.

`Navigator.userAgent` Read only
Returns the user agent string for the current browser.

`Navigator.userAgentData` Read only
Returns a NavigatorUAData object, which gives access to information about the browser and operating system of the user.

`Navigator.webdriver` Read only 
Indicates whether the user agent is controlled by automation.

`Navigator.windowControlsOverlay` Read only
Returns the WindowControlsOverlay interface which exposes information about the geometry of the title bar in desktop Progressive Web Apps, and an event to know whenever it changes.

`Navigator.xr` Read only 
Returns XRSystem object, which represents the entry point into the WebXR API.


`Navigator.canShare()`
Returns true if a call to Navigator.share() would succeed.

`Navigator.clearAppBadge()`
Clears a badge on the current app's icon and returns a Promise that resolves with undefined.

`Navigator.getBattery()`
Returns a promise that resolves with a BatteryManager object that returns information about the battery charging status.

`Navigator.registerProtocolHandler()`
Allows web sites to register themselves as a possible handler for a given protocol.

`Navigator.requestMediaKeySystemAccess()`
Returns a Promise for a MediaKeySystemAccess object.

`Navigator.sendBeacon()`
Used to asynchronously transfer a small amount of data using HTTP from the User Agent to a web server.

`Navigator.setAppBadge()`
Sets a badge on the icon associated with this app and returns a Promise that resolves with undefined.

`Navigator.share()`
Invokes the native sharing mechanism of the current platform.

`Navigator.vibrate()`
Causes vibration on devices with support for it. Does nothing if vibration support isn't available.


## screen对象
Screen接口代表一个屏幕，通常是当前窗口正在渲染的那个屏幕，使用window.screen获取。请注意，浏览器通过检测哪个屏幕具有浏览器窗口的中心来确定将哪个屏幕报告为当前屏幕。

### 查询屏幕可用空间
- `Screen.availHeight` - 指定屏幕的高度，以像素为单位，减去操作系统显示的永久或半永久用户界面功能，例如 Windows 上的任务栏。
- `Screen.availWidth`- 返回窗口可用的水平空间量。 

### 查询屏幕尺寸
- `Screen.height` - 返回屏幕的高度。
- `Screen.width` - 返回屏幕的宽度。

### 锁定屏幕方向
- `Screen.lockOrientation` - 锁定屏幕方向（仅适用于全屏或已安装的应用程序）
- `Screen.unlockOrientation` - 解锁屏幕方向（仅适用于全屏或已安装的应用程序）
