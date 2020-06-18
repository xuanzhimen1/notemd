## 1. Tomcat环境

### 1.1 下载安装

- 下载
	
	```
	http://tomcat.apache.org/
	```
	
- 安装
	解压即可
	
- 卸载
	删除即可

### 1.2 启动关闭与端口修改

- 启动
	- /bin/startup.bat   双击启动
	- 访问: http://localhost:8080
	
- 修改端口
`conf/server.xml`
	
	```xml
	<Connector port="8888" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8445" />
	```

- 关闭
  - /bin/shutdown.bat

### 1.3 项目部署

- 方式1: 直接将项目放到webapps目录下即可
	- 将项目打成一个war包，再将war包放置到webapps目录下。war包会自动解压缩
- 方式2: 通过配置文件部署
	- 方式一:  （server.xml为整个tomcat的配置目录，不安全不推荐 ）
		
		- 修改`/conf/server.xml`在`<Host>`标签中添加如下
		
		```xml
		<Context docBase="项目目录" path="访问路径（虚拟目录）" />
		```
		
	- 方式二: （**推荐使用** 此方式为热部署）
		
		> 访问路径（虚拟目录）为配置的文件名称，下例中为 xxx, 访问时请求：localhost:8080/xxx/
		
		- 自定义配置文件部署：创建`/conf/Catalina/localhost/xxx.xml`文件并加入如下配置
		
		```xml
		<Context docBase="项目目录" path="访问路径（虚拟目录）" />
		```

## 2. 项目集成Tomcat 

- IDEA集成
	
	![image-20200117071212791](media/image-20200117071212791.png)

