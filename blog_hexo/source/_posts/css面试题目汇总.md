---
title: css面试题目汇总
date: 2023-01-14 21:59:32
tags: [前端, 闲聊分享, CSS]
categories:
  - 经验分享
  - 前端
---

# 前言

本篇文章主要总结一些常见的 CSS 面试题,会不断更新。

# CSS 选择器及其优先级

| **选择器**     | **格式**      | **优先级权重** |
| -------------- | ------------- | -------------- |
| id 选择器      | #id           | 100            |
| 类选择器       | #classname    | 10             |
| 属性选择器     | a[ref=“eee”]  | 10             |
| 伪类选择器     | li:last-child | 10             |
| 标签选择器     | div           | 1              |
| 伪元素选择器   | li:after      | 1              |
| 相邻兄弟选择器 | h1+p          | 0              |
| 子选择器       | ul>li         | 0              |
| 后代选择器     | li a          | 0              |
| 通配符选择器   | \*            | 0              |

对于选择器的优先级：

- 标签选择器、伪元素选择器：1

- 类选择器、伪类选择器、属性选择器：10

- id 选择器：100

- 内联样式：1000（内联样式定义在 dom 元素的 style 属性中，格式为：

  ```html
  <div style="background-color:red;">hello</div>
  ```

**注意事项**：

- `!important`声明的样式优先级最高；
- 如果优先级相同，则最后出现的样式生效；
- 继承得到的样式的优先级最低；
- 通用选择器（\*）、子选择器（>）和相邻同胞选择器（+）并不在这四个等级中，所以它们的权值都为 0；
- 样式表的来源不同时，优先级顺序为：内联样式>内部样式>外部样式>浏览器用户自定义样式>浏览器默认样式。_（内部样式：将 css 代码集中写在 HTML 文档的`html`头部标签中，并且用`style`标签定义）_

# display 的属性值及其作用

| **属性值**   | **作用**                                                   |
| ------------ | ---------------------------------------------------------- |
| none         | 元素不显示，并且会从文档流中移除。                         |
| block        | 块类型。默认宽度为父元素宽度，可设置宽高，换行显示。       |
| inline       | 行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。 |
| inline-block | 默认宽度为内容宽度，可以设置宽高，同行显示。               |
| list-item    | 像块类型元素一样显示，并添加样式列表标记。                 |
| table        | 此元素会作为块级表格来显示。                               |
| inherit      | 规定应该从父元素继承 display 属性的值。                    |

- *文档流：*页面从上往下一行一行，其中每行从左往右的顺序，这种排列称为文档流。

- `ul` 里的 `li` 的默认 `display` 就是 `list-item`

# display 的 block、inline 和 inline-block 的区别

（1）**block**：会独占一行，多个元素会另起一行，可以设置`width、height、margin和padding`属性。

（2）**inline**：元素不会独占一行，设置`width、height`属性无效。但可以设置水平方向的`margin和padding`属性，不能设置垂直方向的`padding和margin`；

（3）**inline-block**：将对象设置为 inline 对象，但对象的内容作为`block`对象呈现，之后的内联对象会被排列在同一行内。

对于行内元素和块级元素，其特点如下：

**（1）行内元素**

- 设置宽高无效；
- 可以设置水平方向的`margin`和`padding`属性，不能设置垂直方向的`padding`和`margin`；
- 不会自动换行；

**（2）块级元素**

- 可以设置宽高；
- 设置`margin`和`padding`都有效；
- 可以自动换行；
- 多个块状，默认排列从上到下。

# 隐藏元素的方法有哪些

- **display:none**：渲染树不会包含该渲染对象，因此该元素不会在页面中占据位置，也不会响应绑定的监听事件。
- **visibility:hidden**：元素在页面中仍占据空间，但是不会响应绑定的监听事件。
- **opacity:0**：将元素的透明度设置为 0，以此来实现元素的隐藏。元素在页面中仍然占据空间，并且能够响应元素绑定的监听事件。
- **position:absolute**：通过使用绝对定位将元素移除可视区域内，以此来实现元素的隐藏。
- **z-index:负值**：来使其他元素遮盖住该元素，以此来实现隐藏。
- **clip/clip-path**：使用元素裁剪的方法来实现元素的隐藏，这种方法下，元素仍在页面中占据位置，但是不会响应绑定的监听事件。
- **transform:scale(0,0)**：将元素缩放为 0，来实现元素的隐藏。这种方法下，元素仍在页面中占据位置，但是不会响应绑定的监听事件。

