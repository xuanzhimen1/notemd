## 2020-01-10
### Servlet-api依赖相关
- 问题描述
```bash
启动项目报错 org.apache.catalina.LifecycleException: A child container failed during start
```

- Bug日志
```bash
一月 09, 2020 2:47:01 下午 org.apache.coyote.AbstractProtocol init
信息: Initializing ProtocolHandler ["http-bio-9095"]
一月 09, 2020 2:47:01 下午 org.apache.catalina.core.StandardService startInternal
信息: Starting service Tomcat
一月 09, 2020 2:47:01 下午 org.apache.catalina.core.StandardEngine startInternal
信息: Starting Servlet Engine: Apache Tomcat/7.0.47
一月 09, 2020 2:47:08 下午 org.apache.catalina.core.ContainerBase startInternal
严重: A child container failed during start
java.util.concurrent.ExecutionException: org.apache.catalina.LifecycleException: Failed to start component [StandardEngine[Tomcat].StandardHost[localhost].StandardContext[/policy-center]]
	at java.util.concurrent.FutureTask.report(FutureTask.java:122)
	at java.util.concurrent.FutureTask.get(FutureTask.java:192)
	at org.apache.catalina.core.ContainerBase.startInternal(ContainerBase.java:1123)
	at org.apache.catalina.core.StandardHost.startInternal(StandardHost.java:800)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1559)
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1549)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
Caused by: org.apache.catalina.LifecycleException: Failed to start component [StandardEngine[Tomcat].StandardHost[localhost].StandardContext[/policy-center]]
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:154)
	... 6 more
Caused by: java.lang.LinkageError: loader constraint violation: loader (instance of org/apache/catalina/loader/WebappClassLoader) previously initiated loading for a different type with name "javax/servlet/ServletContext"
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:763)
	at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
	at java.net.URLClassLoader.defineClass(URLClassLoader.java:468)
	at java.net.URLClassLoader.access$100(URLClassLoader.java:74)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:369)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:363)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:362)
	at org.apache.catalina.loader.WebappClassLoader.findClass(WebappClassLoader.java:1191)
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1669)
	at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1547)
	at org.springframework.web.SpringServletContainerInitializer.onStartup(SpringServletContainerInitializer.java:162)
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5423)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	... 6 more
一月 09, 2020 2:47:08 下午 org.apache.catalina.core.ContainerBase startInternal
严重: A child container failed during start
java.util.concurrent.ExecutionException: org.apache.catalina.LifecycleException: Failed to start component [StandardEngine[Tomcat].StandardHost[localhost]]
	at java.util.concurrent.FutureTask.report(FutureTask.java:122)
	at java.util.concurrent.FutureTask.get(FutureTask.java:192)
	at org.apache.catalina.core.ContainerBase.startInternal(ContainerBase.java:1123)
	at org.apache.catalina.core.StandardEngine.startInternal(StandardEngine.java:302)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.core.StandardService.startInternal(StandardService.java:443)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.core.StandardServer.startInternal(StandardServer.java:732)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.startup.Tomcat.start(Tomcat.java:341)
	at org.apache.tomcat.maven.plugin.tomcat7.run.AbstractRunMojo.startContainer(AbstractRunMojo.java:1238)
	at org.apache.tomcat.maven.plugin.tomcat7.run.AbstractRunMojo.execute(AbstractRunMojo.java:592)
	at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:134)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:207)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:153)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:145)
	at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:116)
	at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:80)
	at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build(SingleThreadedBuilder.java:51)
	at org.apache.maven.lifecycle.internal.LifecycleStarter.execute(LifecycleStarter.java:128)
	at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:307)
	at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:193)
	at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:106)
	at org.apache.maven.cli.MavenCli.execute(MavenCli.java:863)
	at org.apache.maven.cli.MavenCli.doMain(MavenCli.java:288)
	at org.apache.maven.cli.MavenCli.main(MavenCli.java:199)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:289)
	at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:229)
	at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:415)
	at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:356)
	at org.codehaus.classworlds.Launcher.main(Launcher.java:47)
Caused by: org.apache.catalina.LifecycleException: Failed to start component [StandardEngine[Tomcat].StandardHost[localhost]]
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:154)
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1559)
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1549)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
Caused by: org.apache.catalina.LifecycleException: A child container failed during start
	at org.apache.catalina.core.ContainerBase.startInternal(ContainerBase.java:1131)
	at org.apache.catalina.core.StandardHost.startInternal(StandardHost.java:800)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	... 6 more
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 13.330 s
[INFO] Finished at: 2020-01-09T14:47:08+08:00
[INFO] Final Memory: 39M/562M
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.tomcat.maven:tomcat7-maven-plugin:2.2:run (default-cli) on project sync-center: Could not start Tomcat: Failed to start component [StandardServer[-1]]: Failed to start component [StandardService[Tomcat]]: Failed to start component [StandardEngine[Tomcat]]: A child container failed during start -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
Process finished with exit code 1
```

- 问题原因
```bash
maven项目依赖的jar包冲突
```

- 解决方案
```xml
# 在pom文件中排除servlet-api的jar包，使用tomcat插件中的servlet-api
# 在项目的pom文件中找到依赖servlet-api的工程再此依赖中排除此servlet-api如下
# 找到哪个项目（jar）依赖了servelt-api  => idea => 右侧MavenProject => 工程的maven右键 => show Dependencies => ctrl+f 查找servlet-api
<dependency>
	<groupId>com.sinosoft</groupId>
	<artifactId>common</artifactId>
	<version>1.0-SNAPSHOT</version>
	<exclusions>
		<exclusion>
			<groupId>org.mortbay.jetty</groupId>
			<artifactId>servlet-api</artifactId>
		</exclusion>
	</exclusions>
</dependency>
```


























