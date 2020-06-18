## 1. springmvc

- Maven环境
	
	```xml
<!--Spring坐标-->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>5.0.5.RELEASE</version>
	</dependency>
	<!--SpringMVC坐标-->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-webmvc</artifactId>
		<version>5.0.5.RELEASE</version>
	</dependency>
	<!--Servlet坐标-->
	<dependency>
		<groupId>javax.servlet</groupId>
		<artifactId>servlet-api</artifactId>
		<version>2.5</version>
	</dependency>
	<!--Jsp坐标-->
	<dependency>
		<groupId>javax.servlet.jsp</groupId>
		<artifactId>jsp-api</artifactId>
		<version>2.0</version>
	</dependency>
	```

- `web.xml`配置SpringMVC核心控制器DispathcerServlet

  ```xml
  <servlet>
  	<servlet-name>DispatcherServlet</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:spring-mvc.xml</param-value>
  	</init-param>
  	<load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
  	<servlet-name>DispatcherServlet</servlet-name>
  	<url-pattern>/</url-pattern>
  </servlet-mapping>
  ```

- `springmvc核心配置文件中`<a name="config1">配置内部资源解析器</a>

  > 默认的视图解析器值:
  >
  > REDIRECT_URL_PREFIX = "redirect:"  --重定向前缀
  > FORWARD_URL_PREFIX = "forward:"    --转发前缀（默认值）
  > prefix = "";     --视图名称前缀
  > suffix = "";     --视图名称后缀

  ```xml
  <!--配置内部资源视图解析器-->
  <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  	<property name="prefix" value="/WEB-INF/views/"></property>
  	<property name="suffix" value=".jsp"></property>
  </bean>
  ```




- SpringMVC核心配置文件

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"  
  	xmlns:mvc="http://www.springframework.org/schema/mvc"
  	xmlns:context="http://www.springframework.org/schema/context" 
  	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  	xsi:schemaLocation="http://www.springframework.org/schema/beans 
  	http://www.springframework.org/schema/beans/spring-beans.xsd 
  	http://www.springframework.org/schema/mvc   
  	http://www.springframework.org/schema/mvc/spring-mvc.xsd  
  	http://www.springframework.org/schema/context   
  	http://www.springframework.org/schema/context/spring-context.xsd">
  	<!--配置注解扫描-->
  	<context:component-scan base-package="com"/>
  </beans>
  ```



## 2. SpringMVC执行流程

- 流程

  ![2020-01-27-001](media/2020-01-27-001.png)

## 3. SpringMVC注解开发

### 3.1 路由分发

- 注解示例

	```java
	@Controller
	public class QuickController {
		@RequestMapping("/quick")
		public String quickMethod(){
			System.out.println("quickMethod running.....");
				return "index";
		}
	}
	```
	
- @RequestMapping

	- 作用：用于建立请求 URL 和处理请求方法之间的对应关系
	
	- 位置：
		1. 类上: 请求URL的第一级访问目录。不写相当于应用的根目录
		2. 方法上: 请求URL的第二级访问目录，与类上的使用@ReqquestMapping标注的一级目录一起组成访问虚拟路径
	
	- 属性:
		- value：用于**指定请求的URL**。它和path属性的作用是一样的
		- method：用于指定**请求的方式**  RequestMethod.GET / RequestMethod.Post
		- params：用于指定限制**请求参数的条件**。它支持简单的表达式。要求请求参数的key和value必须和配置的一模一样
			- params = {"accountName"}，表示请求参数必须有accountName
			- params = {"moeny!100"}，表示请求参数中money不能是100

### 3.2 返回响应

- 直接返回字符串,跳转jsp页面 ([内部资源解析器在web.xml中的定义参考]())

  ```java
  @Controller
  public class QuickController {
  	@RequestMapping("/quick")
  	public String quickMethod(){
  		System.out.println("quickMethod running.....");
  			return "index";
  	}
  }
  ```

  ![2020-01-27-002](media/2020-01-27-002.png)



- 通过ModelAndView对象, 跳转jsp页面

	- 方式一 : 直接返回 new ModelAndView()
	
		> 直接返回 new ModelAndView()
	
		```java
	    @RequestMapping("/test2")
		    public ModelAndView test2() {
		        ModelAndView modelAndView = new ModelAndView();
		        // 在jsp中即可通过 ${"username"}取到数据
		        modelAndView.addObject("username", "zhangsan");
		        // 返回success与视图解析器拼接的jsp页面
		        modelAndView.setViewName("success");
		        return modelAndView;
		    }
		```
		
	- 方式二: 直接返回形参的ModelAndView
		
			> 直接返回形参的ModelAndView springMVC对参数进行了注入
		
		```java
		@RequestMapping(value="/quick3")
		public ModelAndView save3(ModelAndView modelAndView){
			modelAndView.addObject("username","itheima");
			modelAndView.setViewName("success");
			return modelAndView;
		}
		```

- 直接返回字符串不返回视图

  ```java
  @ResponseBody
  @RequestMapping("/test4")
  public String test4() {
      return "test4";
  }
  ```

- 直接返回对象或集合自动转化为json

  ```xml
  <!--pom.xml-->
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.0</version>
  </dependency>
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
  </dependency>
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-annotations</artifactId>
      <version>2.9.0</version>
  </dependency>
  ```

  ```xml
  <!--spring-mvc.xml-->
  <mvc:annotation-driven />
  <!--或在spring-mvc.xml中配置如下适配器  与 上面配置效果相同-->
  <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
      <property name="messageConverters">
          <list>
              <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter" />
          </list>
      </property>
  </bean>
  ```

  ```java
  @RequestMapping("/test8")
  @ResponseBody
  public User test8() {
      return new User("zhangsan", "123456");
  }
  ```

- 重定向

  ```java
  @RequestMapping("/test7")
  public String test7 () {
      return "redirect:/test6";
  }
  ```

### 3.3 向域中存储数据

- 向request域中存储数据

  ```java
  @RequestMapping("/test5")
  public String test5(HttpServletRequest request) {
      request.setAttribute("username", "zhangsan");
      return "success";
  }
  ```

### 3.4 获取请求参数

- 获得Restful风格参数(占位符注解)
  - Restful风格

    - GET：用于获取资源		/user/1 GET
    - POST：用于新建资源		/user/1 POST
    - PUT：用于更新资源		/user/1 PUT
    - DELETE：用于删除资源  	/user/1 DELETE

  - 上述url地址/user/1中的1就是要获得的请求参数，在SpringMVC中可以使用占位符进行参数绑定。

    `http://localhost:8080/quick17/zhangsan`

    ```java
    @RequestMapping(value="/quick17/{name}")
    @ResponseBody
    public void save17(@PathVariable(value="name") String username) throws IOException {
    	System.out.println(username);
     }
    ```

