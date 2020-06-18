## 1. 标签选择器

```css
h3 {
	color: red;
}
```

## 2. 类选择器

```html
<style>
	.blue {
		color: blue;
		font-size: 100px;
	}
	.red {
		color: red;
		font-size: 100px;
	}
	.orange {
		color: orange;
		font-size: 100px;
	}
	.green {
		color: green;
		font-size: 100px;
	}
</style>
<span class="blue">G</span>
<span class="red">o</span>
<span class="orange">o</span>
<span class="blue">g</span>
<span class="green">l</span>
<span class="red">e</span>
```

## 3. id选择器

```html
<style>
	#idName {
		color: green;
		font-size: 100px;
	}
</style>
<p id="idName"></p>
```

## 4. 通配符选择器

```css
* {
	margin: 0;                    /* 定义外边距*/
	padding: 0;                   /* 定义内边距*/
}
```

## 5. 后代选择器

> 子孙后代都可以这么选择。 或者说，它能选择任何包含在内 的标签。

```css
.class h3{color:red;font-size:16px;}
```

## 6. 子元素选择器

> 这里的子 指的是 亲儿子  不包含孙子 重孙子之类。

```css
.class>h3{color:red;font-size:14px;}
```

## 7. 交集选择器

> 两个选择器之间**不能有空格**，如h3.special。
> 比如：   p.one   选择的是： 类名为 .one  的 段落标签。

```css
h3.special{color: red; font-size:25px;}
```

## 8. 并集选择器

```css
/*表示   .one 和 p  和 #test 这三个选择器都会执行颜色为红色。 */
.one, p , #test {color: #F00;}
```

## 9. 链接伪类选择器

> - a:link      /* 未访问的链接 */
> - a:visited   /* 已访问的链接 */
> - a:hover     /* 鼠标移动到链接上 */
> - a:active    /* 选定的链接 */

> 写的时候，他们的顺序尽量不要颠倒  按照  lvha 的顺序。否则可能引起错误。

```css
a {   /* a是标签选择器  所有的链接 */
	font-weight: 700;
	font-size: 16px;
	color: gray;
}
a:hover {   /* :hover 是链接伪类选择器 鼠标经过 */
	color: red; /*  鼠标经过的时候，由原来的 灰色 变成了红色 */
}
```