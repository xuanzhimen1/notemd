## 1. Spring整合SpringMVC





















## 1. Spring和Spring-mvc的整合

### 1.1 导入坐标

- `pom.xml`

	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0"
			 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
		<modelVersion>4.0.0</modelVersion>

		<groupId>cn.itcast</groupId>
		<artifactId>ss</artifactId>
		<version>1.0-SNAPSHOT</version>

		<!--指定打包 -->
		<packaging>war</packaging>

		<!--指定项目信息-->
		<name>ss</name>
		<url>http://www.ss.com</url>

		<!--统一指定spring版本-->
		<properties>
			<spring.version>5.0.5.RELEASE</spring.version>
		</properties>

		<!--项目依赖-->
		<dependencies>
			<!--mysql-->
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
				<version>5.1.32</version>
			</dependency>
			<!--c3p0-->
			<dependency>
				<groupId>c3p0</groupId>
				<artifactId>c3p0</artifactId>
				<version>0.9.1.2</version>
			</dependency>
			<!--druid-->
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>druid</artifactId>
				<version>1.1.10</version>
			</dependency>
			<!--junit-->
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>4.12</version>
			</dependency>
			<!--spring-->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<!--spring-jdbc & spring-tx-->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-jdbc</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-tx</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<!--spring-mvc-->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-web</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-webmvc</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<!--servlet&jsp-->
			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>javax.servlet-api</artifactId>
				<version>3.0.1</version>
				<scope>provided</scope>
			</dependency>
			<dependency>
				<groupId>javax.servlet.jsp</groupId>
				<artifactId>javax.servlet.jsp-api</artifactId>
				<version>2.2.1</version>
				<scope>provided</scope>
			</dependency>
			<!--jackson-->
			<dependency>
				<groupId>com.fasterxml.jackson.core</groupId>
				<artifactId>jackson-databind</artifactId>
				<version>2.9.0</version>
			</dependency>
			<!--文件上传-->
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
			<!--log4j-->
			<dependency>
				<groupId>commons-logging</groupId>
				<artifactId>commons-logging</artifactId>
				<version>1.2</version>
			</dependency>
			<dependency>
				<groupId>org.slf4j</groupId>
				<artifactId>slf4j-log4j12</artifactId>
				<version>1.7.7</version>
			</dependency>
			<!--jstl-->
			<dependency>
				<groupId>jstl</groupId>
				<artifactId>jstl</artifactId>
				<version>1.2</version>
			</dependency>
		</dependencies>
	</project>
	```

### 1.2 web.xml配置

- web.xml

	```xml
	<!--全局的初始化参数-->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</context-param>
	<!--Spring的监听器-->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<!--SpringMVC的前端控制器-->
	<servlet>
		<servlet-name>DispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:spring-mvc.xml</param-value>
		</init-param>
		<load-on-startup>2</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>DispatcherServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	```

### 1.3 spring-mvc.xml配置

- spring-mvc.xml

	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		   xmlns:mvc="http://www.springframework.org/schema/mvc"
		   xsi:schemaLocation="
		   		http://www.springframework.org/schema/beans 
				http://www.springframework.org/schema/beans/spring-beans.xsd 
				http://www.springframework.org/schema/mvc 
				http://www.springframework.org/schema/mvc/spring-mvc.xsd
			">

		<!--1、mvc注解驱动-->
		<mvc:annotation-driven/>

		<!--2、配置视图解析器-->
		<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			<property name="prefix" value="/pages/"/>
			<property name="suffix" value=".jsp"/>
		</bean>

		<!--3、静态资源权限开放-->
		<mvc:default-servlet-handler/>
	</beans>
	```
	
### 1.4 spring 配置数据源

- applicationContext.xml

	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		   xmlns:context="http://www.springframework.org/schema/context"
		   xsi:schemaLocation="
		   http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		   http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
	">
	<!--1、加载jdbc.properties-->
		<context:property-placeholder location="classpath:jdbc.properties"/>

		<!--2、配置数据源对象-->
		<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
			<property name="driverClass" value="${jdbc.driver}"/>
			<property name="jdbcUrl" value="${jdbc.url}"/>
			<property name="user" value="${jdbc.username}"/>
			<property name="password" value="${jdbc.password}"/>
		</bean>

		<!--3、配置JdbcTemplate对象-->
		<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
			<property name="dataSource" ref="dataSource"></property>
		</bean>
	</beans>
	```

- jdbc.properties 请查看[文件](http://119.3.209.59/html/blog/blogDetail.html?aid=17)

### 1.5 集成日志log4j

- log4j.properties

	- 请参照 [文件](http://119.3.209.59/html/blog/blogDetail.html?aid=16)

## 2. SSM整合

### 2.1 数据库与表结构

- Oracle介绍
	为每个项目创建单独user, user拥有表空间, 表空间中存放数据表

- 创建用户
	```sql
	create user 用户名 identified by 口令(密码);
	```

- 授权
	```sql
	grant connect, resource to 用户名;
	```

- pl/sql创建用户及授权

	![](http://119.3.209.59/blog_image/jackiezz/SSM_创建用户及授权1.png)
	![](http://119.3.209.59/blog_image/jackiezz/SSM_创建用户及授权2.png)
	![](http://119.3.209.59/blog_image/jackiezz/SSM_创建用户及授权3.png)
	对象权限是指针对于某一张表的操作权限,系统权限是指对表的CRUD操作权限, 角色权限是系统权限的集合,我们设置时，一般是设置角色权限，设置resource与connect

- 创建表
	
	- 示例
	```sql
	CREATE TABLE product(
		id varchar2(32) default SYS_GUID() PRIMARY KEY,
		productNum VARCHAR2(50) NOT NULL,
		productName VARCHAR2(50),
		cityName VARCHAR2(50),
		DepartureTime timestamp,
		productPrice Number,
		productDesc VARCHAR2(500),
		productStatus INT,
		CONSTRAINT product UNIQUE (id, productNum)
	);
	insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice, productdesc, productstatus) values ('676C5BD1D35E429A8C2E114939C5685A', 'itcast-002', '北京三日游', '北京', to_timestamp('10-10-2018 10:10:00.000000', 'dd-mm-yyyy hh24:mi:ss.ff'), 1200, '不错的旅行', 1);
	insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice, productdesc, productstatus) values ('12B7ABF2A4C544568B0A7C69F36BF8B7', 'itcast-003', '上海五日游', '上海', to_timestamp('25-04-2018 14:30:00.000000', 'dd-mm-yyyy hh24:mi:ss.ff'), 1800, '魔都我来了', 0);
	insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice, productdesc, productstatus) values ('9F71F01CB448476DAFB309AA6DF9497F', 'itcast-001', '北京三日游', '北京', to_timestamp('10-10-2018 10:10:00.000000', 'dd-mm-yyyy hh24:mi:ss.ff'), 1200, '不错的旅行', 1);
	```

- 创建pojo

	```java
	public class Product {
		private String id; // 主键
		private String productNum; // 编号 唯一
		private String productName; // 名称
		private String cityName; // 出发城市
		@DateTimeFormat(pattern="yyyy-MM-dd HH:mm")
		private Date departureTime; // 出发时间
		private String departureTimeStr;
		private double productPrice; // 产品价格
		private String productDesc; // 产品描述
		private Integer productStatus; // 状态 0 关闭 1 开启
		private String productStatusStr;
	}
	```

### 2.2 搭建Maven工程

1. 不使用骨架搭建父工程

	- ssm

2. 创建子工程(Module)

	- ssm-web ssm-domain ssm-service ssm-dao ssm-utils
	- 其中创建ssm-web时注意我们选择一个web工程
		![](http://119.3.209.59/blog_image/jackiezz/SSM_maven创建工程1.png)

3. pom.xml
	- pom中头部内容

	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	```

	- pom中dependency和build

	```xml
	<properties>
        <spring.version>5.0.2.RELEASE</spring.version>
        <slf4j.version>1.6.6</slf4j.version>
        <log4j.version>1.2.12</log4j.version>
        <oracle.version>11.2.0.1.0</oracle.version>
        <mybatis.version>3.4.5</mybatis.version>
        <spring.security.version>5.0.1.RELEASE</spring.security.version>
    </properties>

    <dependencies>        <!-- spring -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.6.8</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>

        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.0</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>        <!-- log start -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
        </dependency>        <!-- log end -->

        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>${mybatis.version}</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.0</version>
        </dependency>
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
            <type>jar</type>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.1.2</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-core</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-taglibs</artifactId>
            <version>${spring.security.version}</version>
        </dependency>

        <dependency>
            <groupId>com.oracle</groupId>
            <artifactId>ojdbc6</artifactId>
            <version>${oracle.version}</version>
        </dependency>

        <dependency>
            <groupId>javax.annotation</groupId>
            <artifactId>jsr250-api</artifactId>
            <version>1.0</version>
        </dependency>
    </dependencies>
    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.2</version>
                    <configuration>
                        <source>1.8</source>
                        <target>1.8</target>
                        <encoding>UTF-8</encoding>
                        <showWarnings>true</showWarnings>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
	```

