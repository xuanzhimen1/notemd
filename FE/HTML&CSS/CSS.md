## 1. CSS的三种引入方式
- 行内引入
	```html
	<div style="color: red; font-size: 12px;">青春不常在，抓紧谈恋爱</div>
	```
- 内部样式表
	```html
	<head>
	    <style>
	    	 div {
	    	 	color: red;
	    	 	font-size: 12px;
	    	 }
	    </style>
	</head>
	<body>
		<div></div>
	</body>
	```
- 外部样式表
	```html
	<head>
		<link rel="stylesheet" type="text/css" href="css文件路径">
	</head>
	```
## 2. CSS样式
### 2.2 文字CSS
- 文字
	```css
  /*字体颜色*/  
	color: #06c; /* #06c相当于#0066cc 颜色可用单词表示如:green */  
	/*字体大小*/  
	font-size: 10px;  
	/*字体粗细*/  
	font-weight: 400; /* 400正常, 700加粗 */ /*字体类型*/  
	font-family: Arial,serif;  
	/*倾斜*/  
	font-style: italic; /* font-style: normal; 使字体不倾斜 */ /*行高*/  
	line-height: 20px;  
	/*首行缩进*/  
	text-indent: 2em;  
	/*字体综合写法  不可更换顺序, `font-size`, `font-family`不可省略, 其它全部可以省略 */   
	font: font-style font-weight font-size/line-height font-family 
	font: italic 700 20px "黑体";
	/*对齐方式*/  
	text-align: center; /* left, right, center */  
	/*文本装饰*/  
	text-decoration: none; /*去掉超链接的下划线*/  
	/*单行文本垂直居中*/  
	line-height: 20px;  
	height: 20px;  
	```
	
### 2.2 背景CSS
- 背景
	```css
	/*背景为一张图片*/  
	background: url(器"logo.png");  
	/*背景为颜色*/  
	background-color: red;  
	/*背景平铺*/  
	background-repeat: no-repeat; /*no-repeat, repeat-x, repeat-y*/  
	/*背景位置 值为:像素, top, right, bottom, left, center*/  
	background-position: 0 0;  
	background-position: right top;  
	background-position: center;  
	/*背景固定*/  
	background-attachment: fixed; /*不随页面滚动而滚动*/  
	/*背景透明度*/  
	background: rgba(0, 0, 0, .3);
	```
- 精灵图标作为背景

  > 此时将图片上移50px左移50px，再截取大小为5×5的左上角的图片展示

  ```css
  width: 5px;
  height: 5px;
  background: url(../img/logo.png) no-repeat -50px -50px;
  ```

  

### 2.3 盒子CSS
- css3盒子阴影与圆角
	```css
	/*盒子阴影*/  
	box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, .3); /*上下,左右,模糊距离,阴影大小*/
	/*圆角边框*/
	border-radius: 50%;
	```
- 盒子边框
    ```css
    /*边框*/
    border: 1px solid red;  /*solid:实线, dashed:虚线, dotted:点线*/
    /*圆角边框*/
	border-radius: 50%;
	border-radius: 0 0 0 10px;  /*左上角 右上角 右下角 左下角*/
	/*每个边边框分别定义*/
	border-top: 1px solid red;  /*border-top, border-right, border-bottom, border-left*/
    ```
- 边框合并
	```css
	/*两个边框合并为一个边框*/
	border-collapse: collapse
	```
- 盒子内边距
	```css
	/*盒子内边距*/
	padding-left: 10px;  /*padding-left, padding-right, padding-top, padding-bottom*/
	/*四个边距简写*/
	padding: 10px;  /*四个边距都为10px*/
    padding: 10px 20px;  /*左右10px, 上下20px*/
    padding: 10px 20px 30px;  /*上10px, 左右20px, 下30px*/
    padding: 10px 20px 30px 40px;  /*上右下左(顺时针)分别为10, 20, 30, 40*/
	```
- 盒子外边距
	```css
	/*盒子外边距*/
	margin-left: 10px;
	/*其余类同于padding*/
	```
