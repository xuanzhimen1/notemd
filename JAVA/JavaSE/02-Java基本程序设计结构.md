# Java基本程序设计结构

## 1. 一个简单的Java程序

- HelloWorld
	```java
	public class HelloWorld {  
	    public static void main(String[] args) {  
	        System.out.println("helloWorld");  
	  }  
	}
	```
- 编译并运行Java程序
	```bash
	javac HelloWorld.java  # 编译
	java HelloWorld  # 运行
	```

## 2. 注释

- 单行注释
	```java
	// 这是java单行注释
	```
-   多行注释
	```java
	/*
	这是多行注释
	*/
	```
-   多行注释
	```java
	/**
	* 这是Java文档注释`
	*/
	```

## 3. 数据类型

- 常量
	- 字符串常量 `"HelloWorld"`
	- 整数常量 `66, -88`
	- 小数常量 `0.1`
	- 字符常量 `'A', '0', '我'`
	- 布尔常量 `true, false`
	- 空常量 `null`

- 计算机存储单位
	- 1B（字节） = 8bit
	- 1KB = 1024B
	- 1MB = 1024KB
	- 1GB = 1024MB
	- 1TB = 1024GB

- 数据类型分类
	```mermaid
	graph TD
	数据类型-->基本数据类型
	数据类型-->引用数据类型
	基本数据类型-->数值型
	基本数据类型-->非数值型
	数值型-->整数(整数 byte,short,int,long)
	数值型-->浮点数(浮点数 float,double)
	数值型-->字符(字符 char)
	非数值型-->布尔(布尔 boolean)
	引用数据类型-->类(类 class)
	引用数据类型-->接口(接口 interface)
	引用数据类型-->数组(数组)
	```

- 数据类型内存占用与取值
	| 数据类型 | 内存占用（字节） |   取舍范围   |
	| :------: | :--------------: | :----------: |
	|   byte   |        1         |   -128~127   |
	|  short   |        2         | -32768~32767 |
	|   int    |        4         | -2^31~2^31-1 |
	|   Long   |        8         | -2^63~2^63-1 |
	|  float   |        4         | -2^31~2^31-1 |
	|  double  |        8         | -2^63~2^63-1 |
	|   char   |        2         |   0-65535    |
	| boolean  |        1         |  true,false  |


- 数据范围从小到大
	> boolean类型不能与其他基本数据类型相互转换。

	```mermaid
	graph LR
	byte --> short
	short --> int
	char --> int
	int --> long
	long -->float
	float-->double
	```

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

## 4. 变量


- 定义
	```java
	// 定义变量： 数据类型 变量名 = 变量值;
	int a = 10;
	```
- 使用（取值与修改值）
	```java
	// 取值：直接使用变量名
	System.out.println(a);
	// 修改变量值：变量名 = 变量值
	a = 20;
	```
- 变量使用注意
	- 变量定义时变量名在同一域中不可重复
	- 变量未赋值不可使用：int a;  System.out.print(a) 会报错
	- long类型变量定义时为：long l = 10L;
	- float类型变量定义时为：float f = 1.0F;

- 成员变量的局部变量
	| 不同点   | 成员变量         | 局部变量            |
	| -------- | ---------------- | ------------------- |
	| 定义位置 | 类中             | 方法中              |
	| 使用范围 | 类中             | 方法中              |
	| 默认值   | 有               | 无, 不赋值,调用异常 |
	| 内存     | 堆               | 栈                  |
	| 生命周期 | 对象垃圾回收消失 | 方法弹栈消失        |

## 5. 运算符

- 算术运算符 `+ - * / %`
	- 算术运算符注意事项
		1. 整数相除要得到小数必须要有浮点数参与运算
		2. 算术表达式中包含多个基本数据类型的时候，整个算术表达式的类型会自动进行类型提升
		3. 提升规则：byte，short，char被提升为int类型、整个表达式自动提升到表达式中最高等级操作数同样的类型
		4. 等级顺序：byte,short,char -> int -> long -> float -> double

  - 使用示例
	```java
	+ - * / %(取余);
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

  - 字符“+”操作
	```java
	int i = 10;
	char c = 'A';  // 65
	char c1 = 'a';  // 97
	char c2 = '0';  // 48
	int i1 = i + c; // 字符与int相加按ASSIC码数值计算，结果为：75
	```
	
  - 字符串的“+”操作

    ```java
    String s1 = "Hello";
    String s2 = "World";
    String s3 = s1 + s2;  // s3: HelloWorld
    String s4 = 99 + 1 + s1;  // 100Hello
    String s5 = s1 + 99 + 1;  // Hello991
    ```

- 赋值运算符

  - 赋值运算符

	```java
	= += -= *= /= %=
	```

    ```java
    // 注意
    short s = 10;
    s += 20;  // 编译通过（原因：扩展的赋值运算符隐含了强制类型转换 s = (short) (s + 20);）
    s = s + 20;  // 编译错误 (原因：short类型不能自动转换为int类型)
    ```

