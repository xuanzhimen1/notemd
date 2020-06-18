## 1. 概述

- Servlet就是一个接口,定义Java类被浏览器访问(Tomcat识别)到的规则

## 2. Servlet实现示例

- 实现`servlet`接口的类并重写`service`方法

  ```java
  public class SevletDemo implements Servlet {
      // 初始化方法
      
      // 服务方法
      @Override
      public void service(...) {
          System.out.println("hello servlet");
      }
      // 销毁方法
  }
  ```

- 修改`web.xml`配置

  ```xml
  <!--    配置servlet-->
  <servlet>
      <servlet-name>demo1</servlet-name>
      <servlet-class>com.jackiezz.web.servlet.SevletDemo</servlet-class>
  </servlet>
  
  <servlet-mapping>
      <servlet-name>demo1</servlet-name>
      <url-pattern>/demo1</url-pattern>
  </servlet-mapping>
  ```

- 启动Tomcat访问

  ```bash
  http://localhost:8080/demo1
  ```

- 实现原理

  ![tomcat中servlet的实现原理](media/2020-01-18-003.png)

  

## 3. Servlet接口的生命周期

- init方法

  - Servlet被创建时执行一次

- service方法

  > Servlet是单例的, 所以在service方法中不要给成员变量赋值，会造成数据安全问题

  - 每次请求都会执行一次

- destroy方法

  - Servlet被销毁时执行一次（服务器正常关闭）

- Servlet何时被创建?

  - 默认情况下第一次访问时被创建

  - 当在web.xml中`<servlet>`中配置了`<load-on-startup>`值非负数时, 服务器启动创建, 反之第一次访问创建



## 4. Servlet注解配置访问

> 注解配置必须servlet3.0+

- 注解

  ```java
  @WebServlet("/demo")  // 或 @WebServlet(urlPatterns="/demo")
  public class SevletDemo implements Servlet {
      ...;
      @Override
      public void service(...) {
          System.out.println("hello servlet");
      }
      ...;
  }
  ```

  

## 6. Servlet接口的实现类

- GenericServlet

  > 实现Servlet接口的抽象类, 对其余方法进行了空实现, 我们只要重写service方法即可

  ```java
  @WebServlet("/demo")
  public class ServletDemo extends GenericServlet {
      @override
      public void service(...) {
          System.out.println("hello GenericServlet");
      }
  }
  ```

- HttpServlet

  > 继承自GenericServlet, 对Http协议进行了封装

  ```java
  @WebServlet("/demo")
  public class ServletDemo extends HttpServlet {
      @Override
      protected void doGet(...) {
          System.out.println("doGet");
      }
      
      @Override
      protected void doPost(...) {
          System.out.println("doPost");
      }
  }
  ```

  

## 7. Servlet的注解配置

- urlPatterns

  > urlPatterns为一个String[], 可以配置多个url
  >
  > 定义规则可以是:  /xxx,  /xxx/xxx,  *.do

## 8. request
- request对象和response对象的原理
	1. request和response对象是由服务器创建的。我们来使用它们
	2. request对象是来获取请求消息，response对象是来设置响应消息


- 继承体系
	RequestFacade(tomcat) =实现=> HttpServletRequest接口 =继承=> ServletRequest接口