- 块级盒子水平居中
	> 前提: 块级元素有宽度
	```css
  margin-left: auto;
  margin-right: auto;
	```
- 滚动条显示超出盒子的内容
	```css
	overflow: hidden; /*auto| scroll*/
	```
- 清除元素默认的内外边距, 是css的第一句代码
	```css
	* {
	    padding: 0;
	    margin: 0;
	}
	```
### 2.4 列表CSS
- 列表样式
	```css
	/*去除列表样式*/  
	li {  
	    list-style: none;  
	}
	```
### 2.5 过渡CSS

- 过渡动画（对某个元素的某个属性生效）

  ```css
  /*transition: 要过渡的属性 花费时间(默认0) 运动曲线(linear, ease(默认), ease-in, ease-out, ease-in-out) 何时开始(默认0)*/
  /* 当元素的宽度变化后执行如下动画效果 */
  transition: width 0.5s ease 1s;
  /* 所有属性都变化用all 就可以了  后面俩个属性可以省略 */
  transition: all .5s;
  ```

### 2.6 获得焦点元素

- 获得input元素的焦点

  ```css
  .total input {
    border: 1px solid #ccc;
    height: 30px;
    width: 40px;
    transition: all .5s;
  }
  .total input:focus {
    width: 80px;
    border: 1px solid skyblue;
  }
  ```

## 3. 选择器
- 标签选择器
	```css
	h3 {
		font-weight: 400
	}
	```
- 类选择器
	```html
	<p class='class1 class2'></p>
	<style>
		.class1 {  
		    color: blue;  
		}  
		.class2 {  
		    font-size: 20px;  
		}
	</style>
	```
- id选择器
	```html
	<p id="id1"></p>
	<style>
		#id1 {  
		    color: blue;  
		}
	</style>
	```
- 通配符选择器
	```css
	* {  
	    text-decoration: none;  
	}
	```
- 后代选择器(子孙后代)
    ```css
    /*.class1中所有h3*/
    .class1 h3 { 
	    font-weight: 400;
	    font-size: 16px;
    }
    ```
-   子元素选择器(儿子)
    ```css
    .class1 > h3 {
      	    font-weight: 400;
	    font-size: 16px;
    }
    ```
-   交集选择器
    > 两个选择器中间没有空格
    ```css
    h3.special {
		color:red;
	}
    ```
-   并集选择器
    > 两个选择器中间用`","`隔开
    ```css
    h3, .special {
		color: red;
	}
    ```
-   链接伪类选择器(重点)
    > 按照 lvha 的顺序, 不可颠倒
    ```css
    a:link {/*未访问链接*/}  
	a:visited{/*访问边的链接*/}  
	a:hover{/*鼠标移动到链接上*/}  
	a:active{/*选定的链接*/}
    ```
- 获得焦点元素 :focus伪类

  ```css
  /*这个input 获得了焦点*/
  .total input:focus {
    width: 80px;
    border: 1px solid skyblue;
  }
  ```

  

## 4. 标签的显示模式
- 标签分类
	- 块元素
		
		- `<h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>`
	- 行内元素
		
		- `<a>、<strong>、<b>、<em>、<i>、<del>、<s>、<ins>、<u>、<span>`
	- 行内块元素
		
		- `<img />、<input />、<td>`
	- 总结
	    | 元素模式   | 元素排列               | 设置样式               | 默认宽度         | 包含                     |
	    | ---------- | ---------------------- | ---------------------- | ---------------- | ------------------------ |
      | 块级元素   | 一行只能放一个块级元素 | 可以设置宽度高度       | 容器的100%       | 容器级可以包含任何标签   |
      | 行内元素   | 一行可以放多个行内元素 | 不可以直接设置宽度高度 | 它本身内容的宽度 | 容纳文本或则其他行内元素 |
      | 行内块元素 | 一行放多个行内块元素   | 可以设置宽度和高度     | 它本身内容的宽度 |                          |
  
