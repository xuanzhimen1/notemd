## 1. Spring环境

### 1.1 Spring开发Maven环境

- 开发基础坐标

  ```xml
  <properties>
  	<spring.version>5.0.5.RELEASE</spring.version>
  </properties>
  <!--导入spring的context坐标，context依赖core、beans、expression-->
  <dependencies> 
      <dependency>  
          <groupId>org.springframework</groupId> 
          <artifactId>spring-context</artifactId> 
          <version>${spring.version}</version>
      </dependency>
  </dependencies>
  ```

### 1.2 Spring核心配置文件

- `web.xml`中指定spring的核心配置文件(默认: classpath:applicationContext.xml)

  ```xml
  <!--全局参数-->
  <context-param>
  	<param-name>contextConfigLocation</param-name>
  	<param-value>classpath:applicationContext.xml</param-value>
  </context-param>
  ```

- `resoruces/applicationContext.xml`

  ```xml
  <beans  xmlns="http://www.springframework.org/schema/beans"
  		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  		xmlns:context="http://www.springframework.org/schema/context"
  		xmlns:aop="http://www.springframework.org/schema/aop"
  		xmlns:tx="http://www.springframework.org/schema/tx"
  		xsi:schemaLocation="
  			http://www.springframework.org/schema/context
  			http://www.springframework.org/schema/context/spring-context.xsd
  			http://www.springframework.org/schema/aop
  			http://www.springframework.org/schema/aop/spring-aop.xsd
  			http://www.springframework.org/schema/tx
  			http://www.springframework.org/schema/tx/spring-tx.xsd
  			http://www.springframework.org/schema/beans
  			http://www.springframework.org/schema/beans/spring-beans.xsd">
  </beans>
  ```

### 1.3 Spring分模块开发

- 目录结构:

  - resources
    - applictionContext.xml
    - applictionContext-user.xml
    - applictionContext-goods.xml

- 包含关系配置

  `applictionContext.xml`

  ```xml
  <beans>
  	<import resource="applictionContext-user.xml"></import>
  	<import resource="applictionContext-goods.xml"></import>
  </beans>
  ```



## 2. Spring的IoC

### 2.1 xml方式配置Bean

- xml实现IoC

  ```xml
  <bean id="userDao"
  	  class="com.dao.impl.UserDaoImpl"
  	  scope="prototype"
  	  init-method="init"
  	  destory-method="destroy">
  </bean>
  ```

  - id

    > 指定唯一性标识

  - class

    > 类的全限定名

  - scope

    > 对象的作用范围
    > `scope="singleton"` 时在ApplicationContext 加载时(即创建ApplicationContext时) 对象既被创建
    > `scope="protopyte"` 时在getBean时创建

    - singleton => 单例(默认)
    - prototype => 多例
    - request => 创建Bean对象时, 存入request域中
    - session => 创建Bean对象时, 存入session域中

  - init-method 与 destroy-method

    > 指定类中对应的初始化方法与销毁方法的方法名称

- Bean实例化的其它方式

  - 工厂静态方法实例化

    `StaticFactory.java`

    ```java
    public static UserDao getUserDao() {
    	return new UserDaoImpl();
    }
    ```

    `applicationContext.xml`

    ```xml
    <bean id="userDao" 
    	  class="com.factory.StaticFactory" 
    	  factory-method="getUserDao">
    </bean>
    ```

  - 工厂实例方法实例化

    `DynamicFactory.java`

    ```java
    public UserDao getUserDao() {
    	return new UserDaoImpl();
    }
    ```

    `applicationContext.xml`

    ```xml
    <bean id="factory" class="com.factory.DynamicFactory"></bean>
    <bean id="userDao" factory-bean="factory" factory-method="getUserDao"></bean>
    ```




### 2.2 注解方式配置Bean

- 作用在类上的注解

  | 注解           | 说明                                           |
  | -------------- | ---------------------------------------------- |
  | @Component     | 使用在类上用于实例化Bean                       |
  | @Controller    | 使用在web层类上用于实例化Bean                  |
  | @Service       | 使用在service层类上用于实例化Bean              |
  | @Repository    | 使用在dao层类上用于实例化Bean                  |
  | @Scope         | 标注Bean的作用范围                             |

