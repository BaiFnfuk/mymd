## CSS3

### 1. 么是CSS

1. css是什么
2. css怎么用
3. css选择器（重点+难点）
4. 美化网页(文字， 阴影， 超链接， 列表， 渐变。。。)
5. 盒子模型
6. 浮动
7. 定位
8. 网页动画（特效效果）

#### 1.1 什么是CSS

Cascading Style Sheet层叠级联样工表

CSS: 表现（美化网页）

字体，颜色，边距，高度，宽度，背景图片，网页定位，网页浮动。。。

#### 1.2 发展史

css1.0

css2.0 DIV(块) + css, html与css结构分离思想，网页变得简单，SEO

css2.1 浮动，定位

css3.0 圆角，阴影，动画。。。浏览器兼容性

#### 1.3 快速入门

```css
<!-- 规范，<style> 可以编写css的代码，没一个声明，最好使用分号结尾
        语法：
            选择器 {
                声明1;
                声明2;
                声明3;
            }
    -->
    <style>
        h1{
           color: cornflowerblue;
        }
    </style>
```

```html
<!-- 引入css文件 -->
<link rel="stylesheet" href="css/index.css">
```

css的优势：

1. 内容和表现分离
2. 网页结构表现统一，可以实现复用
3. 样式十分的丰富
4. 建议使用独立于html的css文件
5. 利用SEO,容易被搜索引擎收录

#### 1.4 css的3种引入方式

`优先级：行内样式 > 内部样式 > 外部样式 `

```html
<!--行内样式：在标签元素中，编写一个style属性-->
    <h1 style="color: coral">我是标题</h1>
```

```html
<!-- 内部样式 -->
<head>
	<style>
    	h1{
        	color: cornflowerblue;
    	}
	</style>
</head>
```

```html
<!-- 外部样式 -->
<link rel="stylesheet" href="css/index.css">
```

### 2. 选择器

```
作用：选择页面上的某一个或者某一类元素
```

#### 2.1 基本选择器

1. 标签选择器: 选中一类标签
2. 类选择器 class： 选中所有class属性一致的标签，跨标签 .类名{}
3. id选择器，id必须是全局唯一，选中id所属的标签， #id{}

```css
/*标签选择器*/
h1{
    color: beige;
}

p{
    font-size: 80px;
}

/*类选择器*/
.h1-class{
    color: aqua;
}

.p-class{
    color: cadetblue;
    align-content: center;
}

/*id选择器*/
#h1-id{
    color: brown;
}

#p-id{
    font-style: italic;
}
```

`优先级：id选择器 > 类选择器 > 标签选择器`

#### 2.2 层次选择器

1. 后代选择器： 在某个元素的后面， 祖爷爷	爷爷	爸爸

   ```css
   /*body标签里的所有p标签*/
   body p{
       background:red;
   }
   ```

2. 子选择器： 一代，儿子

   ```css
   /*body标签的所有子标签*/
   body>p{
       background: cadetblue;
   }
   ```

3. 相邻兄弟选择器：只有一个，向下相邻的第一个兄弟元素

   ```css
   /*相邻兄弟选择器*/
   .p1-class+p{
       background: blueviolet;
   }
   ```

4. 通用选择器：当前选中元素的向下所有兄弟元素（多个）

   ```css
   /*通用兄弟选择器*/
   .p1-class~p{
       background: cornflowerblue;
   }
   ```

#### 2.3 结构伪类选择器

   ```css
   ul li:last-child{
       background: cornflowerblue;
   }
   
   ul li:first-child{
       color: blue;
   }
   ```

  #### 2.4 属性选择器（常用）

```css
/*[属性名] 
[属性名=xx]:等于xx 
[属性名*=xx]：包含xx
[属性名^=xx]:以xx开头
[属性名$=xx]：以xx结尾
*/
a[id=first]{
    background: yellowgreen;
}
```

### 3.美化网页元素

#### 3.1 为什么要美化网页

1. 有效的传递页面信息
2. 美化网页，页面漂亮，才能吸引用户
3. 凸显页面的主题
4. 提高用户体验

span标签

重点要突出的字

```html
<style>
    #title1{
        font-size:50px;
    }
</style>
欢迎学习<span id="title1">java</span>
```

#### 3.2 字体样式

```css
font-familt:楷体;  /*字体，可以设置多个逗号隔开*/
font-size:20px;	  /*字体大小*/
font-weight:bold; /*字体粗细*/
color:#ffffff;	  /*字体颜色*/

```

#### 3.3 文本样式

1. 颜色 color
2. 文本对齐方式 text-aling
3. 首行缩进 text-indent
4. 装饰 

```css
text-align:center; /*排版 居中，左对齐，右对齐*/
text-indent:2em;	/*首行缩进2个空格*/
background:#ff2255;	/*背景色*/
line-height:20px;	/*行高*/
text-decoration:underline;	/*下划线，中划线， 上划线*/
```

#### 3.5超链接伪类

```html
<style>
    /*默认的颜色*/
    a{
        text-decoration:none;
      	color:#000000;
    }
    /*鼠标悬浮的颜色*/
    a:hover{
        color:orange;
    }
    /*鼠标按住未释放的状态*/
    a:active{
        color:green;
    } 
</style>

<a href="#">
	<img src="images/a.jpg" alt"">
</a>
<p>
    <a href="#">码出高效：java开发手册</a>
</p>
<p>
    <a href="#">作者：孤尽老师</a>
</p>
<p>
    ￥99
</p>
```

#### 3.6 列表