- 标签显示模式的转换display
	- 块转行内：`display:inline;`
	- 行内转块：`display:block;`
	- 块、行内元素转换为行内块： `display: inline-block;`
## 5. CSS的权重
- 权重
    | 标签选择器             | 计算权重公式 |
    | ---------------------- | ------------ |
    | 继承或者 *             | 0,0,0,0      |
    | 每个元素（标签选择器） | 0,0,0,1      |
    | 每个类，伪类           | 0,0,1,0      |
    | 每个ID                 | 0,1,0,0      |
    | 每个行内样式 style=""  | 1,0,0,0      |
    | 每个!important  重要的 | ∞ 无穷大     |
- important的使用: `color: red!important`
- 权重叠加
    - div ul  li   ------>      0,0,0,3
    - .nav ul li   ------>      0,0,1,2
    - a:hover      -----—>   0,0,1,1  伪类可以理解为类选择器
    - .nav a       ------>      0,0,1,1
- 继承的权重为 0, 0, 0, 0
## 6. 浮动
- 浮动
	```css
	选择器 {float: 属性值}
	```
	| 属性值    | 描述                     |
	| --------- | ------------------------ |
	| none  | 元素不浮动（**默认值**） |
	| left | 元素向**左**浮动         |
	| right | 元素向**右**浮动         |
- 清除浮动
	```css
	选择器 {clear: 属性值;}
	```
  | 属性值 | 描述                                       |
  | ------ | ------------------------------------------ |
  | left   | 不允许左侧有浮动元素（清除左侧浮动的影响） |
  | right  | 不允许右侧有浮动元素（清除右侧浮动的影响） |
  | both   | 同时清除左右两侧浮动的影响                 |

- 使用after伪元素清除浮动
    ```css
    .clearfix:after {  content: ""; display: block; height: 0; clear: both; visibility: hidden;  }   
    .clearfix {*zoom: 1;}   /* IE6、7 专有 */
    ```
- 使用双伪元素清除浮动
    ```css
    .clearfix:before,.clearfix:after { 
        content:"";
        display:table; 
    }
    .clearfix:after {
        clear:both;
    }
    .clearfix {
        *zoom:1;
    }
    ```
- 浮动可以清除行内块元素之间的空白缝隙

  > 如input搜索框与后面搜索button之间的缝隙

## 7. 定位
- 定位
	```css
	/*定位属性*/
	position: static;
	```
	|值|位置|
	|--|--|
	| static | 所有元素默认static， 标准流 |
	| relative | 相对定位：元素相对于本元素原来的位置的偏移量，占有原来位置 |
	| absolute | 绝对定位：元素相对于父元素位置的偏移量（父级没有定位则以父父级有定位的元素为准），不占有位置 |
	| fixed | 固定定位：元素相对于浏览器位置，不占位置 |
	> **总结： 子绝父相**
- 边偏移
	```css
	top: 80px
	```
	| 边偏移属性 | 示例 |
	|--|--|
	| top | top: 80px |
	| bottom | bottom: 80px |
	| left | left: 80px |
	| right | right: 80px |
- 绝对定位盒子居中
	> 绝对定位用`margin: auto;` 不能实现居中
	
	```css
	.xxx {
		position: absolute;
		left: 50%;
		margin-left: -100px; /*盒子自身元素宽度的一半*/ 
	}
	```
- 堆叠顺序
	> 整数， 负数，0，正数， 数值越大，越在上面
	```css
	.xxx {
		z-index: 2;
	}
	```

## 8. CSS高级技巧

### 8.1 元素的显示与隐藏

- display

  > 特点： 隐藏之后，不再保留位置。
  >
  > 实际开发场景：配合后面js做特效，比如下拉菜单，原先没有，鼠标经过，显示下拉菜单， 应用极为广泛
  >

  ```css
  display: none /*隐藏对象*/
  display：block  /*除了转换为块级元素之外，同时还有显示元素的意思。*/
  ```

![image-20200217172404720](D:\note\FE\HTML&CSS\media\image-20200217172404720.png)