- 生命周期方法注解

  | 注解           | 说明                                           |
  | -------------- | ---------------------------------------------- |
  | @PostConstruct | 使用在方法上标注该方法是Bean的初始化方法       |
  | @PreDestroy    | 使用在方法上标注该方法是Bean的销毁方法         |



- 类上的注解使用示例

  ```java
  // 作用于web层
  @Controller("xxx")
  public class UserController {}
  
  // 作用于Service层
  @Service("userService")
  public class UserServiceImpl implements UserService {}
  
  // 作用于Dao层
  @Repository("userDao")
  public class UserDaoImpl implements UserDao {}
  
  //@Scope("prototype") // 使用多例
  @Scope("singleton")  // 使用单例
  public class UserDaoImpl implements UserDao {
  	@PostConstruct  // 对象初始化方法
  	public void init(){
  		System.out.println("初始化方法....");
  	}
  	@PreDestroy  // 对象销毁方法
  	public void destroy(){
  		System.out.println("销毁方法.....");
  	}
  }
  ```

  

## 3.  依赖注入DI

- 依赖注入: 解决各对象之间的依赖关系 如service依赖dao

### 3.1 Setter注入

- set方法注入

  `UserServiceImpl`类依赖于`UserDao`类

  ```java
  public class UserServiceImpl implements UserService {
      private UserDao userDao;
  
      public void setUserDao(UserDao userDao) {
          this.userDao = userDao;
      }
  }
  ```
  `applicationContext.xml`
  ```xml
  <bean id="userDao1" class="com.dao.impl.UserDaoImpl"></bean>
  <bean id="userService" class="com.service.impl.UserServiceImpl">
      <property name="userDao" ref="userDao1"/>
  </bean>
  ```

### 3.2 构造注入

- 构造方法注入
	
	`UserServiceImpl`
	
	```java
	public class UserServiceImpl implements UserService {
	    private UserDao userDao;
	
	    public UserServiceImpl(UserDao userDao) {
	        this.userDao = userDao;
	    }
	}
	```
	`applicationContext.xml`
	```xml
	<bean id="userDao1" class="com.dao.impl.UserDaoImpl"></bean>
	<bean id="userService" class="com.service.impl.UserServiceImpl">
	    <constructor-arg name="userDao" ref="userDao1"/>
	</bean>
	```

### 3.3 基本类型及集合注入

- 普通数据类型/集合类型的注入
	
	`UserServiceImpl`
	
	```java
	public class UserServiceImpl implements UserService {
	    private int age;
	    private Map<String, User> userMap;
	    private List<String> strList;
	    private Properties properties;
	    
	    public void setAge(UserAge age){
	        this.age = age;
	    }
	    public void setUserMap(Map<String, User> userMap){
	        this.userMap = userMap;
	    }
	    public void setStrList(List<String> strList){
	        this.strList = strList;
	    }
	    public void setProperties(Properties properties){
	        this.properties = properties;
	    }
	}
	```

	`applicationContext.xml`
	
	```xml
	<bean id="user" class="....User">
		<property name="name" value="lucy"></property>
	</bean>
	<bean id="userService" class="com.service.impl.UserServiceImpl">
		<!--普通类型注入-->
		<property name="age" value="18"></property>
		<!--List类型注入-->
		<property name="strList">
			<list>
				<value>"aaa"</value>
				<value>"bbb"</value>
			</list>
		</property>
		<!--Map类型注入-->
		<property name="userMap">
			<map>
				<entry key="user1" value-ref="user"></entry>
				<entry key="user2" value-ref="user"></entry>
			</map>
		</property>
		<!--Properties类型注入-->
		<property name="properties">
			<props>
				<prop key="p1">ppp1</prop>
				<prop key="p2">ppp2</prop>
			</props>
		</property>
	</bean>
	```



### 3.4 注解注入

- DI注解

  | 注解           | 说明                                           |
  | -------------- | ---------------------------------------------- |
  | @Autowired     | 使用在字段上用于根据类型依赖注入               |
  | @Resource      | 相当于@Autowired+@Qualifier，按照名称进行注入  |
  | @Value         | 注入普通属性                                   |

- DI注解使用示例

  ```java
  @Repository("userDao")
  public class UserDaoImpl implements UserDao {
  	@Value("This is DI String")
  	private String str;
  	@Value("${jdbc.driver}")  // 使用Bean将数据加入spring容器
  	private String driver;
  }
  ```

  


