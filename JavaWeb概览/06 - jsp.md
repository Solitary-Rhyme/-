# 06 - jsp

[toc]

### 1. 基础概念

- JSP(java server pages)，java服务器页面。jsp的主要作用是替代Servlet程序回传html页面的数据，因为Servlet程序回传html页面数据是一件非常繁琐的事情。
- jsp页面本质上是一个Servlet程序。当初次访问jsp页面的时候，Tomcat服务器会将jsp页面翻译成为一个java源文件，并且将它编译为.class字节码程序。跟踪源代码发现，源代码内的HttpJspBase类，直接继承了HttpServlet类。也就是说jsp翻译出来的java类，间接的继承了HttpServlet类，它翻译出来了一个Servlet程序



### 2. jsp头部的page指令

- jsp的page指令可以修改jsp页面中一些重要的属性。

  部分属性设置默认隐藏不会显式声明，本笔记只记录一些重要的属性设置

- 一个正常jsp文件的头部page指令

  ```jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  ```

  1. **language属性**：表示jsp翻译后是什么语言文件，暂时只支持java
  2. **contentType属性**：表示jsp返回的数据类型是什么
  3. **pageEncoding属性**：表示当前jsp页面文件本身的字符集
  4. **import属性**：跟java源代码中一样，用于导包，导类
  5. **errorPage属性**：设置当jsp页面运行时出错，自动跳转去的错误页面路径
  6. **isErrorPage属性**：设置当前jsp页面是否是错误信息页面。默认为false，如果为true可以获取异常信息



### 3. jsp中的常用脚本

#### 3.1 声明脚本

- 声明脚本的格式是：`<%! 声明java代码 %>`

  作用：可以给jsp翻译出来的java类定义属性，方法，内部类等

  ```jsp
  <%--1.声明类属性--%>
  <%!
      private Integer id;
  	private String name;
  	private static Map<String,Object> map;
  %>
  
  <%--2.声明类方法--%>
  <%!
      public int abc(){
      return 12;
  	}
  %>
  ```

  

#### 3.2 表达式脚本

- 声明表达式脚本的格式是；`<%=表达式%>`

  表达式脚本的作用是：在jsp页面上输出数据

  ```jsp
  <%--1.输出整型--%>
  <%=12 %>
  
  <%--2.输出字符串--%>
  <%="字符串" %>
  
  <%--3.输出对象--%>
  <%=map %>
  <%=request.getParameter("username")%>
  ```

- 表达式脚本的特点：

  1. 所有的表达式脚本都会被翻译到_jspService()方法中
  2. 表达式脚本都会被翻译成为out.print()输出到页面上
  3. 由于表达式脚本翻译的内容都在`_jspService()`方法中，所以`_jspService()`方法中的对象都可以直接使用
  4. 表达式脚本中的表达式不能以分号结束

#### 3.3 代码脚本

- 声明代码脚本的格式是：`<% java语句 %>`

  代码脚本的作用是：可以在jsp页面中，编写需要的功能(java语句)

  ```jsp
  <%--for循环语句--%>
  <%
  	for(int j = 0;j < 10;j++){
          System.out.println(j);
      }
  %>
  
  <%--翻译后java文件中_jspService方法内的代码都可以写--%>
  <%
  	String username = request.getParameter("username");
  	System.out.println("用户名的请求参数值是:" + username);
  %>
  ```

- 代码脚本的特点：

  1. 代码脚本翻译之后都在`_jspService()`方法中
  2. 代码脚本由于翻译到`_jspService()`方法中，所以在`_jspService()`方法中的现有对象都可以直接使用
  3. 可以由多个代码脚本块组合完成一个完整的java语句
  4. 代码脚本还可以和表达式脚本组合使用，在jsp页面上输出数据



### 4. jsp九大内置对象

- **九大内置对象**

  **request**：请求对象

  **response**：响应对象

  **pageContext**：jsp的上下文对象

  **session**：会话对象

  **application**：ServletContext对象

  **config**：ServletConfig对象

  **out**：jsp输出流对象

  **page**：指向当前jsp的对象

  **exception**：异常对象

#### 4.1 四大域对象

- 域对象是可以像Map一样存取数据的对象。四个域对象功能一样，但对数据的存取范围不一样

- **四大域对象**：
  - **pageContext**(PageContextImpl类)：当前jsp页面范围内有效
  - **request**(HttpServlet类)：一次请求内有效
  - **session**(HttpSession类)：一个会话范围内有效（直到关闭游览器为止）
  - **application**(ServletContext类)：整个web工程范围内都有效（只要web工程不停止，数据一直存在）
- 四个域对象在使用的时候，优先使用范围小的域对象



#### 4.2 response和out

- response表示响应，常用于设置返回给客户端的内容

  out也同样用于输出给用户信息

- 由于jsp的底层实现关系，当jsp页面中所有代码执行完后会进行如下两个操作：

  1. 执行out.flush()操作，将out缓冲区中的数据追加写入导response缓冲区末尾
  2. 执行response的刷新操作，把全部数据写给客户端

  这两步操作会导致，out输出语句在jsp文件中处于response语句的前面，在输出时却处于response语句的后面。

  为了避免这一错误，建议在jsp页面中统一使用out进行输出，避免打乱页面输出内容的顺序

- **out.write()和out.print()**

  out.write() 可以输出字符串；out.print() 可以输出任意数据（都转换为字符串）

  由于底层实现原因，使用out.write()输出非字符串数值，可能会导致出现乱码。所以建议在jsp页面中输出的时候统一使用out.print()



### 5. jsp中的常用标签

#### 5.1 jsp静态包含

- 有时候为了方便页面的维护，需要将单个页面分割为多个jsp文件。包含就可以做到这一点

- 静态包含格式：`<%@ include file="[路径]">`
- 静态包含的特点：
  1. 静态包含不会翻译被包含的jsp页面
  2. 静态包含起始是把被包含的jsp页面的代码拷贝到包含的位置执行输出

#### 5.2 jsp动态包含

- 动态包含也可以像静态包含一样，把被包含的内容执行输出到包含位置

- 动态包含格式：`<jsp:include page="[动态包含]"></jsp:include>`

- 动态包含的特点：

  1. 动态包含会把包含的jsp页面也翻译为java代码

  2. 动态包含底层代码使用如下代码调用到被包含的jsp页面执行输出

     `JspRuntimeLibrary.include(request,response,"[路径]",out,false);`

  3. 动态包含可以传递参数

#### 5.3 jsp标签-转发

- 转发标签的功能就是请求转发
- 转发标签格式：`<jsp:forward page="[路径]"></jsp:forward>`



### 6. 监听器Listener

- Listener监听器是JavaWeb三大组件之一，是JavaEE的规范，是接口。监听器的作用是，监听某种事物的变化，然后通过回调函数，反馈给客户去做一些相应的处理。

#### 6.1 ServletContextListener监听器

- ServletContextListener可以监听ServletContext对象的创建和销毁。ServletContext对象在web工程启动的时候创建，在web工程停止的时候销毁

- 监听到创建和销毁之后，监听器会分别调用：

  ```java
  //在ServletContext对象创建后马上调用，做初始化
  public void contextInitialized(ServletContextEvent sce);
  
  //在ServletContext对象销毁后之后调用
  public void contextDestroyed(ServletContextEvent sce);
  ```

- 如何使用ServletContextListener监听器监听ServletContext对象

  1. 编写一个类实现ServletContextListener
  2. 实现该类的两个回调方法
  3. 在web.xml中配置监听器