# display:none 与 visibility:hidden 的区别

这两个属性都是让元素隐藏，不可见。**两者区别如下**：

（1）**在渲染树中**

- `display：none`会让元素完全从渲染树中消失，渲染时不会占据任何空间；
- `visibility:hidden`不会让元素从渲染树中消失，渲染的元素还会占据相同的空间，只是内容不可见。

（2）**是否是继承属性**

- `display:none`是非继承属性，子孙节点会随着父节点从渲染树消失，通过修改子孙节点的属性也无法显示；
- `visibility:hidden`是继承属性，子孙节点消失是由于继承了`hidden`，通过设置`visibility:visible`可以让子孙节点显示；（3）修改常规文档流中元素的`display`通常会造成文档的重排，但是修改`visibility`属性只会造成本元素的重绘；

（4）如果使用读屏器*（屏幕阅读器是“读取文档”（在您的情况下是网页）并以视觉障碍（例如失明）的人可以轻松消费的方式输出的软件）*，设置为`display:none`的内容不会被读取，设置为`visibility:hidden`的内容会被读取。

# 对盒模型的理解

![iShot_2023-02-05_10.39.26](./css面试题目汇总/iShot_2023-02-05_10.39.26.png)

![iShot_2023-02-05_10.41.38](./css面试题目汇总/iShot_2023-02-05_10.41.38.png)

盒模型都是由四个部分组成的，分别是`margin`、`border`、`padding`、`content`。

标准盒模型和 IE 盒模型的区别在于设置`width`和`height`时，所对应的范围不同：

- 标准盒模型的`width`和`height`属性的范围只包含了`content`，
- IE 盒模型的`width`和`height`属性的范围包含了`border`、`padding`、`content`。

可以通过修改元素的`box-sizing`属性来改变元素的盒模型：

- `box-sizing:content-box`表示标准盒模型（<font color="red">默认值</font>）
- `box-sizing:border-box`表示 IE 盒模型（怪异盒模型）

# CSS3 中有哪些新特性

- 新增各种选择器：（:not(.input):所有 class 不是"input" 的节点）
- 圆角（`border-radius:8px`）
- 所列布局(`multi-column layout`)
- 阴影（`text-shadow和box-shadow`）和反射（`box-reflect`）
- 文字特效（`text-shadow`）
- 文字渲染（`text-decoration`）
- 线性渐变（`gradient`）
- 旋转（`transform`）
- 增加了旋转，缩放，定位，倾斜，动画，多背景

# 单行、多行文本溢出隐藏

- 单行文本溢出

```css
overflow: hidden; //溢出隐藏
text-overflow: ellipsis; //溢出用省略号表示
white-space: nowrap; //规定段落中的文本不进行换行
```

- 多行文本溢出

```css
overflow: hidden; //溢出隐藏
text-overflow: ellipsis; //溢出用省略号表示
display: -webkit-box; //作为弹性伸缩盒子模型显示
-webkit-box-orient: vertical; //设置伸缩盒子的字元素排列方式：从上到下垂直排列
-webkit-line-clamp: 3; //设置的行数
```

webkit-line-clamp 用来限制在一个块元素显示的文本的行数。 为了实现该效果，它需要组合其他的 WebKit 属性。常见结合属性：
display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。
-webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。
text-overflow: ellipsis;，可以用来多行文本的情况下，用省略号“…”隐藏超出范围的文本。
注意：由于上面的三个属性都是 CSS3 的属性，没有浏览器可以兼容，所以要在前面加一个`-webkit-`来兼容一部分浏览器。

