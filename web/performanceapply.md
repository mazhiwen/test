# 前端性能优化

前端性能优化主要是为了提高页面的加载速度，优化用户的访问体验。我认为可以从这些方面来进行优化。

参考：

前端性能优化之雅虎35条军规 ： <https://juejin.im/post/6844903657318645767#heading-10>

<https://juejin.cn/post/6911472693405548557#heading-49>

<https://juejin.cn/post/6844903640902156301#heading-17>

<https://juejin.cn/post/6844903640902156301#heading-1>

## 网络-客户端

### 压缩JavaScript和CSS

通过对 JavaScript 和 CSS 的文件进行压缩，来减小文件的体积

### CSS文件区分

独立公共css文件：

公共样式 ,方便开发维护，浏览器缓存文件，

模块css文件：

按需打包加载，cssinjs

### 减少 HTTP 请求数

通过文件合并、css 雪碧图、使用 base64 等方式来减少 HTTP 请求数，避免过多的请求造成等待的情况

### DNS 缓存

通过 DNS 缓存等机制来减少 DNS 的查询次数

### 缓存策略

具体配置见 主页 nginx

常用不变的资源，添加强缓存头：Expires或Cache-Control响应头。将 Expires 响应头设置为将来很远的时间，实现「永不过期」策略；

一般设置 cache-control 一年

```sh
cache-control: max-age=31536000 
```

html设置no-store

### 延迟加载

使用延迟加载的方式，来减少页面首屏加载时需要请求的资源。延迟加载的资源当用户需要访问时，再去请求加载

### 预加载

通过用户行为，对某些资源使用预加载的方式，来提高用户需要访问资源时的响应速度

### 划分内容到不同域名

浏览器一般会限制每个域的并行线程（一般为6个，甚至更少），使用不同的域名可以最大化下载线程，但注意保持在2-4个域名内，以避免DNS查询损耗。

例如，动态内容放在csspod.com上，静态资源放在static.csspod.com上。这样还可以禁用静态资源域下的Cookie，减少数据传输，详见Cookie 优化。

## 网络-服务端

### 使用CDN

见主页 CDN目录

### 启用Gzip

具体配置见主目录，nginx下相关配置

服务器端启用 Gzip、Deflate 等方式对于传输的资源进行压缩，减小文件的体积

### Ajax请求使用GET方法

浏览器执行XMLHttpRequest POST请求时分成两步，先发送Http Header，再发送data。而GET只使用一个TCP数据包（Http Header与data）发送数据，所以首选GET方法。

根据HTTP规范，GET用于获取数据，POST则用于向服务器发送数据，所以Ajax请求数据时使用GET更符合规范。

### 避免图片src为空[！此条好像错误]

虽然src属性为空字符串，但浏览器仍然会向服务器发起一个HTTP请求：

### 减小cookie

减少 Cookie 大小

静态资源使用无Cookie域名

尽可能减小 cookie 的大小，并且通过将静态资源分配到其他域名下，来避免对静态资源请求时携带不必要的 cookie

## 语法-CSS

### 把样式表放在`<head>`中

把样式表放在页面的 head 标签中，减少页面的首次渲染的时间

### 使用`<link>`替代@import

避免使用 @import 标签 . 对于IE某些版本，@import的行为和放在页面底部一样。所以，不要用它。

### 避免过多的回流与重绘

详细见 主页 回流与重绘 文档

## 语法-JS/HTML

### 减少DOM元素数量

不使用表格布局 ：更多的标签，增加文件大小；不易维护，无法适应响应式设计；性能考量，默认的表格布局算法会产生大量重绘

塞进去更多的<div>仅为了处理布局问题？也许有更好、更语义化的标记。

能通过伪元素实现的功能，就没必要添加额外元素，如清除浮动。

### 减少DOM操作

JavaScript 操作 DOM 很慢，尤其是 DOM 节点很多时。

缓存已经访问过的元素；
使用DocumentFragment暂存DOM，整理好以后再插入DOM树；
操作className，而不是多次读写style；
避免使用JavaScript修复布局。

### 把脚本放在页面底部

尽量把 js 脚本放在页面底部或者使用 defer 或 async 属性，避免脚本的加载和执行阻塞页面的渲染

### 使用高效的事件处理

减少绑定事件监听的节点，如通过事件委托；

尽早处理事件，在DOMContentLoaded即可进行，不用等到load以后。

## webpack

见主目录webpack配置

## 根组件加loading

```html
<div id="root">Loading...</div>
```

## ES2015选择性加载

webpack配置方案 待研究

```html
<script type="module">
```

## 图片懒加载LazyLoad

<https://juejin.cn/post/6844903614138286094>

用户滚动到图片之前，可视区域外的图像不会加载。

当然你也可以实现像 Medium 的那种加载体验（好像知乎已经是这样了），即先加载一张低像素的模糊图片，然后等真实图片加载完毕之后，再替换掉。

### 方法一：监听scroll

vue可以做一个图片组件

监听 window 对象或者父级对象的 scroll 事件，触发 load；首先将页面上的图片的 src 属性设为空字符串，而图片的真实路径则设置在data-original属性中，当页面滚动的时候需要去监听scroll事件，在scroll事件的回调中，判断我们的懒加载的图片是否进入可视区域,如果图片在可视区内将图片的 src 属性设置为data-original 的值，这样就可以实现延迟加载。

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lazyload</title>
    <style>
      .image-item {
     display: block;
     margin-bottom: 50px;
     height: 200px;//一定记得设置图片高度
 }
    </style>
</head>
<body>
<img src="" class="image-item" lazyload="true"  data-original="images/1.png"/>
<img src="" class="image-item" lazyload="true"  data-original="images/2.png"/>
<img src="" class="image-item" lazyload="true"  data-original="images/3.png"/>
<script>
  var viewHeight =document.documentElement.clientHeight//获取可视区高度
  function lazyload(){
    var eles=document.querySelectorAll（'img[data-original][lazyload]'）
    Array.prototype.forEach.call(eles,function(item,index){
      var rect
      if(item.dataset.original==="")
        return
      rect=item.getBoundingClientRect()// 用于获得页面中某个元素的左，上，右和下分别相对浏览器视窗的位置
      if(rect.bottom>=0 && rect.top < viewHeight){
        !function(){
          var img=new Image()
          img.src=item.dataset.original
          img.onload=function(){
            item.src=img.src
          }
          item.removeAttribute（"data-original"）//移除属性，下次不再遍历
          item.removeAttribute（"lazyload"）
        }()
      }
    })
  }
  lazyload()//刚开始还没滚动屏幕时，要先触发一次函数，初始化首页的页面图片
  document.addEventListener（"scroll"，lazyload)
</script>
</body>
</html>
```

### 方法二：使用 Intersection Observer API 来获取元素的可见性