## 1. 显示与隐藏

- 盒子转为**块**元素

	```css
	display: block;
	```

- 盒子转为**行内**元素

	```css
	display: inline;
	```

- 盒子转为**行内块**元素

	```css
	display: inline-block;
	```

- 盒子转为**隐藏**元素

	> 不占有原来盒子位置
	
	```css
	display: none;
	```

- 盒子转为**隐藏**元素

	> 占有原来盒子位置
	
	```css
	visibility: hidden;
	```
	
## 2. 元素溢出

> overflow取值如下
> - visible：溢出盒子显示（默认）
> - hidden：超出盒子部分内容隐藏
> - scroll：总显示滚动条
> - auto：超出盒子时显示滚动条

```css
overflow: visible;
```