### 2.3 环境搭建

- 配置文件统一放在ssm_web子模块下

#### 2.3.1 Spring

- applicationContext.xml 配置dao/service扫描包

	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		   xmlns:context="http://www.springframework.org/schema/context"
		   xmlns:aop="http://www.springframework.org/schema/aop"
		   xmlns:tx="http://www.springframework.org/schema/tx"
		   xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd
		http://www.springframework.org/schema/aop
		http://www.springframework.org/schema/aop/spring-aop.xsd
		http://www.springframework.org/schema/tx
		http://www.springframework.org/schema/tx/spring-tx.xsd">
		<!-- 配置 spring 创建容器时要扫描的包 -->
		<!-- 开启注解扫描，管理service和dao -->
		<context:component-scan base-package="com.itheima.ssm.service"></context:component-scan>
		<context:component-scan base-package="com.itheima.ssm.dao"></context:component-scan>
	</beans>
	```

#### 2.3.2 Spring MVC

- web.xml头部内容

	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			 xmlns="http://xmlns.jcp.org/xml/ns/javaee"
			 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
			 version="3.1">
	
	</web-app>
	```

- web.xml配置SpringMVC核心控制器

	```xml
    <!-- 前端控制器（加载classpath:springmvc.xml 服务器启动创建servlet） -->
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 配置初始化参数，创建完DispatcherServlet对象，加载springmvc.xml配置文件 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <!-- 服务器启动的时候，让DispatcherServlet对象创建 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>

    <!-- 解决中文乱码过滤器 -->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.htm</welcome-file>
        <welcome-file>index.jsp</welcome-file>
        <welcome-file>default.html</welcome-file>
        <welcome-file>default.htm</welcome-file>
        <welcome-file>default.jsp</welcome-file>
    </welcome-file-list>
	```

