## 1. JSP

- 概念与本质

  - 既可以指定定义html标签，又可以定义java代码的JavaServerPage
  - 服务器启动时所有jsp页面执行
    - xxx.jsp文件 => 生成xxx_jsp.java(继承自servelet)及对应的class文件
  - jsp请求访问服务器时通过url映射找到通过class文件创建的servlet实例并执行其service方法

  - jsp页面中所有html内容都是通过response获得的outputStream对象写入到响应中的

- JSP中的java代码(jsp脚本)

  ```jsp
  <% code %>   <%--定义servlet的service方法中的代码--%>
  <%! code %>  <%--定义servlet的类成员变量--%>
  <%= code %>  <%--定义servlet的service方法response写入响应体的内容--%>
  ```

- JSP内置对象(jsp脚本中使用)

  | 变量名      | 真实类型            | 作用                                  |
  | ----------- | ------------------- | ------------------------------------- |
  | pageContext | PageContext         | 当前页面共享数据, 获取其它8个内置对象 |
  | request     | HttpServletRequest  | 请求对象,一次请求访问多个资源(转发)   |
  | session     | HttpSession         | 一次会话的多个请求间                  |
  | application | ServletContext      | 所有用户间共享数据                    |
  | response    | HttpServletResponse | 响应对象                              |
  | page        | Object              | 当前页面(Servlet)的对象,this          |
  | out         | JspWriter           | 输出对象,数据输出到页面上             |
  | config      | ServletConfig       | Servlet的配置对象                     |
  | exception   | Throwable           | 异常对象                              |

  > out: 字符输出流对象  response.getWriter()数据输出永远在out.write()之前写入响应体

- JSP指令

  - 作用：用于配置JSP页面，导入资源文件

  - 格式 `<%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ... %>`

  - 常用JSP指令

    ```
    <%--配置jsp页面响应信息属性 响应体的mime类型,当前页面字符集编码,缓冲区大小--%>
    <%@ page contentType="text/html;charset=UTF-8" pageEncoding="UTF-8" buffer="16kb"%>
        
    <%--导入java中的包--%>
    <%@ page import="java.util.ArrayList" %>
        
    <%-- 当前页面发生异常后，会自动跳转到指定的错误页面 如下错误时跳转到500.jsp页面 --%>
    <%@ page errorPage="500.jsp" %>
        
    <%-- 标识当前也是是否是错误页面  true: 可以使用内置对象exception  false反之 --%>
    <%@ page isErrorPage="true" %>
    
    <%-- 页面包含, 导入页面的资源文件--%>
    <%@include file="top.jsp"%>
    
    <%-- 导入资源(标签库)--%>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    ```



## 2. EL表达式

- 作用：替换和简化jsp页面中java代码的编写

  > `<%@ page isELIgnored="true" %>` 忽略当前jsp页面中所有的el表达式
  >
  > `\${表达式} ` 忽略当前这个el表达式

- 语法：${表达式}

- 应用

  - 运算 

    - 算术.比较.逻辑: `+, -, *, /, %, >, >=, < , <=, ==, !=, &&, ||, !`
    - 空运算: `${empty list}` list是否为空,  `${not empty list}`:list是否不为空

  - 获取值: 从域对象中获取值

    ```java
    // 获取域中的数据
    ${pageScope.username}
    ${requestScope.username}
    ${sessionScope.username}
    ${applicationScope.username}
    
    // 获取对象,list,map的值
    ${requestScope.student.username}  // 获取对象属性
    ${requestScope.stuList[0]}  // 获取List索引对象的值
    ${requestScope.stuMap.key1}  // 获取键为key1的value
    ${requestScope.stuMap["key1"]}  // 同上
    ```

  - jsp的隐式对象

    ```java
    // pageContext：获取jsp其他八个内置对象
    ${pageContext.request.contextPath}  // 动态获取虚拟目录
    ```



## 3. JSTL

- 概念

  - JavaServer Pages Tag Library  JSP标准标签库

- 使用

  ```
  <%--导入jstl相关jar包-->
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  
  <%-- if标签 -->
  <c:if test="${not empty list}">
  	遍历集合
  </c:if>
  <c:if test="${number % 2 != 0}">
  	${number}为奇数
  </c:if>
  
  <%-- choose标签 -->
  <c:choose>
      <c:when test="${number == 1}">星期一</c:when>
      <c:when test="${number == 2}">星期二</c:when>
      <c:when test="${number == 3}">星期三</c:when>
      <c:when test="${number == 4}">星期四</c:when>
      <c:when test="${number == 5}">星期五</c:when>
      <c:when test="${number == 6}">星期六</c:when>
      <c:when test="${number == 7}">星期天</c:when>
  	<c:otherwise>数字输入有误</c:otherwise>
  </c:choose>
  
  <%--foreach完成特定次数任务  var与varStatus.index初始值相同且按步长递增, varStatus.count为次数 --%>
  <c:forEach begin="1" end="10" var="i" step="2" varStatus="s">
      ${i} --- ${s.index} --- ${s.count}
  </c:forEach>
  
  <%-- foreach遍历容器  s.index(取值参照上面, 默认0开始), s.count为循环次数从1开始  --%>
  <c:forEach items="${list}" var="str" varStatus="s">
  	${s.index} --- ${s.count} --- ${str}
  </c:forEach>
  ```