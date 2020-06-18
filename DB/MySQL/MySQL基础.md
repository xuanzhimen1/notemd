## 1. 安装配置卸载

- linux安装配置

  ```bash
  # 安装服务器
  sudo apt install mysql-server
  sudo service mysql start
  sudo service mysql stop
  sudo service mysql restart
  ps ajx|grep mysql
   
  # 安装客户端
  sudo apt install mysql-client
  
  # 配置MySQL
  sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
  # 数据库授权
  grant all privileges on *.* to 'root'@'192.168.199.133' identified by 'root' with grant option;
  flush privileges;
  # 修改数据库密码
  sudo cat /etc/mysql/debian.cnf
  # 这条指令的密码输入是输入第一条指令获得的信息中的 password = ZCt7QB7d8O3rFKQZ 得来。
  mysql -u debian-sys-maint -p
  # 修改密码，本篇文章将密码修改成 root , 用户可自行定义。
  use mysql;
  update mysql.user set authentication_string=password('root') where user='root' and Host ='localhost';
  update user set plugin="mysql_native_password"; 
  flush privileges;
  quit;
  # 重新启动mysql:
  sudo service mysql restart
  
  # 卸载
  # 首先用dpkg --list|grep mysql查看自己的mysql有哪些依赖
  sudo apt-get remove mysql-common
  sudo apt-get autoremove --purge mysql-server-5.0 
  # 再用dpkg --list|grep mysql查看，还剩什么就卸载什么
  # 最后清楚残留数据：
  dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
  ```

- win安装
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_安装1.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_安装2.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_安装3.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_安装4.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_安装5.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_安装6.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_安装7.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_安装8.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_安装9.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_安装10.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_安装11.png)


