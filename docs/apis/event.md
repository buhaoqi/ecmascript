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
