

## 1. Filter：过滤器

- 概念：
  - 当访问服务器的资源时，过滤器可以将请求拦截下来，完成一些特殊的功能。

  * 一般用于完成通用的操作。如：登录验证、统一编码处理、敏感字符过滤...

- 注解实现Filter

    ```java
    @WebFilter("/*")
    public class FilterDemo implements Filter {
        @Override
        public void init(FilterConfig filterConfig) throws ServletException {}

        @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
            System.out.println("filter execute");
            filterChain.doFilter(servletRequest, servletResponse);
        }

        @Override
        public void destroy() {}
}
    ```

- xml实现Filter

  ```xml
  <filter>
      <filter-name>demo</filter-name>
      <filter-class>com.filter.FilterDemo</filter-class>
  </filter>
  <filter-mapping>
      <filter-name>demo</filter-name>
      <!-- 拦截路径 -->
      <url-pattern>/*</url-pattern>
  </filter-mapping>
  ```

- 过滤器的生命周期
  - init: 在服务器启动后，会创建Filter对象，然后调用init方法。只执行一次。用于加载资源
  - destroy:在服务器关闭后，Filter对象被销毁。如果服务器是正常关闭，则会执行destroy方法。只执行一次。用于释放资源

- 拦截路由配置

    - 具体资源路径： /index.jsp   只有访问index.jsp资源时，过滤器才会被执行
    - 拦截目录： /user/*	访问/user下的所有资源时，过滤器都会被执行
    - 后缀名拦截： *.jsp		访问所有后缀名为jsp资源时，过滤器都会被执行
    - 拦截所有资源：/*		访问所有资源时，过滤器都会被执行

- 拦截指定方法

  ```xml
  <filter-mapping>
      <filter-name>demo</filter-name>
      <url-pattern>/*</url-pattern>
      <dispatcher>REQUEST</dispatcher>
  </filter-mapping>
  ```

  ```java
  @WebFilter(value = "/*", dispatcherTypes = DispatcherType.REQUEST)
  ```

    - REQUEST：默认值。浏览器直接请求资源
    - FORWARD：转发访问资源
    - INCLUDE：包含访问资源
    - ERROR：错误跳转资源
    - ASYNC：异步访问资源

- 过滤器链(多个Filter)
  - 执行顺序: filter1 => filter2 => 资源执行 => filter2 => filter1
  - 注解配置按字母顺序先后执行
  - xml配置前面的filter先执行

## 2. 动态代理

- 共同接口

  ```java
  public interface TestInterface {
      int paly(int num);
  }
  ```

- 接口实现

  ```java
  public class TestImpl implements TestInterface {
      @Override
      public int paly(int num) {
          System.out.println("play...." + num);
          return num;
      }
  }
  ```

- 实现动态代理

  ```java
  public static void main(String[] args) {
      // 创建原对象调用方法
      final TestImpl test = new TestImpl();
      int paly = test.paly(3);
      System.out.println(paly);
  
      // 动态代理
      TestInterface test2 = (TestInterface) Proxy.newProxyInstance(test.getClass().getClassLoader(), test.getClass().getInterfaces(), new InvocationHandler() {
          @Override
          public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
              // 对参数增强
              int num = (int) args[0] + 1;
              // 对方法体增强
              Object invoke = method.invoke(test, args[0]);
              System.out.println("方法体增强了");
              // 增强返回结果
              return (int)invoke + 1;
          }
      });
  
      // 使用动态代理对象调用方法
      int paly2 = test2.paly(3);
      System.out.println(paly2);
  }
  ```

  

## 3. Listener：监听器

- 











* 概念：web的三大组件之一。
	* 事件监听机制
		* 事件	：一件事情
		* 事件源 ：事件发生的地方
		* 监听器 ：一个对象
		* 注册监听：将事件、事件源、监听器绑定在一起。 当事件源上发生某个事件后，执行监听器代码
* ServletContextListener:监听ServletContext对象的创建和销毁
	* 方法：
		* void contextDestroyed(ServletContextEvent sce) ：ServletContext对象被销毁之前会调用该方法
		* void contextInitialized(ServletContextEvent sce) ：ServletContext对象创建后会调用该方法
	* 步骤：
		1. 定义一个类，实现ServletContextListener接口
		2. 复写方法
		3. 配置
			1. web.xml
				```xml
				<listener>
				 					 <listener-class>cn.itcast.web.listener.ContextLoaderListener</listener-class>
  			 				</listener>
				```
				* 指定初始化参数<context-param>
			2. 注解：
				
				* @WebListener