- spring-mvc头部声明

	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		   xmlns:mvc="http://www.springframework.org/schema/mvc"
		   xmlns:context="http://www.springframework.org/schema/context"
		   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		   xmlns:aop="http://www.springframework.org/schema/aop"
		   xsi:schemaLocation="
			   http://www.springframework.org/schema/beans
			   http://www.springframework.org/schema/beans/spring-beans.xsd
			   http://www.springframework.org/schema/mvc
			   http://www.springframework.org/schema/mvc/spring-mvc.xsd
			   http://www.springframework.org/schema/context
			   http://www.springframework.org/schema/context/spring-context.xsd
			   http://www.springframework.org/schema/aop
			http://www.springframework.org/schema/aop/spring-aop.xsd
			   ">
	
	</beans>
	```

- spring-mvc.xml

	```xml
	<!-- 扫描controller的注解，别的不扫描 -->
	<context:component-scan base-package="com.itheima.ssm.controller"></context:component-scan>
	<!-- 配置视图解析器 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<!-- JSP文件所在的目录 -->
		<property name="prefix" value="/pages/" />
		<!-- 文件的后缀名 -->
		<property name="suffix" value=".jsp" />
	</bean>
	<!-- 设置静态资源不过滤 -->
	<mvc:resources location="/css/" mapping="/css/**" />
	<mvc:resources location="/img/" mapping="/img/**" />
	<mvc:resources location="/js/" mapping="/js/**" />
	<mvc:resources location="/plugins/" mapping="/plugins/**" />
	<!-- 开启对SpringMVC注解的支持 -->
	<mvc:annotation-driven />
	<!--
	支持AOP的注解支持，AOP底层使用代理技术
	JDK动态代理，要求必须有接口
	cglib代理，生成子类对象，proxy-target-class="true" 默认使用cglib的方式
	-->
	<aop:aspectj-autoproxy proxy-target-class="true"/>
	```

#### 2.3.3 Spring与SpringMVC整合

- web.xml配置
	```xml
	<!-- 配置加载类路径的配置文件 -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath*:applicationContext.xml</param-value>
	</context-param>
	<!-- 配置监听器 -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	```

#### 2.3.4 Spring与MyBatis整合

- Spring接管mybatis的SqlSessionFactoryBean工厂

	- db.properties
	
		```json
		jdbc.driver=oracle.jdbc.driver.OracleDriver
		jdbc.url=jdbc:oracle:thin:@192.168.161.10:1521:orcl
		jdbc.username=ssm
		jdbc.password=itcast
		```

	- applicationContext.xml
		```xml
		<context:property-placeholder location="classpath:db.properties"/>
		<!-- 配置连接池 -->
		<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
			<property name="driverClass" value="${jdbc.driver}" />
			<property name="jdbcUrl" value="${jdbc.url}" />
			<property name="user" value="${jdbc.username}" />
			<property name="password" value="${jdbc.password}" />
		</bean>
		<!-- 把SqlSessionFactory交给IOC管理 -->
		<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
			<property name="dataSource" ref="dataSource" />
			<!-- 传入PageHelper的插件 -->
			<property name="plugins">
				<array>
					<!-- 传入插件的对象 -->
					<bean class="com.github.pagehelper.PageInterceptor">
						<property name="properties">
							<props>
								<prop key="helperDialect">oracle</prop>
								<prop key="reasonable">true</prop>
							</props>
						</property>
					</bean>
				</array>
			</property>
		</bean>
		```

- 自动扫描所有Mapper接口和文件

	```xml
	<!-- 扫描dao接口 -->
	<bean id="mapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="com.itheima.ssm.dao"/>
	</bean>
	```

- 配置Spring事务

	```xml
	<!-- 配置Spring的声明式事务管理 -->
	<!-- 配置事务管理器 -->
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"/>
	</bean>
	<tx:annotation-driven transaction-manager="transactionManager"/>
	```

### 2.4 编写实体类,业务接口,持久层接口,表现层测试

- 实体类

	```java
	public class Product {
		private String id; // 主键
		private String productNum; // 编号 唯一
		private String productName; // 名称
		private String cityName; // 出发城市
		private Date departureTime; // 出发时间
		private String departureTimeStr;
		private double productPrice; // 产品价格
		private String productDesc; // 产品描述
		private Integer productStatus; // 状态 0 关闭 1 开启
		private String productStatusStr;
	}
	```

- 业务接口

	```java
	public interface ProductService {
		List<Product> findAll() throws Exception;
	}
	
	@Service
	@Transactional
	public class ProductServiceImpl implements ProductService {
		@Autowired
		private ProductDao productDao;

		@Override
		public List<Product> findAll() throws Exception {
			return productDao.findAll();
		}
	}
	```

- 持久层接口

	```java
	public interface ProductDao {
		@Select("select * from product")
		List<Product> findAll() throws Exception;
	}
	```

- 控制层

	```java
	@Controller
	@RequestMapping("/product")
	public class ProductController {
		@Autowired
		private ProductService productService;

		@RequestMapping("/findAll.do")
		public ModelAndView findAll() throws Exception {
			ModelAndView mv = new ModelAndView();
			List<Product> produlist = productService.findAll();
			mv.setViewName("product-list");
			mv.addObject("productList", produlist);
			return mv;
		}
	}
	```

## 3. SSM电商框架

### 3.1 搭建框架

- 父工程: pinyougou-parent(POM)

	- 锁定版本信息 dependencyManagement 与 pluginManagement
	
		```xml
		<!-- 集中定义依赖版本号 -->
		<properties>
			<junit.version>4.12</junit.version>
			<spring.version>4.2.4.RELEASE</spring.version>
			<pagehelper.version>4.0.0</pagehelper.version>
			<servlet-api.version>2.5</servlet-api.version>
			<dubbo.version>2.8.4</dubbo.version>
			<zookeeper.version>3.4.7</zookeeper.version>
			<zkclient.version>0.1</zkclient.version>		
			<mybatis.version>3.2.8</mybatis.version>
			<mybatis.spring.version>1.2.2</mybatis.spring.version>
			<mybatis.paginator.version>1.2.15</mybatis.paginator.version>
			<mysql.version>5.1.32</mysql.version>		
			<druid.version>1.0.9</druid.version>
			<commons-fileupload.version>1.3.1</commons-fileupload.version>
			<freemarker.version>2.3.23</freemarker.version>
			<activemq.version>5.11.2</activemq.version>
			<security.version>3.2.3.RELEASE</security.version>		
			<solrj.version>4.10.3</solrj.version>
			<ik.version>2012_u6</ik.version>		
		</properties>
		<!-- 依赖版本管理 -->
		<dependencyManagement>
			<dependencies>	
				<!-- Spring -->
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-context</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-beans</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-webmvc</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-jdbc</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-aspects</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-jms</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-context-support</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-test</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<!-- dubbo相关 -->
				<dependency>
					<groupId>com.alibaba</groupId>
					<artifactId>dubbo</artifactId>
					<version>${dubbo.version}</version>
				</dependency>
				<dependency>
					<groupId>org.apache.zookeeper</groupId>
					<artifactId>zookeeper</artifactId>
					<version>${zookeeper.version}</version>
				</dependency>
				<dependency>
					<groupId>com.github.sgroschupf</groupId>
					<artifactId>zkclient</artifactId>
					<version>${zkclient.version}</version>
				</dependency>
				<dependency>
					<groupId>junit</groupId>
					<artifactId>junit</artifactId>
					<version>4.9</version>
				</dependency>
				<dependency>
					<groupId>com.alibaba</groupId>
					<artifactId>fastjson</artifactId>
					<version>1.2.28</version>
				</dependency>
				<dependency>
					<groupId>javassist</groupId>
					<artifactId>javassist</artifactId>
					<version>3.11.0.GA</version>
				</dependency>
				<dependency>
					<groupId>commons-codec</groupId>
					<artifactId>commons-codec</artifactId>
					<version>1.10</version>
				</dependency>
				<dependency>
					<groupId>javax.servlet</groupId>
					<artifactId>servlet-api</artifactId>
					<version>2.5</version>
					<scope>provided</scope>
				</dependency>
				<dependency>
					<groupId>com.github.pagehelper</groupId>
					<artifactId>pagehelper</artifactId>
					<version>${pagehelper.version}</version>
				</dependency>		
				<!-- Mybatis -->
				<dependency>
					<groupId>org.mybatis</groupId>
					<artifactId>mybatis</artifactId>
					<version>${mybatis.version}</version>
				</dependency>
				<dependency>
					<groupId>org.mybatis</groupId>
					<artifactId>mybatis-spring</artifactId>
					<version>${mybatis.spring.version}</version>
				</dependency>
				<dependency>
					<groupId>com.github.miemiedev</groupId>
					<artifactId>mybatis-paginator</artifactId>
					<version>${mybatis.paginator.version}</version>
				</dependency>		
				<!-- MySql -->
				<dependency>
					<groupId>mysql</groupId>
					<artifactId>mysql-connector-java</artifactId>
					<version>${mysql.version}</version>
				</dependency>
				<!-- 连接池 -->
				<dependency>
					<groupId>com.alibaba</groupId>
					<artifactId>druid</artifactId>
					<version>${druid.version}</version>
				</dependency>		
				<dependency>
					<groupId>org.csource.fastdfs</groupId>
					<artifactId>fastdfs</artifactId>
					<version>1.2</version>
				</dependency>
				<!-- 文件上传组件 -->
				<dependency>
					<groupId>commons-fileupload</groupId>
					<artifactId>commons-fileupload</artifactId>
					<version>${commons-fileupload.version}</version>
				</dependency>		
				<!-- 缓存 -->
				<dependency> 
				  <groupId>redis.clients</groupId> 
				  <artifactId>jedis</artifactId> 
				  <version>2.8.1</version> 
				</dependency> 
				<dependency> 
				  <groupId>org.springframework.data</groupId> 
				  <artifactId>spring-data-redis</artifactId> 
				  <version>1.7.2.RELEASE</version> 
				</dependency>		
				<dependency>
					<groupId>org.freemarker</groupId>
					<artifactId>freemarker</artifactId>
					<version>${freemarker.version}</version>
				</dependency>		
				<dependency>
					<groupId>org.apache.activemq</groupId>
					<artifactId>activemq-all</artifactId>
					<version>${activemq.version}</version>
				</dependency>
				<!-- 身份验证 -->
				<dependency>
					<groupId>org.springframework.security</groupId>
					<artifactId>spring-security-web</artifactId>
					<version>4.1.0.RELEASE</version>
				</dependency>
				<dependency>
					<groupId>org.springframework.security</groupId>
					<artifactId>spring-security-config</artifactId>
					<version>4.1.0.RELEASE</version>
				</dependency>		
				<dependency>
					<groupId>com.github.penggle</groupId>
					<artifactId>kaptcha</artifactId>
					<version>2.3.2</version>
					<exclusions>
						<exclusion>
							<groupId>javax.servlet</groupId>
							<artifactId>javax.servlet-api</artifactId>
						</exclusion>
					</exclusions>
				</dependency>		
				<dependency>  
					<groupId>org.springframework.security</groupId>  
					<artifactId>spring-security-cas</artifactId>  
					<version>4.1.0.RELEASE</version>  
				</dependency>  
				<dependency>  
					<groupId>org.jasig.cas.client</groupId>  
					<artifactId>cas-client-core</artifactId>  
					<version>3.3.3</version>  
					<!-- 排除log4j包冲突 -->  
					<exclusions>  
						<exclusion>  
							<groupId>org.slf4j</groupId>  
							<artifactId>log4j-over-slf4j</artifactId>  
						</exclusion>  
					</exclusions>  
				</dependency> 	    
				<!-- solr客户端 -->
				<dependency>
					<groupId>org.apache.solr</groupId>
					<artifactId>solr-solrj</artifactId>
					<version>${solrj.version}</version>
				</dependency>
				<dependency>
					<groupId>com.janeluo</groupId>
					<artifactId>ikanalyzer</artifactId>
					<version>${ik.version}</version>
				</dependency>	
				<dependency>
					<groupId>org.apache.httpcomponents</groupId>
					<artifactId>httpcore</artifactId>
					<version>4.4.4</version>
				</dependency>  		
				<dependency>
					<groupId>org.apache.httpcomponents</groupId>
					<artifactId>httpclient</artifactId>
					<version>4.5.3</version>
				</dependency>
				<dependency>
					<groupId>dom4j</groupId>
					<artifactId>dom4j</artifactId>
					<version>1.6.1</version>
				</dependency>  		
				<dependency>  
					<groupId>xml-apis</groupId>  
					<artifactId>xml-apis</artifactId>  
					<version>1.4.01</version>  
				</dependency> 		
			</dependencies>	
		</dependencyManagement>

		<build>
			<plugins>			
				<!-- java编译插件 -->
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-compiler-plugin</artifactId>
					<version>3.2</version>
					<configuration>
						<source>1.7</source>
						<target>1.7</target>
						<encoding>UTF-8</encoding>
					</configuration>
				</plugin>
			</plugins>
		</build>
		```

- 通用实体类模块 pinyougou-pojo(JAR)

- 通用数据访问模块 pinyougou-dao(JAR)

	- 添加依赖 Mybatis 和 pinyougou-pojo
		```xml
		<dependencies>
			<!-- Mybatis -->
			<dependency>
				<groupId>org.mybatis</groupId>
				<artifactId>mybatis</artifactId>
			</dependency>
			<dependency>
				<groupId>org.mybatis</groupId>
				<artifactId>mybatis-spring</artifactId>
			</dependency>
			<dependency>
				<groupId>com.github.miemiedev</groupId>
				<artifactId>mybatis-paginator</artifactId>
			</dependency>
			<!-- MySql -->
			<dependency>
				<groupId
		```









## 1. 路由及视图

### 1.1 返回普通视图

- 示例
	
	```java
	@Controller
	@RequestMapping("/product")
	public class ProductController {
		@Autowired
		private ProductService productService;

		@RequestMapping("/findAll.do")
		public ModelAndView findAll () throws Exception {
			ModelAndView mv = new ModelAndView();
			List<Product> productList = productService.findAll();
			mv.setViewName("product-list");
			mv.addObject("productList", productList);
			return mv;
		}
	}
	```

### 1.2 转发视图

- 示例

	```java
	@RequestMapping("/save.do")
	public String save (Product product) throws Exception {
		productService.save(product);
		return "redirect:findAll.do";
	}
	```

## 2. 绑定参数的类型转换

### 2.1 时间与字符串转换

- 实体类中加日期转换注解  --局部处理,本类可用

	```java
	@DateTimeFormat(pattern="yyyy-MM-dd HH:mm")
	private Date startTime;
	```

- 类型转换器Converter -- 全局处理,所有类都可用

## 3. 插件相关

### 3.1 pageHelper分页插件

- 介绍

	> [gitee源代码及介绍](https://gitee.com/free/Mybatis_PageHelper)

- 集成到maven

	```xml
	<!--pom.xml中定义pageHelper分页插件的坐标-->
	<dependency>
		<groupId>com.github.pagehelper</groupId>
		<artifactId>pagehelper</artifactId>
		<version>5.1.2</version>
	</dependency>
	```

- 集成到spring中

	```xml
	<!-- 把SqlSessionFactory交给IOC管理 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="dataSource" ref="dataSource"/>
        <property name="plugins">
            <array>
                <bean class="com.github.pagehelper.PageInterceptor">
                    <property name="properties">
                        <props>
                            <prop key="helperDialect">oracle</prop>
                            <prop key="reasonable">true</prop>
                        </props>
                    </property>
                </bean>
            </array>
        </property>
    </bean>
	```

- 路由

	```
	${pageContext.request.contextPath}/orders/findAll.do?page=1&pageSize=3
	```

- controller

	```java
	public ModelAndView findAll(@RequestParam(name = "page", required = true, defaultValue = "1") int page,
                                @RequestParam(name = "pageSize", required = true, defaultValue = "10") int pageSize
                                ) throws Exception {
        ModelAndView mv = new ModelAndView();
        List<Orders> ordersList = orderService.findAll(page, pageSize);
        PageInfo pageInfo = new PageInfo(ordersList);
        mv.setViewName("orders-list");
        mv.addObject("pageInfo", pageInfo);
        return mv;
    }
	```

- service

	```java
	public List<Orders> findAll(Integer page, Integer pageSize) throws Exception{
        PageHelper.startPage(page, pageSize);
        return orderDao.findAll();
    }
	```

- 页面显示

	```jsp
	<c:forEach items="${pageInfo.list}" var="orders">  <!--apgeInfo.list 为封装为数据列表-->
		${pageInfo.pageSize}  <!--每页显示条数-->
		${pageInfo.pageNum}  <!--当前页数-->
		${pageInfo.total}  <!--总页数-->
	</c:forEach>
	```

### 3.2 Spring Security

#### 3.2.1 配置
- maven依赖

	```xml
		<!--Spring Security-->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-core</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-taglibs</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
	```

- web.xml

	- 使用Spring Security提供的登录页面配置

	```xml
	<context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-security.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <filter>
        <filter-name>springSecurityFilterChain</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>springSecurityFilterChain</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
	```

	- 使用自定义页面
	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		   xmlns:security="http://www.springframework.org/schema/security"
		   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		   xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/security
		http://www.springframework.org/schema/security/spring-security.xsd">

		<!-- 配置不拦截的资源 -->
		<security:http pattern="/login.jsp" security="none"/>
		<security:http pattern="/failer.jsp" security="none"/>
		<security:http pattern="/css/**" security="none"/>
		<security:http pattern="/img/**" security="none"/>
		<security:http pattern="/plugins/**" security="none"/>
		<!--
			配置具体的规则
			auto-config="true"	不用自己编写登录的页面，框架提供默认登录页面
			use-expressions="false"	是否使用SPEL表达式（没学习过）
		-->
		<security:http auto-config="true" use-expressions="false">
			<!-- 配置具体的拦截的规则 pattern="请求路径的规则" access="访问系统的人，必须有ROLE_USER的角色" -->
			<security:intercept-url pattern="/**" access="ROLE_USER,ROLE_ADMIN"/>

			<!-- 定义跳转的具体的页面 -->
			<security:form-login
					login-page="/login.jsp"
					login-processing-url="/login.do"
					default-target-url="/index.jsp"
					authentication-failure-url="/failer.jsp"
					authentication-success-forward-url="/pages/main.jsp"
			/>

			<!-- 关闭跨域请求 -->
			<security:csrf disabled="true"/>
			<!-- 退出 -->
			<security:logout invalidate-session="true" logout-url="/logout.do" logout-success-url="/login.jsp" />

		</security:http>

		<!-- 切换成数据库中的用户名和密码 -->
		<security:authentication-manager>
			<security:authentication-provider user-service-ref="userService">
				<!-- 配置加密的方式
				<security:password-encoder ref="passwordEncoder"/>-->
			</security:authentication-provider>
		</security:authentication-manager>

		<!-- 配置加密类 -->
		<bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>

		<!-- 提供了入门的方式，在内存中存入用户名和密码
		<security:authentication-manager>
			<security:authentication-provider>
				<security:user-service>
					<security:user name="admin" password="{noop}admin" authorities="ROLE_USER"/>
				</security:user-service>
			</security:authentication-provider>
		</security:authentication-manager>
		-->
	</beans>
	```