> [单行、多行文本溢出详解博客](https://blog.csdn.net/weixin_52834435/article/details/122515850)

# 两栏布局的实现

一般两栏布局指的是**左边一栏宽度固定，右边一栏宽度自适应**，两栏布局的具体实现：

- 利用浮动，将左边元素宽度设置为 200px，并且设置向左浮动。将右边元素的`margin-left`设置为 200px，宽度设置为`auto`（默认为`auto`，撑满整个父元素）。

  ```css
  .outer {
    height: 200px;
  }
  .left {
    float: left;
    width: 200px;
    background: tomato;
  }
  .right {
    margin-left: 200px;
    width: auto;
    background: gold;
    height: 50px;
  }
  ```

  `float`使左边的元素脱离文档流，右边的元素可以和左边的元素显示在同一行，设置`margin-left`让右边的元素不覆盖掉左边的元素，将其挤到右边去

- 利用浮动，左侧元素设置固定大小，并左浮动，右侧元素设置`overflow:hidden`，这样右边就触发了 BFC,BFC 的区域不会与浮动元素发生重叠，所以两侧就不会发生重叠。

  ```css
  <style>
    * {
      margin: 0;
      padding: 0;
    }
    .aside {
      float: left;
      width: 200px;
      background-color: pink;
    }
    .main {
      /* 开启BFC，清除左侧浮动的影响 */
      overflow: hidden;
      /* 这里不设置宽度也可以 */
      width: auto;
      /*
      清除了浮动，会将两个子元素处于同一水平线
      此时不能将宽度设置为100%，会将.main挤压到第二行去 */
      /* width: 100%; */
      min-height: 200px;
      background-color: skyblue;
    }
  </style>
  <body>
      <div class="contain">
          <div class="aside">这是左侧的广告</div>
          <div class="main">这是右侧的内容</div>
      </div>
  </body>
  ```

- 利用 flex 布局，将左边元素设置为固定给宽度 200px，将右边的元素设置为`flex:1`

  ```css
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    .contain {
      /* 将父容器设置为弹性容器 */
      display: flex;
    }

    .aside {
      background-color: pink;
    }

    .main {
      /* 内容区会自动放大占满剩余空间 */
      flex: 1;
      min-height: 200px;
      background-color: skyblue;
    }
  </style>
  <body>
    <div class="contain">
      <div class="aside">这是左侧的广告</div>
      <div class="main">这是右侧的内容</div>
    </div>
  </body>
  ```

- 利用绝对定位，将父级元素设置为相对定位。左边元素设置为 absolute 定位，并且宽度设置为 200px。将右边元素的`margin-left`的值设置为 200px。

  ```css
  .outer {
    position: relative;
    height: 100px;
  }
  .left {
    position: absolute;
    width: 200px;
    height: 100px;
    background: tomato;
  }
  .right {
    margin-left: 200px;
    background: gold;
  }
  ```

- 利用绝对定位，将父级元素设置为相对定位。左边元素高度设置为 200px，右边元素设置为绝对定位，左边定位为 200px，其余方向定位为 0。

  ```css
  .outer {
    position: relative;
    height: 100px;
  }
  .left {
    width: 200px;
    background: tomato;
  }
  .right {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 200px;
    background: gold;
  }
  ```

# 三栏布局的实现

三栏布局一般指的是页面中一共有三栏，**左右两栏宽度固定，中间自适应的布局**，三栏布局的具体实现：

- 利用**绝对定位**，左右两栏设置为绝对定位，中间设置对应方向大小的 margin 的值。

  ```css
  .outer {
    position: relative;
    height: 100px;
  }

  .left {
    position: absolute;
    width: 100px;
    height: 100px;
    background: tomato;
  }

  .right {
    position: absolute;
    top: 0;
    right: 0;
    width: 200px;
    height: 100px;
    background: gold;
  }

  .center {
    margin-left: 100px;
    margin-right: 200px;
    height: 100px;
    background: lightgreen;
  }
  ```

- 利用 flex 布局，左右两栏设置固定大小，中间一栏设置为`flex:1`。

  ```css
  .outer {
    display: flex;
    height: 100px;
  }

  .left {
    width: 100px;
    background: tomato;
  }

  .right {
    width: 100px;
    background: gold;
  }

  .center {
    flex: 1;
    background: lightgreen;
  }
  ```

- 利用浮动，左右两栏设置固定大小，并设置对应方向的浮动。中间一栏设置左右两个方向的 margin 值，**_注意这种方式，中间一栏必须放到最后_**？

  ```css
  .outer {
    height: 100px;
  }

  .left {
    float: left;
    width: 100px;
    height: 100px;
    background: tomato;
  }

  .right {
    float: right;
    width: 200px;
    height: 100px;
    background: gold;
  }

  .center {
    height: 100px;
    margin-left: 100px;
    margin-right: 200px;
    background: lightgreen;
  }
  ```

- 圣杯布局，利用浮动和负边距来实现。父级元素设置左右的`padding`，三列均设置向左浮动，中间一列放在最前面，宽度设置为父级元素的宽度，因此后面两列都被挤到了下一行，通过设置 margin 负值将其移动到上一行，再利用相对定位，定位到两边。

  ```css
  .outer {
    height: 100px;
    padding-left: 100px;
    padding-right: 200px;
  }

  .left {
    position: relative;
    left: -100px;
    float: left;
    margin-left: -100%;
    width: 100px;
    height: 100px;
    background: tomato;
  }

  .right {
    position: relative;
    left: 200px;
    float: right;
    margin-left: -200px;
    width: 200px;
    height: 100px;
    background: gold;
  }

  .center {
    float: left;
    width: 100%;    /* 要设置其自适应，所以要100% */
    height: 100px;
    background: lightgreen;
  }

  <div>
  	<div class="middle"><h4>中间弹性区</h4></div>
  <div class="left"><h4>左边栏</h4></div>
  <div class="right"><h4>右边栏</h4></div>
  </div>
  ```

  圣杯局部的重点是**_中间盒子要被优先渲染！！！，所以中间盒子要放在最前面_**

> [圣杯布局和双飞翼布局详解](http://mespace.fun/2023/02/08/CSS%E9%9D%A2%E8%AF%95%E9%AB%98%E9%A2%91%E4%B9%8B%E5%9C%A3%E6%9D%AF%E5%B8%83%E5%B1%80%E4%B8%8E%E5%8F%8C%E9%A3%9E%E7%BF%BC%E5%B8%83%E5%B1%80/)

# 水平垂直居中的实现

- 利用绝对定位，先将元素的左上角通过 top:50%和 left:50%定位到页面的中心，然后再通过 translate 来调整元素的中心点到页面的中心。该方法需要**考虑浏览器兼容问题。**

```css
.parent {
  position: relative;
}
.child {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
} /* 元素的2d转换。 ， translate是移动*/
```

- 利用绝对定位，设置四个方向的值都为 0，并将 margin 设置为 auto，由于宽高固定，因此对应方向实现平分，可以实现水平和垂直方向上的居中。该方法适用于**盒子有宽高**的情况：

  ```css
  .parent {
    position: relative;
  }

  .child {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
  }
  ```

- 利用绝对定位，先将元素的左上角通过 top:50%和 left:50%定位到页面的中心，然后再通过 margin 负值来调整元素的中心点到页面的中心。该方法适用于**盒子宽高已知**的情况

  ```css
  .parent {
    position: relative;
  }

  .child {
    position: absolute;
    top: 50%;
    left: 50%;
    margin-top: -50px; /* 自身 height 的一半 */
    margin-left: -50px; /* 自身 width 的一半 */
  }
  ```

- 利用 flex 布局，通过`align-items:center`和`justify-content:center`设置容器的垂直和水平方向上为居中对齐，然后它的子元素也可以实现垂直和水平的居中。该方法要**考虑兼容的问题**，该方法在移动端用的较多：

  ```css
  .parent {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  ```

# 对 Flex 布局的理解及其使用场景

**简单来说：** flex 布局是 CSS3 新增的一种布局方式，可以通过将一个元素的 display 属性值设置为 flex 从而使它成为一个 flex 容器，它的所有子元素都会成为它的项目。一个容器默认有两条轴：一个是水平的主轴，一个是与主轴垂直的交叉轴。可以使用 flex-direction 来指定主轴的方向。可以使用 justify-content 来指定元素在主轴上的排列方式，使用 align-items 来指定元素在交叉轴上的排列方式。还可以使用 flex-wrap 来规定当一行排列不下时的换行方式。对于容器中的项目，可以使用 order 属性来指定项目的排列顺序，还可以使用 flex-grow 来指定当排列空间有剩余的时候，项目的放大比例，还可以使用 flex-shrink 来指定当排列空间不足时，项目的缩小比例。

> [flex 布局的基本概念](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)

# 为什么要清除浮动？清除浮动的方式

- 浮动的定义： 非 IE 浏览器下，容器不设高度且子元素浮动时，容器高度不能被内容撑开。此时，内容会溢出到容器外面而影响布局。这种现象被称为浮动（溢出）。

**浮动的工作原理**：

- 浮动元素脱离文档流，不占据空间（引起“高度塌陷”现象）
- 浮动元素碰到包含它的边框或者其他浮动元素的边框停留。

浮动元素可以左右移动，直到遇到另一个浮动元素或者遇到它外边缘的包含框。浮动框不属于文档流中的普通流，当元素浮动之后，不会影响块级元素的布局，只会影响内联元素布局。此时文档流中的普通流就会表现得该浮动框不存在一样的布局模式。当包含框的高度小于浮动框的时候，此时就会出现“高度塌陷”。

**浮动元素引起的问题？**

- 父元素的高度无法被撑开，影响与父元素同级的元素
- 与浮动元素同级的非浮动元素会跟随其后
- 若浮动的元素不是第一个元素，则该元素之前的元素也要浮动，否则会影响页面的显示结构

**消除浮动的方式如下**：

- 给父级 div 定义`height`属性
- 最后一个浮动元素之后添加一个空的 div 标签，并添加`clear:both`样式
- 包含浮动元素的父级标签添加`overflow:hidden`或者`overflow:auto`,这是为了让父元素形成 BFC。
- 使用 :after 伪元素。由于 IE6-7 不支持 :after，使用 zoom:1 触发 hasLayout\*\*

```css
.clearfix:after {
  content: "\200B";
  display: table;
  height: 0;
  clear: both;
}
.clearfix {
  *zoom: 1;
}
```

> [为什么要清楚浮动？清除浮动的方式](https://blog.csdn.net/qq_51066068/article/details/124424863)

# 对 BFC 的理解，如何创建 BFC

先来看两个相关的概念：

- Box: Box 是 CSS 布局的对象和基本单位，一个页面是由很多个 Box 组成的，这个 Box 就是我们所说的盒模型。
- Formatting context: 块级上下文格式化，它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

**块格式化上下文（Block Formatting context,BFC）**是 Web 页面的可视化 CSS 渲染的一部分，是布局过程中生成块级盒子的区域，也是浮动元素与其他与阿苏的交互限定区域。

**_通俗来讲：BFC 是一个独立的布局环境，可以理解为一个容器，在这个容器中按照一定规则进行物品摆放，并且不会影响其他环境中的物品。如果一个元素符合触发 BFC 的条件，则 BFC 中的元素布局不受外部影响。_**

**创建 BFC 的条件：**

- 根元素： body；
- 元素设置浮动：float 除 none 以外的值；
- 元素设置绝对定位： position(absolute、fixed)；
- display 的值为：inline-block、table-cell、table-caption、flex 等；
- overflow 值为：hidden、auto、scroll；

**BFC 的特点**：

- 垂直方向上，自上而下排列，和文档流的排列方式一致。
- 在 BFC 中上下相邻的两个容器的 margin 会重叠
- 计算 BFC 的高度时，需要计算浮动元素的高度
- BFC 区域不会与浮动的容器发生重叠
- BFC 是独立的容器，容器内部元素不会影响外部元素
- 每个元素的左 margin 值和容器的左 border 相接触

**BFC 的作用：**

- **解决 margin 的重叠问题**：由于 BFC 是一个独立的区域，内部的元素和外部的元素互不影响，将两个元素变为两个 BFC，就解决了 margin 重叠的问题。
- **解决高度塌陷的问题**：在对子元素设置浮动后，父元素会发生高度塌陷，也就是父元素的高度变为 0。解决这个问题，只需要把父元素变成一个 BFC。常用的办法是给父元素设置`overflow:hidden`。
- **创建自适应两栏布局**：可以用来创建自适应两栏布局：左边的宽度固定，右边的宽度自适应。

```css
.left{
     width: 100px;
     height: 200px;
     background: red;
     float: left;
 }
 .right{
     height: 300px;
     background: blue;
     overflow: hidden;
 }

<div class="left"></div>
<div class="right"></div>
复制代码
```

左侧设置`float:left`，右侧设置`overflow: hidden`。这样右边就触发了 BFC，BFC 的区域不会与浮动元素发生重叠，所以两侧就不会发生重叠，实现了自适应两栏布局。

# position 的属性有哪些？区别是什么？

position 有以下属性值：

| 属性值   | 概述                                                                                                                                                 |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| absolute | 生成绝对定位的元素，相对于 static 定位以外的一个父元素进行定位。元素的位置通过 left、top、right、bottom 属性进行规定。                               |
| relative | 生成相对定位的元素，相对于其原来的位置进行定位。元素的位置通过 left、top、right、bottom 属性进行规定。                                               |
| fixed    | 生成绝对定位的元素，指定元素相对于屏幕视⼝（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变，⽐如回到顶部的按钮⼀般都是⽤此定位⽅式。 |
| static   | 默认值，没有定位，元素出现在正常的文档流中，会忽略 top, bottom, left, right 或者 z-index 声明，块级元素从上往下纵向排布，⾏级元素从左向右排列。      |
| inherit  | 规定从父元素继承 position 属性的值                                                                                                                   |