- visibility 

  > 特点： 隐藏之后，继续保留原有位置。（停职留薪）

  ```css
  visibility：visible; 　对象可视
  visibility：hidden; 　  对象隐藏
  ```

  ![image-20200217172502270](D:\note\FE\HTML&CSS\media\image-20200217172502270.png)

- overflow 

  > 实际开发场景：
  >
  > 1. 清除浮动
  > 2. 隐藏超出内容，隐藏掉,  不允许内容超过父盒子。


| 属性值      | 描述                                       |
| ----------- | ------------------------------------------ |
| **visible** | 不剪切内容也不添加滚动条                   |
| **hidden**  | 不显示超过对象尺寸的内容，超出的部分隐藏掉 |
| **scroll**  | 不管超出内容否，总是显示滚动条             |
| **auto**    | 超出自动显示滚动条，不超出不显示滚动条     |

   ![image-20200217172934969](D:\note\FE\HTML&CSS\media\image-20200217172934969.png)

- 隐藏总结

  | 属性           | 区别                   | 用途                                                         |
  | -------------- | ---------------------- | ------------------------------------------------------------ |
  | **display**    | 隐藏对象，不保留位置   | 配合后面js做特效，比如下拉菜单，原先没有，鼠标经过，显示下拉菜单， 应用极为广泛 |
  | **visibility** | 隐藏对象，保留位置     | 使用较少                                                     |
  | **overflow**   | 只是隐藏超出大小的部分 | 1. 可以清除浮动 2. 保证盒子里面的内容不会超出该盒子范围      |

### 8.2 CSS用户界面样式

- 鼠标样式cursor

  ```css
  <ul>
    <li style="cursor:default">我是小白</li>
    <li style="cursor:pointer">我是小手</li>
    <li style="cursor:move">我是移动</li>
    <li style="cursor:text">我是文本</li>
    <li style="cursor:not-allowed">我是文本</li>
  </ul>
  ```

- 轮廓线outline

  ![image-20200217173651087](D:\note\FE\HTML&CSS\media\image-20200217173651087.png)

  ```css
  我们平时都是去掉的
  outline: 0;   或者  outline: none;
  ```

- 防止拖拽文本域resize

  ![image-20200217173711005](D:\note\FE\HTML&CSS\media\image-20200217173711005.png)

  ```css
  <textarea  style="resize: none;"></textarea>
  ```

- 总结

  | 属性         | 用途                 | 用途                                                         |
  | ------------ | -------------------- | ------------------------------------------------------------ |
  | **鼠标样式** | 更改鼠标样式cursor   | 样式很多，重点记住 pointer                                   |
  | **轮廓线**   | 表单默认outline      | outline 轮廓线，我们一般直接去掉，border是边框，我们会经常用 |
  | 防止拖拽     | 主要针对文本域resize | 防止用户随意拖拽文本域，造成页面布局混乱，我们resize:none    |

### 8.3 vertical-align 垂直对齐

- 有宽度的块级元素水平居中，是margin: 0 auto;

- 让文字水平居中，是 text-align: center;

- 让文字垂直居中，（vertical-align : baseline |top |middle |bottom ）

  > vertical-align 不影响块级元素中的内容对齐，它只针对于**行内元素**或者**行内块元素**，
  >
  > 特别是行内块元素， **通常用来控制图片/表单与文字的对齐**。
  >
  > ![image-20200217174421561](D:\note\FE\HTML&CSS\media\image-20200217174421561.png)

- 让块级元素垂直居中，是行高=高度

### 8.4 去除图片及表单空白缝隙

- 方法一：给img vertical-align:middle | top| bottom等等。  让图片不要和基线对齐。
- 方法二：给img 添加 display：block; 转换为块级元素就不会存在问题了。

### 8.5 溢出的文字省略号显示

- css代码

    ```css
    /*1. 先强制一行内显示文本*/
    white-space: nowrap;
    /*2. 超出的部分隐藏*/
    overflow: hidden;
    /*3. 文字用省略号替代超出的部分*/
    text-overflow: ellipsis;
    ```

