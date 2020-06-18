## 1. 字体与段落

### 1.1 字体大小

```css
/*
* 谷歌浏览器默认的文字大小为16px
* 但是不同浏览器可能默认显示的字号大小不一致，我们尽量给一个明确值大小，不要默认大小。一般给body指定整个页面文字的大小
*/
p {
	font-size:20px;
}
```

### 1.2 字体类型

> 1. 中文字体加引号，英文字体有特殊字符或空格时加引号
> 2. 英文字体名必须位于中文字体名之前。
```css
p{
    font-family: Arial,"Microsoft Yahei", "微软雅黑";
}
```

> 尽量使用英文名称或Unicode编码

| 字体名称    | 英文名称        | Unicode 编码         |
| ----------- | --------------- | -------------------- |
| 宋体        | SimSun          | \5B8B\4F53           |
| 新宋体      | NSimSun         | \65B0\5B8B\4F53      |
| 黑体        | SimHei          | \9ED1\4F53           |
| 微软雅黑    | Microsoft YaHei | \5FAE\8F6F\96C5\9ED1 |
| 楷体_GB2312 | KaiTi_GB2312    | \6977\4F53_GB2312    |
| 隶书        | LiSu            | \96B6\4E66           |
| 幼园        | YouYuan         | \5E7C\5706           |
| 华文细黑    | STXihei         | \534E\6587\7EC6\9ED1 |
| 细明体      | MingLiU         | \7EC6\660E\4F53      |
| 新细明体    | PMingLiU        | \65B0\7EC6\660E\4F53 |

### 1.3 字体粗细

```css
h1{
    font-weight: 400px;  /*让标题标签不加粗*/
}
```

| 属性值  | 描述                                                      |
| ------- | :-------------------------------------------------------- |
| normal  | 默认值（不加粗的）                                        |
| bold    | 定义粗体（加粗的）                                        |
| 100~900 | 400 等同于 normal，而 700 等同于 bold  我们重点记住这句话 |

>   我们平时更喜欢用数字来表示加粗和不加粗。

### 1.4 字体斜体

| 属性   | 作用                                                    |
| ------ | :------------------------------------------------------ |
| normal | 默认值，浏览器会显示标准的字体样式  font-style: normal; |
| italic | 浏览器会显示斜体的字体样式。                            |

### 1.5 字体综合设置

> 必须保留font-size和font-family属性，否则font属性将不起作用。

```css
选择器 { 
    font: font-style  font-weight  font-size/line-height  font-family;
}
p {
    font: italic 700 20px/20px SimSun;
}
```

### 1.6 <span id="a1">文本颜色</span>

> 文本颜色表示的几种方式
> 1. 直接写颜色英文名
> 2. 十六进制（#FFBBAA可简写为#FBA其它类似）
> 3. rgb值（rgba最后a 表示透明度，css3才支持）

```css
color: red;
color: #FFF;
color: #FFF000;
color: rgb(255, 1, 1);
color: rgba(0, 0, 0, .1);
```

### 1.7 文本水平对齐

> left, right, center : 左对齐，右对齐，居中对齐

```css
text-align: left;
```

### 1.8 行间距（行高）

> 一般情况下，行距比字号大7.8像素左右就可以了。
> 行高（line-height）= 盒子高度（height） => 文字垂直居中

```css
line-height: 10px;
```

### 1.9 首行缩进

> em表示两个字符

```css
text-indent: 2em;
```

### 1.10 text-decoration

> 取值：
> - none：默认值，应用：取消链接下划线
> - underline：下划线
> - overline：上划线
> - line-through：删除线

```css
text-decoration: none;
```

## 2. 背景

### 背景颜色

> 背景取值请参照上文<a href="#a1">文本颜色</a>

```css
background-color: #FFF;
```

### 背景图片

```css
background-image : url(images/demo.png);
```

### 背景平铺

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/CSS/%E8%83%8C%E6%99%AF%E5%B9%B3%E9%93%BA.png)

> background-repeat取值：
> - repeat：x与y轴都平铺
> - repeat-x：x轴平铺
> - repeat-y：y轴平铺
> - no-repeat：不平铺

```css
background-repeat : repeat;
```

### 背景位置

> background-position取值
> - top, bottom, left, right
> - 10px
> 如果指定一个数值，那则为x坐标，另一个居中，指定两个时，前一个为x坐标，后一个为y坐标
```css
background-position: left top;
```

### 背景附着

> background-attachment取值
> - scroll：当向下翻页时背景滚动
> - fixed：当向下翻页时背景固定

```css
background-attachment: fixed
```

### 背景简写

> background: 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;

```css
background: transparent url(image.jpg) repeat-y  scroll center top;
```

### 背景缩放

> 背景缩放取值如下图

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/CSS/%E8%83%8C%E6%99%AF%E7%BC%A9%E6%94%BE.png)

```css
background-size: 50px;
```