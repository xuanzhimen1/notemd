## 1. XML概述
- Extensible Markup Language 可扩展标记语言
	- 可扩展: 标签都是自定义的
- 功能: 
	- 存储数据
		1. 配置文件
		2. 在网络中传输
- xml与html的区别
	- xml标签是自定义的, html标签是预定义的
	- xml语法严格, html语法松散
	- xml存储数据, html展示数据


## 2. XML语法

- HelloWorld示例
	```xml
	<?xml version="1.0" ?>
	<users>
		<user id="1">
			<name>zhangsan</name>
		</user>
	</users>
	```

- 组成部分
	- 文档声明
		- <?xml 属性列表 ?>
			- 属性1: version: 版本号必须的属性
			- 属性2: encoding: 编码方式,默认ISO
			- 属性3: standalone: 是否独立于其它文件
	- 标签
		- 自定义标签
	- 属性
		- id属性值唯一
	- 文本 即标签体内容
		- CDATA区: 标签体中在该区域中的数据会被原样展示  <![CDATA[]]>

- 约束: 规定xml的文档书写规则
	- DTD约束：
		- 内部dtd：将约束规则定义在xml文档中
		- 外部dtd：将约束的规则定义在外部的dtd文件中
			- 本地：<!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">
			- 网络：<!DOCTYPE 根标签名 PUBLIC "dtd文件名字" "dtd文件的位置URL">
	- Schema约束:
		1. 填写xml文档的根元素
		2. 引入xsi前缀.  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		3. 引入xsd文件命名空间.  xsi:schemaLocation="http://www.itcast.cn/xml  student.xsd"
		4. 为每一个xsd约束声明一个前缀,作为标识  xmlns="http://www.itcast.cn/xml" 

		示例: 
			`<students xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
				xmlns="http://www.itcast.cn/xml"
				xsi:schemaLocation="http://www.itcast.cn/xml  student.xsd">`


## 3. XML解析
- Jsoup： HTML解析器，可直接解析某个URL地址，HTML文本内容
- 使用步骤：
	1. 导入jar
	2. 获取Document对象
	3. 获取对应的标签Element对象
	4. 获取数据
- 代码示例
	```java
	// 1. 获取student.xml的path
	String path = JsoupDemo1.class.getClassLoader().getResource("student.xml").getPath();
	// 2. 解析xml文档,加载文档进内存,获取dom树
	Document document = Jsoup.parse(new File(path), "utf-8");
	// 3. 获取元素对象
	Elements elements = document.getElementsByTags("name");
	System.out.println(elements.size());
	Element element = elements.get(0);
	// 4. 获取元素数据
	String name = element.text();
	System.out.println(name);
	```

- Jsoup对象的使用：
	- Jsoup：工具类，可以解析html或xml文档，返回Document
		- parse：解析html或xml文档，返回Document
			- parse​(File in, String charsetName)：解析xml或html文件的。
			- parse​(String html)：解析xml或html字符串
			- parse​(URL url, int timeoutMillis)：通过网络路径获取指定的html或xml的文档对象
		- Document：文档对象。代表内存中的dom树
			- 获取Element对象
				- getElementById​(String id)：根据id属性值获取唯一的element对象
				- getElementsByTag​(String tagName)：根据标签名称获取元素对象集合
				- getElementsByAttribute​(String key)：根据属性名称获取元素对象集合
				- getElementsByAttributeValue​(String key, String value)：根据对应的属性名和属性值获取元素对象集合
		- Elements：元素Element对象的集合。可以当做 ArrayList<Element>来使用
		- Element：元素对象
			- 获取子元素对象
				- getElementById​(String id)：根据id属性值获取唯一的element对象
				- getElementsByTag​(String tagName)：根据标签名称获取元素对象集合
				- getElementsByAttribute​(String key)：根据属性名称获取元素对象集合
				- getElementsByAttributeValue​(String key, String value)：根据对应的属性名和属性值获取元素对象集合

			- 获取属性值
				- String attr(String key)：根据属性名称获取属性值
			- 获取文本内容
				- String text():获取文本内容
				- String html():获取标签体的所有内容(包括字标签的字符串内容)
		- Node：节点对象
			- 是Document和Element的父类















