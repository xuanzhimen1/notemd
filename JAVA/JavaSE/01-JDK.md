# JDK
## 1. JDK下载zsangsh〔拼音〕

- [jdk下载](https://www.oracle.com/java/technologies/javase-downloads.html)


## 2. JDK安装

### 2.1 Windows系统下安装JDK

- 安装路径不要包含中文空格
- path环境变量的配置
	```bash
	此电脑右键 => 属性 => 高级系统设置 => 点击高级 => 点击环境变量 => 在系统变量中新建如下
	变量名：JAVA_HOME 变量值：jdk安装路径
	点击系统变量中的path => 点击编辑 => 点击新建如下
	%JAVA_HOME%\bin
	点击新建如下
	%JAVA_HOME%\jre\bin
	```

### 2.2 Ubuntu系统安装JDK

- Ubuntu系统安装JDK
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

	# 问题：bad ELF interpreter: 没有那个文件或目录
	sudo yum install glibc.i686
	```