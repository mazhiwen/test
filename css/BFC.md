# BFC

(Block Formatting Context)

<https://github.com/ljianshu/Blog/issues/15>

BFC直意为 块格式化上下文（Block Formatting Context，BFC）

W3C官方解释为：BFC它决定了元素如何对其内容进行定位，以及与其它元素的关系和相互作用，当涉及到可视化布局时，Block Formatting Context提供了一个环境，HTML在这个环境中按照一定的规则进行布局。

## 创建BFC的方式

- float 为 left|right

- overflow 为 hidden|auto|scroll

- display 为 table-cell|table-caption|inline-block|inline-flex|flex

- position 为 absolute|fixed

- 根元素

## 具体特性

BFC就是一个块级元素，块级元素会在垂直方向一个接一个的排列

BFC盒子之间垂直的间距是由 margin 决定的（不会重叠）。但在同一个 BFC 中，两个相邻的块级盒子的垂直外边距会发生重叠。

一个元素形成了BFC之后，那么它内部元素产生的布局不会影响到外部元素，外部元素的布局也不会影响到BFC中的内部元素。一个BFC就像是一个隔离区域，和其他区域互不影响。(比如解决重叠)

BFC 能够识别并包含浮动元素，当计算其区域的高度时，浮动元素也可以参与计算了。

## 用法一：边距不会重叠

BFC会阻止垂直外边距重叠

### 1. 解决相邻元素margin重叠

只有同属于一个BFC时，两个元素才有可能发生垂直margin的重叠

这个包括相邻元素或者嵌套元素，只要他们之间没有阻挡（比如边框、非空内容、padding等）就会发生margin重叠。

例子中两个相邻p发生margin重叠。

解决margin重叠：在发生重叠的其中一个p外面包裹一层容器,如 .wrap ，并触发该容器生成一个BFC，如overflow:hidden。那么两个div便不属于同一个BFC，就不会发生margin重叠了。

```html
<p>ABC</p>
<div class="wrap">
  <p>abc</p>
</div>
```

```css
p{
  color: #fff;
  background: #888;
  width: 200px;
  line-height: 100px;
  text-align:center;
  margin: 100px;
}
.wrap {
  overflow:hidden;
}
```

### 2. 解决父子元素margin重叠（子元素的margin会转移到父元素margin）

wrap元素与h1元素之间理论上本该有个40px的上下margin值,然而实际上父子元素并没有存在margin值，子元素h1的margin加在了父元素的margin值上。使父元素与上个元素div元素的间距为40px。

处理方法其实有很多，详细见[margin](./margin.md):

BFC方法: 在wrap元素中添加:overflow:hidden;或者overflow：auto；使其父元素形成一个BFC；

添加阻隔: 也可以在wrap元素中添加border：1px solid；或是padding：1px；

```html
<div class="box">box</div>
<div class="wrap">
  <h1>h1</h1>
</div>
```

```css
.box{
width:100px;
height:100px;
background:#ccc;
}
.wrap {
  background:yellow;
}
.wrap h1{
  background:pink;
  margin:40px;
}
```

## 用法二：高度不会塌陷，可以撑起高度

让父元素包含浮动，并清除子元素浮动给父元素造成的高度塌陷效果

3. 清除子元素给父元素造成的浮动效果：给父元素造BFC

```html
<div class="one">
  <div class="two">Hello World!</div>
</div>
你好世界！
```

```css
.one {
  background-color: pink;
  overflow: hidden;
}
.two {
  float: left;
}
```

## 用法三：浮动元素与下层元素重叠问题

4. 取消float元素的相邻元素的浮动重叠效果

.box1 是 float 的元素。 .box2 用属性 overflow: hidden; 清除了.box1 的 float 造成的 .box2被压在.box1下面的情况

```html
<div class="box1">box1</div>
<div class="box2">box2</div>
```

```css
.box1 {
  float: left;
  width: 100px;
  height: 100px;
  background-color: pink;
}
.box2 {
  width: 200px;
  height: 200px;
  background-color: skyblue;
  overflow: hidden;
  /*或者 float: left;*/
}
```
