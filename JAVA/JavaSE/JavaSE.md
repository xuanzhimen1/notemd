## 1. Java环境

- JDK下载安装
	- 下载
		```bash
		www.oracle.com/index.html  # 下载java对应版本
		# java10 => Java Archive => Java SE9
		```
	- win安装
		- 一路下一步，注意修改安装路径
		- path环境变量的配置
			我的电脑 => 右键 => 属性 => 高级 => 环境变量 => 系统变量中点击新建 => 变量名:JAVA_HOME 变量值:安装路径 => 点击确定 => 点击系统变量中的Path再点击编辑 => 点击新建: %JAVA_HOME%\bin => 点击确定 => 对刚新建的选项上移到最前面 => 确定确定 => 完成
	
	- Ubuntu安装
	
	  ```bash
	  # 安装
	  tar -zxvf jdk-8u231-linux-x64.tar.gz
	  mv jdk1.8.0_231 /home/xuanzhimen/softInstall/
	  # 修改配置
	  vi ~/.bashrc  # 追加如下
	  export JAVA_HOME=/home/xuanzhimen/softInstall/jdk1.8.0_231
	  export JRE_HOME=${JAVA_HOME}/jre
	  export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
  export PATH=${JAVA_HOME}/bin:$PATH
	  # 使配置文件生效
	  source ~/.bashrc
	  ```
	
	- Linux安装
		```bash
		# 下载解包
		tar -zxvf xxx.tar.gz
		# 配置环境变量
		sudo cp /etc/profile /etc/profile.backup
		sudo vi /etc/profile  # 追加下面内容
	
		export JAVA_HOME=/home/jackiezz/development/java/jdk-9.0.4
		export PATH=$JAVA_HOME/bin:$PATH
		export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
		# 重新加载profile
		source /etc/profile
		# 注销生效
		```
	
- HelloWorld

	- java程序开发的步骤
		
	- 编写 => 编译 => 运行
		
	- 编写HelloWorld
	
	  ```java
	  // 编写HelloWorld程序
	  public class HelloWorld {
	  	public static void main(String[] args) {
	  		System.out.println("Hello World!");
	  	}
	  }
	  ```
	
	- 编译并运行
	
	  ```bash
	  javac HelloWorld.java  # 编译
	  java HelloWorld  # 运行
	  ```
	
	  

## 2. Java基础语法

### 2.1 注释/关键字/标识符

- 注释
	```java
	// 这是单行注释文字
	
	/*
	这是多行注释文字
	这是多行注释文字
	这是多行注释文字
	*/
	
	/**
	文档注释
	*/
	```

- 关键字
	- 被java赋予了特殊含义的单词
	- 特点: 全小写, 高亮显示


- 标识符

	- 标识符: 类/方法/变量/常量等的名字
	- 组成规则
		- 由a~z, A~Z, 0~9, _, $组成, 不以数字开头
		- 命名约定: 小驼峰式命名变量方法, 大驼峰式命名类名

### 2.2 常量/变量/数据类型

- 常量
	- 分类
		- 整数常量 10
		- 小数常量 1.0
		- 字符常量 'a'
		- 布尔常量 true/false
		- 空常量 null
	
- 数据类型

	- 计算机存储单位
		1B（字节） = 8bit
		1KB = 1024B
		1MB = 1024KB
		1GB = 1024MB
		1TB = 1024GB

	- Java中的数据类型
	
    | 数据类型 | 关键字       | 内存占用 | 取值范围                                                     |
    | :------- | ------------ | -------- | :----------------------------------------------------------- |
    | 整数类型 | byte         | 1        | -128~127                                                     |
    |          | short        | 2        | -32768~32767                                                 |
    |          | int(默认)    | 4        | -2的31次方到2的31次方-1                                      |
    |          | long         | 8        | -2的63次方到2的63次方-1                                      |
    | 浮点类型 | float        | 4        | 负数：-3.402823E+38到-1.401298E-45                                                             正数：   1.401298E-45到3.402823E+38 |
    |          | double(默认) | 8        | 负数：-1.797693E+308到-4.9000000E-324                                              正数：4.9000000E-324   到1.797693E+308 |
    | 字符类型 | char         | 2        | 0-65535                                                      |
    | 布尔类型 | boolean      | 1        | true，false                                                  |

- 变量

	- 定义变量
		```java
		int age = 18;  // 声明变量并赋值
		double money;
		money = 55.5;  // 先声明，后赋值（使用前赋值即可）
		long id = 21L;  // 定义long类型变量时加L
		float id2 = 21F;  // 定义float类型变量时加F
		```
	- 使用变量
		1. 变量名不可重复
		2. 变量不能超出数据类型范围
		3. 变量没有赋值,不能使用
		4. 变量使用不能超过作用域范围

	- 成员变量的局部变量

    | 不同点   | 成员变量         | 局部变量            |
    | -------- | ---------------- | ------------------- |
    | 定义位置 | 类中             | 方法中              |
    | 使用范围 | 类中             | 方法中              |
    | 默认值   | 有               | 无, 不赋值,调用异常 |
    | 内存     | 堆               | 栈                  |
    | 生命周期 | 对象垃圾回收消失 | 方法弹栈消失        |