- 基本类型参数
	
	> Controller中的业务方法的参数名称要与请求参数的name一致，参数值会自动映射匹配。并且能自动做类型转换；
	> 当请求的参数名称与Controller的业务方法参数名称不一致时，就需要通过@RequestParam注解显示的绑定

	`http://localhost:8080/itheima_springmvc1/quick9?username=zhangsan&age=12`
	
	```java
	@RequestMapping(value="/quick11")
	@ResponseBody
	public void save11(String username, int age) throws IOException {
		System.out.println(username);
		System.out.println(age);
	}
	```
	```java
	// 请求参数名称与Controller的业务方法参数名称不一致
	@RequestMapping(value="/quick11")
	@ResponseBody
	public void save11(@RequestParam(value="username",required = false,defaultValue = "itcast") String name, int age) throws IOException {
		System.out.println(name);
		System.out.println(age);
	}
	```
	
- pojo参数

  ```java
  @RequestMapping("/test9")
  @ResponseBody
  public void test9(User user) {}
  ```

- 数组类型参数

  ```java
  @RequestMapping("/test10")
  @ResponseBody
  public void test10(String[] strs) {
      System.out.println(strs);
  }
  ```

- 集合类型参数

  - 表单提交的数据:

    ```html
    <form action="${pageContext.request.contextPath}/user/quick14" method="post">
          <input type="text" name="userList[0].username"/><br/>
          <input type="text" name="userList[0].age" /><br/>
          <input type="text" name="userList[1].username"/><br/>
          <input type="text" name="userList[1].age"/><br/>
          <input type="submit" value="提交"/>
      </form>
    ```

    ```java
    // POJO
    public class VO {
    	private List<User> userList;
    }
    ```

    ```java
    @RequestMapping(value="/quick14")
    @ResponseBody
    public void save14(VO vo) throws IOException {
    	System.out.println(vo);
    }
    ```

  - ajax提交的数据:

    > 当使用ajax提交时，可以指定contentType为json形式，那么在方法参数位置使用@RequestBody可以直接接收集合数据而无需使用POJO进行包装

    ```js
    <script src="${pageContext.request.contextPath}/js/jquery-3.3.1.js"></script>
    <script>
        var userList = new Array();
        userList.push({username:"zhangsan",age:18});
        userList.push({username:"lisi",age:28});
    
        $.ajax({
            type:"POST",
            url:"${pageContext.request.contextPath}/user/quick15",
            data:JSON.stringify(userList),
            contentType:"application/json;charset=utf-8"
        });
    </script>
    ```

    ```java
    @RequestMapping(value="/quick15")
    @ResponseBody
    public void save15(@RequestBody List<User> userList) throws IOException {
        System.out.println(userList);
    }
    ```