- 卸载
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_卸载1.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_卸载2.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_卸载3.png)
	![](http://119.3.209.59/blog_image/jackiezz/MySQL_卸载4.png)

  1. 去mysql的安装目录找到my.ini文件

     复制 datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"

  2. 卸载MySQL
  3. 删除C:/ProgramData目录下的MySQL文件夹。
- 配置
  - MySQL服务启动
    - cmd--> services.msc 打开服务的窗口
    - 使用管理员打开cmd
      - net start mysql : 启动mysql的服务
      - net stop mysql:关闭mysql服务
  - MySQL登录
    - `mysql -uroot -p密码`
    - `mysql -hip -uroot -p连接目标的密码`
    - `mysql --host=ip --user=root --password=连接目标的密码`
  - MySQL退出
    - exit
    - quit
  - MySQL目录结构
    - MySQL安装目录：basedir="D:/develop/MySQL/"
      * 配置文件 my.ini
    - MySQL数据目录：datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"

## 2. SQL语法

- 概念

  > Structured Query Language 结构化查询语言
  >
  > 定义操作所有关系型数据库的规则
  >
  > 每种数据库操作不一样的方法称为"方言"

- 通用语法规则

  ```sql
  -- 1. 一条语句可以单行或多行书写, 以分号结尾
  -- 2. 语句不区分大小写, 关键字建议大写
  -- 3. 注释: "-- 注释"--后有一个空格, 或MySQL中可以用"#注释"
  SELECT * FROM student;
  ```

- 操作分类

  - DDL(Data Definition Language) 操作数据库,表  关键字:create, drop, alter
  - DML(Date Manipulation Language) 增删改表中的数据  关键字:insert, delete, update
  - DQL(Data Query Language) 查询表中的数据  关键字: select, where
  - DCL(Data Control Language) 授权  关键字: grant, revoke

## 3. DDL操作数据库/表

- CURD(Create, Retrieve, Update, Delete)

  ```sql
  use testdb;  -- 使用数据库
  select database();  -- 查询当前使用的数据库
  ```

  

- Create

  > 数据类型
  >
  > - int: 整数类型
  > - double: 小数类型 如: score double(5, 2)  整数+小数共5位, 小数2位
  > - date: 日期类型 如: date 格式为(yyyy-MM-dd)
  > - datetime: 时间类型 如: datetime  格式为(yyyy-MM-dd HH:mm:ss)
  > - timestamp: 时间戳 如: 默认使用当前时间(赋值为无或null)
  > - varchar: 字符串 如:varchar(20)  最大20个字符

  ```sql
  -- 创建数据库
  create database if not exists testdb charset=utf8;  -- charset和判断可以省略
  -- 创建表
  create table 表名(列名1 数据类型1, 列名2 数据类型2, 列名n 数据类型n);  -- 最后一个不加,
  create table student(
      id int, 
      name varchar(32), 
      age int, 
      score double(5,2), 
      birthday date, 
      insert_time timestamp
  );
  -- 复制表
  create table stu like student;
  ```

- Retrieve

  ```sql
  -- 查看所有数据库
  show databases;
  -- 查看某个数据库创建语法  :  可以查看编码
  show create database testdb;
  -- 查看数据库中所有表
  show tables;
  -- 查看表结构
  desc 表名;
  ```

- Update

  ```sql
  -- 修改数据库字符集
  alter database testdb charset=gbk;
  -- 修改表名
  alter table student rename to newstudent;
  -- 修改表字符集
  alter table student charset=gbk;
  -- 修改列
  alter table student change 列名 新列名 新类型;
  alter table student modify 列名 新类型;
  -- 添加列
  alter table student add 列名 数据类型;
  -- 删除列
  alter table student drop 列名;
  ```

  

- Delete

  ```sql
  -- 删除数据库
  drop database if exists testdb;  -- 判断 if exists没有且testdb不存在时会报错
  -- 删除表
  drop talbe if exists 表名;
  drop table 表名;
  ```



## 4. DML操作数据

- DML

- Create

  > 时间和字符串类型用""引起来, 日期默认格式: yyyy-MM-dd

  ```sql
  insert into 表名(列名1, 列名2,...列名n) values (值1, 值2, ...值n);
  insert into student(id, name, age, score, birthday) values (1, "zhangsan", 18, 20.22, "2019-01-01");
  insert into student values(2, "lisi", 20, 100.0, null, null);  -- 简化写法,必须输入全部列
  ```

- Delete

  > delete  条件: 如id=3, 没有条件时,删除全部记录(不推荐使用, 效率低 原因: 一条记录删除一次)
  >
  > truncate 删除表并创建一个相同的新表, 删除全部记录时用此方式(推荐使用)

  ```sql
  delete from 表名 [where 条件]
  truncate table 表名
  ```

- Update

  > 如果条件为空, 表中的所有记录全部修改

  ```sql
  update 表名 set 列名1=值1, ... 列名n=值n [where 条件]
  ```



## 5. DQL查询数据

- 语法

  > 列表中的值用","分隔  如 Name, age

  ```sql
  select 
  	字段列表 
  from
  	表名列表
  where
  	条件列表
  group by
  	分组字段
  having
  	分组后的条件
  order by
  	排序
  limit
  	分页限定
  ```

- 基础查询

  ```sql
  -- 查询特定字段数据
  select 字段列表 from 表名;  -- *表示全部字段
  -- 去重
  select distinct 字段列表 from 表名;  -- 两条记录字段列表值完全相同才算distinct
  -- 字段运算
  select 字段列表 from 表名;  -- 字段列表中的字段可以进行运算 
  					-- 如: 字段:num1+num2, null参与的运算都为null
  					--又如字段: num1+ifnull(num2, 0) 表示如果num2为null时,运算取默认值0
  -- 别名
  字段列表: name as 名字, age 年龄  -- as可以省略
  ```

- 条件查询

  > 条件
  >
  > - `=, >, >=, <, <=, != ` 如: age>30
  > - `and or` 如: age>20 and age<30
  > - `between` 如: age between 20 and 30
  > - `in` 如: age in (22, 18, 30)
  > - `is null,  is not null` 如: age is null,   age=null(错误写法)
  > - `like` 如: 示例

  ```sql
  select ... where(条件列表);
  where name like _zhang%;   -- "_"表示单个占位, "%"表示多个字符占位
  ```

- 排序查询

  > 默认排序为升序 asc
  >
  > 降序: desc
  >
  > 字段列表:  如: name [asc], age desc  先按name升序排, 相同时再按age降序排

  ```sql
  select ... order by 字段列表 asc;  -- asc可省略
  select ... order by 字段列表 desc;
  ```

- 聚合函数

  > 计算结果排除null字段: 解决方法:  ifnull(字段, 0)  用默认值0替换null
  >
  > count: 获取个数  一般个数用id字段
  >
  > max: 最大值
  >
  > min: 最小值
  >
  > sum: 求和
  >
  > avg: 平均值

  ```sql
  select count(id) ....
  ```

- 分组

  > 分组后查询字段列表为: 分组字段或聚合函数, 其它字段没有意义
  >
  > where 在分组前限定, having在分组之后进行限定, having用聚合函数,where不可以

  ```sql
  select gender count(id) avg(score) where ... group by gender having count(id) > 2;
  ```

- 分页

  > 开始索引 = 每页条数*(当前页码-1)   索引从0开始

  ```sql
  select ... limit 3, 2;  -- 3开始索引, 2为每页显示几条
  ```



## 6. 约束

- 字段的约束

  - 主键约束 primary key
  - 非空约束 not null
  - 唯一约束 unique
  - 外键约束 foreign key

  

- 向字段添加或删除约束

  - 创建表时添加约束

    ```sql
    create table student(
    	id int primary key auto_increment,  -- 主键约束+自动增长  (非空且唯一 <==> 主键, 一个表中只能有一个)
        name varchar(20) not null,  -- 非空约束
        age varchar(20) unique  -- 唯一约束
    );
    ```

  - 向表中添加或删除约束

    ```sql
    -- 删除约束
    alter table student  modify name varchar(20)  -- 删除非空约束
    alter table student drop index name;  -- 删除唯一约束
    alter table student drop primary key;  -- 删除主键约束
    alter table student modify id int;  -- 删除自动增长
    -- 添加约束
    alter table student modify name varchar(20) not null;  -- 添加非空约束
    alter table student modify name varchar(20) unique;  -- 添加唯一约束
    alter table student modify id int primary key;  -- 添加主键约束
    alter table student modify id int auto_increment;  -- 添加主键约束
    ```

- 外键约束

  - 创建表时定义外键

    ```sql
    constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)
    ```

  - 添加外键和删除外键

    ```sql
    alter table 表名 drop foreign key 外键名称;  -- 删除外键
    alter table 表名 add constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)  -- 添加外键约束
    ```

  - 级联操作

    > 级联更新: 当一的一方主键变化时, 多的一方关联外键同时改变
    >
    > 级联删除: 当一的一方删除一条记录时, 多的一方关联外键的所有记录全部删除

    ```sql
    alter table 表名 add constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称) on update cascade on update cascade;  -- 添加级联外键约束
    ```



## 7. 数据库设计

- 数据库设计范式

  > 设计数据库时，需要遵循的一些规范。要遵循后边的范式要求，必须先遵循前边的所有范式要求
  >
  > 设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。
  >
  > 目前关系数据库有六种范式：第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）。

- 三大范式

  > ```
  > 几个概念：
  > 	1. 函数依赖：A-->B,如果通过A属性(属性组)的值，可以确定唯一B属性的值。则称B依赖于A
  > 		例如：学号-->姓名。  （学号，课程名称） --> 分数
  > 	2. 完全函数依赖：A-->B， 如果A是一个属性组，则B属性值得确定需要依赖于A属性组中所有属性值。
  > 		例如：（学号，课程名称） --> 分数
  > 	3. 部分函数依赖：A-->B， 如果A是一个属性组，则B属性值得确定只需要依赖于A属性组中某一些值
  > 		例如：（学号，课程名称） -- > 姓名
  > 	4. 传递函数依赖：A-->B, B -- >C . 如果通过A属性(属性组)的值，可以确定唯一B属性的值，
  > 	   在通过B属性（属性组）的值可以确定唯一C属性的值，则称 C 传递函数依赖于A
  > 		例如：学号-->系名，系名-->系主任
  > 	5. 码：一个属性或属性组，被其他所有属性所完全依赖，则称这个属性(属性组)为该表的码
  > 		例如：该表中码为：（学号，课程名称）
  > 		主属性：码属性组中的所有属性
  > 		非主属性：除过码属性组的属性
  > ```

  1. 第一范式（1NF）：每一列都是不可分割的原子数据项
  2. 第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于码（在1NF基础上消除非主属性对主码的部分函数依赖）
  3. 第三范式（3NF）：在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）



## 8. 备份和还原

- 备份

  ```sql
  mysqldump -u用户名 -p密码 数据库名称 > 保存的路径
  ```

- 还原

  ```sql
  1. 登录数据库
  2. 创建数据库
  3. 使用数据库
  4. 执行文件。source 文件路径
  ```

  

## 9. 多表操作

- 多表查询

  - 内连接查询

    ```sql
    select * from emp t1, dept t2 where t1.'dept_id' = t2.'id';  -- 隐式内连接
    select * from emp [inner] join dept on emp.'dept_id' = temp.'id';  -- 显式内连接
    ```

  - 外连接查询

    > 一般只用左外, 右外是相对于左外的

    ```sql
    select * from emp left [outer] join dept on emp.'dept_id' = temp.'id';  -- 左外连接
    select * from emp right [outer] join dept on emp.'dept_id' = temp.'id';  -- 右外
    ```

- 子查询

  > 查询中嵌套查询，称嵌套查询为子查询。

  - 子查询的结果是单行单列的

    > 子查询可以作为条件，使用运算符去判断。 运算符： > >= < <= =

    ```sql
    SELECT * FROM emp WHERE emp.salary < (SELECT AVG(salary) FROM emp);
    ```

  - 子查询的结果是多行单列的：

    > 子查询可以作为条件，使用运算符in来判断

    ```sql
    SELECT * FROM emp WHERE dept_id IN (SELECT id FROM dept WHERE NAME = '财务部' OR NAME = '市场部');
    ```

  - 子查询的结果是多行多列的

    > 子查询可以作为一张虚拟表参与查询

    ```sql
    SELECT * FROM dept t1 ,(SELECT * FROM emp WHERE emp.`join_date` > '2011-11-11') t2 WHERE t1.id = t2.dept_id;
    ```

    

- 练习

  ``` sql
  -- 部门表
  CREATE TABLE dept (
      id INT PRIMARY KEY PRIMARY KEY, -- 部门id
      dname VARCHAR(50), -- 部门名称
      loc VARCHAR(50) -- 部门所在地
  );
  
  -- 添加4个部门
  INSERT INTO dept(id,dname,loc) VALUES 
  (10,'教研部','北京'),
  (20,'学工部','上海'),
  (30,'销售部','广州'),
  (40,'财务部','深圳');
  
  -- 职务表，职务名称，职务描述
  CREATE TABLE job (
      id INT PRIMARY KEY,
      jname VARCHAR(20),
      description VARCHAR(50)
  );
  
  -- 添加4个职务
  INSERT INTO job (id, jname, description) VALUES
  (1, '董事长', '管理整个公司，接单'),
  (2, '经理', '管理部门员工'),
  (3, '销售员', '向客人推销产品'),
  (4, '文员', '使用办公软件');
  
  -- 员工表
  CREATE TABLE emp (
      id INT PRIMARY KEY, -- 员工id
      ename VARCHAR(50), -- 员工姓名
      job_id INT, -- 职务id
      mgr INT , -- 上级领导
      joindate DATE, -- 入职日期
      salary DECIMAL(7,2), -- 工资
      bonus DECIMAL(7,2), -- 奖金
      dept_id INT, -- 所在部门编号
      CONSTRAINT emp_jobid_ref_job_id_fk FOREIGN KEY (job_id) REFERENCES job (id),
      CONSTRAINT emp_deptid_ref_dept_id_fk FOREIGN KEY (dept_id) REFERENCES dept (id)
  );
  
  -- 添加员工
  INSERT INTO emp(id,ename,job_id,mgr,joindate,salary,bonus,dept_id) VALUES 
  (1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
  (1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
  (1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
  (1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
  (1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
  (1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
  (1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
  (1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
  (1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
  (1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
  (1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
  (1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
  (1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
  (1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);
  
  -- 工资等级表
  CREATE TABLE salarygrade (
      grade INT PRIMARY KEY,   -- 级别
      losalary INT,  -- 最低工资
      hisalary INT -- 最高工资
  );
  
  -- 添加5个工资等级
  INSERT INTO salarygrade(grade,losalary,hisalary) VALUES 
  (1,7000,12000),
  (2,12010,14000),
  (3,14010,20000),
  (4,20010,30000),
  (5,30010,99990);
  
  -- 需求：
  -- 1.查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述
  /*
  分析：
  1.员工编号，员工姓名，工资，需要查询emp表  职务名称，职务描述 需要查询job表
  2.查询条件 emp.job_id = job.id
  */
  SELECT 
  t1.`id`, -- 员工编号
  t1.`ename`, -- 员工姓名
  t1.`salary`,-- 工资
  t2.`jname`, -- 职务名称
  t2.`description` -- 职务描述
  FROM 
  emp t1, job t2
  WHERE 
  t1.`job_id` = t2.`id`;
  
  -- 2.查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置
  /*
  分析：
  1. 员工编号，员工姓名，工资 emp  职务名称，职务描述 job  部门名称，部门位置 dept
  2. 条件： emp.job_id = job.id and emp.dept_id = dept.id
  */
  SELECT 
  t1.`id`, -- 员工编号
  t1.`ename`, -- 员工姓名
  t1.`salary`,-- 工资
  t2.`jname`, -- 职务名称
  t2.`description`, -- 职务描述
  t3.`dname`, -- 部门名称
  t3.`loc` -- 部门位置
  FROM 
  emp t1, job t2,dept t3
  WHERE 
  t1.`job_id` = t2.`id` AND t1.`dept_id` = t3.`id`;
  
  -- 3.查询员工姓名，工资，工资等级
  /*
  分析：
  1.员工姓名，工资 emp  工资等级 salarygrade
  2.条件 emp.salary >= salarygrade.losalary and emp.salary <= salarygrade.hisalary
  emp.salary BETWEEN salarygrade.losalary and salarygrade.hisalary
  */
  SELECT 
  t1.ename ,
  t1.`salary`,
  t2.*
  FROM emp t1, salarygrade t2
  WHERE t1.`salary` BETWEEN t2.`losalary` AND t2.`hisalary`;
  
  -- 4.查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级
  /*
  分析：
  1. 员工姓名，工资 emp ， 职务名称，职务描述 job 部门名称，部门位置，dept  工资等级 salarygrade
  2. 条件： emp.job_id = job.id and emp.dept_id = dept.id and emp.salary BETWEEN salarygrade.losalary and salarygrade.hisalary
  */
  SELECT 
  t1.`ename`,
  t1.`salary`,
  t2.`jname`,
  t2.`description`,
  t3.`dname`,
  t3.`loc`,
  t4.`grade`
  FROM 
  emp t1,job t2,dept t3,salarygrade t4
  WHERE 
  t1.`job_id` = t2.`id` 
  AND t1.`dept_id` = t3.`id`
  AND t1.`salary` BETWEEN t4.`losalary` AND t4.`hisalary`;
  
  -- 5.查询出部门编号、部门名称、部门位置、部门人数
  /*
  分析：
  1.部门编号、部门名称、部门位置 dept 表。 部门人数 emp表
  2.使用分组查询。按照emp.dept_id完成分组，查询count(id)
  3.使用子查询将第2步的查询结果和dept表进行关联查询
  */
  SELECT 
  t1.`id`,t1.`dname`,t1.`loc` , t2.total
  FROM 
  dept t1,
  (SELECT
  dept_id,COUNT(id) total
  FROM 
  emp
  GROUP BY dept_id) t2
  WHERE t1.`id` = t2.dept_id;
  
  -- 6.查询所有员工的姓名及其直接上级的姓名,没有领导的员工也需要查询
  /*
  分析：
  1.姓名 emp， 直接上级的姓名 emp
  * emp表的id 和 mgr 是自关联
  2.条件 emp.id = emp.mgr
  3.查询左表的所有数据，和 交集数据
  * 使用左外连接查询
  */
  /*
  select
  t1.ename,
  t1.mgr,
  t2.`id`,
  t2.ename
  from emp t1, emp t2
  where t1.mgr = t2.`id`;
  */
  SELECT 
  t1.ename,
  t1.mgr,
  t2.`id`,
  t2.`ename`
  FROM emp t1
  LEFT JOIN emp t2
  ON t1.`mgr` = t2.`id`;
  ```



## 10. 事务

- 概述

  > 如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败
  >
  > 事务的四大特征:
  >
  > 1. 原子性：是不可分割的最小操作单位，要么同时成功，要么同时失败。
  >  2. 持久性：当事务提交或回滚后，数据库会持久化的保存数据。
  > 3. 隔离性：多个事务之间。相互独立。
  > 4. 一致性：事务操作前后，数据总量不变

- 使用事务

  > MySQL数据库中事务默认自动提交
  >
  > 一条DML(增删改)语句会自动提交一次事务。
  >
  > Oracle 数据库默认是手动提交事务

  ```sql
  开启事务： start transaction;
  回滚：rollback;
  提交：commit;
  ```

- 修改事务的默认提交方式

  ```sql
  SELECT @@autocommit; -- 查看事务的默认提交方式 1 代表自动提交  0 代表手动提交
  set @@autocommit = 0;  -- 修改默认提交方式： 
  ```

- 事务的隔离级别

  > 多个事务之间隔离的，相互独立的。但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。
  >
  > 存在问题:
  >
  > - 脏读：一个事务，读取到另一个事务中没有提交的数据
  > - 不可重复读(虚读)：在同一个事务中，两次读取到的数据不一样。
  > - 幻读：一个事务操作(DML)数据表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改。

  ```sql
  -- 隔离级别：
  read uncommitted：读未提交
  	-- 产生的问题：脏读、不可重复读、幻读
  read committed：读已提交 （Oracle）
  	-- 产生的问题：不可重复读、幻读
  repeatable read：可重复读 （MySQL默认）
  	-- 产生的问题：幻读
  serializable：串行化
  	-- 可以解决所有的问题
  ```



## 11. DCL

- 用户管理

  ```sql
  -- 添加用户：
  CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
  -- 删除用户：
  DROP USER '用户名'@'主机名';
  -- 修改用户密码：
  UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';
  SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');
  SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123');
  -- win中mysql中忘记了root用户的密码？
  	cmd -- > net stop mysql 停止mysql服务  需要管理员运行该cmd
  	使用无验证方式启动mysql服务： mysqld --skip-grant-tables
  	打开新的cmd窗口,直接输入mysql命令，敲回车。就可以登录成功
  	use mysql;
  	update user set password = password('你的新密码') where user = 'root';
  	关闭两个窗口
  	打开任务管理器，手动结束mysqld.exe 的进程
  	启动mysql服务
  	使用新密码登录。
  -- linux忘记密码参照MySQL安装配置linux部分
  -- 查询用户：
  USE myql;
  SELECT * FROM USER;  -- * 通配符： % 表示可以在任意主机使用用户登录数据库
  ```

- 权限管理

  ```sql
  -- 查询权限
  SHOW GRANTS FOR '用户名'@'主机名';
  SHOW GRANTS FOR 'lisi'@'%';
  
  -- 授予权限
  grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
  GRANT ALL ON *.* TO 'zhangsan'@'localhost';  -- 给张三用户授予所有权限，在任意数据库任意表上
  -- 撤销权限：
  revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
  REVOKE UPDATE ON db3.`account` FROM 'lisi'@'%';
  ```

  

## 12. Linux操作Mysql



- 数据库授权

  - 创建用户

    ```mysql
    CREATE USER 'username'@'host' IDENTIFIED BY 'password';
    ```

    说明：

    - username：你将创建的用户名
    - host：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以**从任意远程主机登陆**，可以使用通配符`%`
    - password：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器

    例如：

    ```mysql
    CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';
    CREATE USER 'pig'@'192.168.1.101_' IDENDIFIED BY '123456';
    CREATE USER 'pig'@'%' IDENTIFIED BY '123456';
    CREATE USER 'pig'@'%' IDENTIFIED BY '';
    CREATE USER 'pig'@'%';
    ```

  - 授权

    ```mysql
    GRANT privileges ON databasename.tablename TO 'username'@'host'
    ```

    说明：

    - privileges：用户的操作权限，如`SELECT`，`INSERT`，`UPDATE`等，如果要授予所的权限则使用`ALL`
    - databasename：数据库名
    - tablename：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用`*`表示，如`*.*`

    例如：

    ```mysql
    GRANT SELECT, INSERT ON test.user TO 'pig'@'%';
    GRANT ALL ON *.* TO 'pig'@'%';
    GRANT ALL ON maindataplus.* TO 'pig'@'%';
    ```

    注意:

    > 用以上命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令:

    ```
    GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;
    ```

  - 设置与更改用户密码

    - 命令:

    ```mysql
    SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
    
    
    ```

    - 如果是当前登陆用户用:

    ```mysql
    SET PASSWORD = PASSWORD("newpassword");
    ```

  - 撤销用户权限

    - 命令

    ```mysql
    REVOKE privilege ON databasename.tablename FROM 'username'@'host';
    ```

  - 删除用户

    - 命令

    ```mysql
    DROP USER 'username'@'host';
    ```

- 远程访问数据库

  ```bash
  1. 控制台 开放端口3306
  2. 修改配置
  	vim /etc/mysql/mysql.conf.d/mysqld.cnf
  	注释#bind-address = 127.0.0.1
  3. 给用户授权允许远程访问:
  	grant all privileges on *.* to root@"%" identified by "pwd" with grant option;
  	flush privileges;
  4. 重启mysql
  	当mysql重启不了的时候,看看日志目录比如/var/log/mysql 是否存在,属组和属主是否是mysql
  	如果没有,创建目录,并更改目录的所有者 chown mysql:mysql 
  ```

### 1. 快速开始

- 初始化数据库操作

  ```bash
  create database meiduo_mall default charset=utf8;
  
  # 创建用户
  create user meiduo identified by 'meiduo';  
  # 授权meiduo_mall数据库下的所有表（meiduo_mall.*）的所有权限（all）给用户meiduo在以任何ip访问数据库的时候（'meiduo'@'%'）
  grant all on meiduo_mall.* to 'meiduo'@'%';  
  # 刷新生效用户权限
  flush privileges;
  ```

  

### 2. MySQL数据类型和约束

- **常用数据类型**

  - 整数：int，bit
  - 小数：decimal
  - 字符串：varchar,char
  - 日期时间: date, time, datetime
  - 枚举类型(enum)

  > decimal表示浮点数，如decimal(5,2)表示共存5位数，小数占2位
  > char表示固定长度的字符串，如char(3)，如果填充'ab'时会补一个空格为'ab '
  > varchar表示可变长度的字符串，如varchar(3)，填充'ab'时就会存储'ab'
  > text字符串表示存储大文本，当字符大于4000时推荐使用
  > 对于图片、音频、视频等文件，不存储在数据库中，而是上传到某个服务器上，然后在表中存储这个文件的保存路径



- **约束**

  - 主键primary key：物理上存储的顺序
  - 非空not null：此字段不允许填写空值
  - 惟一unique：此字段的值不允许重复
  - 默认default：当不填写此值时会使用默认值，如果填写时以填写为准
  - 外键foreign key：对关系字段进行约束，当为关系字段填写值时，会到关联的表中查询此值是否存在，如果存在则填写成功，如果不存在则填写失败并抛出异常

  > 说明:
  >
  > 虽然外键约束可以保证数据的有效性，但是在进行数据的crud（增加、修改、删除、查询）时，都会降低数据库的性能，所以不推荐使用，那么数据的有效性怎么保证呢？答：可以在逻辑层进行控制



### 3.对数据库及数据的操作

- **登录及版本**

  ```bash
  mysql -uroot -p  # 登录数据库
  quit exit ctrl+d  # 退出数据库
  select version();  # 查看当前版本
  select now();  # 查看当前时间
  ```

- 操作数据库

  ```bash
  show databases;  # 查看所有数据库
  use test1;  # 使用数据库
  create database test1 charset=utf8;  # 创建数据库
  select database();  # 查看当前使用的数据库
  drop database test1;  # 删除数据库
  ```

- 操作数据表

  ```sql
  show tables;  # 查看数据库中所有表
  desc classes;  # 查看表结构
  show create table classes;  # 查看表创建语句
  # 创建表
  create table classes(
      id int unsigned auto_increment primary key not null,
      name varchar(10)
  );
  create table students(
      id int unsigned auto_increment primary key not null,
      name varchar(20) default '',
      age tinyint unsigned default 0,
      gender enum('男','女','人妖','保密'),
      cls_id int unsigned default 0
  );
   
  # 修改表
  alter table students add birthday datetime;  # 添加字段
  alter table students modify birthday varchar(200);  # 修改字段属性,原属性丢失,modify修改时字段必须带有数据类型可以带有属性,也可以没有
  alter table students modify birthday date after name;  # 将birthday移到name后面
  alter table students modify birthday date first;  # 将birthday移到第一个
  alter table students change birthday birth datetime not null;  # 重命名修改表字段
  alter table students alter age set default 18;  # 给字段添加默认值
  alter table students alter age drop default;  # 删除默认值
  alter table students add primary key(id);  # 添加主键
  alter table students drop primary key;  # 删除主键
  alter table students drop birthday;   # 删除字段
   
  # 删除表
  drop table students;
  ```

- **数据备份和恢复**

  ```sql
  mysqldump -uroot -p test1 > python.sql  # 备份
  mysql -uroot -p test2 < python.sql  # 恢复
  ```

- **操作数据**

  - 基本操作CRUD

  ```sql
  -- 基本查询
  select * from classes;
  select id,name from classes;
  -- 增加
  insert into students values(0,'张三',1,'甘肃','2018-1-1');  -- 全列插入  主键用0或者 default 或者 null 来占位
  insert into students(name,hometown,birthday) values('黄蓉','桃花岛','2018-1-1');  -- 部分插入
  insert into classes values(0,'python1'),(0,'python2');
  insert into classes(name) values('java'),('php'),('c')
  -- 修改
  update students set gender=0,hometown='北京' where id=5;
  -- 删除
  delete from student where id=5;
  update students set isdelete=1 where id=1;  -- 逻辑删除
  ```

  - 查询操作

    - 基础查询

    ```sql
    select * from 表名   -- 查询所有字段
    select 列1, 列2, ... from 表名;  -- 查询指定字段
    select s.id as 序号, s.name as 名字 from students as s  -- as起别名
    select distinct 列1, ... from 表名;  -- 消除重复行
    ```

    - 条件查询

    ```sql
    -- 比较查询    = > >= < <= !=
    select * from students where id > 3;
    -- 逻辑运算符 and or not
    select * from students where id > 3 and gender=0;
    -- 模糊查询  like (%任意多个任意字符, _任意一个字符)
    select * from students where name like '黄%';
    -- 范围查询   in between...and...
    select * from students where id in(1,3,8);
    select * from students where id between 3 and 8;
    -- 空判断	null    is not null
    select * from students where height is null;
    ```

    - 排序

    ```sql
    select * from 表名 order by 列1 asc|desc [,列2 asc|desc,...]
    ```

    - 聚合函数

    ```sql
    count(*)  -- 计算总行数
    max(列)  -- 求此列最大值
    min(列)  -- 求此列最小值
    sum(列)  -- 求此列的和
    avg(列)  -- 求此列的平均值
    eg:  select avg(id) from students where is_delete=0 and gender=2;
    ```

    - 分组

    ```sql
    -- 1.group by + group_concat(name)
    eg: select gender,group_concat(name) from students group by gender;
	+--------+-----------------------------------------------------------+
    | gender | group_concat(name)                                        |
    +--------+-----------------------------------------------------------+
    | 男     | 彭于晏,刘德华,周杰伦,程坤,郭靖                                 |
    | 女     | 小明,小月月,黄蓉,王祖贤,刘亦菲,静香,周杰                        |
    | 中性   | 金星                                                       |
    | 保密   | 凤姐                                                       |
    +--------+-----------------------------------------------------------+

    -- 2.group by +  聚合函数
    select gender,avg(age) from students group by gender;  -- 分别统计性别为男女的平均年龄
    -- 3.group by + having
    select gender,count(*) from students group by gender having count(*)>2;
    -- 4.group by + with rollup
    	-- with rollup的作用是：在最后新增一行，来记录当前列里所有记录的总和
    select gender,count(*) from students group by gender with rollup;
    +--------+----------+
    | gender | count(*) |
    +--------+----------+
    | 男     |        5 |
    | 女     |        7 |
    | 中性   |        1 |
    | 保密   |        1 |
    | NULL   |       14 |
    +--------+----------+

    ```

    - 分页

    ```sql
    select * from 表名 limit start,count  # 从start开始，获取count条数据
    -- 求第n页的数据
    select * from students where is_delete=0 limit (n-1)*m,m
    ```

    - 连接查询

    ```sql
    -- 内连接查询
    select * from students inner join classes on students.cls_id = classes.id;  -- 公共部分
    -- 右连接查询
    select * from students as s right join classes as c on s.cls_id = c.id;  --公共+B  字段变多了
    -- 左连接查询
    select * from students as s right join classes as c on s.cls_id = c.id;  -- 公共+A
    -- 自关联
    	-- 创建areas表
        create table areas(
            aid int primary key,
            atitle varchar(20),
            pid int
        );
        -- 查询省的名称为“山西省”的所有城市
    select city.* from areas as city inner join areas as province on city.pid=province.aid where province.atitle='山西省';

    ```

    - 子查询

    ```sql
    -- 概念: 在一个 select 语句中,嵌入了另外一个 select 语句, 那么被嵌入的 select 语句称之为子查询语句
    -- 标量子查询
    select * from students where age > (select avg(age) from students);
    -- 列级子查询
    select name from classes where id in (select cls_id from students);
    -- 行级子查询
    select * from students where (height,age) = (select max(height),max(age) from students);
    ```

    - 完整的select语句

    ```sql
    select distinct *
    from 表名
    where ....
    group by ... having ...
    order by ...
    limit start,count
    ```

### 4. Python操作MySQL

```python
import pymysql

conn = pymysql.connect(host='192.168.199.113',
                       port=3306,
                       database='flask_demo',
                       user='root',
                       password='root',
                       charset='utf8')

cs = conn.cursor()  # 获取执行对象
line_count = cs.execute("select * from author")  # 用执行对象执行sql语句,返回受影响的行数
fetchall_tuple = cs.fetchall()  # 获取sql查询的结果

conn.commit()  # 有增改删时, 执行提交
cs.close()  # 关闭
conn.close()
```

> connection对象
> conn = pymysql.connect(参数列表)
> ​	参数host：连接的mysql主机，如果本机是'localhost'
> ​	参数port：连接的mysql主机的端口，默认是3306
> ​	参数database：数据库的名称
> ​	参数user：连接的用户名
> ​	参数password：连接的密码
> ​	参数charset：通信采用的编码方式，推荐使用utf8
> connection对象的方法
> ​	close()关闭连接
> ​	commit()提交
> ​	cursor()返回Cursor对象，用于执行sql语句并获得结果

- 参数化防止SQL注入

```python
# 对于 sql = 'select * from goods where name="%s" ' % find_name 这样的sql手动写入占位符会造成SQL注入攻击, 如: find_name 为 " or 1=1 or "(双引号也要)时, 可造成攻击
# 安全的方式, 构造参数列表
params = [find_name]
count = cs1.execute('select * from goods where name=%s', params)
```

### 5.MySQL高级

#### 1) 视图

- 概念

  视图就是一条SELECT语句执行后返回的结果集

  作用: 像函数提高重用性

- 定义

  `create view 视图名称 as select语句;`

- 查看已定义的视图

  `show tables;`

- 使用
  `select * from 视图;`

- 删除
  `drop view 视图名称;`

#### 2) 事务

- 概念

  一个操作序列，这些操作要么都执行，要么都不执行，它是一个不可分割的工作单位

- 事务四大特性

  - 原子性(Atomicity)
    ​	一个事务必须被视为一个不可分割的最小工作单元
  - 一致性(Consistency)
    ​	从一个一致性的状态转换到另一个一致性的状态
    隔离性(Isolation)
  - 一个事务所做的修改在最终提交以前，对其他事务是不可见的。a=100, 事务提交前,a=100
    持久性(Durability)
  - 一旦事务提交，则其所做的修改会永久保存到数据库

- 开启事务

  开启事务后执行修改命令，变更会维护到本地缓存中，而不维护到物理表中, 所以事务内部数据变更, 事务外部数据不变更

  `begin; | start transaction;`

- 提交事务
  `commit;`

- 回滚事务

  `rollback;`

> 注意
> 修改数据的命令会自动的触发事务，包括insert、update、delete 而在SQL语句中有手动开启事务的原因是：可以进行多次数据的修改，如果成功一起成功，否则一起会滚到之前的数据

#### 4) 索引

- 概念
  索引是一种特殊的文件, 它们包含着对数据表里所有记录的引用指针。
  目的:  提高速度

- 使用

  - 查看索引	

    `show index from 表名`

  - 创建索引

    `create index 索引名称 on 表名(字段名称(长度))`

  - 删除索引

    `drop index 索引名称 on 表名`

  > 注意
  > ​	索引影响更新和插入速度，因为它需要同样更新每个索引文件。
  > ​	对于一个经常需要更新和插入的表格或比较小的表，就没有必要为一个很少使用的where字句单独建立索引了

### 6. 导入测试数据

- 导入

  ```sql
  source xxx.sql
  ```

  