## 4. 加载其它配置文件

- 在`applicationContext.xml`中指定加载的properties配置文件

  ```xml
  <context:property-placeholder location="classpath:xxx.properties"/>
  ```

- 使用properties配置文件中的属性注入到对象中

  ```xml
  <property name="" value="${key}"/>
  ```



## 5. 获取Spring容器中的Bean

- ApplicationContext接口
  - 接口实现类

    - ClassPathXmlApplicationContext

    	> 配置文件在resources下时获取spring容器

    - AnnotationConfigApplicationContext

    	> 注解配置容器对象时获取Spring容器

- ApplicationContext接口getBean()方法获取Bean对象

  - `public Object getBean(String name)`

  	> 用于配置文件中有多个此类型的bean时

  	```java
  	applicationContext.getBean("userDao1")
  	```

  - `public <T> T getBean(Class<T> requiredType)`
  	
  	> 用于配置文件中只有一个此类型的bean时
  	
  	```java
  	applicationContext.getBean(UserDao.class)
  	```

## 6. Spring配置数据源

- 导入Maven坐标

	- c3p0和druid
	
		`pom.xml`
		```xml
		<!-- C3P0连接池 -->
		<dependency>
			<groupId>c3p0</groupId>
			<artifactId>c3p0</artifactId>
			<version>0.9.1.2</version>
		</dependency>
		<!-- Druid连接池 -->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.1.10</version>
		</dependency>
		```

	- mysql驱动坐标
	
		`pom.xml`
		```xml
		<!-- mysql驱动 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.39</version>
		</dependency>
		```

- 数据库配置文件`resources/jdbc.properties`

	```json
	jdbc.driver=com.mysql.jdbc.Driver
	jdbc.url=jdbc:mysql://localhost:3306/test
	jdbc.username=root
	jdbc.password=root
	```

- `applicationContext.xml`加载jdbc.properties配置文件配置信息并创建DataSource的Bean对象

	```xml
	<!--加载外部的properties文件-->
	<context:property-placeholder location="classpath:jdbc.properties"/>
	<!--bean创建DataSource对象-->
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	    <property name="driverClass" value="${jdbc.driver}"></property>
	    <property name="jdbcUrl" value="${jdbc.url}"></property>
	    <property name="user" value="${jdbc.username}"></property>
	    <property name="password" value="${jdbc.password}"></property>
	</bean>
	```

## 7. Spring注解开发

### 7.1 注解开发环境配置

1. 在`applicationContext.xml`中指定注解扫描来启用注解

   ```xml
   <!--注解的组件扫描-->
   <context:component-scan base-package="com" />
   ```

### 7.2 注解配置Spring

- 代替applicationContext.xml的注解

  | 注解            | 说明                                                         |
  | --------------- | ------------------------------------------------------------ |
  | @Configuration  | 用于指定当前类是一个 Spring配置类，当创建容器时会从该类上加载注解 |
  | @ComponentScan  | 指定扫描注解包                                               |
  | @Bean           | 使用在方法上，标注将该方法的返回值存储到Spring容器中         |
  | @PropertySource | 用于加载.properties文件中的配置                              |
  | @Import         | 用于导入其他配置类                                           |

- 使用示例

  ```java
  @Configuration
  @ComponentScan("com")
  @Import({DataSourceConfiguration.class})
  public class SpringConfiguration {
  }
  ```

  ```java
  @PropertySource("classpath:jdbc.properties")
  public class DataSourceConfiguration {
  	@Value("${jdbc.driver}")
  	private String driver;
  	@Value("${jdbc.url}")
  	private String url;
  	@Value("${jdbc.username}")
  	private String username;
  	@Value("${jdbc.password}")
  	private String password;
  	
  	@Bean(name="dataSource")
  	public DataSource getDataSource() throws PropertyVetoException { 
  		ComboPooledDataSource dataSource = new ComboPooledDataSource(); 
  		dataSource.setDriverClass(driver);
  		dataSource.setJdbcUrl(url);
  		dataSource.setUser(username);
  		dataSource.setPassword(password);
  		return dataSource;
  	} 
  }
  ```

  


## 8. Spring的AOP