```html
<div id="nav">
    <h2 class="title">全部商品分类</h2>
    <ul>
        <li><a href="#">图书</a>&nbsp;&nbsp;<a href="#">音像</a>&nbsp;&nbsp;<a href="#">数字商品</a></li>
        <li><a href="#">家用电器</a>&nbsp;&nbsp;<a href="#">手机</a>&nbsp;&nbsp;<a href="#">数码</a></li>
        <li><a href="#">电脑</a>&nbsp;&nbsp;<a href="#">办公</a></li>
        <li><a href="#">家居</a>&nbsp;&nbsp;<a href="#">家装</a>&nbsp;&nbsp;<a href="#">厨具</a></li>
        <li><a href="#">服饰鞋帽</a>&nbsp;&nbsp;<a href="#">个护化妆</a></li>
        <li><a href="#">礼品箱包</a>&nbsp;&nbsp;<a href="#">钟表</a>&nbsp;&nbsp;<a href="#">珠宝</a></li>
        <li><a href="#">食品饮料</a>&nbsp;&nbsp;<a href="#">保健食品</a></li>
        <li><a href="#">彩票</a>&nbsp;&nbsp;<a href="#">旅行</a>&nbsp;&nbsp;<a href="#">充值</a>&nbsp;&nbsp;<a href="#">票务</a></li>
    </ul>
</div>
```

```css
.title{
    background: orange;
    font-size: 18px;
    font-weight: bold;
    text-indent: 1em;
    line-height: 35px;
}
ul li{
    height: 30px;
    list-style: none;
    text-indent: 1em;
}
a{
    text-decoration: none;
    color: #000000;
    font-size: 14px;
}
a:hover{
    color: orange;
    text-decoration: underline;
}
```

#### 3.7 背景

背景图

```css
div{
    width:1000px;
    height:700px;
    border:1px solid red;
    background-image:url("images/tx.jpg"); /*默认是平铺*/
    background-repeat:repeat-x; /*repeat-x：水平平铺  repeat-y:垂直平铺 no-repeat:不平铺*/
    
}
```

#### 3.8 渐变

```css
body{
    background-image:linear-gradient(19ed, #21D4DFD 0%, #B721FF 100%);
}
```

### 4. 盒子模型

#### 4.1 什么是盒子

1. margin: 外边距

2. padding: 内边距

3. border: 边框

#### 4.2 边框

```css
div{
    border:1px solid blakc; /*边框粗细	边框形状	边框颜色*/
}
```



#### 4.3 内外边距

```css
div{
    margin: 0 auto;	/*居中*/
    margin:1px 1px 1px 1px; /*顺时针 上右下左*/
    padding:1px 1px 1px 1px;
}
```

#### 4.4 圆角边框

4个角

```css
div{
    width:100px;
    height:100px;
    border:10px;
    border-radius:30px; /*圆角*/
   	border-radius: 50px 20px 10px 5px; /*左上 右上 右下 左下*/
}
```

### 5. 浮动

块级元素： 独占一行

```
h1~h6 p div 列表。。。
```

行内元素： 不独占一行

```
span a img strong...
```

行内元素可以被包含在块级元素中，反之则不可以。。。

#### 5.1 display

```css
block 块元素
inline 行内元素
inline-block 是块元素，但是可以内联，在一行
none 不显示
```

1. 这个也是一种实现行内元素排列的方式，但是我们很多情况都是用float

#### 5.2 float

1. 左右浮动 float

   ```css
   float:left;
   float:right;
   ```

2. 父级边框塌陷问题

   ```css
   clear:right; 左侧不允许有浮动元素
   clear:left;	右侧不允许有浮动元素
   clear:both; 两侧都不允许有浮动元素
   clear:none; 不浮动
   ```

   解决方案：

   1. 增加父级元素的调度（不推荐使用）

   2. 增加一个空的div标签，清除浮动

   3. overflow

      ```css
      overflow:hidden;
      ```

   4. 父类添加一个伪类:after

      ```css
      #father:after{
          content:'';
          display:block;
          clear:both;
      }
      ```

      `小结`

      1. 浮动元素后面增加空div

         简单，代码中尽量避免空div

      2. 设置父元素的调度

         简单，元素假设有了固定的高度，就会被限制

      3. overflow

         简单，下拉的一些场景避免使用

      4. 父类添加一个伪类：after(推荐)

         写法稍微复杂一点，但是没有副作用。

#### 5.3 对比

- display

  方向不可以控制

- float

  浮动起来的话会脱离标准文档流，所以要解决父级边框塌陷的问题

### 6. 定位

#### ６.1 相对定位

```css
position: relative;
top: -20px;
left: 20px;
```

相对定位：position:relative;

相对于原来的位置，进行指定的偏移,相对定位后的话，它仍然在标准文档流中，原来的位置会被保留

```css
position:relative;
top:-20px;
left:20px;
bottom:-10px;
right:20px;
```

#### 6.2  绝对定位

定位：基于xx定位，上下左右

1. 没有父级元素定位的前提下，相对于浏览器定位

2. 假设父级元素存在定位，我们通常会相对于父级元素进行偏移

3. 在父级元素范围内移动

   相对于父级或浏览器的位置，进行指定的偏移，绝对定位的话，它不在标准文档流中，原来的位置不会被保留

```css
position: absolute;
top: 20px;
left: 50px;
```



#### 6.3 固定定位fixed

位置固定不会动

```css
position: fixed;
top: 20px;
left: 50px;
```

#### 6.4 z-index

z-index:默认是0，最高无限

```css
z-index:0;	/*相当于图层，数值越大越前面*/
z-index:999;
```

### 7. 动画