- 类型转换
	![](http://119.3.209.59/blog_image/jackiezz/JavaSE_数据范围大小图.png)
	boolean类型不能与其他基本数据类型相互转换。
	- 自动类型转换
		```java
		double num = 10; // 将int类型的10直接赋值给double类型
		System.out.println(num); // 输出10.0
		
		int a = 'a';  // char类型的数据转换为int类型是按照码表中对应的int值进行计算的
		System.out.println(a); // 将输出97
		
		// 整数默认是int类型，byte、short和char类型数据参与运算均会自动转换为int类型。
		byte b1 = 10;
		byte b2 = 20;
		byte b3 = b1 + b2;  // 代码会报错，b1和b2会自动转换为int类型，计算结果为int，int赋值给byte需要强制类型转换。
		// 修改为:
		int num = b1 + b2;
		// 或者：
		byte b3 = (byte) (b1 + b2);
		```
	- 强制类型转换
		```java
		double num1 = 5.5;
		int num2 = (int) num1; // 将double类型的num1强制转换为int类型
		System.out.println(num2); // 输出5（小数位直接舍弃）
		```
	- 注意
		- 字符串不是基本类型
		- 浮点型并非精确值
		- 整数默认int, 浮点数默认double
		- boolean不能进行类型转换,不能参加算术运算
		- "+": byte/short/char都可以进行"+"运算, 运算时自动提升类型为int和double, 用long和float时在结尾加上F或L，如：3.14F， 100L
		- 对于byte/short/char三种类型赋值时编译器会自动补上(byte)(short)(char)进行强转, 例如: `byte num = 12`  本质上是`byte num = (byte)12`
			```java
			short a = 5;  // 编译器本质: short a = (short)5
			short b = 8;
			short sum = a + b;  // 错误 编译器中: short sum = (int)a + (int)b
			short sum = 5 + 8;  // 正确 编译器中: short sum = (short)13  
			//因为编译器可以对全为常量的表达式进行优化, 若表达式中有变量,则不进行优化
			```


### 2.3 运算符

- 运算符
	- 算术运算符
		- `+ - * / %(取余)`
		- 注意:
			1. char类型参与算术运算，使用的是unicode对应的十进制数值 'a'=>97 'A'=>65 '0'=>48
			2. 算术表达式中包含不同的基本数据类型的值的时候，整个算术表达式的类型会自动进行提升。 等级顺序：byte,short,char --> int --> long --> float --> double
				```java
				byte b1 = 10;
				byte b2 = 20;
				// byte b3 = b1 + b2; // 该行报错，因为byte类型参与算术运算会自动提示为int，int赋值给byte可能损失精度
				int i3 = b1 + b2; // 应该使用int接收
				byte b3 = (byte) (b1 + b2); // 或者将结果强制转换为byte类型
				-------------------------------
				int num1 = 10;
				double num2 = 20.0;
				double num3 = num1 + num2; // 使用double接收，因为num1会自动提升为double类型
				```
			3. 当“+”操作中出现字符串时，这个”+”是字符串连接符，而不是算术运算, 在”+”操作中，如果出现了字符串，就是连接运算符，否则就是算术运算。当连续进行“+”操作时，从左到右逐个执行。
				```java
				System.out.println(1 + 99 + "年黑马"); // 输出：199年黑马
				System.out.println(1 + 2 + "itheima" + 3 + 4); // 输出：3itheima34
				// 可以使用小括号改变运算的优先级 
				System.out.println(1 + 2 + "itheima" + (3 + 4)); // 输出：3itheima7
				```

	- 赋值运算符
		- `=, +=, -=, *=, /= %=`
		- 注意
			```java
			short s = 10;
			s = s + 10; // 此行代码报出，因为运算中s提升为int类型，运算结果int赋值给short可能损失精度
			s += 10; // 此行代码没有问题，隐含了强制类型转换，相当于 s = (short) (s + 10);
			```

	- 自增自减运算符
		- `++, --`
		- 注意:
			```java
			int x = 1;
			int y = x++;  // y为1  原因++在后,先赋值再+1
			int y = ++x;  // y为3  原因++在前,先+1再赋值, --同理
			```

	- 关系运算符
		- `==, !=, >, <, >=, <=`
		- 关系运算符结果为boolean

	- 逻辑运算符
		- `&, |, ^, !` 逻辑与,或,异或,非 (异或:左右结果不同为true)
		- `&&, ||` 短路与,或

	- 三元运算符
		- 关系表达式 ? 表达式1 : 表达式2;
		- 示例
			```java
			int a = 10;
			int b = 20;
			int c = a > b ? a : b;  // 判断 a>b 是否为真，如果为真取a的值，如果为假，取b的值
			```

### 2.4 数据输入/输出

- 数据输入

	```java
	import java.util.Scanner;
	
	Scanner sc = new Scanner(System.in);
	sc.nextInt();  // 将键盘录入的值作为int数返回。
	sc.next();  //将键盘录入的值作为string返回。
	```
	
- 数据输出
	```java
	System.out.println();
	```

### 2.5 流程控制

- 流程控制
	- 分支结构之if
		> `else if` 和 `else` 可省略
	
		```java
		int a = 10;
		int b = 20;
		if (a == b) {
			System.out.println("a=b");
		} else if ( a > b ) {
			System.out.println("a>b");
		} else {
			System.out.println("a<b")
		}
		```
	- 分支结构之switch
		> case没有break会出现case穿透现象,如示例中的2,3,4
		
		```java
		int a = 10;
		switch (a) {
			case 1:
				System.out.println("1")
				break;
			case 2:
			case 3:
			case 4:
				System.out.println("2, 3, 4")
				break;
			default:
				System.out.println("0")
		}
		```
	- 循环结构之for
		```java
		for(int i = 1; i <= 5; i++) {
			System.out.println(i);
		}
		```
	- 循环结构之while
		```java
		while (条件表达式) {
			循环体
		}
		do {
			循环体
		} while (条件表达式)
		```
	- 死循环的三种方式
		```java
		for(;;){}
		while(true){}
		do{}while(true)
		```
	- 跳转控制语句
		- break 跳出循环体,结束循环
		- continue 跳过本次循环,继续下次循环

- 数组

	- 定义
		```java
		int[] intArr;  // 声明一个数组
		int[] intArr1 = new int[3];  // 声明数组长度并初始化为默认值
		int[] intArr2 = {1, 2, 3};  // 声明并初始化
		```
	- 访问
		```java
		int[] arr = new int[3];
		System.out.println(arr[0]);
		```
	- 遍历
		```java
		for (int x = 0; x < arr.length; x++) {
			System.out.println(arr[x])
		}
		```

### 2.6 方法
- 定义
	> 不能嵌套定义
	> return; 与 void对应 可以省略不写
	
	```java
	权限修饰符 [static] 返回值 方法名([参数列表]) {
		方法体
		return;
	}

	// 示例
	public int add(int a, int b){
	return a + b;
	}
	```

- 调用
	```java
	add(10, 20);  // 注意: 静态方法中不能调用非静态方法
	```

- 方法重载
	> 方法方法名相同, 参数列表不同(类型或顺序不同)即为重载, 与返回值无关
	
	```java
	public static void open(){}
	private static int open(int a){}  // 与上面重载
	```
- 参数

  - 可变参数

    > java5+, 个数不定, 类型一定时使用, 底层是一个数组
    > 只能有一个可变参数, 并且放在参数列表的末尾

    ```java
    public static int add(Object...obj){}
    ```


### 2.7 权限修饰符

- 权限修饰符
	![](http://119.3.209.59/blog_image/jackiezz/JavaBasicSyntax_权限修饰符.png)

- private
	- private 本类可以访问, 其它类不可访问
	- private boolean属性， 方法名为isxxx和setxxx, 而非getxxx

- final
	- final修饰类, 该类不能被继承
	- final修饰方法, 该方法不能被重写
	- final修饰变量, 表明该变量是一个常量,不能再次被赋值
	```java
	public final int price;
	```
	
- static
	- 被static修饰的成员方法和变量可以通过类名直接调用(单例)
	- 本类调用可以省略类名
	- 静态不能直接访问非静态 => 先有静态内容,后有非静态内容
	- 静态内容优于非静态 => 静态内容(代码块, 方法, 变量) 在普通内容前执行
	- 静态内容不能使用this
	```java
	public class Demo{
		public static name;  // 静态变量
		public static void staticMethod(){
			// 静态方法通过类名调用, 不推荐对象调用=>javac会翻译为类调用
		}
		static {
			// 静态代码块, 静态代码块在第一次用到时执行唯一一次, 用于给静态变量赋值
		}
	}
	```


## 3. JavaSE常用API

### 3.1 Random

> 产生一个随机数

	```java
	import java.util.Random;
	
	Random random = new Random();
	random.nextInt(10);  // 产生一个0~9的随机数
	random.next();
	```

### 3.2 String相关

- String
	> 字符串
	
	```java
	// 构造
	public String()
	public String(char[] chs)
	public String(byte[] bys)
	String str = "abc"  // ""创建的字符串在常量池中地址相同,new创建的在内存中地址不同
	
	// 实例方法
	// 比较两个字符串内容是否相同 "str".equals(变量),如果写成 变量.equals("str")可以发生NullPointException
	public boolean equals(String s)
	// 返回字符串某个位置的值
	public char charAt(int index)
	// 返回此字符串的长度
	public int length()
	```

- StringBuilder
	
	```java
	// 构造
	public StringBuilder()  // 空白字符串对象
	// String转为StringBuilder
	public StringBuilder(String str)
	
	// 实例方法
	public StringBuilder append(任意类型)
	public int length()
	public StringBuilder reverse()
	// StringBuilder转为String
	public String toString()
	```

### 3.3 Math

- Math包含常用的数字运算的方法
- Math类中没有构造方法,都是静态方法, 可直接通过类名.进行调用
	```java
	// 返回参数绝对值
	public static int abs(int a)
	// 参数向上取整
	public static double ceil(double a)
	// 参数向下取整
	public static double floor(double a)
	// 四舍五入取整
	public static round(float a)
	// 最值
	public static min(int a, int b)
	public static max(int a, int b)
	// 0.0 ~ 1.0 随机数
	public static double random()
	```

### 3.4 System
- 常用方法
	> 位于java.lang中,不用导包
	
	```java
	// 终止当前运行的java虚拟机
	public static void exit(int status)  // 非0表示异常终止
	// 返回当前时间(毫秒)
	public static long currentTimeMills()
	// copy复制数组
	int[] listOld = {1, 2, 3, 4};
	int[] listNew = new int[]{4, 5, 6, 7};
	System.arraycopy(listOld, 1, listNew, 1, 3);  // 结果: listNew: [4, 2, 3, 4] 参数: 原数组,起始index,新数组, 结束index, 复制个数count
	```

### 3.5 Object
- Object 每个类直接或间接继承自object类作为超类
- 常用方法
	> 重写方法: 
	> toString: 返回该对象的字符串表示
	> equals: Object类中默认进行`==`运算符的对象地址比较, 重写可改变比较方式为: 对比对象属性
	> `public static requireNoNull(obj)` 判断对象是否为null
	
	```java
	// 手动定义输出对象是时输出具体内容(如对象属性值),而非对象物理地址
	public String toString()
	// 类重写该方法可以实现比较两个对象的属性值是否相等,而非物理地址
	public boolean equals(Object o)
	// 对于Person类，
	```

### 3.6 Arrays
- 数组工具类
- 常用方法
	```java
	// 返回数组内容以字符串表示
	public static String toString(int[] a)
	// 数组排序
	public static void sort(int[] a)
	```

### 3.7 包装类

- 包装类: 基本数据类型封装成对象的目的: 更多的功能操作该数据
- 包装类名: 基本类型首字母大写, 特殊的 int=>Integer char=>Character
- Integer类方法
	```java
	// 构造方法
	public static Integer valueOf(int i)
	public static Integer valueOf(String s)
	// 
	```
	- int和String类型的转换
	```java
	// int=>String
	int a = 10;
	String strA = a + "";  // 方法一: 直接和""拼接
	strA = String.valueOf(a);  // 方法二: 调用String的valueOf方法
	// String=>int
	String str = "10";
	Integer i = Integer.valueOf(str);  // 方法一: 先调用Integer的valueOf方法转为Integer再调用intValue转为int
	int x1 = i.intValue();
	int x2 = Integer.parseInt(s)  // 方法二: 调用Integer的parseInt方法
	```

- 自动拆箱和自动装箱
	```java
	Integer i = 100;  // 自动装箱
	i += 100;  // i + 100 自动拆箱 i = i + 100; 自动装箱
	```

### 3.8 时间日期类

- Date类
	- Date表示一个特定的时间, 精确到毫秒
	- 方法
		```java
		// 构造
		public Date()  // 返回当前时间对象
		public Date(long l)  // 返回指定毫秒值对应的时间对象
		// Date类常用方法
		// 获取日期对象的毫秒值
		public long getTime()
		// 设置日期对象的毫秒值
		public void setTime(long time)
		```

- SimpleDateFrmat
	- 日期格式化和解析日期的工具类
	- 方法
		```java
		// 构造
		public SimpleDateFormat()  // 构造一个默认模式和日期格式的对象
		public SimpleDateFormat(String pattern)  // 构造指定格式的对象
		// 方法
		// 格式化
		public final String format(Date date);
		// 解析
		public Date parse(String source)
		```

- Calendar类
	- 为特定瞬间的时间日期字段提供了操作方法
	- 方法
		```java
		// 返回指定日历字段的值
		public int get(int field)
		// 给指定日历字段增减值
		public abstract void add(int field, int amount)
		// 设置当前日历的年月日
		public final void set(int year, int month, int date)
		```
	- Demo
		```java
		Calendar c = Calendar.getInstance();
		int year = c.get(Calendar.YEAR);
		int month = c.get(Calendar.MONTH) + 1;
		int date = c.get(Calendar.DATE);
		c.add(Calendar.YEAR, 10);
		c.add(Calendar.DATE, -10);
		c.set(2050, 10, 10);
		```

## 4. 面向对象

### 4.1 概述

- 类和对象

	- 定义类和对象
		```java
		public class Person {  // 定义一个类
			// 成员变量
			private String name;
			// 成员方法
			public String getName() {
				return name;
			}
		}
		
		Person p1 = new Person()  // 创建一个对象
		System.out.println(p1.name)  // 调用对象的属性
		p1.getName();  // 调用对象的方法
		```

- 成员变量和局部变量
	- 成员变量: 类中, 堆内存中, 有默认值, 对象消失则消失
	- 局部变量: 方法中, 栈内存中, 无默认值, 方法调用完毕消失

### 4.2 封装

- private

	> private 修饰方法或变量, 被修饰的方法或变量只能在本类中使用, 对于属性可以提供set/get方法进行访问
	
- this

	> this指调用者对象本身

- 封装思想
	- 封装就是把某些信息隐藏在类内部, 提供相应的方法进行访问或操作
	- 封装的好处: 提高代码的复用性

- 构造方法
	> 作用: 创建对象时调用该方法给成员变量赋值
	
	```java
	public class Student {
		private String name;
		private int age;
		// 省略get/set方法
		
		public Student () {  // 无参构造方法
		}
		public Student (String name, int age) {  // 有参构造方法
			this.name = name;
			this.age = age;
		}
	}
	
	// 调用构造方法创建对象
	Student stu1 = new Student();
	Student stu2 = new Student("zs", 18);
	```

### 4.3 继承

- 继承实现
	> 子类可以使用父类非私有成员(变量,方法)
	> java只支持单继承, 支持多层继承
	> 继承好处与弊端: 提高代码的复用性和可维护性,但增强了代码的耦合性

	```java
	public class Fu {
		public void show() {
			System.out.println("show方法被调用");
		}
	}
	public class Zi extends Fu {
		public void method() {
			System.out.println("method方法被调用");
		}
	}
	public class Demo {
		public static void main(String[] args) {
			//创建对象，调用方法
			Fu f = new Fu();
			f.show();

			Zi z = new Zi();
			z.method();
			z.show();
		}
	}
	```

- super
	> this: 本类对象的引用
	> super: 父类对象引用
	
	```java
	this.成员  // 访问本类成员(属性/方法)
	super.成员  // 访问父类成员
	this(...)  // 访问本类构造
	super(...)  // 访问父类构造
	```

- 继承中访问成员变量和方法的访问特点
	
- 就近原则: 局部, 本类, 父类, 父类的父类
	
- 继承中构造方法的访问特点
	- 子类所有构造方法默认访问父类无参构造 即构造方法默认第一句都是:super();
	- 父类没有无参构造时, 子类构造要手动调用父类的有参构造

- 方法重写
	- 子类的方法 方法名,参数列表与父类一样则为重写
	- 注意
		- 子类方法权限>=父类
		- 子类异常<=父类

### 4.4 多态

- 概述
	- 多态: 一个对象在不同时刻的不同形态
	- 多态前提:
		1. 有继承或实现关系
		2. 有方法重写
		3. 父类引用指向子类对象
	- 优缺点
		- 优点: 提高程序的扩展性, 定义方法时用父类作为参数,使用方法时用子类作为具体实现
		- 缺点: 不能调用子类的特有方法
- 成员访问特点
	- 成员变量 编译看左边, 运行看左边
	- 成员方法 编译看左边, 运行看右边
	
- 向上向下转型
	- 向上转型(即多态的调用, 父类引用指向子类对象): 父类型 对象名 = 子类引用
	- 向下转型: 子类型 对象名 = (子类型)父类引用

### 4.5 抽象类/接口

- 抽象类
	- Java中一个没有具体实现的方法应该定义为抽象方法, 一个含有抽象方法的类应该定义为抽象类
	- 抽象类用abstract修饰
		```java
		// 定义抽象类
		public abstract class 类名 {}
		// 定义抽象方法
		public abstract void 方法名();
		```
- 接口
	- 接口: 一种公共的规范标准, Java中接口更多的是对行为的抽象
	- 接口用interface修饰
		```java
		public interface 接口名 {}
		```
	- 类实现接口用implements修饰
		```java
		public class 类名 implements 接口名 {}
		```
	- 接口中成员变量都是常量 用public static final 修饰
	- 接口中没有构造方法,成员方法都是的阄方法 用public abstract修饰

### 4.6 内部类
- 内部类: 一个类中定义一个类
	```java
	class Outer {
		public class Inner{
		
		}
	}
	```
- 内部类的方法特点:
	- 内部类可以直接访问外部类的成员,包括私有
	- 外部类要访问内部类的成员,必须创建对象

- 成员内部类的访问
	- Outer.Inner oi = new Outer().new Inner();
	- 推荐使用方案: 内部类的目的大多是不让外界访问,通常将内部类定义为私有,外界通过调用方法,方法内部创建内部类对象并调用
		```java
		class Outer {
			private int num = 10;
			private class Inner {
				public void show() {
					System.out.println(num);
				}
			}
			public void method() {
				Inner i = new Inner();
				i.show();
			}
		}
		public class InnerDemo {
			public static void main(String[] args) {
				//Outer.Inner oi = new Outer().new Inner();
				//oi.show();
				Outer o = new Outer();
				o.method();
			}
		}
		```

- 局部内部类
	- 外界无法直接使用
	```java
	class Outer {
		private int num = 10;
		public void method() {
			int num2 = 20;
			class Inner {
				public void show() {
					System.out.println(num);
					System.out.println(num2);
				}
			}
			Inner i = new Inner();
			i.show();
		}
	}
	public class OuterDemo {
		public static void main(String[] args) {
			Outer o = new Outer();
			o.method();
		}
	}
	```

- 匿名内部类
	- 本质: 一个继承了该类或实现了廖接口的子类匿名对象
	```java
	interface Inter{
		void method();
	}

	class Test{
		public static void main(String[] args){
			new Inter(){
				@Override
				public void method(){
					System.out.println("我是匿名内部类");
				}
			}.method();	// 直接调用方法
		}
	}
	```
	- 使用场景: 当一个方法需要接口或抽象类的子类对象作为参数时, 可传递一个匿名内部类过去


## 5. 集合
![](http://119.3.209.59/blog_image/jackiezz/Java笔试_集合图示.png)

### 5.1 Collection
- collection集合的常用方法
	```java
	boolean add(E e);
	boolean remove(Object o)
	void clear()
	boolean contains(Object o)
	boolean isEmpty()
	int size()
	```

- Collections工具类
	```java
	public static void sort(List<T> list)
	public static void reverse(List<T> list)
	public static void shuffle(List<T> list)
	```


### 5.2 List集合

- List接口方法
	```java
	// 增加元素
	void add(int index, E element)
	// 删除元素
	E remove(int index)
	// 修改元素
	E set(int index, E element)
	// 查找元素
	E get(int index)
	```


- ArrayList

	```java
	// 构造方法
	public ArrayList();
	// 成员方法
	// 增加元素
	public boolean add(E e)  // 在集合末尾添加元素
	public void add(int index, E element)  // 指定位置添加元素
	// 删除元素
	public boolean remove(Object o) // 删除指定元素
	public E remove(int index)  // 删除指定位置的元素
	// 修改元素
	public E set(int index, E element)  // 修改指定位置元素
	// 查找元素
	public E get(int index)  // 查找指定位置元素
	public int size()  // 返回集合元素个数
	```

- 增强for循环的使用
	```java
	for(元素类型 变量名 : 数组/集合) {
		循环体
	}
	```
	
- LinkedList特有方法
	```java
	public void addFirst(E e)
	public void addLast(E e)
	public E getFirst()
	public E getLast()
	public E removeFirst()
	public E removeLast()
	```

### 5.3 Set集合
### 5.4 Map集合
- Map集合的方法
	```java
	// 添加元素
	V put(K key, V value)
	// 删除元素
	V remove(Object key)
	// 清空集合
	void clear()
	// 包含关系
	boolean containsKey(Object key)  // 是否包含指定键
	boolean containsValue(Object value)  // 是否包含指定的值
	boolean isEmpty()  // 判断集合是否为空
	// 返回集合元素个数
	int size()
	// 查询
	V get(Object key)  // 根据键查询值
	Set<K> keySet()  // 获取所有键的集合
	Collection<V> values()  // 获取所有值的集合
	Set<Map.Entry<K, V>> entrySet()  // 获取所有键值对对象的集合
	```

## 6. IO流

### 6.1 字节流

- 字节流抽象基类

  - InputStream：这个抽象类是表示字节输入流的所有类的超类
  - OutputStream：这个抽象类是表示字节输出流的所有类的超类
  - 子类名特点：子类名称都是以其父类名作为子类名的后缀

- 字节输出流

  - FileOutputStream(String name)：创建文件输出流以指定的名称写入文件

  - 使用示例
	  ```java
	  //创建字节输出流对象FileOutputStream(String name)：创建文件输出流以指定的名称写入文件
	  FileOutputStream fos = new FileOutputStream("myByteStream\\fos.txt");
	  //将指定的字节写入此文件输出流void write(int b)
	  fos.write(97);
	  //释放资源void close()：关闭此文件输出流并释放与此流相关联的任何系统资源。
	  fos.close();
	  ```

- 字节流对象方法

| 方法名                                   | 说明                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| void   write(int b)                      | 将指定的字节写入此文件输出流   一次写一个字节数据            |
| void   write(byte[] b)                   | 将 b.length字节从指定的字节数组写入此文件输出流   一次写一个字节数组数据 |
| void   write(byte[] b, int off, int len) | 将 len字节从指定的字节数组b偏移量off开始写入此文件输出流   一次写一个字节数组的部分数据 |

- 字节流写数据如何实现换行

  - windows:\r\n
  - linux:\n
  - mac:\r

- 字节流写数据如何实现追加写入

  - public FileOutputStream(String name,boolean append) 如果第二个参数为true则追加


- 异常完整写法

  ```java
  //加入finally来实现释放资源
  FileOutputStream fos = null;
  try {
	  fos = new FileOutputStream("myByteStream\\fos.txt");
	  fos.write("hello".getBytes());
  } catch (IOException e) {
  	e.printStackTrace();
  } finally {
	  if(fos != null) {
		  try {
		  	fos.close();
		  } catch (IOException e) {
		  	e.printStackTrace();
		  }
	  }
  }
  ```

- 字节输入流FileInputStream(String name)

	- 通过打开与实际文件的连接来创建一个FileInputStream,用于读文件数据

	- 使用示例
	  ```java
	  //创建字节输入流对象FileInputStream(String name)
	  FileInputStream fis = new FileInputStream("myByteStream\\fos.txt");
	  int by;
	  // 一次读一个字节数组数据, 如果fis.read()返回-1表示没有数据了
	  while ((by=fis.read())!=-1) {
		System.out.print((char)by);
	  }
	  //释放资源
	  fis.close();
	  ```

- 一次读一个字节数组的方法

  - public int read(byte[] b)：从输入流读取最多b.length个字节的数据, 返回的是读入缓冲区的总字节数,也就是实际的读取字节个数

	- 使用示例

	  ```java
	  FileInputStream fis = new FileInputStream("myByteStream\\fos.txt");
	  byte[] bys = new byte[1024]; //1024及其整数倍
	  int len;
	  while ((len=fis.read(bys))!=-1) {
		System.out.print(new String(bys,0,len));
	  }
	  fis.close();
	  ```

- 字节缓冲流BufferedOutputStream：不必为写入的每个字节导致底层系统的调用

	- 构造方法：

| 方法名                                 | 说明                   |
| -------------------------------------- | ---------------------- |
| BufferedOutputStream(OutputStream out) | 创建字节缓冲输出流对象 |
| BufferedInputStream(InputStream in)    | 创建字节缓冲输入流对象 |


### 6.2 字符流

- 编解码

	- 由于字节流操作中文不是特别的方便，所以Java就提供字符流
	- 字符流 = 字节流 + 编码表

	- String编解码相关方法

| 方法名                                   | 说明                                               |
| ---------------------------------------- | -------------------------------------------------- |
| byte[] getBytes()                        | 使用平台的默认字符集将该 String编码为一系列字节    |
| byte[] getBytes(String charsetName)      | 使用指定的字符集将该 String编码为一系列字节        |
| String(byte[] bytes)                     | 使用平台的默认字符集解码指定的字节数组来创建字符串 |
| String(byte[] bytes, String charsetName) | 通过指定的字符集解码指定的字节数组来创建字符串     |

- 字符输入输出流

  - InputStreamReader字符输入流, 使用其子类 FileInputStream 实例化
  - OutputStreamWriter：字符输出流, 使用其子类 FileOutputStream 实例化

	- 构造方法

| 方法名                                              | 说明                                         |
| --------------------------------------------------- | -------------------------------------------- |
| InputStreamReader(InputStream in)                   | 使用默认字符编码创建InputStreamReader对象    |
| InputStreamReader(InputStream in,String chatset)    | 使用指定的字符编码创建InputStreamReader对象  |
| OutputStreamWriter(OutputStream out)                | 使用默认字符编码创建OutputStreamWriter对象   |
| OutputStreamWriter(OutputStream out,String charset) | 使用指定的字符编码创建OutputStreamWriter对象 |

	- 使用示例
	  ```java
	  OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("myCharStream\\osw.txt"),"GBK");
	  osw.write("中国");
	  osw.close();
	
	  InputStreamReader isr = new InputStreamReader(new FileInputStream("myCharStream\\osw.txt"),"GBK");
	  int ch;
	  while ((ch=isr.read())!=-1) {
		System.out.print((char)ch);
	  }
	  isr.close();
	  ```
	
	- 字符流写数据的5种方式

| 方法名                                    | 说明                 |
| ----------------------------------------- | -------------------- |
| void   write(int c)                       | 写一个字符           |
| void   write(char[] cbuf)                 | 写入一个字符数组     |
| void write(char[] cbuf, int off, int len) | 写入字符数组的一部分 |
| void write(String str)                    | 写一个字符串         |
| void write(String str, int off, int len)  | 写一个字符串的一部分 |

	- 刷新和关闭的方法

| 方法名  | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| flush() | 刷新流，之后还可以继续写数据                                 |
| close() | 关闭流，释放资源，但是在关闭之前会先刷新流。一旦关闭，就不能再写数据 |

	- 字符流读数据的2种方式

| 方法名                | 说明                   |
| --------------------- | ---------------------- |
| int read()            | 一次读一个字符数据     |
| int read(char[] cbuf) | 一次读一个字符数组数据 |


- 字符缓冲流
	- BufferedWriter：将文本写入字符输出流，缓冲字符，以提供单个字符，数组和字符串的高效写入，可以指定缓冲区大小，或者可以接受默认大小。默认值足够大，可用于大多数用途
	- BufferedReader：从字符输入流读取文本，缓冲字符，以提供字符，数组和行的高效读取，可以指定缓冲区大小，或者可以使用默认大小。 默认值足够大，可用于大多数用途

	- 构造方法

| 方法名                     | 说明                   |
| -------------------------- | ---------------------- |
| BufferedWriter(Writer out) | 创建字符缓冲输出流对象 |
| BufferedReader(Reader in)  | 创建字符缓冲输入流对象 |

	- 代码演示
	  ```java
	  public class BufferedStreamDemo01 {
		  public static void main(String[] args) throws IOException {
			  //BufferedWriter(Writer out)
			  BufferedWriter bw = new BufferedWriter(new                                                            FileWriter("myCharStream\\bw.txt"));
			  bw.write("hello\r\n");
			  bw.write("world\r\n");
			  bw.close();
	
			  //BufferedReader(Reader in)
			  BufferedReader br = new BufferedReader(new                                                           FileReader("myCharStream\\bw.txt"));
	
			  //一次读取一个字符数据
	  //        int ch;
	  //        while ((ch=br.read())!=-1) {
	  //            System.out.print((char)ch);
	  //        }
	
			  //一次读取一个字符数组数据
			  char[] chs = new char[1024];
			  int len;
			  while ((len=br.read(chs))!=-1) {
				  System.out.print(new String(chs,0,len));
			  }
	
			  br.close();
		  }
	  }
	  ```

### 6.3 IO流小结

- 字节流

  ![](http://119.3.209.59/blog_image/jackiezz/JavaAPI_IO小结字节流.jpg)

- 字符流

  ![](http://119.3.209.59/blog_image/jackiezz/JavaAPI_IO小结字符流.jpg)




## 7. 异常
- Java异常体系
	![](http://119.3.209.59/blog_image/jackiezz/JavaBasicSyntax_异常体系.png)

- 异常的处理方式
	- JVM处理异常的方式
		1. 将异常的名称原因及异常出现的位置输出在控制台
		2. 终止程序
	- try...catch方式处理异常
		> 若catch中有return, finally中的代码依然执行
		
		```java
		try {
			可能出现异常的代码
		} catch (异常类名 变量名) {
			出现异常时执行catch中代码
		} finally {
			无论是否会出现异常, 此处代码都会被执行
		}
		
		// 示例
		public static void foo(int i) {
			try {
				if (i == 1) {
					throw new Exception();
				}
				output += "1";
			} catch (Exception e) {
				output += "2";
				return;
			} finally {
				output += "3";
			}
			output += "4";
		}
		foo(0);
		foo(1);
		// 最后结果为13423 想想是为什么
		```

	- throws抛出异常, 让调用者处理
		```java
		public static void foo(int i) throws Exception {
			throw new Exception();
		}
		```

- Throwable成员方法
	```java
	// 返回此throwable的详细消息
	public String getMessage()
	// 返回此可抛出异常的简短描述
	public String toString()
	// 将错误信息输出到控制台
	public void printStackTrace()
	```

- 自定义异常

	- 定义
		```java
		public class ScoreException extends Exception {
			public ScoreException() {}
			public ScoreException(String message){
				super(message)
			}
		}
		
		// 方法中调用
		throw new ScoreException("给出的分数有误")
		```

## 8. 泛型

- 泛型概述
	> 泛型本质是参数化类型, 即所操作的数据类型被指定为一个参数
	> 泛型把运行时异常提前到了编译间解决
	> 避免了强制类型转换

- 泛型的定义
	```java
	// 定义泛型类
	public class Generic<T> {}
	// 定义泛型方法
	public <T> void show(T t) {}
	// 定义泛型接口
	public interface Generic<T> {}
	```

- 类型通配符
	```java
	// 匹配任何类型的数据
	List<?> list = new ArrayList<Object>();
	// 匹配上限
	List<? extends Throwable> list1 = new ArrayList<Error>();
	// 匹配下限
	List<? super Throwable> list2 = new ArrayList<Throwable>();
	```


## 9. 多线程

- Thread
	
	```java
	// 构造方法
	public Thread()  // 分配一个新的线程对象。
	public Thread(String name)  // 分配一个指定名字的新的线程对象
	public Thread(Runnable target)  // 分配一个带有指定目标新的线程对象。
	public Thread(Runnable target,String name)  // 分配一个带有指定目标新的线程对象并指定名字。
	// 实例方法
	public String getName()  // 获取当前线程名称
	public void start()  // 导致此线程开始执行; Java虚拟机调用此线程的run方法。
	public void run()  // 此线程要执行的任务在此处定义代码
	public static void sleep(long millis)  // 使当前正在执行的线程以指定的毫秒数暂停(暂时停止执行)。
	public static Thread currentThread()  // 返回对当前正在执行的线程对象的引用。
	```

- 定义线程
	- 方法一: 继承Thread类, 重写run()方法
    ```java
    public class MyThread extends Thread {
    	//定义指定线程名称的构造方法
    	public MyThread(String name) {
    		//调用父类的String参数的构造方法,指定线程的名称
    		super(name);
    	}

    	// 重写run方法,完成该线程执行的逻辑
    	@Override
    	public void run() {
    		// 线程执行逻辑
    	}
    }
    ```

    - 方式二: 实现Runnable接口, 实现run()方法
	> Thread类也是实现了Runable接口

    ```java
    public class MyRunnable implements Runnable{
    	@Override
    	public void run() {
    		for (int i = 0; i < 20; i++) {
    			System.out.println(Thread.currentThread().getName()+" "+i);
    		}
    	}
    }
    ```

- 使用线程
	> 线程也是一个对象, 创建对象并调用线程start()方法即可启动线程
	```java
	// 继承自Thread类的线程
	MyThread mt = new MyThread("小强");
	mt.start();
	// 实现Runable接口的线程
	MyRunnable mr = new MyRunnable(mr, "小强");
	mr.start();
	```

- Runable和Thread的区别
	> Runable实现资源共享
	> 线程池只能放入实现Runalbe和Callable的类线程, 不能放入Thread类线程

- 示例: 匿名内部类实现线程创建

    ```java
    public class NoNameInnerClassThread {
    	public static void main(String[] args) {
    		Runnable r = new Runnable(){  // 匿名内部类实现接口
    			public void run(){
    				for (int i = 0; i < 20; i++) {
    					System.out.println("张宇:"+i);
    				}
    			}
    		};
    		new Thread(r).start();
    		for (int i = 0; i < 20; i++) {
    			System.out.println("费玉清:"+i);
    		}
    	}
    }
    ```

- 线程安全问题
	- 同步方式解决多线程写操作存在线程安全问题
		```java
		// 同步代码块
		Object lock = new Object();
		synchronized (lock) {
			// 操作有安全问题的变量
		}
		
		// 同步方法
		public synchronized void method(){
			// 操作有安全问题的变量
		}
		```
	
	- Lock锁解决安全问题
		```java
		Lock lock = new ReentrantLock();
		lock.lock();
		num += 1;
		lock.unlock();
		```
	
- 线程状态
	- Blocked(阻塞): 线程A与线程B代码中使用同一锁,如果线程A获取到锁,线程A进入到Runnable状态,那么线程B就进入到Blocked锁阻塞状态
    - Waiting(无限等待): 一个调用了某个对象的 Object.wait 方法的线程会等待另一个线程调用此对象的Object.notify()方法 或 Object.notifyAll()方法
    - notify/notifyAll(唤醒): 当多个线程协作时,比如A,B线程,如果A线程在Runnable(可运行)状态中调用了wait()方法那么A线程就进入了Waiting(无限等待)状态,同时失去了同步锁。假如这个时候B线程获取到了同步锁,在运行状态中调用了notify()方法,那么就会将无限等待的A线程唤醒。注意是唤醒,如果获取到锁对象,那么A线程唤醒后就进入Runnable(可运行)状态;如果没有获取锁对象,那么就进入到Blocked(锁阻塞状态)。

    ```java
    public class WaitingTest {
    	public static Object obj = new Object();
    	public static void main(String[] args) {
    		// 演示waiting
    		new Thread(new Runnable() {
    			@Override
    			public void run() {
    				while (true){
    					synchronized (obj){
    						try {
    							System.out.println( Thread.currentThread().getName() +"=== 获取到锁对象,调用wait方法,进入waiting状态,释放锁对象");
    							obj.wait();  //无限等待
    									//obj.wait(5000); //计时等待, 5秒 时间到,自动醒来
    						} catch (InterruptedException e) {
    							e.printStackTrace();
    						}
    						System.out.println( Thread.currentThread().getName() + "=== 从waiting状态醒来,获取到锁对象,继续执行了");
    					}
    				}
    			}
    		},"等待线程").start();
            
    		new Thread(new Runnable() {
    			@Override
    			public void run() {
    				while (true){
    					//每隔3秒 唤醒一次
    					try {
    						System.out.println( Thread.currentThread().getName() +"‐‐‐‐‐ 等待3秒钟");
    						Thread.sleep(3000);
    					} catch (InterruptedException e) {
    						e.printStackTrace();
    					}
    					synchronized (obj){
    						System.out.println( Thread.currentThread().getName() +"‐‐‐‐‐ 获取到锁对象,调用notify方法,释放锁对象");
    						obj.notify();
    					}
    				}
    			}
    		},"唤醒线程").start();
    	}
    }
    ```

- 使用线程池

  ```java
  import java.util.concurrent.ExecutorService;
  import java.util.concurrent.Executors;
  
  public class TheadPool {
      public static void main(String[] args) {
          // 创建线程池对象
          ExecutorService service = Executors.newFixedThreadPool(2);//包含2个线程对象
          // 创建Runnable实例对象
          MyRunnable r = new MyRunnable();
          // 从线程池中获取线程对象,然后调用MyRunnable中的run()
          service.submit(r);
          // 再获取个线程对象,调用MyRunnable中的run()
          service.submit(r);
          service.submit(r);
          // 注意:submit方法调用结束后,程序并不终止,是因为线程池控制了线程的关闭。
          // 将使用完的线程又归还到了线程池中
          // 关闭线程池
          //service.shutdown();
      }
  }
  ```

## 10. 网络编程

- InetAddress Internet协议IP地址
	```java
	static InetAddress getByName(String host)  // 确定主机名称的IP地址
	String getHostName()  // 获取此IP地址的主机名
	String getHostAddress()  // 获取文本显示中的IP地址字符串
	```

### 10.1 UDP通信
- UDP相关java方法
	```java
	// 构造
	DatagramSocket()  // 创建数据报套接字并绑定到梧桐地址上的任何可用商品
	DategramPacket(byte[] buf, int len, InteAddress add, int port)  // 创建数据包, 发送长度为len的数据包到指定主机的端口
	// 相关方法
	void send(DatagramPacket p)  // 发送数据包
	void close()  // 关闭套接字
	void receive(DatagramPacket p)  // 从此套接字接收数据包
	```

- UDP通信步骤
	```java
	/**
	1. 创建发送端Socket对象(DatagramSocket)
	2. 创建数据包, 打包数据
	3. 调用DatagramSocket的方法发送数据
	4. 关闭发送端
	*/
	DatagramSocket ds = new DatagramSocket();
	byte[] bys = "hello , udp ".getBytes();
	DatagramPacket dp = new DatagramPacket(bys, bys.length, InetAddress.getByName("192.168.1.66"), 10086);
	ds.send(dp);
	ds.close();
	```

### 10.2 TCP通信

- TCP通信相关构造和方法
	```java
	// 构造方法
	Socket(InetAddress address, int port)  // 创建流套接字并将其连接到指定IP指定端口
	Socket(String host, int port)
	// Socket相关方法
	InputStream getInputStream()  // 返回此套接字的输入流
	OutputStream getOutputStream()  // 返回此套接字的输出流
	```

- 示例
	```java
	public class ServerDemo {
		public static void main(String[] args) throws IOException {
			//创建服务器端的Socket对象(ServerSocket)
			//ServerSocket(int port) 创建绑定到指定端口的服务器套接字
			ServerSocket ss = new ServerSocket(10000);

			//Socket accept() 侦听要连接到此套接字并接受它
			Socket s = ss.accept();

			//获取输入流，读数据，并把数据显示在控制台
			InputStream is = s.getInputStream();
			byte[] bys = new byte[1024];
			int len = is.read(bys);
			String data = new String(bys,0,len);
			System.out.println("数据是：" + data);

			//释放资源
			s.close();
			ss.close();
		}
	}
	```

## 11. Junit
- 规范

  > 以测试Calculator类为例

  - 测试类所在包: `cn.itcast.test`
  - 测试类名: `CalculatorTest`
  - 测试方法名: `testDemoMethod`
  - 测试返回值及参数列表: `空`
  - 测试结果不输出到控制台 用`Assert` 断言

- `@Before`
	```java
	/**
	* 初始化方法：
	*  用于资源申请，所有测试方法在执行之前都会先执行该方法
	*/
	@Before
	public void init(){
		System.out.println("init...");
	}
	```

- `@After`
	```java
	/**
	* 释放资源方法：
	* 在所有测试方法执行完后，都会自动执行该方法
	*/
	@After
	public void close(){
		System.out.println("close...");
	}
	```

- `@Test`
	```java
	/**
	* 测试add方法
	*/
	@Test
	public void testAdd(){
		Calculator c  = new Calculator();
		int result = c.add(1, 2);
		// 断言  我断言这个结果是3
		Assert.assertEquals(3,result);
	}
	```

## 12. 反射
- 概述
	- 反射的本质: 将类的各个部分封装为其他对象
	- 作用: 1. 在程序运行过程中,操作这些对象  2. 解耦,提高程序的可扩展性

- Class对象
	> 同一个字节码文件(.class)在一次程序运行过程中,只会加载一次,即Class对象是单例的

	```java
	// 获取Class对象
	Class personClass1 = Class.forName("com.itcast.domain.Person");
	Class personClass2 = Person.class;
	Class personClass3 = person.getClass();
	```

- 通过Class对象获取成员 Field, Constructor, Method 的对象
	```java
	// 获取成员变量对象
	Field[] getFields()  // 获取所有public修饰的成员变量
	Field getField(String name)  // 获取指定名称的 public修饰的成员变量
	Field[] getDeclaredFields()  // 获取所有的成员变量，包括private
	Field getDeclaredField(String name)  // 获取指定名称的的成员变量包括private
	
	// 获取构造方法对象
	Constructor<?>[] getConstructors()
	Constructor<T> getConstructor(类<?>... parameterTypes)
	Constructor<T> getDeclaredConstructor(类<?>... parameterTypes)
	Constructor<?>[] getDeclaredConstructors()
	
	// 获取成员方法对象
	Method[] getMethods()
	Method getMethod(String name, 类<?>... parameterTypes)
	Method[] getDeclaredMethods()
	Method getDeclaredMethod(String name, 类<?>... parameterTypes)
	```

- Field对象的方法
	```java
	// 设置值
	void set(Object obj, Object value)
	// 获取值
	get(Object obj)
	// 暴力反射(忽略访问权限修饰符的安全检查)
	setAccessible(true)
	
	// 示例
	Field aField = personClass1.getField("a");
	// 获取成员变量对象的值,设置值
	aField.set(person, "aaa");
	Object a = aField.get(person);

	// 获取全部public成员变量
	Field[] fields = personClass1.getFields();
	for (Field field : fields) {
		Object o1 = field.get(person);
	}

	// 获取所有的成员变量，不考虑修饰符
	Field declaredNameField = personClass1.getDeclaredField("name");
	Field[] declaredFields = personClass1.getDeclaredFields();
	declaredNameField.setAccessible(true);  // 暴力访问(private)
	```
	
- Constructor对象的方法
	```java
	// 创建对象
	T newInstance(Object... initargs)
	
	// 示例
	personClass1.getConstructor(String.class, int.class);
	Object instance2 = constructor2.newInstance("zhangsan", 18);
	```

- Method对象的方法
	```java
	// 执行方法
	Object invoke(Object obj, Object... args)
	// 获取方法名
	String getName()
	
	// 示例
	Method eat = personClass1.getMethod("eat");
	Object invoke = eat.invoke(person);
	```

- 反射示例
	`reflectTest.java`
	```java
	package cn.itcast.reflect;

	import java.io.InputStream;
	import java.lang.reflect.Method;
	import java.util.Properties;

	public class ReflectTest {
		public static void main(String[] args) throws Exception {
			// 加载配置文件
			ClassLoader classLoader = ReflectTest.class.getClassLoader();
			InputStream inputStream = classLoader.getResourceAsStream("pro.properties");
			Properties properties = new Properties();
			properties.load(inputStream);

			// 根据配置文件中的类名和方法,利用反射执行指定类的指定方法
			String className = properties.getProperty("className");
			String methodName = properties.getProperty("methodName");
			Class cls = Class.forName(className);
			Object object = cls.newInstance();
			Method method = cls.getDeclaredMethod(methodName);
			method.invoke(object);
		}
	}
	```

## 13. 注解

- 概述
  
> 本质：注解本质上就是一个接口，该接口默认继承Annotation接口

- JDK预定义注解
  ```java
  // 1. javadoc注解
  /**
   * 注解javadoc
   * @author itcast
   * @version 1.0
   * @since 1.5
   */
  public class Demo1 {
      /**
       * 函数注解
       * @param a 整数
       * @return 返回整数本身
       */
      public static int add(int a) {
          return a;
      }
  }
  
  // 2. @Override  检测被该注解标注的方法是否是继承自父类(接口)的
  // 3. @Deprecated  该注解标注的内容，表示已过时
  // 4. @SuppressWarnings  压制警告  一般传递参数all 例:@SuppressWarnings("all")
  ```

- 自定义注解
	> 属性：接口中的抽象方法
	> * 取值要求：
	> 	* 基本数据类型
	> 	* String
	> 	* 枚举
	> 	* 注解
	> 	* 以上类型的数组

	```java
	// 自定义注解
	public @interface MyAnno {
		int value();
		String name() default "zhangsan";
		Person per();
		MyAnno2 anno2();

		int[] values();
		String[] names();
		Person[] pers();  // 枚举
		MyAnno2[] anno2s();
	}
	
	// 使用注解
	// 1. 如果定义属性时，使用default关键字给属性默认初始化值，则使用注解时，可以不进行属性的赋值。
	// 2. 如果只有一个属性需要赋值，并且属性的名称是value，则value可以省略，直接定义值即可
	// 3. 数组赋值时，值使用{}包裹。如果数组中只有一个值，则{}可以省略
	@MyAnno(value = 18, 
		name = "zhangsan", 
		per = Person.p1, 
		anno2 = @MyAnno2, 
		values = 18, 
		names = {"zs", "ls"}, 
		pers = {Person.p1, Person.p2}, 
		anno2s = @MyAnno2)
	public class Demo2 {}
	```

- 元注解
	> 用于描述注解的注解
	
	```java
	@Target：描述注解能够作用的位置
		* ElementTypeTYPE：可以作用于类上
		* ElementTypeMETHOD：可以作用于方法上
		* ElementTypeFIELD：可以作用于成员变量上
	@Retention：描述注解被保留的阶段
		* @Retention(RetentionPolicy.RUNTIME)：当前被描述的注解，会保留到class字节码文件中，并被JVM读取到
	@Documented：描述注解是否被抽取到api文档中
	@Inherited：描述注解是否被子类继承
	
	// 示例
	@Target({ElementType.TYPE, ElementType.METHOD, ElementType.FIELD})
	@Retention(RetentionPolicy.RUNTIME)
	@Documented
	@Inherited
	public @interface MyAnno2 {}
	```

- 解析注解
	> 获取注解中的属性值
	> 获取注解对象: `MyAnnotation annotation = MyAnnotationTest.class.getAnnotation(MyAnnotation.class);`
	> 获取注解属性: `String methodName = annotation.methodName();`
	> 是否有某注解: `boolean b = cls.isAnnotationPresent(MyAnnotation.class);`  Class,Method,Field对象都有该方法
	
	`MyAnnotation`
	```java
	package cn.itcast.annotation;

	import java.lang.annotation.ElementType;
	import java.lang.annotation.Retention;
	import java.lang.annotation.RetentionPolicy;
	import java.lang.annotation.Target;

	@Target({ElementType.TYPE})
	@Retention(RetentionPolicy.RUNTIME)
	public @interface MyAnnotation {
		String className();
		String methodName();
	}
	```
	
	`MyAnnotationTest.java`
	```java
	package cn.itcast.annotation;

	import java.lang.reflect.Method;

	@MyAnnotation(className = "cn.itcast.domain.Person", methodName = "active")
	public class MyAnnotationTest {
		public static void main(String[] args) throws Exception {

			MyAnnotation annotation = MyAnnotationTest.class.getAnnotation(MyAnnotation.class);
			String className = annotation.className();
			String methodName = annotation.methodName();

			Class<?> cls = Class.forName(className);
			Method method = cls.getDeclaredMethod(methodName);
			method.setAccessible(true);
			Object obj = cls.newInstance();
			method.invoke(obj);
		}
	}
	```