### 8.1 Spring的AOP环境

- `pom.xml`导入AOP依赖

  ```xml
  <!--导入spring的context坐标，context依赖aop-->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.0.5.RELEASE</version>
  </dependency>
  <!-- aspectj的织入 -->
  <dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.13</version>
  </dependency>
  ```

- `applicationContext.xml`中指定aop自动代理

  ```xml
  <!--aop的自动代理-->
  <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
  ```

  

### 8.2 xml配置实现AOP

1. 指定目标接口和目标类和切面类

   ```java
   // 目标接口
   public interface TargetInterface {
   	public void method();
   }
   
   // 目标类(实现目标接口)
   public class Target implements TargetInterface {
   	@Override
   	public void method() {
   		System.out.println("Target running....");
   	}
   }
   
   // 切面类
   public class MyAspect {
   	//前置增强方法
   	public void before(){
   		System.out.println("前置代码增强.....");
   	}
       // before, after, throwing, after 通知定义与一般方法相同
       // around的定义如下
       public Object around(ProceedingJoinPoint pjp) throws Throwable {
           System.out.println("环绕前置增强");
           Object proceed = pjp.proceed();  // 切点方法
           System.out.println("环绕后置增强");
           return proceed;
       }
   }
   ```

2. `applicationContext.xml`中配置目标类及切面类进行织入

   ```xml
   <!--配置目标类-->
   <bean id="target" class="com.aop.Target"></bean>
   <!--配置切面类-->
   <bean id="myAspect" class="com.aop.MyAspect"></bean>
   <!--配置织入关系-->
   <aop:config>
   	<!--引用myAspect的Bean为切面对象-->
   	<aop:aspect ref="myAspect">
   		<!--配置Target的method方法执行时要进行myAspect的before方法前置增强-->
   		<aop:before method="before" pointcut="execution(public void com.aop.Target.method())" />
   	</aop:aspect>
   </aop:config>
   ```

- 注意:

	```xml
	<!--切点表达式:-->
	execution(public void com.itheima.aop.Target.method())  <!----> <!--指定类的指定方法method 无参-->
	execution(void com.itheima.aop.Target.*(..))  <!--Target类下任意方法, 参数任意-->
	execution(* com.itheima.aop.*.*(..))  <!--aop包下任意类的任意方法 返回值任意 参数任意-->
	execution(* com.itheima.aop..*.*(..))  <!--aop包及子包下任意类和任意方法 参数任意-->
	<!--通知方法类型-->
	<aop:before></aop:before>		<!--前置通知-->
	<aop:after></aop:after>			<!--后置通知-->
	<aop:around></aop:around>		<!--环绕通知 前置+后置-->
	<aop:throwing></aop:throwing>	<!--异常通知-->
	<aop:after></aop:after>			<!--最终通知-->
	```

### 8.4 注解实现AOP

1. 指定目标接口和目标类和切面类

   ```java
   @Component("target")
   public class Target implements TargetInterface {
   	@Override
   	public void method() {
   		System.out.println("Target running....");
   	}
   }
   @Component("myAspect")
   public class MyAspect {
   	public void before(){
   		System.out.println("前置代码增强.....");
   	}
   }
   ```

   

2. 在切面类中进行织入

   ```java
   @Component("myAspect")
   @Aspect
   public class MyAspect {
   	@Before("execution(* com.aop.*.*(..))")
   	public void before(){
   		System.out.println("前置代码增强.....");
   	}
   }
   ```

- 注意
  ```java
  @Before				// 前置通知
  @AfterReturning		// 后置通知
  @Around				// 环绕通知
  @AfterThrowing		// 异常通知
  @After				// 最终通知
  ```

## 9. Spring整合

### 9.1 Spring整合Junit

1. 导入Spring集成Junit坐标

   ```xml
   <!--此处需要注意的是，spring5 及以上版本要求 junit 的版本必须是 4.12 及以上-->
   <dependency>
   	<groupId>org.springframework</groupId>
   	<artifactId>spring-test</artifactId>
   	<version>5.0.2.RELEASE</version>
   </dependency>
   <dependency>
   	<groupId>junit</groupId>
   	<artifactId>junit</artifactId>
   	<version>4.12</version>
   	<scope>test</scope>
   </dependency>
   ```

