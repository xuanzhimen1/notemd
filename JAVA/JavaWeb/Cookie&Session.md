
## 1. Cookie
- 概述及原理

  - cookie存储数据在客户端浏览器

  - 浏览器对于单个cookie 的大小有限制(4kb) 以及 对同一个域名下的总cookie数量也有限制(20个)

  - 原理

    - 浏览器在发送请求时会携带本网站的cookie信息

    - 响应头: `Set-Cookie: username=zs` 在响应头中设置cookie

    - 请求头: `Cookie: username=zs` 在请求头中获取cookie

      

- 使用
	
	```java
	// 创建Cookie对象，设置响应头cookie绑定数据
	Cookie cookie = new Cookie(String name, String value);
	response.addCookie(cookie);
	
// 从请求头中获取Cookie，拿到数据
	Cookie[] cookies = request.getCookies();
	for (Cookie co : cookies) {
	    String cookieValue = co.getValue();
	}
	```
	
- cookie在浏览器中存储时长
	
	```java
	// 默认浏览器关闭,Cookie被销毁
	// 设置cookie的持久存储, 时间单位 "s"
	request.setMaxAge(24*60*60);  // cookie存活1天
	request.setMaxAge(-1);  // 负数: 浏览器关闭cookie销毁
	request.setMaxAge(0);  // 直接删除全部cookie信息
	```
	
- cookie中文乱码

  > tomcat8+可直接存储中文

  ```java
  // 编码: 将中文进行编码再放入cookie中(%E6这种字符)
  String username1 = URLEncoder.encode(username, "utf-8");
  // 解码
  String cookie = URLDecoder.decode(cookieValue, "utf-8");
  ```

- cookie不同应用,不同服务器之间的共享

  - 一个tomcat中部署多个应用: 默认不共享(原因: 虚拟目录不同)

    ```java
    // 通过修改cookie的访问path可实现共享
    Cookie cookie = new Cookie(String name, String value);
    cookie.setPath("/")  // 在该tomcat下的所有应用有可以共享这个cookie
    ```

  - 不同tomcat: 默认不共享(原因: 域名不同)

    ```java
    // 通过修改cookie的一级域名Domain实现共享
    cookie.setDomain(".baidu.com");  // tieba.baidu.com和news.baidu.com中cookie可以共享
    ```

## 2. Session
- 概念及原理

  - 服务器端会话技术，在一次会话的多次请求间共享数据，将数据保存在服务器端的对象中。
  - 原理: 
    - 服务器第一次获取session对象时, 在内存中创建session对象并设置响应头`set-cookie: SESSIONID=xxxxx`, 浏览器获取响应,并在本地中设置名为sessionid的cookie
    - 之后的请求统一在cookie中带上key为sessionid的cookie就可以与服务器实现会话
    - session本质上不是cookie

-  session会话方式
	
	```java
	// 获取HttpSession对象
	HttpSession session = req.getSession();
	// 获取session对象属性
	Object username = session.getAttribute("username");
	// 设置session对象属性
	session.setAttribute("password", "123456");
	// 移除session对象属性
	session.removeAttribute("username");
	// 清空session对象属性
	session.invalidate();
	```
	
- session的有效期
	
	```java
	// 取决于key为SESSIONID的cookie的有效时间, 如下设置则session的有效期为1小时
	// 注意, 1小时到了session并不会销毁,只是在浏览器中cookie失效, 会新生成一套session及cookie
	HttpSession session = req.getSession();
	Cookie cookie = new Cookie("JSESSIONID", session.getId());
cookie.setMaxAge(60*60);
	response.addCookie(c);
	```

- 服务器重启session是否还存活

  - tomcat通过命令正常关闭并再次开启时,session会存活
    - 原因: tomcat正常关闭时,在tomcat/work/下生成钝化文件将session对象存储到钝化文件中,tomcat再次启动钝化文件活化将session文件转化为内存中的session对象
  - tomcat非正常关闭(点击窗口x按钮关闭)或idea中重启则会将work目录删除重建所以重启session不存在

- session的生命周期

  - session销毁时机

    - tomcat服务器非正常关闭
    - session对象调用invalidate方法
    - session默认存活时间30min

  - 延长session的存活时间

    ```xml
    <!--在tomcat的配置文件web.xml中配置session存活时间-->
    <session-config>
    	<session-timeout>30</session-timeout>
    </session-config>
    <!--在项目的web.xml中配置session的存活时间将覆盖tomcat/conf/web.xml中的配置-->
    ```



## 3. Session与Cookie比较

|          | Cookie                    | Session               |
| -------- | ------------------------- | --------------------- |
| 存储位置 | 浏览器                    | 服务器                |
| 存储大小 | 最多20个cookie每个最多4KB | 没有限制              |
| 是否安全 | 数据安全                  | 相对不安全,可能被篡改 |