### 8.6 CSS精灵技术（sprite) 

- 作用：一次请求一张图片，图片上有多个图标，避免多次向服务器请求图片

- 使用：

  ```html
  
  ```

### 8.7 滑动门背景

![image-20200217180907467](D:\note\FE\HTML&CSS\media\image-20200217180907467.png)

- 使用

  ```html
  <li>
    <a href="#">
      <span>导航栏内容</span>
    </a>
  </li>
  ```

  ```css
  li a {
      padding-left: 16px;
      height: 33px;
      line-height: 33px;
      background: url(./images/to.png) no-repeat left;
  }
  li a span {
      padding-right: 16px;
      height: 33px;
      display: inline-block;
      background: url(./images/to.png) no-repeat right;
  }
  ```

- 原理
  - a 设置 背景左侧，padding撑开合适宽度。    
  - span 设置背景右侧， padding撑开合适宽度 剩下由文字继续撑开宽度。
  - 之所以a包含span就是因为 整个导航都是可以点击的。

### 8.8 margin负值之美

- 负边距+定位：水平垂直居中

  绝对定位盒子，父级盒子的(margin-left或margin-top)50%，  然后(left或top)自己宽度的一半 ，可以实现盒子水平垂直居中

  ```css
  /* 绝对定位盒子水平居中 */
  left: 50%;
  margin-left: 盒子宽度/2;
  /* 绝对定位盒子垂直居中 */
  top: 50%;
  margin-top: 盒子高度/2;
  ```

- 压住盒子相邻边框

  ```css
  margin-left: -1px;  /*边框为1px*/
  div:hover {  /*突出显示*/
      border: 1px solid red;
      z-index: 1;
  }
  ```

### 8.9 CSS三角形

- 代码

  ```css
  div {
      width: 0;
      height: 0;
      border-style: solid;
      border-width:10px;
      border-color: red transparent transparent transparent;  /*上三角，其它三角同理*/
      /*为了照顾兼容性 低版本的浏览器，加上*/
      font-size: 0;  
      line-height: 0;
  }
  ```

  ![image-20200217184853696](D:\note\FE\HTML&CSS\media\image-20200217184853696.png)



## 9. 全局样式

- base.css

  ```css
  
  ```

## 10. CSS3

### 10.1 过渡

- 作用：盒子过渡动画：是从一个状态 渐渐的过渡到另外一个状态。可以让我们页面更好看，更动感十足

- 使用：

  > 我们现在经常和 :hover 一起 搭配使用
  >
  > 默认值：
  >
  > ​	花费时间：0； 运动曲线：ease（逐渐慢）；何时开始：0；
  >
  > 运行曲线：linear（匀速） ease（渐慢）ease-in（渐快）ease-in-out(先加速后减速)

  ```css
  .tran {
      width: 199px;
      height: 99px;
      background-color: pink;
      /* transition: 要过渡的属性  花费时间  运动曲线  何时开始; */
      transition: width 3s linear 1s, height 1s ease 0s;
      /*transition: all 6s;  !* 所有属性都变化用all 就可以了  后面俩个属性可以省略 *!*/
  }
  .tran:hover {  /* 鼠标经过盒子，我们的宽度变为399 */
      width: 599px;
      height: 299px
  }
  ```

### 10.2 CSS3选择器

- 属性选择器

  >类选择器、属性选择器、伪类选择器权重为10

  ![image-20200218080746213](D:\note\FE\HTML&CSS\media\image-20200218080746213.png)

- 结构伪类选择器

  > 第n个元素可以为数字  也可以为 even（偶数）odd（奇数）

  ![image-20200218081244770](D:\note\FE\HTML&CSS\media\image-20200218081244770.png)

  ![image-20200218081604884](D:\note\FE\HTML&CSS\media\image-20200218081604884.png)

- 伪元素选择器

  ![image-20200218091432977](D:\note\FE\HTML&CSS\media\image-20200218091432977.png)

  - 使用

    ```css
    div::before {
        content: "我";
        display: inline-block;
        width: 100px;
        height:100px;
    }
    /*after同理*/
    ```

    