#### 3.2.2数据库认证

- 使用UserDetails和UserDetailsService方式完成

- UserDetails
	
> UserDetails是一个接口，我们可以认为UserDetails作用是于封装当前进行认证的用户信息，但由于其是一个 接口，所以我们可以对其进行实现，也可以使用Spring Security提供的一个UserDetails的实现类User来完成 操作 

- UserDetailsService

	> 对认证系统提供一些方法为一个接口







## 1. 数据交互层

### 1.1 新增数据时返回对应ID

- 新增数据时返回对应ID, 使用`<selectKey>`标签

	```xml
	  <insert id="insert" parameterType="com.pinyougou.pojo.TbSpecification" >
		<selectKey resultType="java.lang.Long" order="AFTER" keyProperty="id">
			SELECT LAST_INSERT_ID() AS id
		</selectKey>
		insert into tb_specification (id, spec_name) values (#{id,jdbcType=BIGINT}, #{specName,jdbcType=VARCHAR})
	</insert>
	```





## 1. 工程说明

- pinyougou-parent 聚合工程
- pinyougou-pojo 通用实体类层
- pinyougou-dao 通用数据访问层
- pinyougou-xxxxx-interface  某服务层接口
- pinyougou-xxxxx-service   某服务层实现
- pinyougou-xxxxx-web     某web工程

