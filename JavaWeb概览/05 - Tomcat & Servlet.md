# 05 - Tomcat & Servlet

[toc]

## Tomcat

### 1. 基本概念

- JavaWeb是所有通过Java语言编写，可以通过游览器访问的程序的总称。

  JavaWeb是基于请求和响应来开发的。请求和响应是成对出现的：

  - 请求(Request)是指客户端给服务器发送数据
  - 响应(Response)是指服务器给客户端回传数据

- Web资源按照实现的技术和呈现的效果不同，又分为静态资源和动态资源：
  - 静态资源：html，css，js，txt，mp4，jpg
  - 动态资源：jsp页面，Servlet程序



### 2. 部署web工程到Tomcat中

- **方法一**

  直接把web工程的目录拷贝到Tomcat的webapps目录下即可

  访问方式：`http://ip:port/工程名/目录下/文件名`

- **方法二**

  找到Tomcat下的cof目录`\Catalina\localhost\`下，创建如下的配置xml文件

  ```xml
  <!--path表示工程的访问路径:/abc docBase表示工程目录-->
  <Context path="/abc" docBase="E:\book" />
  ```

  

## Servlet

### 1. Servlet基础

- Servlet是JavaEE规范之一，规范就是接口

  Servlet是JavaWeb三大组件之一。三大组件分别是：Servlet程序，Filter过滤器，Listener监听器

  Servlet是运行在服务器上的一个java程序，它可以接收客户端发送过来的请求，并响应数据给客户端

#### 1.1 实现Servlet程序

手动实现方法以及详细配置参见[这里](https://www.cnblogs.com/Solitary-Rhyme/p/15579732.html)

常规情况下，只需要右键文件夹，新建Serlet文件，然后按需求重写doGet和doPost方法即可

#### 1.2 url地址如何定位到Servlet程序 

以`http://localhost:8080/HelloServlet/hello`为例，分析链接的组成成分

- `http://`表示http协议

  `localhost`——通过ip地址定位服务器

  `:8080`是服务器端口号——通过端口号，定位Tomcat

  `/HelloServlet`是工程路径——通过工程路径确定访问哪个工程

  `/hello`资源路径——联系下列代码理解

  ```xml
  <servlet>
  	<servlet-name>HelloServlet</servlet-name>
  	<servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>HelloServlet</servlet-name>
  	<url-pattern>/hello</url-pattern>
  </servlet-mapping>
  ```

  `hello`被设置为`url-pattern`，随后映射到`HelloServlet`。由于`HelloServlet`是`com.atguigu.servlet.HelloServlet`的别名，所以最后资源路径会访问到HelloServlet类

#### 1.3 Servlet的生命周期

1. 执行Servlet构造器方法：在第一次访问，创建Servlet程序时会调用
2. 执行init初始化方法：在第一次访问，创建Servlet程序时会调用
3. 执行service方法：每次访问时都会调用
4. 执行destroy销毁方法：在web工程停止的时候调用



### 2. ServletConfig & ServletContext

#### 2.1 ServletConfig

- ServletConfig是Servlet程序的配置信息类

  ServletConfig类的作用：

  1. 可以获取Servlet程序的别名servlet-name的值
  2. 获取初始化参数init-param
  3. 获取ServletContext对象

- Servlet程序和ServletConfig对象都是由Tomcat负责创建，我们负责使用

  Servlet程序默认是第一次访问的时候创建，ServletConfig是每个Servlet程序创建时，就创建一个对应的ServletConfig对象。也就是说每个Servlet程序都有独立的ServletConfig

- 如果重写了init类，必须要在开头添加super.init()；否则使用Servlet类会报空指针

#### 2.2 ServletContext

- ServletContext是一个接口，它表示Servlet上下文对象。一个web工程，只有一个ServletContext对象实例

  ServletContext对象是一个域对象。域对象是指可以像Map一样存取数据的对象，这里的域指的是存取数据的操作范围（即整个web工程）

  ServletContext在web工程部署启动的时候创建，在web工程停止的时候销毁

- ServletContext类的作用：

  1. 获取web.xml中配置的上下文参数context-param
  2. 获取当前的工程路径
  3. 获取工程**部署在服务器硬盘上的**绝对路径
  4. 像Map一样存取数据



**以下是完整的ServletConfig和ServletContext相关代码展示**

