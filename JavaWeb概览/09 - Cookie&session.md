# 09 - Cookie&session

[toc]

## 1. Cookie

- Cookie是服务器通知客户端保存键值对的一种技术。客户端有了Cookie后，每次请求，Cookie都会发送给服务器。每个Cookie得到大小不能超过4KB

### 1.1 创建Cookie对象

```java
protected void createCookie(HttpServletRequest req,HttpServletResponse resp) throws ServletException,IOException{
    //1.创建Cookie对象
    Cookie cookie = new Cookie("key2","value2");
    //2.通知客户端保存Cookie
    resp.addCookie(cookie);
}
```

### 1.2 获取Cookie对象

```java
protected void getCookie(HttpServletRequest req,HttpServletResponse resp) throws ServletException,IOException{
    
    Cookie[] cookies = req.getCookies();
    
    for(Cookie cookie : cookies){
        //getName方法返回Cookie的key
        //getValue方法返回Cookie的value值
        resp.getWrite().write("Cookie[" + cookie.getName() + "=" + cookie.getValue() + "]");
    }
}
```

### 1.3 修改Cookie对象

```java
protected void changeCookie(HttpServletRequest req,HttpServletResponse resp) throws ServletException,IOException{
    
    //1.找到需要修改的Cookie对象
    Cookie cookie = new Cookie("key1","value1");
    //2.调用setValue()方法赋予新的Cookie值
    cookie.setValue("newValue1");
    //3.调用response.addCookie()通知客户端保存修改
    resp.addCookie(cookie);
    
}
```

### 1.4 Cookie生命周期

- Cookie的生命周期指的是如何管理Cookie什么时候被销毁(删除)
- 可以通过setMaxAge()设置Cookie的生命周期：
  - 正数，表示在指定的秒数后过期
  - 负数，表示游览器关闭后，Cookie会被立刻删除（默认）
  - 0，表示立刻删除Cookie

### 1.5 Cookie有效路径Path的设置

- Cookie的path属性可以通过请求的地址，有效的过滤哪些Cookie可以发送给服务器。

  例如：

  `CookieA path=/工程路径`

  `CookieB path=/工程路径/abc`

  - 请求地址；`http://ip:port/工程路径/a.html`
    - CookieA 发送
    - CookieB 不发送
  - 请求地址：`http://ip:port/工程路径/abc/a.html`
    - CookieA 发送
    - CookieB 发送

  ```java
  protected void testPath(HttpServletRequest req,HttpServletResponse resp) throws ServletException,IOException{
      
      Cookie cookie = new Cookie("path1","path1");
      //getContextPath() 得到工程路径
      cookie.setPath(req.getContextPath() + "/abc");
      resp.addCookie(cookie);
  }
  ```

  

## 2. Session

- Session对话，是一个接口，是用来维护客户端和服务器之间关联的一种技术。每个客户端都有自己的一个Session会话。Session会话常用来保存用户登录之后的信息
- Session技术，底层其实是基于Cookie技术来实现的

### 2.1 Session的创建与获取

- 每个Session会话都有一个唯一的ID值，`getID()`可以得到Session会话的ID值

- 创建与获取Session的API是一样的，都是`request.getSession()`

  第一次调用会创建Session会话，之后调用都会获取前面创建好的Session会话对象

- `isNew()`可以用于判断指定Session是否为新建的，返回true表示是新建的，false表示不是

  ```java
  protected void createOrGetSession(HttpServletRequest req,HttpServletResponse resp) throws ServletException,IOException{
      //创建和获取Session会话对象
      HttpSession session = req.getSession();
      //判断当前Session会话，是否是新建出来的
      boolean isNew = session.isNew();
      //获取Session会话的唯一标识id
      String id = session.getId();
      
      resp.getWriter().write("得到的Session,它的id是:" + id);
      resp.getWriter().write("这个Session是否是新创建的:" + isNew);
      
  }
  ```

### 2.2 Session的生命周期

- session的超时指的是，客户端两次请求的最大间隔时长

- `setMaxInactiveInterval(int interval)`：设置Session的超时时间

  - 值为正数的时候，设定Session的超时时长
  - 负数标识永不超时（极少使用）
  - 如果要立即销毁Session可以使用`invalidate()`

  `getMaxInactiveInterval()`：获取Session的超时时间

  Session会话的默认超时时间为30分钟