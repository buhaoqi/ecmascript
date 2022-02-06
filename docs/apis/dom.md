
# DOM
客户端JavaScript的存在使得静态的HTML文档编程了交互式的web应用。脚本化HTML页面内容是JavaScript的核心目标。

## DOM简介
- DOM是Document Object Model的缩写，翻译为“文档对象模型”。
- DOM是文档内容的对象表示。
    - 当页面加载之后，浏览器就创建了基于当前页面的DOM(文档对象模型)。
    - Document：整个文档被看成document对象；
    - Element：所有的标签被看做`Element`对象.Element对象表示HTML元素。
    - Text：标签的内容被看做`Text`对象
    - Attributes：标签的属性被看做`Attribute`对象
    - Node：所有的对象都被看成节点对象。 
    - CSSStyleDeclaration：行间样式对象
- DOM用于操作文档的内容。(增、删、改、查)
- DOM为所有的HTML元素定义了事件处理程序

DOM Tree

![The HTML DOM Tree of Objects](../assets/images/pic_htmltree.gif)


## Document对象
定义：

- `Document`对象表示整个web文档。
- `Document`对象是DOM的根对象、核心对象，也是页面其他所有对象的主人。
- `Document`对象是访问页面内容(DOM树)的入口。

>
在程序运行时，总是在操作一个或多个文档元素。为了操作文档中的元素，必须获得或这些元素的Element对象。为此，document对象定义了许多方式来选取元素。

**获取元素的快捷属性**

- Document.documentElment: 选取`<html>`标签
- Document.head: 选取`<head>`标签
- document.body: 选取`<body>`标签
- document.title: 选取`<title>`标签
- document.scripts: 选取`<script>`标签
- document.links: 选取页面中的`<a>`标签
- document.anchors: 选取页面中的`<a>`标签
- document.forms: 选取页面中的`<form>`标签
- document.images: 选取页面中的`<img>`标签

**获取元素的方法**

- Document.querySelector()
- document.querySelectorAll()
- document.`getElementsByClassName(name)`
- document.`getElementById(id)`
- document.`getElementsByTagName(name)`

**创建元素的方法**

- document.`createElement(element)`: Create an HTML element

**其他方法**

- document.`write(text)`:Write into the HTML output stream

Document对象的事件处理程序

- document.getElementById(id).`onclick` = function(){code} ：为点击事件添加事件处理程序



> 一旦从文档中选取了一个元素之后，有时需要基于此元素查找与之相关的元素，此时有两种API可供使用：

> **Node API：把文档树看成节点树;**

> **Element API：把文档树看成元素树。**



## Node API

DOM中的一切对象都是节点。

**节点类型**

根据文档中对象类型的不同，节点对象一共分为以下几种类型：

- 1 => elment : 元素节点。任一个HTML元素都是元素节点。

- 2 => attribute ：属性节点。Every HTML attribute is an attribute node (deprecated)

- 3 => text : 文本节点。表示HTML元素或属性的文本内容。

- 8 => comment : 注释节点。表示一条注释。

- 9 => document ：根节点。即document，表示整个文档。

  

**节点关系**

从节点之间的层级关系上讲，

- 父节点：在一个节点之上的直接节点
- 子节点：在一个节点之下的直接节点
- 兄弟节点：具有相同父节点的节点
- 后代节点：在一个节点之下的所有层级的节点



**查找节点的名值类型**

- node.nodeName: 查询元素的标签名
- node.nodeType
- node.nodeValue : 查询Text节点和Comment节点的文本内容。其他节点返回Null

|                              | Node type | nodeName  | nodeValue |
| ---------------------------- | --------- | --------- | --------- |
| < li >< /li >                | 1         | LI        | null      |
| Node类型定义了attributes属性 | 2         | class     | lists     |
| 换行符                       | 3         | #text     | 文本内容  |
| < !--这里是注释内容-- >      | 8         | #comment  | 注释内容  |
| document                     | 9         | #document | null      |

**查找节点的关系**

- node.parentNode
- Node.childNodes
- Node.nextSibling
- Node.previoursSibling
- Node.firstChild
- Node.lastChild

**查询或设置节点的内容**

- Node.textContent:表示当前节点和后代节点的HTML内容。
- 注意：`Node.textContent`和`HTMLElement.innerText`很相似，但有区别：
	- `textContent`获取所有元素的内容，包括`<script>`和`<style>`元素；
	- `innerText`只展示’人类可读‘的元素

**操作节点的方法**

- node.`remove()`: 从DOM中删除指定元素。参数：无。返回值：无。
- node.`removeChild(node)`: 删除指定元素的指定子节点。返回值：删除的子节点对象或null。
- Node.`appendChild(element)`:Add an HTML element
- Node.`replaceChild(new, old)`	:Replace an HTML element
- node.`insertBefore(new, old)` :为父节点插入子节点
- Node.`hasChildNodes()`



## Element API