### 3.5 放行静态资源

- 静态资源默认不可访问到的原因

  > 当有静态资源需要加载时，SpringMVC的前端控制器DispatcherServlet的url-pattern配置的是/,代表对所有的资源都进行过滤操作

- 在`spring-mvc.xml`中配置放行静态资源

  ```xml
  <!--放行webapp/js下的所有资源  mapping指资源  location指资源路径-->
  <mvc:resources mapping="/js/**" location="/js/"/>
  <mvc:resources mapping="/css/**" location="/css/"/>
  <mvc:resources mapping="/image/**" location="/image/"/>
  
  <!--或    配置为如下  表示放行到tomcat容器中找静态资源-->
  <mvc:default-servlet-handler/>
  ```

### 3.6 配置全局乱码过滤器

- `web.xml`中配置全局乱码过滤器

  ```xml
  <!--配置全局过滤的filter-->
  <filter>
      <filter-name>CharacterEncodingFilter</filter-name>
      <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
      <init-param>
          <param-name>encoding</param-name>
          <param-value>UTF-8</param-value>
      </init-param>
  </filter>
  <filter-mapping>
      <filter-name>CharacterEncodingFilter</filter-name>
      <url-pattern>/*</url-pattern>
  </filter-mapping>
  ```



### 3.7 设置类型转换器(将请求参数封装为指定类型)

- 定义类型转换器类实现Converter接口

  ```java
  /**
   * 将String类型的数据转换为Date类型的数据
   */
  public class DateConverter implements Converter<String, Date> {
      @Override
      public Date convert(String s) {
          SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
          Date date = null;
          try {
              date = simpleDateFormat.parse(s);
          } catch (ParseException e) {
              e.printStackTrace();
          }
          return date;
      }
  }
  ```

- 在`spring-mvc.xml`配置文件中声明转换器并在`<annotation-driven>`中引用转换器

  ```xml
  <!--声明转换器-->
  <bean id="conversionService2" class="org.springframework.context.support.ConversionServiceFactoryBean">
      <property name="converters">
          <list>
              <bean class="com.converter.DateConverter"/>
          </list>
      </property>
  </bean>
  
  <!--mvc注解驱动  conversion-service注册转换器-->
  <mvc:annotation-driven conversion-service="conversionService2"/>
  ```

- 使用示例

  ```java
  // 日期的默认转换器路由中需要输入 localhost:8080/test11?date=2011/01/01  输入2011-01-01无法转换,使用上述转换器时即可转换
  @RequestMapping("/test11")
  @ResponseBody
  public void test11(Date date) {}
  ```

### 3.8 获得Servlet相关API

- SpringMVC支持使用原始ServletAPI对象作为控制器方法的参数进行注入
- 常用的对象: HttpServletRequest, HttpServletResponse, HttpSession

	```java
	@RequestMapping(value="/quick19")
	@ResponseBody
	public void save19(HttpServletRequest request, HttpServletResponse response, HttpSession session) throws IOException {
		System.out.println(request);
		System.out.println(response);
		System.out.println(session);
	}
	```

### 3.9 获得请求头信息

- @RequestHeader 获得请求头信息，相当于web阶段学习的request.getHeader(name)

	- 该注解的属性
		- value：请求头的名称
		- required：是否必须携带此请求头

- @CookieValue 获得指定Cookie的值
	
	- 该注解属性 同上

- 示例
	```java
	@RequestMapping(value="/quick20")
  @ResponseBody
  public void save20(@RequestHeader(value = "User-Agent",required = false) String user_agent) throws IOException {
      System.out.println(user_agent);
  }
	
	@RequestMapping(value="/quick21")
	@ResponseBody
	public void save21(@CookieValue(value = "JSESSIONID") String jsessionId) throws IOException {
	    System.out.println(jsessionId);
	}
	```

### 3.10 SpringMVC异常处理机制

- SpringMVC框架中Dao、Service、Controller产生的异常通过throws Exception向上抛出，最后默认由SpringMVC前端控制器交由异常处理器(SimpleMappingExceptionResolver)进行异常处理

  ```xml
  <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
      <!--默认异常页面-->
      <property name="defaultErrorView" value="error" />
      <property name="exceptionMappings">
          <map>
              <!--定义发生特定异常时的视图-->
              <entry key="java.lang.Exception" value="exception" />
          </map>
      </property>
  </bean>
  ```

  > 如果对指定状态码返回指定视图
  >
  > ```xml
  > <!--web.xml-->
  > <error-page>
  >     <error-code>404</error-code>
  >     <location>/test1</location>
  > </error-page>
  > ```

