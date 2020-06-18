## 1. SpringBoot环境

- SpringBoot即spring工程的脚手架





## 2. SpringBoot入门

1. 创建maven工程

2. 添加SpringBoot的起步依赖

   ```xml
   <!--pom.xml-->
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.0.1.RELEASE</version>
   </parent>
   
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
   </dependencies>
   ```

3. 编写SpringBoot引导类

   ```java
   /**
    * springBoot工程的启动引导类
    */
   @SpringBootApplication
   public class Application {
       public static void main(String[] args) {
           SpringApplication.run(Application.class, args);
       }
   }
   ```

4. 编写Controller

   ```java
   /**
    * Controller
    */
   @RestController
   public class HelloController {
       @GetMapping("hello")
       public String hello() {
           System.out.println("hello");
           return "hello";
       }
   }
   ```




## 3. SpringBoot配置文件

### 3.1 配置文件作用与修改

- 类型与作用

  > SpringBoot默认从Resources目录下加载application.properties或application.yml

- 配置信息查询

  配置查阅: [文档地址](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#common-application-properties)

- 常用配置

  ```properties
  # QUARTZ SCHEDULER (QuartzProperties)
  spring.quartz.jdbc.initialize-schema=embedded # Database schema initialization mode.
  spring.quartz.jdbc.schema=classpath:org/quartz/impl/jdbcjobstore/tables_@@platform@@.sql # Path to the SQL file to use to initialize the database schema.
  spring.quartz.job-store-type=memory # Quartz job store type.
  spring.quartz.properties.*= # Additional Quartz Scheduler properties.
  
  # ----------------------------------------
  # WEB PROPERTIES
  # ----------------------------------------
  
  # EMBEDDED SERVER CONFIGURATION (ServerProperties)
  server.port=8080 # Server HTTP port.
  server.servlet.context-path= # Context path of the application.
  server.servlet.path=/ # Path of the main dispatcher servlet.
  
  # HTTP encoding (HttpEncodingProperties)
  spring.http.encoding.charset=UTF-8 # Charset of HTTP requests and responses. Added to the "Content-Type" header if not set explicitly.
  
  # JACKSON (JacksonProperties)
  spring.jackson.date-format= # Date format string or a fully-qualified date format class name. For instance, `yyyy-MM-dd HH:mm:ss`.
  
  # SPRING MVC (WebMvcProperties)
  spring.mvc.servlet.load-on-startup=-1 # Load on startup priority of the dispatcher servlet.
  spring.mvc.static-path-pattern=/** # Path pattern used for static resources.
  spring.mvc.view.prefix= # Spring MVC view prefix.
  spring.mvc.view.suffix= # Spring MVC view suffix.
  
  # DATASOURCE (DataSourceAutoConfiguration & DataSourceProperties)
  spring.datasource.driver-class-name= # Fully qualified name of the JDBC driver. Auto-detected based on the URL by default.
  spring.datasource.password= # Login password of the database.
  spring.datasource.url= # JDBC URL of the database.
  spring.datasource.username= # Login username of the database.
  
  # JEST (Elasticsearch HTTP client) (JestProperties)
  spring.elasticsearch.jest.password= # Login password.
  spring.elasticsearch.jest.proxy.host= # Proxy host the HTTP client should use.
  spring.elasticsearch.jest.proxy.port= # Proxy port the HTTP client should use.
  spring.elasticsearch.jest.read-timeout=3s # Read timeout.
  spring.elasticsearch.jest.username= # Login username.
  ```

  > 通过修改application.properties或application.yml修改默认配置

  ```properties
  server.port=8888
  server.servlet.context-path=demo
  ```

  ```yml
  server:
    port: 8888
    servlet:
      context-path: /demo
  ```

### 3.2 yml语法

- 普通数据

  ```yml
  name: zhangsan
  ```

- 对象数据

  ```yml
  person:
    name: zhangsan
    age: 18
  # 或
  person: {name: zhangsan,age:18}
  ```

- map类型同对象

- List,set数据

  ```yml
  city:
    - beijing
    - tianjin
  # 或
  city: [beijing,tianjing]
  # 集合中为对象元素
  student:
    - name: zhangsan
      age: 18
      score: 100
    - name: lisi
      age: 28
      score: 88
  ```

  

### 3. 3 配置文件与配置类属性的映射方式

#### 3.3.1 @Value映射

- 映射application.properties或application.yml中的配置信息

  ```yml
  person:
    name: zhangsan
    age: 18
  ```

  ```java
  @RestController
  public class HelloController {
      @Value("${person.name}")
      private String name;
      @Value("${person.age}")
      private Integer age;
  
      @GetMapping("hello")
      public String hello() {
          return "springboot 访问成功! name="+name+",age="+age;
      }
  }
  ```

- 映射自定义配置文件的配置信息 

  classpath下的配置文件: `jdbc.properties`

  ```properties
  jdbc.driverClassName=com.mysql.jdbc.driver
  jdbc.url=jdbc:mysql://192.168.3.108
  jdbc.username=root
  jdbc.password=root
  ```

  `@Value`读取配置文件配置项注入到对象中 `JdbcConfig.java`

  ```java
  @PropertySource("classpath:jdbc.properties")
  public class JdbcConfig {
      @Value("${jdbc.url}")
      String url;
      @Value("${jdbc.driverClassName}")
      String driverClassName;
      @Value("${jdbc.username}")
      String username;
      @Value("${jdbc.password}")
      String password;
  
      @Bean
      public DataSource dataSource() {
          DruidDataSource dataSource= new DruidDataSource();
          dataSource.setDriverClassName(driverClassName);
          dataSource.setUrl(url);
          dataSource.setUsername(username);
          dataSource.setPassword(password);
          return dataSource;
      }
  }
  ```
#### 3.3.2 @ConfigurationProperties

- 解决@ConfigurationProperties使用中的报错问题

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-configuration-processor</artifactId>
      <optional>true</optional>
  </dependency>
  ```

- 通过@ConfigurationProperties(prefix="配置文件中key的前缀")将配置文件中的配置自动与实体进行映射

  `application.yml`

  ```yml
  person:
    name: zhangsan
    age: 18
  ```

  `Bean`

  > 实体类中必须提供set方法

  ```java
  @RestController
  @ConfigurationProperties(prefix = "person")
  public class HelloController {
      private String name;
      private Integer age;
      // 省略get/set
      
      @GetMapping("hello")
      public String hello() {
          return "springboot 访问成功! name="+name+",age="+age;
      }
  }
  ```

- 直接在类的方法中引入配置文件配置项

  配置文件`jdbc.properties`

  ```properties
  jdbc.driverClassName=com.mysql.jdbc.driver
  jdbc.url=jdbc:mysql://192.168.3.108
  jdbc.username=root
  jdbc.password=root
  ```

  `@ConfigurationProperties`在类方法中注入

  ```java
  public class JdbcConfig {
      @Bean
      @ConfigurationProperties(prefix = "jdbc")
      public DataSource dataSource() {
          return new DruidDataSource();
      }
  }
  ```

- @EnableConfigurationProperties注解的使用

  ```properties
  # application.properties
  jdbc.driverClassName=com.mysql.jdbc.driver
  jdbc.url=jdbc:mysql://192.168.3.108
  jdbc.username=root
  jdbc.password=root
  ```

  ```java
  @ConfigurationProperties(prefix = "jdbc")
  public class JdbcProperties {
      private String url;
      private String driverClassName;
      private String username;
      private String password;
      // 省略get/set方法
  }
  ```

  ```java
  @EnableConfigurationProperties(JdbcProperties.class)
  public class JdbcConfig {
      @Bean
      public DataSource dataSource(JdbcProperties jdbcProperties) {
          DruidDataSource dataSource= new DruidDataSource();
          dataSource.setDriverClassName(jdbcProperties.getDriverClassName());
          dataSource.setUrl(jdbcProperties.getUrl());
          dataSource.setUsername(jdbcProperties.getUsername());
          dataSource.setPassword(jdbcProperties.getPassword());
          return dataSource;
      }
  }
  ```


### 3.4 多个配置文件及yml的加载

- 多个yml配置文件在springBoot中配置文件名必须为`application-***.yml`

- 必须在`application.yml`中激活才可以使用

- 使用示例

  yml配置文件`application.yml`

  ```yml
  jdbc:
    driverClassName: com.mysql.jdbc.driver
    url: jdbc:mysql://192.168.3.108
    username: root
    password: root
  
  #激活其它配置文件
  spring:
    profiles:
      active: abc,def
  ```

  yml其它配置文件`application-abc.yml`

  ```yml
  com:
    abc: ABC
  ```

  读取yml配置(上节二种方式都可以)

  ```java
  @Value("${com.abc}")
  private String isABC;
  ```




## 4. lombok插件的使用

1. IDEA安装lombok插件

   - 在IDEA=>settings=>plugins=>搜索lombok安装

2. 添加lombok对应的依赖到项目pom.xml中

   ```xml
   <!--lombok-->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
   </dependency>
   ```

3. 改造实体类使用lombok注解(@Data @Getter)

   ```java
   /**
    * 在编译时自动生成对应的方法,
    * @Data包括get/set/hashCode/equals/tostring方法
    * @Getter自动提供getter方法
    * @Setter自动提供settet方法
    * @Slf4j在bean中提供log变量,其实使用的是slf4j的日志功能
    */
   @Data
   @Slf4j
   public class user {
       private String username;
       private String password;
   }
   ```

   

## 5. SpringBoot整合

### 5.1 SpringBoot整合SpringMVC端口和资源

- 修改端口与路径

  ```yml
  server:
    port: 8888
    servlet:
      context-path: /demo
  ```

- 访问静态资源

  > 静态资源可放置的目录: 点击如下依赖中的ResourceProperties.class查看
  >
  > /org/springframework/boot/spring-boot-autoconfigure/2.0.1.RELEASE/spring-boot-autoconfigure-2.0.1.RELEASE.jar!/org/springframework/boot/autoconfigure/web/ResourceProperties.class
  >
  > 可放置的目录有: "classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/"

  在resources目录下添加static文件夹在其中添加静态资源(png, css, js)

  访问: tomcat启动路径+静态资源路径

### 5.2 添加拦截器

1. 编写拦截器(实现HandlerInterceptor)

   ```java
   @Slf4j
   public class MyInterceptor implements HandlerInterceptor {
   
       /**
        * 前置方法中如果是返回false则会拦截所有的请求
        */
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           log.debug("这是MyInterceptor的preHandle方法执行...");
           return true;
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           log.debug("这是MyInterceptor的postHandle方法执行...");
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
       }
   }
   ```

   

2. 编写配置类实现WebMvcConfigurer, 在该类中添加各种组件

   ```java
   @Configuration
   public class MvcConfig implements WebMvcConfigurer {
       // 注册拦截器
       @Bean
       public MyInterceptor myInterceptor() {
           return new MyInterceptor();
       }
       // 添加拦截器到springmvc拦截器拦截器链
       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           registry.addInterceptor(myInterceptor()).addPathPatterns("/*");
       }
   }
   ```



### 5.3 设置日志记录级别

- `application.yml`

  ```yml
  # 设置日志的记录级别
  logging:
    level: debug
  ```

  





































