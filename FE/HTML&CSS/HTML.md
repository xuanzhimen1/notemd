
## 骨架标签

```html
<!DOCTYPE html>  <!--HTML文件必须加上 DOCTYPE 声明，并统一使用 HTML5 的文档声明-->
<html lang="en">  <!--指定html 语言种类 en：英语，zh-CN：中文-->
	<head>
		<meta charset="UTF-8" />  <!--这句话是让 html 文件是以 UTF-8 编码保存的， 浏览器根据编码去解码对应的html内容-->
		<title></title>
	</head>
	<body>
	</body>
</html>
```



## 排版标签

### 标题标签

```html
<h1>一级标题</h1> <!--h1, h2, h3, h4, h5, h6-->
```

### 段落标签

```html
<p>段落标签</p>
```

### 换行标签

```html
<br /> <!--换行标签-->
```

### 水平线标签

```html
<hr />
```

### 盒子标签

```html
<div></div>  <!--块元素-->
<span></span>  <!--行内元素-->
```

### 加粗斜体删除线下划线标签

```html
<b></b><strong></strong> <!--加粗-->
<i></i><em></em> <!--斜体-->
<s></s><del></del> <!--删除线-->
<u></u><ins></ins> <!--下划线-->
```

### 图像标签

```html
<img src="图片地址" title="鼠标悬停时的文字提示" alt=“图片不存在时显示文字” />
```

### 链接标签

```html
<a href="url地址" target="目标窗口的弹出方式取值为 _self(当前页打开)， _blank(在新窗口打开)"></a>
```

## 表格、列表、表单

### 表格标签

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/HTML/%E8%A1%A8%E6%A0%BC%E6%A0%87%E7%AD%BE.png)

```html
<!-- * 跨行合并：rowspan="合并单元格的个数" * 跨列合并：colspan="合并单元格的个数" -->
<table width="500" height="200" border="1" cellpadding="20" cellspacing="0" align="center">
    <caption>我是表格标题</caption>
    <thead>
    <tr>
        <th>姓名</th>
        <th>性别</th>
        <th>年龄</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>刘德华</td>
        <td>男</td>
        <td rowspan="2">52</td>
    </tr>
    <tr>
        <td>郭富城</td>
        <td>男</td>
    </tr>
    <tr>
        <td>滋润</td>
        <td colspan="2">无</td>
    </tr>
    </tbody>
    <tfoot>
    <tr>
        <td>合计</td>
        <td>男/女</td>
        <td>52</td>
    </tr>
    </tfoot>
</table>
```



表格标签属性

  ![表格属性](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/HTML/%E8%A1%A8%E6%A0%BC%E6%A0%87%E7%AD%BE%E5%B1%9E%E6%80%A7.jpg)   

### 列表标签

无序列表

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/HTML/%E6%97%A0%E5%BA%8F%E5%88%97%E8%A1%A8%E6%A0%87%E7%AD%BE.png)

```html
<!--
1. ul中只能嵌套li，直接在ul标签中输入其他标签或者文字的做法是不被允许的。
2. li相当于一个容器，可以容纳所有元素。
-->
<ul>
    <li>列表项1</li>
    <li>列表项2</li>
    <li>列表项3</li>
</ul>
```

有序列表

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/HTML/%E6%9C%89%E5%BA%8F%E5%88%97%E8%A1%A8%E6%A0%87%E7%AD%BE.png)

```html
<ol>
    <li>列表项1</li>
    <li>列表项2</li>
    <li>列表项3</li>
</ol>
```

自定义列表

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/HTML/%E8%87%AA%E5%AE%9A%E4%B9%89%E5%88%97%E8%A1%A8.png)

```html
<dl>
    <dt>名词1</dt>
    <dd>名词1解释1</dd>
    <dd>名词1解释2</dd>
    <dt>名词2</dt>
    <dd>名词2解释1</dd>
    <dd>名词2解释2</dd>
</dl>
```

### 表单标签

#### input控件

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/HTML/%E8%A1%A8%E5%8D%95%E6%8E%A7%E5%88%B6input%E5%8F%8Alable%E7%BB%91%E5%AE%9Ainput.gif)

```html
<input type="text" name="username" value="input控件默认值" placeholder="开始搜索吧！">
男
<input type="radio" name="sex" checked="checked"/>
<label for="sexName">女</label> <!--当我们鼠标点击 label标签里面的文字时， 光标会定位到指定的表单里面-->
<input type="radio" name="sex" id="sexName"/>
```

input控件属性

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/HTML/%E8%A1%A8%E5%8D%95%E6%A0%87%E7%AD%BE-input%E6%8E%A7%E5%88%B6%E5%B1%9E%E6%80%A7.jpg)

#### textarea控件(文本域)  

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/HTML/%E8%A1%A8%E5%8D%95%E6%8E%A7%E4%BB%B6-textarea%E6%96%87%E6%9C%AC%E5%9F%9F.png)

```html
<textarea> 文本内容 </textarea>
```

#### select下拉控件

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/HTML/%E4%B8%8B%E6%8B%89%E6%8E%A7%E4%BB%B6select.gif)

```html
<select>
    <option selected=" selected ">选项1</option>
    <option>选项2</option>
    <option>选项3</option>
</select>
```

#### form表单域

```html
<form action="url地址" method="提交方式:get/post" name="表单名称">
    各种表单控件
</form>
```

## 锚点定位标签

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/HTML/%E9%94%9A%E7%82%B9%E5%AE%9A%E4%BD%8D.gif)

```html
<h3 id="two">第2集</h3>
<a href="#two">点击此处跳转到第2集</a>
```

## 全局定义链接打开方式

可以设置整体链接的打开状态，

`_blank`统一在新页面打开

`_self`统一在当前页打开

```html
<base target="_blank" />
```

## 预格式化文本标签

![](https://xuanzhimennodemd.oss-cn-beijing.aliyuncs.com/HTML/%E9%A2%84%E6%A0%BC%E5%BC%8F%E5%8C%96%E6%96%87%E6%9C%AC%E6%A0%87%E7%AD%BE.png)

```html
<!--按照我们预先写好的文字格式来显示页面， 保留空格和换行等。 -->
<pre>
    此例演示如何使用 pre 标签 
    
    对空行和 空格 
    进行控制 
</pre>
```



