2. 使用示例

   > @ContextConfiguration指定配置文件或配置类
   >
   > @Autowired注入需要测试的对象

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   //@ContextConfiguration(value = {"classpath:applicationContext.xml"})
   @ContextConfiguration(classes = {SpringConfiguration.class})
   public class SpringJunitTest {
   	@Autowired
   	private UserService userService;
       
   	@Test
   	public void testUserService(){
   		userService.save();
   	}
   }
   ```



### 9.2 Spring整合JdbcTemplate

1. 导入坐标

	```xml
	<!--导入spring的jdbc坐标-->
	<dependency>
	  <groupId>org.springframework</groupId>
	  <artifactId>spring-jdbc</artifactId>
	  <version>5.0.5.RELEASE</version>
	</dependency>
	<!--导入spring的tx坐标-->
	<dependency>
	  <groupId>org.springframework</groupId>
	  <artifactId>spring-tx</artifactId>
	  <version>5.0.5.RELEASE</version>
	</dependency>
	```

3. 创建JdbcTemplate对象,执行数据库CRUD操作

	```java
	@RunWith(SpringJUnit4ClassRunner.**class**)
	@ContextConfiguration("classpath:applicationContext.xml")
	public class JdbcTemplateCRUDTest {
		@Autowired
		private JdbcTemplate jdbcTemplate;
		
		@Test
		public void testCRUD(){
	        // 新增
			String sql = "insert into account values(?,?)";
			jdbcTemplate.update(sql, "lucy", 5000);
	        
	        // 修改
        String sql = "update account set money=? where name=?";
			jdbcTemplate.update(sql, 1000, "tom");
	        
	        // 删除
	        String sql = "delete from account where name=?";
			jdbcTemplate.update(sql, "tom");
	        
	        // 查询全部
	        String sql = "select * from account";
			List<Account> accounts = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Account>(Account.class));
	        
	        // 查询单个
	        String sql = "select * from account where name=?";
	Account account = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Account>(Account.class), "tom");
	        // 查询聚合
	        String sql = "select count(*) from account";
			jdbcTemplate.queryForObject(sql, Long.class);
		}
	}
	```
	
### 9.3  Spring整合事务

- 事务环境准备

  ```xml
  <!--pom.xml中指定事务坐标-->
  <dependency>
  	<groupId>org.springframework</groupId>
  	<artifactId>spring-tx</artifactId>
  	<version>5.0.5.RELEASE</version>
  </dependency>
  ```

- xml实现aop声明式事务

    - `applicationContext.xml`配置aop的声明式事务

        ```xml
        <!--配置平台事务管理器-->
        <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
            <property name="dataSource" ref="dataSource"></property>
        </bean>

        <!--通知-->
        <tx:advice id="txAdvice" transaction-manager="transactionManager">
            <tx:attributes>
                <tx:method name="transfer" isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="-1" read-only="false"/>
            </tx:attributes>
        </tx:advice>

        <!--将通知织入目标对象实现aop事务-->
        <aop:config>
            <aop:pointcut id="txPointcut" expression="execution(* com.service.impl.*.*(..))"/>
            <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
        </aop:config>
        ```

    - 注意

      ```xml
      <tx:method> 参数说明
      - name：切点方法名称
      - isolation:事务的隔离级别
      - propogation：事务的传播行为
      - timeout：超时时间
      - read-only：是否只读
      ```

- 注解实现声明式事务控制

  - `applicationContext.xml`配置事务注解驱动

    ```xml
    <!--事务的注解驱动-->
    <tx:annotation-driven transaction-manager="transactionManager"/>
    ```

  - 在需要事务的类上使用注解事务

    ```java
    @Transactional(isolation = Isolation.REPEATABLE_READ)
    public class AccountServiceImpl implements AccountService {
    	@Transactional(isolation = Isolation.READ_COMMITTED, propagation = Propagation.REQUIRED)
    	public void transfer(String outMan, String inMan, double money) {
    		accountDao.out(outMan,money);
    		int i = 1/0;
    		accountDao.in(inMan,money);
    	}
    }
    ```

  - 注解配置声明式事务控制解析

  2. @Transactional注解使用在类上，那么该类下的所有方法都使用同一套注解参数配置。
  3. @Transactional使用在方法上，不同的方法可以采用不同的事务参数配置。