```java
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        ServletConfig servletConfig = getServletConfig();
        //1.可以获取Servlet程序的别名servlet-name的值
        System.out.println("HelloServlet程序的别名是" + servletConfig.getServletName());
        //2.获取初始化参数init-param
        System.out.println("初始化参数username的值是" + servletConfig.getInitParameter("username"));
        System.out.println("初始化参数url的值是" + servletConfig.getInitParameter("url"));
        //3.获取ServletContext对象
        System.out.println(servletConfig.getServletContext());


        //1.获取web.xml中配置的上下文参数context-param
        ServletContext servletContext = getServletContext();
        System.out.println("context-param参数usernames的值是:" + servletContext.getInitParameter("usernames"));
        //2.获取当前的工程路径
        System.out.println("当前工程路径:" + servletContext.getContextPath());
        //3.获取工程**部署在服务器硬盘上的**绝对路径
        System.out.println("工程部署的路径是" + servletContext.getRealPath("/"));
        System.out.println("工程下css目录的绝对路径是:" + servletContext.getRealPath("/css"));
        //4.像Map一样存取数据
        servletContext.setAttribute("key1","value1");
        System.out.println("Context1中获取域数据key1的值是:" + servletContext.getAttribute("key1"));
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--context-param是上下文参数(它属于整个web工程)-->
    <context-param>
        <param-name>usernames</param-name>
        <param-value>roots</param-value>
    </context-param>

    <!--servlet标签给Tomcat配置Servlet程序-->
    <servlet>
        <!--servlet-name标签:给Servlet程序起别名-->
        <servlet-name>HelloServlet</servlet-name>
        <!--servlet-class标签是Servlet程序的全类名-->
        <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>

        <!--init-param是初始化参数-->
        <init-param>
            <!--param-name是参数名-->
            <param-name>username</param-name>
            <!--param-value是参数值-->
            <param-value>root</param-value>
        </init-param>
        <init-param>
            <param-name>url</param-name>
            <param-value>https://www.baidu.com/</param-value>
        </init-param>
    </servlet>

    <!--servlet-mapping标签给servlet程序配置访问地址-->
    <servlet-mapping>
        <!--servlet-name标签的作用是告诉服务器，当前配置的地址给哪个servlet程序使用-->
        <servlet-name>HelloServlet</servlet-name>
        <!--url-pattern标签配置访问地址-->
        <!--/斜杠在服务器解析的时候表示地址为:http://ip:port/工程路径
        在底下这个例子中/hello表示http://ip:port/工程路径/hello-->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```



### 3.HTTP协议响应

- HTTP协议就是指，客户端和服务器之间通信时，发送的数据，需要遵守的规则，叫HTTP协议。HTTP协议中的数据又叫报文。

#### 3.1 GET请求和POST请求HTTP协议

##### GET请求

1. 请求行
   - 请求的方式：GET
   - 请求的资源路径+?+请求参数
   - 请求的协议的版本号:HTTP/1.1
2. 请求头
   - 由键值对组成(key:value)，不同的键值对表示不同的含义

- GET请求一般包含：
  - form标签method=get
  - a标签
  - link标签引入css
  - Script标签引入js文件
  - img标签引入图片
  - iframe引入html页面
  - 在游览器地址栏中输入地址后敲回车

##### POST请求

1. 请求行
   - 请求的方式：GET
   - 请求的资源路径+?+请求参数
   - 请求的协议的版本号:HTTP/1.1
2. 请求头
   - 由键值对组成(key:value)，不同的键值对表示不同的含义
3. 请求体
   - 发送给服务器的数据

- POST请求一般包含：
  - form标签method=post



##### 常用请求头

- **Accept**：表示客户端可以接收的数据类型
- **Accept-Language**：表示客户端可以接收的语言类型
- **User-Agent**：表示客户端游览器的信息
- **Host**：表示请求时的服务器ip和端口号



#### 3.2 响应的HTTP协议格式

1. 响应行
   - 响应的协议和版本号
   - 响应状态码
   - 响应状态描述符
2. 响应头
   - key:value — 不同的响应头，有不同的含义
3. 响应行
   - 回传给客户端的数据

##### 常用响应头

- **Server**：表示服务器的信息
- **Content-Type**：表示响应体的数据类型
- **Content-Length**：响应体的长度
- **Date**：请求响应的事件

##### 常用响应码

- **200**：表示请求成功
- **302**：表示请求重定向
- **404**：表示请求服务器已经收到，但是所要的数据不存在（请求地址错误）
- **500**：表示服务器已经收到请求，但是服务器内部错误（代码错误）



### 4. HttpServletRequest类 & HttpServletResponse类

#### 4.1 HttpServletRequest类

- 每次只要有请求进入Tomcat服务器，Tomcat服务器就会把请求过来的HTTP协议信息解析好封装到Request对象中，然后传递到service方法(doGet和doPost方法)中给我们使用。可以通过HttpServletRequest类对象，获取到所有请求的信息