- 实现Spring的异常处理接口HandlerExceptionResolver 自定义自己的异常处理器

  1. 创建异常处理类实现 HandlerExceptionResolver

     ```java
     public class MyExceptionResolver implements HandlerExceptionResolver {
         @Override
         public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
             // 处理异常代码实现
             ModelAndView modelAndView = new ModelAndView();
             modelAndView.setViewName("error");
             return modelAndView;
         }
     }
     ```

  2. 配置异常处理器

     ```xml
     <bean id="exceptionResolver" class="com.Exception.MyExceptionResolver"></bean>
     ```

  

## 4. SpringMVC拦截器

- 定义及作用
	
	- SpringMVC类似于 Filter对处理器进行预处理和后处理
- filter对所有资源拦截, interceptor只拦截访问的控制器方法对静态资源不拦截
	
- 拦截器实现

	1. 编写拦截器 实现HandlerInterceptor
		
		>  preHandle方法返回true则会执行目标资源，如果返回false则不执行目标资源
		
		```java
		public class MyInterceptor implements HandlerInterceptor {
		    // 在目标方法执行之前 执行
		    @Override
		    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
		        System.out.println("this is prehandle");
		        return true;
		    }
		
		    // 在目标方法执行之后 视图对象返回之前执行
		    @Override
		    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
		        System.out.println("postHandle");
		    }
		
		    //在流程都执行完毕后 执行
		    @Override
		    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion");
		    }
		}
		```
		
	2. 配置拦截器 在SpringMVC的配置文件中配置
		
		```xml
		<!--配置拦截器-->
		<mvc:interceptors>
			<mvc:interceptor>
				<!--对哪些资源执行拦截操作-->
				<mvc:mapping path="/**"/>
				<!--配置哪些资源排除拦截操作-->
				<mvc:exclude-mapping path="/user/login"/>
			<bean class="com.itheima.interceptor.MyInterceptor1"/>
			</mvc:interceptor>
		</mvc:interceptors>
		```
	
	3. 使用
	
		> 拦截器中的方法执行顺序是：preHandler-------目标资源----postHandle---- afterCompletion
	
		```java
		@RequestMapping("/test11")
		@ResponseBody
		public void test11() {
		    System.out.println("目标资源");
		}
		```



## 5. 文件上传

- 表单要求

  > 表单项type=“file”
  > 表单的提交方式是post
  > 表单的enctype属性是多部分表单形式，即enctype=“multipart/form-data” 此时request.getParameter()等方法失效, 原因是请求体数据格式不同,请查看抓包结果

  ```html
  <form action="${pageContext.request.contextPath}/user/quick22" method="post" enctype="multipart/form-data">
  	名称<input type="text" name="username"><br/>
  	文件1<input type="file" name="uploadFile"><br/>
  	<input type="submit" value="提交">
  </form>
  ```

- 导入坐标

  ```xml
  <dependency>
  	<groupId>commons-fileupload</groupId>
  	<artifactId>commons-fileupload</artifactId>
  	<version>1.3.1</version>
  </dependency>
  <dependency>
  	<groupId>commons-io</groupId>
  	<artifactId>commons-io</artifactId>
  	<version>2.3</version>
  </dependency>
  ```

- 配置多媒体解析器

  ```xml
  <!--配置文件上传解析器-->
  <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
  	<property name="defaultEncoding" value="UTF-8"/>
  	<property name="maxUploadSize" value="500000"/>
  </bean>
  ```

- 后台实现

  `单文件上传`

  ```java
  @RequestMapping(value="/quick22")
  @ResponseBody
  public void save22(String username, MultipartFile uploadFile) throws IOException {
  	System.out.println(username);
  	//获得上传文件的名称
  	String originalFilename = uploadFile.getOriginalFilename();
  	uploadFile.transferTo(new File("C:\\upload\\"+originalFilename));
  }
  ```

  `多文件上传`

  ```html
  <form action="${pageContext.request.contextPath}/user/quick23" method="post" enctype="multipart/form-data">
  	名称<input type="text" name="username"><br/>
  	文件1<input type="file" name="uploadFile"><br/>
  	文件2<input type="file" name="uploadFile"><br/>
  	<input type="submit" value="提交">
  </form>
  ```

  ```java
  @RequestMapping(value="/quick23")
  @ResponseBody
  public void save23(String username, MultipartFile[] uploadFile) throws IOException {
  	System.out.println(username);
  	for (MultipartFile multipartFile : uploadFile) {
  		String originalFilename = multipartFile.getOriginalFilename();
  		multipartFile.transferTo(new File("C:\\upload\\"+originalFilename));
  	}
  }
  ```
