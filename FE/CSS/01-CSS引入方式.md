## 1. 行内式

```html
<div style="color: red; font-size: 12px;">青春不常在，抓紧谈恋爱</div>
```

## 2. 内部样式表

```html
<!--
- style标签一般位于head标签中，当然理论上他可以放在HTML文档的任何地方。
- type="text/css"  在html5中可以省略。
- 只能控制当前的页面
-->
<style>
	div {
		color: red;
		font-size: 12px;
	}
</style>
```

## 3. 外部样式表

```html
<head>
	<link rel="stylesheet" type="text/css" href="css文件路径">
</head>
```