### 1.什么是 Flex 布局

   Flex 是 Flexible Box 的缩写，意为“弹性的盒子”，所以 Flex 布局一般也叫作“Flex 弹性布局”

   任何一个 HTML 元素都可以指定为 Flex 布局

   display: flex | inline-flex;

### 2.什么是 Flex 容器（flex container）

   采用 Flex 布局的元素，称为 Flex 容器



### 3.什么是 Flex 项目（flex item）

   Flex 容器的所有 **子元素** 自动成为容器成员，称为 Flex 项目



### 4.什么是主轴，什么是交叉轴

   默认情况下，水平方向的是主轴，垂直于主轴方向的是交叉轴

   Flex 项目默认**沿主轴起始排列**

![flex](../../assets/flex.png)

### display: flex 

块级元素

### inline-flex;

行内块元素

## Flex**容器的属性**

### flex-direction

flex-direction: row;默认**主轴为横轴**，从左往右排

flex-direction: row-reverse;默认主轴为横轴，从右向左排

flex-direction: column;默认主轴为**纵轴**，从上往下排

flex-direction: column-reverse;默认主轴为纵轴，从下向上排

### flex-wrap 属性（一行排不下）

flex-wrap:nowrap 默认属性，不换行，显示不下就压缩宽度

flex-wrap: wrap换行，原始大小显示

flex-wrap:wrap-reverse 换行，从下往上排

### 3.flex-flow 属性

flex-direction和flexwrap的简写

默认值：row nowrap

```css
flex-flow: row wrap;
```

### 4.justify-content 属性主轴方向

justify-content: flex-start;默认靠左排列



justify-content: flex-end;靠右排列

justify-content: center;水平居中对齐

justify-content: space-between;项目之间的距离相等

justify-content: space-around;项目左右间隔相等，项目之间的间隔是边框的两倍

### align-items交叉轴方向布局

align-items: stretch；默认值，不设置项目高度或auto，会占满交叉轴方向

align-items: flex-start交叉轴起点对齐，

align-items: flex-end;交叉轴终点对齐

align-items: center;居中对齐

align-items: baseline;文字基线对齐，不常用

### align-content 多层的情况

align-content: stretch（默认值）;主轴线平分Flex容器交叉轴方向上的空间

align-content: flex-start;与交叉轴起点对齐

align-content: flex-end;与交叉轴终点对齐

align-content: center;整体居中对齐

align-content: space-between:与交叉轴两端对齐，轴线之问的向隔平均分布

align-content: space-around:每根轴线两侧的问隔都相等，所以轴线之间的间隔比轴线与边框的问隔大一倍

## Flex项目的属性

### order 属性

定义了Flex项目的排列顺序，数值越小，排列越靠前，**默认为0**

order：-1；

### flex-grow属性

> Flex项目在**主轴方向上**的放大比例，**默认为0**,有剩余空间，不放大
>

如果所有项目的flex grow属性都为1,将等分剩余空间(如果有的话)

如果一个项目的 flex grow属性为2,其他项目都为1,2占据的剩余空间将是其他项目点两倍

如果有的Flex项目有flex grow属性，有的项目没有flex grow属性，但有width这样的属性，有flex- grow属性的项目将等分剩余空间

### flex-shrink

> Flex项目在**主轴方向上**的缩小比例，**默认为1**,空间不足，该项目将**缩小**
>

如果所有项目的flex-shrink 属性都一样且不为0,当空间不足时，都将等比例缩小

如果一个项目的 flex-shrink属性为0,其他项目都为1,则空间不足时，前者不缩小

### 4.flex-basis

定义了在分配多余空间之前, Flex项目占据的**主轴大小**(main size)

浏览器根据这个属性，计算主轴是否有多余空间，它的默认值为**auto**,即项目的本来大小

相当于宽度

flex-basis: 400px;

### 5.flex

是flex-grow, flex-shrink和flex-basis的简写，默认值为0 1 auto

该属性有两个快捷值: auto (1.1 auto）：如果有剩余空间就放大，如果空间不足就缩小

和none (0 0 auto)，如果有剩余空间也不放大，如果空间不足也不缩小

### align-self

align-self: auto (默认值)

允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性

auto，继承父属性

align-self：flex-start交叉轴方向起始方向开始排列

align-self：flex-end交叉轴方向终点方向开始排列

align-self：center交叉方向居中排列

align-self：stretch占满交叉轴方向

align-self：baseline基线对齐

### 圣杯布局

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <title>圣杯布局</title>
  <style>
    * {
      padding: 0;
      margin: 0;
    }

    body {
      display: flex;
      /* 纵向排列 */
      flex-direction: column;
      /* 视口高度的100% */
      height: 100vh;

      /* background-color: pink; */
      font-size: 24px;
    }

    .header-layout,
    .footer-layout {
      height: 80px;
    }

    .header-layout {
      background-color: red;
    }

    .footer-layout {
      background-color: yellow;
    }

    .body-layout {
      /* 默认flex-grow是0，不撑开主轴项目 */
      flex-grow: 2;

      display: flex;

      /* align-items: stretch; */
    }

    .main-layout {
      /* 主体部分占用剩余空间 */
      flex-grow: 1;
      background-color: gray;
    }

    .nav-layout {
      width: 200px;
      background-color: green;
      /* 导航写在后面但是也可以通过order排到前面 */
      order: -1;
    }

    .aside-layout {
      width: 200px;
      background-color: lightblue;
    }

    /* 垂直水平居中 */
    .flex-center {
      display: flex;
      /* 文字也可以看做项目 */
      /* 主轴方向居中 */
      justify-content: center;
      /* 交叉轴方向居中 */
      align-items: center;
    }
  </style>
</head>

<body>
  <header class="header-layout flex-center">头部</header>

  <div class="body-layout">
    <!-- 一般程序从上向下加载，主体部分优先，所以写前面 -->
    <main class="main-layout flex-center">主体部分</main>
    <nav class="nav-layout flex-center">导航</nav>
    <aside class="aside-layout flex-center">侧栏</aside>
  </div>

  <footer class="footer-layout flex-center">底部</footer>
</body>

</html>
```

### overflow：auto

如果内容过多可以出滚动条

![img](https://climg.mukewang.com/60d3ee8009db532814511622.png)