## 2. 创建数据库表

## 3. 搭建框架

### 3.1 父工程

- pinyougou-parent(POM)

	- pom.xml 添加锁定版本信息
	
		```xml
		<!-- 集中定义依赖版本号 -->
		<properties>
			<junit.version>4.12</junit.version>
			<spring.version>4.2.4.RELEASE</spring.version>
			<pagehelper.version>4.0.0</pagehelper.version>
			<servlet-api.version>2.5</servlet-api.version>
			<dubbo.version>2.8.4</dubbo.version>
			<zookeeper.version>3.4.7</zookeeper.version>
			<zkclient.version>0.1</zkclient.version>		
			<mybatis.version>3.2.8</mybatis.version>
			<mybatis.spring.version>1.2.2</mybatis.spring.version>
			<mybatis.paginator.version>1.2.15</mybatis.paginator.version>
			<mysql.version>5.1.32</mysql.version>		
			<druid.version>1.0.9</druid.version>
			<commons-fileupload.version>1.3.1</commons-fileupload.version>
			<freemarker.version>2.3.23</freemarker.version>
			<activemq.version>5.11.2</activemq.version>
			<security.version>3.2.3.RELEASE</security.version>		
			<solrj.version>4.10.3</solrj.version>
			<ik.version>2012_u6</ik.version>		
		</properties>

		<dependencyManagement>
			<dependencies>	
				<!-- Spring -->
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-context</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-beans</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-webmvc</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-jdbc</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-aspects</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-jms</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-context-support</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-test</artifactId>
					<version>${spring.version}</version>
				</dependency>
				<!-- dubbo相关 -->
				<dependency>
					<groupId>com.alibaba</groupId>
					<artifactId>dubbo</artifactId>
					<version>${dubbo.version}</version>
				</dependency>
				<dependency>
					<groupId>org.apache.zookeeper</groupId>
					<artifactId>zookeeper</artifactId>
					<version>${zookeeper.version}</version>
				</dependency>
				<dependency>
					<groupId>com.github.sgroschupf</groupId>
					<artifactId>zkclient</artifactId>
					<version>${zkclient.version}</version>
				</dependency>
				<dependency>
					<groupId>junit</groupId>
					<artifactId>junit</artifactId>
					<version>4.9</version>
				</dependency>
				<dependency>
					<groupId>com.alibaba</groupId>
					<artifactId>fastjson</artifactId>
					<version>1.2.28</version>
				</dependency>
				<dependency>
					<groupId>javassist</groupId>
					<artifactId>javassist</artifactId>
					<version>3.11.0.GA</version>
				</dependency>
				<dependency>
					<groupId>commons-codec</groupId>
					<artifactId>commons-codec</artifactId>
					<version>1.10</version>
				</dependency>
				<dependency>
					<groupId>javax.servlet</groupId>
					<artifactId>servlet-api</artifactId>
					<version>2.5</version>
					<scope>provided</scope>
				</dependency>
				<dependency>
					<groupId>com.github.pagehelper</groupId>
					<artifactId>pagehelper</artifactId>
					<version>${pagehelper.version}</version>
				</dependency>		
				<!-- Mybatis -->
				<dependency>
					<groupId>org.mybatis</groupId>
					<artifactId>mybatis</artifactId>
					<version>${mybatis.version}</version>
				</dependency>
				<dependency>
					<groupId>org.mybatis</groupId>
					<artifactId>mybatis-spring</artifactId>
					<version>${mybatis.spring.version}</version>
				</dependency>
				<dependency>
					<groupId>com.github.miemiedev</groupId>
					<artifactId>mybatis-paginator</artifactId>
					<version>${mybatis.paginator.version}</version>
				</dependency>		
				<!-- MySql -->
				<dependency>
					<groupId>mysql</groupId>
					<artifactId>mysql-connector-java</artifactId>
					<version>${mysql.version}</version>
				</dependency>
				<!-- 连接池 -->
				<dependency>
					<groupId>com.alibaba</groupId>
					<artifactId>druid</artifactId>
					<version>${druid.version}</version>
				</dependency>		
				<dependency>
					<groupId>org.csource.fastdfs</groupId>
					<artifactId>fastdfs</artifactId>
					<version>1.2</version>
				</dependency>
				<!-- 文件上传组件 -->
				<dependency>
					<groupId>commons-fileupload</groupId>
					<artifactId>commons-fileupload</artifactId>
					<version>${commons-fileupload.version}</version>
				</dependency>		
				<!-- 缓存 -->
				<dependency> 
				  <groupId>redis.clients</groupId> 
				  <artifactId>jedis</artifactId> 
				  <version>2.8.1</version> 
				</dependency> 
				<dependency> 
				  <groupId>org.springframework.data</groupId> 
				  <artifactId>spring-data-redis</artifactId> 
				  <version>1.7.2.RELEASE</version> 
				</dependency>		
				<dependency>
					<groupId>org.freemarker</groupId>
					<artifactId>freemarker</artifactId>
					<version>${freemarker.version}</version>
				</dependency>		
				<dependency>
					<groupId>org.apache.activemq</groupId>
					<artifactId>activemq-all</artifactId>
					<version>${activemq.version}</version>
				</dependency>
				<!-- 身份验证 -->
				<dependency>
					<groupId>org.springframework.security</groupId>
					<artifactId>spring-security-web</artifactId>
					<version>4.1.0.RELEASE</version>
				</dependency>
				<dependency>
					<groupId>org.springframework.security</groupId>
					<artifactId>spring-security-config</artifactId>
					<version>4.1.0.RELEASE</version>
				</dependency>		
				<dependency>
					<groupId>com.github.penggle</groupId>
					<artifactId>kaptcha</artifactId>
					<version>2.3.2</version>
					<exclusions>
						<exclusion>
							<groupId>javax.servlet</groupId>
							<artifactId>javax.servlet-api</artifactId>
						</exclusion>
					</exclusions>
				</dependency>		
				<dependency>  
					<groupId>org.springframework.security</groupId>  
					<artifactId>spring-security-cas</artifactId>  
					<version>4.1.0.RELEASE</version>  
				</dependency>  
				<dependency>  
					<groupId>org.jasig.cas.client</groupId>  
					<artifactId>cas-client-core</artifactId>  
					<version>3.3.3</version>  
					<!-- 排除log4j包冲突 -->  
					<exclusions>  
						<exclusion>  
							<groupId>org.slf4j</groupId>  
							<artifactId>log4j-over-slf4j</artifactId>  
						</exclusion>  
					</exclusions>  
				</dependency> 	    
				<!-- solr客户端 -->
				<dependency>
					<groupId>org.apache.solr</groupId>
					<artifactId>solr-solrj</artifactId>
					<version>${solrj.version}</version>
				</dependency>
				<dependency>
					<groupId>com.janeluo</groupId>
					<artifactId>ikanalyzer</artifactId>
					<version>${ik.version}</version>
				</dependency>	
				<dependency>
					<groupId>org.apache.httpcomponents</groupId>
					<artifactId>httpcore</artifactId>
					<version>4.4.4</version>
				</dependency>  		
				<dependency>
					<groupId>org.apache.httpcomponents</groupId>
					<artifactId>httpclient</artifactId>
					<version>4.5.3</version>
				</dependency>
				<dependency>
					<groupId>dom4j</groupId>
					<artifactId>dom4j</artifactId>
					<version>1.6.1</version>
				</dependency>  		
				<dependency>  
					<groupId>xml-apis</groupId>  
					<artifactId>xml-apis</artifactId>  
					<version>1.4.01</version>  
				</dependency> 		
			</dependencies>	
		</dependencyManagement>

		<build>
			<plugins>			
				<!-- java编译插件 -->
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-compiler-plugin</artifactId>
					<version>3.2</version>
					<configuration>
						<source>1.7</source>
						<target>1.7</target>
						<encoding>UTF-8</encoding>
					</configuration>
				</plugin>
			</plugins>
		</build>
		```