- **常用API演示**

  ```java
  //1.获取请求的资源路径
  System.out.println("URI -> " + request.getRequestURI());
  //2.获取请求的统一资源定位符(绝对路径)
  System.out.println("URL -> " + request.getRequestURL());
  //3.获取客户端的ip的地址
  System.out.println("客户端 ip地址 -> " + request.getRemoteHost());
  //4.获取请求头
  System.out.println("请求头User-Agent -> " + request.getHeader("User-Agent"));
  //5.获取请求的方式(post或get)
  System.out.println("请求的方式 -> " + request.getMethod());
  
  //获取请求参数
  String username = request.getParameter("username");
  String password = request.getParameter("password");
  //如果要同时接收多个参数
  String hobby = req.getParameterValues("hobby");
  ```

#### 4.2 HttpServletResponse类

- HttpServletResponse类和HttpServletRequest类类似。每次请求传入，Tomcat服务器都会创建一个Response对象传递给Servlet程序使用。HttpServletRequest类表示请求的信息，HttpServletResponse类表示响应的信息。如果需要设置返回给客户端的信息，都可以通过HttpServletResponse对象来设置

- HttpServletResponse使用输出流（字符流或字节流）输出，两个流同时只能使用一个

  ```java
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      PrintWriter writer = resp.getWriter();
      writer.write("response's content!");
  }
  ```

  

### 5. 请求转发

- 请求转发是指，服务器收到请求后，从一次资源跳转到另一个资源的操作叫请求转发

- 请求转发的具体操作，下列包含了两个servlet程序和一个xml文件用于添加servlet程序到服务器

  ```java
  public class Servlet1 extends HttpServlet {
  
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  
          //获取请求的参数
          String username = req.getParameter("username");
          System.out.println("在Servlet1中查看参数" + username);
  
          //将需要传递给Servlet2程序的参数保存
          req.setAttribute("key1","给Servlet2看的值");
  
          //确定请求转发的路径
          RequestDispatcher requestDispatcher = req.getRequestDispatcher("/servlet2");
          //开始转发
          requestDispatcher.forward(req,resp);
      }
  }
  ```

  ```java
  public class Servlet2 extends HttpServlet {
  
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  
          //获取请求的参数
          String username = req.getParameter("username");
          System.out.println("在Servlet2中查看参数" + username);
  
          //获取在Servlet1程序中被保存的值
          Object a1 = req.getAttribute("key1");
          System.out.println("在Servlet1程序中被保存的值" + a1);
      }
  }
  ```

  ```xml
      <servlet>
          <servlet-name>servlet1</servlet-name>
          <servlet-class>com.atguigu.servlet.Servlet1</servlet-class>
      </servlet>
      <servlet>
          <servlet-name>servlet2</servlet-name>
          <servlet-class>com.atguigu.servlet.Servlet2</servlet-class>
      </servlet>
  ```

- **请求转发的特点：**

  - 游览器地址栏没有变化
  - 请求转发是一次请求
  - 请求转发的servlet程序之间共享Request域中的数据
  - 可以转发到WEB-INF目录下（常规游览器访问不行）
  - 不可以访问工程以外的资源

#### 5.1 路径相关补充

- **绝对路径和相对路径**
  - **相对路径**：`.`表示当前目录，`..`表示上一级目录，`[资源名]`表示当前目录/资源名
  - **绝对路径**：`http://ip:port/工程路径/资源路径`

- **web中`/`的不同含义**
  - `/`如果被游览器解析，得到的地址是`http://ip:port/`
    - `<a href="/">斜杠</a>`
  - `/`如果被服务器解析，得到的地址是`http://ip:port/工程路径`
    - `<url-pattern>/servlet1</url-pattern>`
    - `servletContext.getRealPath("/");`
    - `request.getRequestDispatcher("/");`
  - 特殊情况：`response.sendRediect("/");`会把斜杠发送给游览器解析，得到`http://ip:port/`



### 6.请求重定向

- 请求重定向是指，客户端给服务器发送请求后，服务器告知客户端前往新地址访问（因为之前的地址可能已经被废弃）

  ```java
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  	
      //1.方法一
  	//设置响应状态码，表示已重定向
  	resp.setStatus(302);
  	//设置响应头，说明新的地址在哪里
  	resp.setHeader("Location","http://localhost:8080/servlet2/response2");
      
      //2.方法二
      resp.sendRedirect("http://localhost:8080");
  }
  ```

- **请求重定向的特点：**

  1. 游览器地址栏会发生变化
  2. 两次请求
  3. 不共享Request域中数据
  4. 不能访问WEB-INF下的资源
  5. 可以访问工程外的资源