- 自增自减运算符

  - 自增自减

    ```java
    ++ --
    ```

  - 使用

    ```java
    int i = 0;
    i++;  // i为1 与 ++i效果相同
    int j = i++;  // i为2，j为1
    int k = ++i;  // i为3，k为3
    ```

- 关系运算符

	```java
	== != > >= < <=
	```

- 逻辑运算符

  - 逻辑运算符

    ```java
    & | ! ^(异或：不同为true，相同为false)
    ```

  - 短路逻辑运算符

    ```java
    && ||
    ```

- 三元运算符

  ```java
  关系表达式 ? 表达式1 : 表达式2;  // 如果关系表达式为true，结果取表达式1，否则取表达式2
  int max = a > b ? a : b;
  ```

## 6. 字符串

## 7. 输入输出


- 数据输入
	```java
	import java.util.Scanner;

	Scanner scanner = new Scanner(System.in);
	int i = scanner.nextInt();
	```

- 数据输出
	```java
	System.out.println();
	```

## 8. 控制流程


- 分支结构

  - if
    ```java
    if (关系表达式1) {
      语句体1;
    } else if (关系表达式2) {
      语句体2;
    } else {
      语句体3;
    }
    ```

    ```java
    int a = 10;
    int b = 11;
    if (a == b) {
      System.out.println("a等于b")
    } else {
      System.out.println("a不等于b")
    }
    ```

  - switch

    ```java
    switch (表达式) {
      case 表达式值1:
        语句体1;
        break;
      case 表达式值2:
        语句体2;
        break;
      default:
        语句体n;
        break;
    }
    ```

    ```java
    int week = scanner.nextInt();
    switch (week) {
      case 1:
        System.out.println("星期一");
        break;
      case 2:
        System.out.println("星期二");
        break;
      case 3:
        System.out.println("星期三");
        break;
      case 4:
        System.out.println("星期四");
        break;
      case 5:
        System.out.println("星期五");
        break;
      case 6:
        System.out.println("星期六");
        break;
      case 7:
        System.out.println("星期日");
        break;
      default:
        System.out.println("输入的内容有误");
        break;
    }
    ```

    ```java
    // case穿透
    switch (month) {
      case 3:
      case 4:
      case 5:
        System.out.println("春季");
        break;
      case 6:
      case 7:
      case 8:
        System.out.println("夏季");
        break;
      case 9:
      case 10:
      case 11:
        System.out.println("秋季");
        break;
      case 12:
      case 1:
      case 2:
        System.out.println("冬季");
        break;
      default:
        System.out.println("输入的月份有误");
    }
    ```

- 循环结构
  - for
    ```java
    // 执行顺序为：1 => 2 => 4 => 3 => 243 => 243 ... 直到2不满足条件为止
    for (初始化语句1; 条件判断语句2; 条件控制语句3) {
      循环体语句4;
    }
    ```

    ```java
    // 输出1~5
    for (int i = 0; i < 5; i++) {
      System.out.println(i);
    }
    ```

  - while

    ```java
    初始化语句;
    while (条件判断语句) {
      循环体语句;
      条件控制语句;
    }
    ```

    ```java
    int i = 0;
    while (i > 10) {
      System.out.println(i);
      i++;
    }
    ```

  - do...while

    ```java
    int i = 0;
    do {
      System.out.println(i);
      i++;
    } while (i > 10);
    ```

  - 死循环

    ```java
    for (;;) {
      System.out.println("for 死循环")
    }
    
    while (true) {
      System.out.println("while 死循环")
    }
    
    do {
      System.out.println("do...while 死循环")
    } while (true);
    ```

- 跳转控制语句
  - `continue`：在循环中基于条件控制跳出某次循环体内容的执行，继续下一次循环的执行
  - `break`：在循环体中基于条件控制终止循环体内容的执行，结束当前整个循环

## 9. 大数值

## 10. 数组

- 定义
	```java
	变量类型[] 数组名;
	int[] arr;
	```

- 数组初始化
	```java
	// 动态初始化
	int[] arr1 = new int[3];
	// 静态初始化
	int[] arr2 = {1, 2, 3};
	```

- 数组元素访问
	```java
	int i = arr[0]  // 获取数组第一个元素
	```

- 数组中常见异常
	```java
	数组下标越界异常
	空指针异常
	```

- 数组的遍历
	```java
	int[] arr = {1, 2, 3};
	for (int i = 0; i < arr.length; i++) {
		System.out.println(arr[i]);
	}
	```

- 实例：获取数组最大值
	```java
	int[] arr = {1, 2, 3, 1};
	int max = 0;
	for (int i = 0; i < arr.length; i++) {
		if (arr[i] > max) {
			max = arr[i];
		}
	}
	System.out.println(max)
	```