- request常用方法
	- 获取请求行数据
		```java
		// 请求: GET /day14/demo1?name=zhangsan HTTP/1.1
		// 获取请求方式: GET
		String getMethod()
		// 获取虚拟目录: /day14
		String getContextPath()
		// 获取get方式请求参数：name=zhangsan
		String getQueryString()
		// 获取请求URI：/day14/demo1
		String getRequestURI()  // /day14/demo1
		StringBuffer getRequestURL()  // http://localhost/day14/demo1
		// 获取协议及版本：HTTP/1.1
		String getProtocol()
		// 获取客户机的IP地址
		String getRemoteAddr()
		```
		
	- 获取请求头数据
		```java
		// 通过请求头的名称获取请求头的值
		String getHeader(String name)
		// 获取所有的请求头名称
		Enumeration<String> getHeaderNames()
		```
		
	- 获取请求体数据
		> 只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数
		
		```java
		// 获取字符输入流，只能操作字符数据
		BufferedReader getReader()
		// 获取字节输入流，可以操作所有类型数据
		ServletInputStream getInputStream()
		```
		
	- 获取请求参数
		```java
		// 根据参数名称获取参数值
		String getParameter(String name)
		// 根据参数名称获取参数值的数组  hobby=xx&hobby=game
		String[] getParameterValues(String name)
		// 获取所有请求的参数名称
		Enumeration<String> getParameterNames()
	// 获取所有参数的map集合
		Map<String,String[]> getParameterMap()
		```
	
	- 中文乱码问题
	
		```java
		request.setCharacterEncoding("utf-8")
		```
	```
		
		- tomcat 8 已经将get方式乱码问题解决了
		
	- 请求转发
		> 1. 浏览器地址栏路径不发生变化
		> 2. 只能转发到当前服务器内部资源中。
		> 3. 转发是一次请求
		
		```java
		// 通过request对象获取请求转发器对象
		RequestDispatcher getRequestDispatcher(String path)
	// 使用RequestDispatcher对象来进行转发
		forward(ServletRequest request, ServletResponse response) 
		
		// 示例
		req.getRequestDispatcher("/failServlet").forward(req,resp);
	```
	
	- 共享数据
		> 域对象：一个有作用范围的对象，可以在范围内共享数据
		> request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
	
		```java
		// 存储数据
	void setAttribute(String name,Object obj)
		// 通过键获取值
		Object getAttitude(String name)
		// 通过键移除键值对
		void removeAttribute(String name)
		```
	
	- 获取ServletContext
		```java
		ServletContext getServletContext()
		```

## 9. Response
- response常用方法
	- 设置响应行
		```java
		setStatus(int sc)
		```
	- 设置响应头
		```java
		setHeader(String name, String value) 
		```
	- 设置响应体
		```java
		// 获取输出流
		PrintWriter getWriter()  // 字符输出流
		ServletOutputStream getOutputStream()  // 字节输出流
		// 使用输出流，将数据输出到客户端浏览器
		```
		
	
- 中文解决乱码
	```java
	response.setContentType("text/html;charset=utf-8");
	```
	
- 重定向
	```java
	response.sendRedirect("/day15/responseDemo2");
	```

- response设置浏览器不缓存
	```java
	//服务器通知浏览器不要缓存
	response.setHeader("pragma","no-cache");
	response.setHeader("cache-control","no-cache");
	response.setHeader("expires","0");
	```

- 响应json数据
	```java
	ObjectMapper mapper = new ObjectMapper();
	String json = mapper.writeValueAsString(info);  // 将info对象转换为json字符串
	response.setContentType("application/json;charset=utf-8");
	response.getWriter().write(json);
	```

## 10. ServletContext

- 概念：代表整个web应用，可以和程序的容器(服务器)来通信

- 获取：

  ```java
  // 方式一: (request对象获取)
  request.getServletContext();
  // 方式二: (HttpServlet对象获取)
  this.getServletContext();
  ```

- 作用

  - 获取MIME类型：( text/html, image/jpeg) 可设置浏览器的数据类型

    ```java
    // 获取字符串file的mime类型 如file为a.jpg时, 其mine类型为image/jpeg
    String getMimeType(String file);
    // 使用:
    ServletContext context = request.getServletContext();
    String type = context.getMimeType("a.jpg");
    // 设置响应体的mime类型
    response.setContentType(type);
    ```

  - 域对象：共享数据

    > ServletContext对象范围：所有用户所有请求的数据

    ```java
    setAttribute(String name,Object value)
    getAttribute(String name)
    removeAttribute(String name)
    ```

  - 获取文件的真实(服务器)路径

    > 获取文件的真实路径可实现文件的读写, 例如案例:文件的上传与下载
    >
    > 文件的下载设置响应头为: `content-disposition:attachment;filename=xxx`

    ```java
    String getRealPath(String path);
    // 使用示例
    String b = context.getRealPath("/b.txt");  //web目录下资源访问
    String c = context.getRealPath("/WEB-INF/c.txt");  //WEB-INF目录下的资源访问
    String a = context.getRealPath("/WEB-INF/classes/a.txt");  //src目录下的资源访问
    ```

    

## 11. BeanUtils封装对象数据

- Map与对象的转换
	```java
	import org.apache.commons.beanutils.BeanUtils;
	
	BeanUtils.populate(user, map);  // 将map数据转为user对象
	```

## 12. Jackson封装json数据并响应
- 响应数据
	```java
	import com.fasterxml.jackson.databind.ObjectMapper;
	
	//响应数据
	ObjectMapper mapper = new ObjectMapper();
	String json = mapper.writeValueAsString(info);  // 将info对象序列化为json
	mapper.writeValue(response.getOutputStream(), json);
	```

## 13. ImageIO响应图片数据并响应
- 利用ImageIO输出response
	```java
	import javax.imageio.ImageIO;
	
	ImageIO.write(image,"PNG",response.getOutputStream());
	```