### 10.3 CSS3 2D转换

- 移动：translate

  > translate最大的优点：不会影响其它盒子的位置

  ```css
  transform: translate(100px, 50%)  /*盒子向右100px，向下盒子高度的50%*/
  ```

- 旋转：rotate

  ```css
  transform: rotate(45deg)  /*盒子顺时针旋转45度*/
  ```

- 缩放：scale

  > scale最大的优点：不会影响其它盒子的位置

  ```css
  transform: scale(2, 0.5)  /*宽变为2倍，高度变为0.5倍*/
  transform: scale(2)  /*等比例缩放2倍*/
  ```

- 修改转换基点

  ```css
  transform-origin: left bottom  /*以左下角为中心旋转  默认为50% 50% (中心位置)*/
  ```

- 2D转换综合写法

  ```css
  transform: translate(100px, 50%) rotate(45deg) scale(2, 0.5)
  ```

  

### 10.4 CSS3 3D

- 3D写法

  ```css
  body {
      /* 透视写到被观察元素的父盒子上面 */
      perspective: 200px;
  }
  
  div {
      width: 200px;
      height: 200px;
      background-color: pink;
      transform: translate3d(400px, 100px, 100px);
  }
  ```

- 旋转

- 

  ```css
  img {
      display: block;
      margin: 100px auto;
      transition: all 1s;
  }
  
  img:hover {
      transform: rotateX(180deg);  /*rotateX沿着x轴旋转， rotateY, rotateZ同理*/
  }
  ```

- 3D呈现 transform-style

  ![image-20200219153506942](D:\note\FE\HTML&CSS\media\image-20200219153506942.png)

  ```css
  body {
      perspective: 500px;
  }
  
  .box {
      position: relative;
      width: 200px;
      height: 200px;
      margin: 100px auto;
      transition: all 2s;
      /* 让子元素保持3d立体空间环境 */
      transform-style: preserve-3d;
  }
  
  .box:hover {
      transform: rotateY(60deg);
  }
  
  .box div {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: pink;
  }
  
  .box div:last-child {
      background-color: purple;
      transform: rotateX(60deg);
  }
  ```

  

  

### 10.5 CSS3动画

- 定义并调用动画

  动画从0~100%

  ```css
  @keyframes move {
      定义动画
      /* 开始状态 */
      0% {
          transform: translateX(0px);
      }
      /* 结束状态 */
      100% {
          transform: translateX(1000px);
      }
  }
  
  div {
      width: 200px;
      height: 200px;
      background-color: pink;
      /* 2. 调用动画 */
      /* 动画名称 */
      animation-name: move;
      /* 持续时间 */
      animation-duration: 2s;
  }
  ```

  动画从0到25%到50%到75%到100%

  ```css
  /* 动画序列 */
  /* 1. 可以做多个状态的变化 keyframe 关键帧 */
  /* 2. 里面的百分比要是整数 */
  /* 3. 里面的百分比就是 总的时间（我们这个案例10s）的划分 25% * 10  =  2.5s */
  @keyframes move {
      0% {
          transform: translate(0, 0);
      }
      25% {
          transform: translate(1000px, 0)
      }
      50% {
          transform: translate(1000px, 500px);
      }
      75% {
          transform: translate(0, 500px);
      }
      100% {
          transform: translate(0, 0);
      }
  }
  div {
      width: 100px;
      height: 100px;
      background-color: pink;
      animation-name: move;
      animation-duration: 10s;
  }
  ```

  > animation简写：
  >
  > animation: name duration timing-function delay iteration-count direction
  >
  > 如：animation: move 2s linear 0s 1 alternate forwards;

- 动画常用属性

  ![image-20200219151328520](D:\note\FE\HTML&CSS\media\image-20200219151328520.png)

  ![image-20200219152235550](D:\note\FE\HTML&CSS\media\image-20200219152235550.png)

























