### 3.2 通用实体类模块

- pinyougou-pojo(JAR)

	- 利用反向工程generatorSqlmapCustom实现实体类代码的自动生成
	
		- 将com.pinyougou.pojo包拷贝到pojo工程
		- 每个pojo实现Serializable接口
	
	- 创建entity.PageResult类
	
		```java
		package entity;

		import java.io.Serializable;
		import java.util.List;
		/**
		 * 分页结果类
		 */
		public class PageResult implements Serializable{
			private long total;//总记录数
			private List rows;//当前页记录
			public PageResult(long total, List rows) {
				super();
				this.total = total;
				this.rows = rows;
			}
			get set ...
		}
		```
	
	- 创建entity.Result类
	
		```java
		package entity;
		import java.io.Serializable;
		/**
		 * 返回结果
		 */
		public class Result implements Serializable{
			private boolean success;//是否成功
			private String message;//返回信息
			
			public Result(boolean success, String message) {
				super();
				this.success = success;
				this.message = message;
			}
			get set ...
		}
		```

### 3.3 通用数据访问模块

- pinyougou-dao(JAR)

	- 利用反向工程generatorSqlmapCustom实现数据访问层代码的自动生成
	
		- 将com.pinyougou.mapper包和resouce下的com.pinyougou.mapper文件夹拷贝到dao工程

	- pom.xml 依赖mybatis和pinyougou-pojo
	
		```xml
		<dependencies>
			<!-- Mybatis -->
			<dependency>
				<groupId>org.mybatis</groupId>
				<artifactId>mybatis</artifactId>
			</dependency>
			<dependency>
				<groupId>org.mybatis</groupId>
				<artifactId>mybatis-spring</artifactId>			
			</dependency>
			<dependency>
				<groupId>com.github.miemiedev</groupId>
				<artifactId>mybatis-paginator</artifactId>
			</dependency>		
			<!-- MySql -->
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
			</dependency>
			<!-- 连接池 -->
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>druid</artifactId>
			</dependency>	
			<!-- 依赖pojo手动添加 -->
		</dependencies>
		```
	
	- 配置文件部分
	
		- /src/main/resources/mybatis/SqlMapConfig.xml
		
			```xml
			<?xml version="1.0" encoding="UTF-8" ?>
			<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
			<configuration>
				<plugins>
					<!-- com.github.pagehelper 为 PageHelper 类所在包名 -->
					<plugin interceptor="com.github.pagehelper.PageHelper">
						<!-- 设置数据库类型 Oracle,Mysql,MariaDB,SQLite,Hsqldb,PostgreSQL 六种数据库-->
						<property name="dialect" value="mysql"/>
					</plugin>
				</plugins>
			</configuration>
			```

		- /src/main/resources/properties/db.properties
		
			```xml
			jdbc.driver=com.mysql.jdbc.Driver
			jdbc.url=jdbc:mysql://localhost:3306/pinyougoudb?characterEncoding=utf-8
			jdbc.username=root
			jdbc.password=root
			```

		- /src/main/resources/spring/applicationContext-dao.xml
		
			```xml
			<?xml version="1.0" encoding="UTF-8"?>
			<beans xmlns="http://www.springframework.org/schema/beans"
				xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
				xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
				xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
				xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
				http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
				http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd 
				http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
				http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">

				<!-- 数据库连接池 -->
				<!-- 加载配置文件 -->
				<context:property-placeholder location="classpath*:properties/*.properties" />
				<!-- 数据库连接池 -->
				<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
					destroy-method="close">
					<property name="url" value="${jdbc.url}" />
					<property name="username" value="${jdbc.username}" />
					<property name="password" value="${jdbc.password}" />
					<property name="driverClassName" value="${jdbc.driver}" />
					<property name="maxActive" value="10" />
					<property name="minIdle" value="5" />
				</bean>

				<!-- 让spring管理sqlsessionfactory 使用mybatis和spring整合包中的 -->
				<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
					<!-- 数据库连接池 -->
					<property name="dataSource" ref="dataSource" />
					<!-- 加载mybatis的全局配置文件 -->
					<property name="configLocation" value="classpath:mybatis/SqlMapConfig.xml" />
				</bean>
				<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
					<property name="basePackage" value="com.pinyougou.mapper" />
				</bean>
			</beans>
			```