Element对象树把文档看成元素对象树，忽略Text和Comment节点。

**查找元素**

- Element.`parentElement`
- Element.`children`
- Element.`firstElementChild`
- Element.`lastElmentChild`
- Element.`nextElementSibling`
- Element.`previousElementSibling`
- Element.`childElementCount`

**查找或设置元素内容**

- element.innerText
- element.innerHTML：Change the inner HTML of an element

**查找或设置元素的属性**

- element.attribute：Change the attribute value of an HTML element
- element.style.property ：Change the style of an HTML element
- element.`setAttribute(attribute, value)`:	设置非标准的HTML属性
- Element.`getAttribute(attributeName)`: 查询非标准的HTMl属性
- Element.`removeAttribute(attrName);`: 删除属性
- element.hasAttribute() ：检测命名属性是否存在
- element.removeAttribute() :  完全删除属性

**操作元素的class属性**

- `element.className = ''`: 传统操作元素的class属性的方式.
- `element.classList`: H5新增的属性。返回值`DOMtokenList`是一个只读的类数组对象
- DOMTokenList.add('类名') : 从元素的class属性中添加一个类名
- DomTokenList.remove('类名') : 从元素的class属性中删除一个类名
- DomTokenList.toggle('类名') ：表示如果不存在括号里的类名就添加一个，否则就删除它。

## Attribute对象
HTML元素是由一个标签和一组称为属性的名/值对组成。

HTMLElement对象定义了映射HTML属性的属性。
- HTMLElement.id
- HTMLElement.lang
- HTMLElement.events
- HTMLElement.src

HTML属性名转换JS属性名的规则：
- 1个单词：采用小写
- 两个单词：小驼峰
- 属性名是js保留字：
    - htmlFor属性
    - className属性
- 属性名是布尔值


CSSStyleDeclaration对象


获取和设置非标准的HTML属性
HTMLElement的非标准属性
- getAttribute()
- setAttribute()

Element的非标准属性
- hasAttribute():检测命名属性是否存在
- removeAttribute()：完全删除属性

**数据集属性**

`Element.data-`

有时候在HTML元素上绑定一些额外的信息。在H5文档中，任意以“data-”为前缀的小写属性名字都是合法的。这些“数据集属性”不会对其元素的表现产生影响。

`Element.dataset`
该属性指代一个对象，它的各个属性对应于去掉前缀的data-属性的值。

**作为Attr节点的属性**
Node.attributes


## DOM尺寸
>理解scroll和client，你需要知道元素的实际内容可能比容纳内容的盒子更大，可能会有滚动条，内容区就是视口.

### 查询元素的视口尺寸
- 视口尺寸(无单位) = 可视内容尺寸 + 内边距
- Element.clientWidth: 返回元素的可视内容宽度
- Element.clientHeight: 返回元素的可视宽度
- document.documentElement.clientWidth
- document.documentElement.clientHeight

- `window.innerHeight`: 返回窗口的内部/视口高度，包括水平滚动条的高度。
- `window.innerWidth`: 返回窗口的内部/视口宽度，包括水平滚动条的高度。

### 查询元素的可视尺寸
- 可视尺寸(无单位) = 可视内容尺寸 + 内边距 + 边框 + 滚动条 
- Element.offsetWidth :返回元素的可视宽度
- Element.offsetHeight:返回元素的可视高度
### 查询元素内容的尺寸
- 内容尺寸(无单位) = 可视内容尺寸 + 不可视内容尺寸 + 内边距 
- Element.scrollHeight:   返回元素的全部内容高度
- Element.scrollWidth：返回元素的全部内容宽度
### 查询或设置元素内容的滚动尺寸
- 滚动尺寸(无单位) = 滚动条的滚动距离
- element.scrollTop: 查询元素内容的垂直滚动距离(它测量的是元素顶部(在视口之外)到元素可见内容顶部的距离)
- 如：document.documentElement.scrollTop: 相当于返回window.scrollY（任何浏览器都支持，包括IE8）
- element.scrollLeft: 查询元素内容的垂直滚动距离
- 如：document.documentElement.scrollLeft: 查询滚动条的滚动距离（任何浏览器都支持，包括IE8）

## DOM位置
### 查询定位元素的坐标
- Element.offsetParent: 返回当前元素的定位父级元素。
- Element.offsetTop: 返回定位元素的垂直位置(边框到定位父级元素的垂直距离,含margin，有单位）
- Element.offsetLeft: 返回定位元素的水平位置(边框到定位父级元素的水平距离，含margin，有单位）

### 查询元素的坐标
- getBoundingClientBox返回元素在视口坐标中的位置。
    - 元素位置：以像素度量，向右代表X轴增加，向下代表Y轴增加。
    - 文档坐标 = 滚动距离 + 视口坐标
- element.getBoundingClientRect()：查询元素的视口坐标。
    - 1.无参数；
    - 2.返回一个对象
    - 3.包含边框内边距不包含外边距