### 3.4 通用工具类模块

- pinyougou-common(JAR)

### 3.5 服务接口模块

- pinyougou-sellergoods-interface(JAR)

	- pom.xml依赖于pojo
	
	- 使用代码生成器HeimaCodeUtil_V2.4生成interface代码
	
		- 拷贝服务接口到com.pinyougou.sellergoods.service包下

### 3.6 服务模块

- pinyougou-sellergoods-service(WAR)
	- 使用代码生成器HeimaCodeUtil_V2.4生成service代码
	
		- 拷贝服务接口到com.pinyougou.sellergoods.service.impl包下

	- pom.xml依赖于spring, dubbo, dao, interface, common
	
		```xml
		<dependencies>
			<!-- Spring -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-beans</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-webmvc</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-jdbc</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-aspects</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-jms</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context-support</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-test</artifactId>		
			</dependency>
			<!-- dubbo相关 -->
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>dubbo</artifactId>	
			</dependency>
			<dependency>
				<groupId>org.apache.zookeeper</groupId>
				<artifactId>zookeeper</artifactId>	
			</dependency>
			<dependency>
				<groupId>com.github.sgroschupf</groupId>
				<artifactId>zkclient</artifactId>		
			</dependency>
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
			</dependency>
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>fastjson</artifactId>
			</dependency>
			<dependency>
				<groupId>javassist</groupId>
				<artifactId>javassist</artifactId>		
			</dependency>
			<dependency>
				<groupId>commons-codec</groupId>
				<artifactId>commons-codec</artifactId>	  
			</dependency>
			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>servlet-api</artifactId>	
				<scope>provided</scope>
			</dependency>  
		</dependencies>  
		<build>
			<plugins>
				<!-- 配置Tomcat插件 -->
				<plugin>
					<groupId>org.apache.tomcat.maven</groupId>
					<artifactId>tomcat7-maven-plugin</artifactId>
					<configuration>
						<path>/</path>
						<port>9001</port>
					</configuration>
				</plugin>
			</plugins>
		</build>
		```

	- /src/main/webapp/WEB-INF/web.xml 加载spring容器
	
		```xml
		<!-- 加载spring容器 -->
		<context-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath*:spring/applicationContext*.xml</param-value>
		</context-param>
		<listener>
			<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
		</listener>
		```

	- /src/main/resources/spring/applicationContext-service.xml 配置spring核心配置文件
	
		```xml
		<?xml version="1.0" encoding="UTF-8"?>
		<beans xmlns="http://www.springframework.org/schema/beans"
			xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
			xmlns:context="http://www.springframework.org/schema/context"
			xmlns:dubbo="http://code.alibabatech.com/schema/dubbo" xmlns:mvc="http://www.springframework.org/schema/mvc"
			xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
				http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
				http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd
				http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

			<dubbo:protocol name="dubbo" port="20881"></dubbo:protocol>
			<dubbo:application name="pinyougou-sellergoods-service"/>
			<dubbo:registry address="zookeeper://192.168.25.129:2181"/>
			<dubbo:annotation package="com.pinyougou.sellergoods.service.impl" />
		</beans>
		```

### 3.7 应用模块

- pinyougou-manager-web(WAR)
	
	- 使用代码生成器HeimaCodeUtil_V2.4生成controller代码
	
		- 拷贝服务接口到com.pinyougou.manager.controller包下

	- pom.xml 依赖spring, dubbo, common, interface
	
		```xml
		<dependencies>
			<!-- Spring -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-beans</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-webmvc</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-jdbc</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-aspects</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-jms</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context-support</artifactId>		
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-test</artifactId>		
			</dependency>
			<!-- dubbo相关 -->
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>dubbo</artifactId>	
			</dependency>
			<dependency>
				<groupId>org.apache.zookeeper</groupId>
				<artifactId>zookeeper</artifactId>	
			</dependency>
			<dependency>
				<groupId>com.github.sgroschupf</groupId>
				<artifactId>zkclient</artifactId>		
			</dependency>
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
			</dependency>
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>fastjson</artifactId>
			</dependency>
			<dependency>
				<groupId>javassist</groupId>
				<artifactId>javassist</artifactId>		
			</dependency>
			<dependency>
				<groupId>commons-codec</groupId>
				<artifactId>commons-codec</artifactId>	  
			</dependency>
			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>servlet-api</artifactId>	
				<scope>provided</scope>
			</dependency>  
		</dependencies>
		<build>
			 <plugins>
				<!-- 配置Tomcat插件 -->
				<plugin>
					<groupId>org.apache.tomcat.maven</groupId>
					<artifactId>tomcat7-maven-plugin</artifactId>
					<configuration>
						<path>/</path>
						<port>9101</port>
					</configuration>
				</plugin>
			 </plugins>
		</build>
		```

	- /src/main/webapp/WEB-INF/web.xml 加载spring容器,添加post乱码filter
	
		```xml
	   <!-- 解决post乱码 -->
		<filter>
			<filter-name>CharacterEncodingFilter</filter-name>
			<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
			<init-param>
				<param-name>encoding</param-name>
				<param-value>utf-8</param-value>
			</init-param>
			<init-param>  
				<param-name>forceEncoding</param-name>  
				<param-value>true</param-value>  
			</init-param>  
		</filter>
		<filter-mapping>
			<filter-name>CharacterEncodingFilter</filter-name>
			<url-pattern>/*</url-pattern>
		</filter-mapping>	
		
		<servlet>
			<servlet-name>springmvc</servlet-name>
			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
			<!-- 指定加载的配置文件 ，通过参数contextConfigLocation加载-->
			<init-param>
				<param-name>contextConfigLocation</param-name>
				<param-value>classpath:spring/springmvc.xml</param-value>
			</init-param>
		</servlet>
		<servlet-mapping>
			<servlet-name>springmvc</servlet-name>
			<url-pattern>*.do</url-pattern>
		</servlet-mapping>
		```

	- /src/main/resources/spring/springmvc.xml
	
		```xml
		<?xml version="1.0" encoding="UTF-8"?>
		<beans xmlns="http://www.springframework.org/schema/beans"
			xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
			xmlns:context="http://www.springframework.org/schema/context"
			xmlns:dubbo="http://code.alibabatech.com/schema/dubbo" xmlns:mvc="http://www.springframework.org/schema/mvc"
			xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
				http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
				http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd
				http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
			<mvc:annotation-driven>
				<mvc:message-converters register-defaults="true">
					<bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter">  
						<property name="supportedMediaTypes" value="application/json"/>
						<property name="features">
						<array>
							<value>WriteMapNullValue</value>
							<value>WriteDateUseDateFormat</value>
						</array>
					  </property>
					</bean>
				</mvc:message-converters>  
			</mvc:annotation-driven>
			<!-- 引用dubbo 服务 -->
			<dubbo:application name="pinyougou-manager-web" />
			<dubbo:registry address="zookeeper://192.168.25.132:2181"/>
			<dubbo:annotation package="com.pinyougou.manager.controller" />  	
		</beans>
		```

## 4. 后端SSM+Dubbo测试

- 测试

	- 启动pinyougou-sellergoods-service  
	- 启动pinyougou-manager-web 
	- 地址栏输入http://localhost:9101/brand/findAll.do

	- 成功: 可以看到浏览器输出了json数据