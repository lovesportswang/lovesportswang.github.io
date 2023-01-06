

# 1.http

##### 超文本传输协议，是一种规则，规定了浏览器和服务器之前的数据传输规则

http协议的特点:

1.基于tcp协议：面向连接，安全

2.基于请求-响应模型的，	一次请求对应一次响应

3.HTTP协议是无状态的协议:对于事务处理没有记忆能力。每次请求-响应都是独立的。缺点:多次请求间不能共享数据。Java中使用会话技术(Cookie、Session）来解决这个问题.优点:速度快

# 2.tomcat配置

#### 项目要部署的两种方式：

1.直接将项目的war包放到webapps

2.在config/server.xml中<host>标记下配置<Context path="项目名" docBase="资源路径" />



# 3.jsp 元素：

##### 1.html

##### 2.<%%> 小脚本

##### 3.<%=%> 表达式

##### 4.<%! %> 声明

##### 5.<jsp: /> 动作标记

##### 6.<%@page %>   <%@include %> 指令

##### 7.注释

# 4.jsp内置对象

九大内置对象

request 、response 、out

session 、application

pageContext、 page、config、 exception

作用域对象：设置属性serAttribute ("key","val") ，获取属性getAttribute("key")



# 5.post & get请求区别

###### 1.传参方式不同：get 是地址栏传参 post 是隐式的请求体方式提交传参

###### 2.安全性：get 不安全 ;post 安全 

###### 3.**传输数据的大小**：

get一般传输数据大小不超过2k-4k

post请求传输数据的大小根据php.ini 配置文件设定，也可以无限大。

**4、缓存性：**

get请求是可以缓存的

post请求不可以缓存

**5、后退页面的反应**

get请求页面后退时，不产生影响

post请求页面后退时，会重新提交请求、



# 6.重定向和转发

![重定向和转发](../java/javaweb/images/重定向和转发.png)

# 7.作用域对象的区别

###### 1、pageContext对象的范围只适用于当前页面范围，即超过这个页面就不能够使用了。所以使用pageContext对象向其它页面传递参数是不可能的。

###### 2、request对象的范围是指在一JSP网页发出请求到另一个JSP网页之间，随后这个属性就失效。

###### 3、session的作用范围为一段用户持续和服务器所连接的时间，但与服务器断线后，这个属性就无效。比如断网或者关闭浏览器。

###### 4、application的范围在服务器一开始执行服务，到服务器关闭为止。它的范围最大，生存周期最长



# 8.状态码

**404：找不到资源或url错误或该资源不存在**

**200：访问成功**

**500：服务器端应用程序异常**

**405：确实dopost或doget方法**

**400：请求url异常**



# 9.Servlet 

**server Applet**

 称为小服务程序或**服务连接器**，用Java编写的**服务器端程序**

定义一个HttpServlet

自定义一个类去实现**HttpServlet** ()         **HttpServlet是[Servlet](https://so.csdn.net/so/search?q=Servlet&spm=1001.2101.3001.7020)接口的一个实现类，并且它是一个抽象类**

HttpServlet的实现由两种方式：

1.xml配置实现

```xml
<servlet>
  <servlet-name>demo1</servlet-name>
  <servlet-class>cn.wang.servlet.ServletDemo1</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>demo1</servlet-name>
  <url-pattern>/demo1</url-pattern>
</servlet-mapping>
```

2.注解配置

**@WebServlet（urlPatterns = "/demo1"）**



# 10.Servlet生命周期

**Servlet 从被创建到使用，再到最后被销毁整个生命周期，都是由容器（eg:tomcat）来决定的**

## init( ),service( ),destroy( )是Servlet生命周期的方法

1.**init( ),当Servlet第一次被请求时，Servlet容器就会开始调用这个方法来初始化一个Servlet对象出来，但是这个方法在后续请求中不会在被Servlet容器调用**,  **Servlet容器会传入一个ServletConfig对象进来从而对Servlet对象进行初始化**

**2.service( )方法，每当请求Servlet时，Servlet容器就会调用这个方法。**

**3.destory,当要销毁Servlet时，Servlet容器就会调用这个方法**



# 11.service() 和 doGet()及doPost()关系

1. **service（）方法，如果Servlet中有service（）方法，则优先调用Service()方法。**

2. **doGet()，在没有service()的情况下，如果是get请求，则调用get请求处理的方法。**

3. **doPost（），在没有service()，如果是post请求，则调用post请求处理的方法。**

  **总结：即每次会先调用Service（）方法，在service()方法中获取本次请求的的method,判断是否是get还是post**
    **再调用doGet和doPost**

  

# 12.init()初始化

**在Servlet实例化之后，Servlet容器会调用init()方法，来初始化该对象，主要是为了让Servlet对象在处理客户请求前可以完成一些初始化的工作，例如，建立数据库的连接，获取配置信息等（`config.getInitParameter("initCount");`）。对于每一个Servlet实例，init()方法只能被调用一次。**



#  13.如何获取application对象

  **a.request.getServletContext()**

  **b.在Servlet中调用 this.getServletContext()**



#  14.页面显示乱码

###   乱码原因：由于在解析过程中默认使用的编码方式为 ISO-8859-1（此编码不支持中文），所以解析时会出现乱码。



​		设置响应的文本类型及编码方式

​      resp.setContentType("text/html;charset=UTF-8"); /

​      //获取writer字符输出流

​       PrintWriter writer= resp.getWriter();

#  15.Servlet实例化和初始化时间？

  默认情况下，**第一次被访问时**才实例化和初始化



  自定义配置Servlet加载实例化初始化的时机和顺序：

```java
@WebServlet(urlPatterns = "/demo2",loadOnStartup = 1)//服务器一启动的时候就创建好servlet
```

```java
loadOnStartup = -1（默认值为-1）   	负整数： servlet第一次被访问时创建Servlet 对象
*                                  0或正整数：{服务器一启动的时候就创建好servlet}
```

#  16.session创建

获取Session 

方式一：

**HttpSession session = request.getSession(true);**

**当params为true时，先查看请求当中是否有sessionId,如果没有**
**sessionId,则创建一个session对象；如果有sessionId,则**
**依据该sessionId去查找对应的session对象，如果找到了，则**
**返回该session对象,**  **如果没有找到，则创建一个新的session对象**



方式二：

**HttpSession session = request.getSession(false);**

**当flag为false，先查看请求当中是否有sessionId,如果没有，**
**返回null;如果有sessionId,则依据该sessionId去查找对应的**
**session对象，如果找到了，则返回该session对象，**

**如果没有找到,返回null**

​	

#### 使用session进行状态管理

**a.将数据绑订到session对象上:**
	**session.setAttribute(String name,Object obj);**
**b.依据绑订名获得绑订值:**
	**Object session.getAttribute(String name);**
**c.解除绑订:**
	**session.removeAttribute(String name);**



#### 销毁Session

**session.invalidate();**



# 17.el表达式

### page和pageContext的区别

page代表JSP网页本身

pageContext，该对象代表该JSP 页面上下文，使用该对象可以访问页面中的共享数据。

总的来说，pageContext和page都是jsp中的隐含对象，pageContext代表jsp页面的上下文关系，能够调用、存取其他隐含对象；page代表处理当前请求的时候，这个页面的实现类的实例。



**1、EL算术运算五个：** 

```
+ 、- 、 * 、 /或div 、 %或mod(取余)
```

**2、EL关系运算符六个**

<table border="1" cellpadding="0" cellspacing="0"><tbody><tr><td><p>关系运算符</p></td><td><p>说明</p></td><td><p>范例</p></td><td><p>结果</p></td></tr><tr><td><p>== 或 eq</p></td><td><p>等于</p></td><td><p>${5==5}或${5eq5}</p></td><td><p>true</p></td></tr><tr><td><p>!= 或 ne</p></td><td><p>不等于</p></td><td><p>${5!=5}或${5ne5}</p></td><td><p>false</p></td></tr><tr><td><p>&lt; 或 lt</p></td><td><p>小于</p></td><td><p>${3&lt;5}或${3lt5}</p></td><td><p>true</p></td></tr><tr><td><p>&gt; 或 gt</p></td><td><p>大于</p></td><td><p>${3&gt;5}或{3gt5}</p></td><td><p>false</p></td></tr><tr><td><p>&lt;= 或 le</p></td><td><p>小于等于</p></td><td><p>${3&lt;=5}或${3le5}</p></td><td><p>true</p></td></tr><tr><td><p>&gt;= 或 ge</p></td><td><p>大于等于</p></td><td><p>${3&gt;=5}或${3ge5}</p></td><td><p>false</p></td></tr></tbody></table>

**3、EL逻辑运算符三个**

<table border="1" cellpadding="0" cellspacing="0"><tbody><tr><td><p>逻辑运算符</p></td><td><p>范例</p></td><td><p>结果</p></td></tr><tr><td><p>&amp;&amp;或and</p></td><td><p>交集${A &amp;&amp; B}或${A and B}</p></td><td><p>true/false</p></td></tr><tr><td><p>||或or</p></td><td><p>并集${A || B}或${A or B}</p></td><td><p>true/false</p></td></tr><tr><td><p>!或not</p></td><td><p>非${! A }或${not A}</p></td><td><p>true/false</p></td></tr></tbody></table>

**4、** **获取域对象的内容**

**${key}  等价于 从小到大在所有域对象中查找 ，如果找到则返回，找不到则不显示**

### 注意：

```java
 //        request.getAttribute("num1");  是从服务器端的域对象中获取参数  		等价${num1}
//        		request.getParameter("num1");    是从客户端的请求参数中获取   等价${param.num2}
```



# 18.JSTL

> **1.导入jsp库  
> <%@ taglib prefix=”c” uri=”http://java.sun.com/jsp/jstl/core”%>**

1.`<c:out>`标签

```
<c:out value="<c:out> Tag"/> 把value所包含的字符串输出<符号会自动转义。
<c:out value="${account}" default="none"/>默认值输出
```

2.< c:set>标签

```
<c: set value="wang" var = "name" ></c:set>  等价于  setAttribute("name","wang")
```

2.`<c:forEach>` var属性定义一个键 键对应循环体的每个值，var属性其实就是一个缓冲引用  

```
 <c:forEach var="i" begin="1" end="10" step="2">
 <LI>i = ${i}</LI>
 </c:forEach>
<% 
java.util.List list = new java.util.ArrayList();
list.add("One");
list.add("Two");
list.add("Three");
list.add("Four");
list.add("Five");
request.setAttribute("list", list);
%>
<UL>
 <c:forEach var="item" items="${list}">
 <LI>${item}</LI>
 </c:forEach>
</UL>
```

3.`<c:if>`  
由test来测试

```
 <c:if test="${i > 3}">
  (greater than 3)
  </c:if>
```

# 19.过滤器

>* 过滤器可以过滤请求
>  * 比如：在用户未登录之前，不让用户访问需要登录才能看到的资源
>  * 比如：用户发送请求时，统一设置字符集 ，解决中文乱码问题
>  * **过滤器可以获取请求参数，可以对请求进行判断和处理**
>  * 符合条件就放行
>  * 不符合条件可以重定向到其他资源
>
>* 过滤器可以过滤响应
>
>  * 放行请求后，可以获取响应信息 可以对响应信息进行判断和处理
>
>    

### 配置过滤器

> **有两种方式：分别是使用 配置文件(web.xml) 和 使用注解**
>
> * web.xml
>
> ```xml
> <filter>
>   <filter-name>filter1</filter-name>
>   <filter-class>cn.wang.filter.FilterDemo1</filter-class>
> </filter>
> <filter-mapping>
>   <filter-name>filter1</filter-name>
>   <url-pattern>/*</url-pattern>
> </filter-mapping>
> ```

* 注解

  @WebFilter(filterName = "charsetFilter", urlPatterns = "/*")

### 过滤器的实现

1.  写一个普通类,实现filter接口
2.  重写接口中的抽象方法(指定拦截规则)
3.  在web.xml文件中注册过滤器或使用注解
4.  过滤器执行顺序问题

*   使用配置文件
    *   先配置的先执行，后配置的后执行
*   使用注解
    *   根据filterName的字符顺序排序

### 过滤器的生命周期

1.  构造方法 web容器启动 1次
2.  init方法 对象创建后 1次
3.  doFilter方法 请求被拦截后 N次
4.  destroy方法 web容器关闭 1次

# 20.监听器

>**监听器是用来监听你的web应用，监听许多信息的初始化、销毁、增加、修改、删除等等**
>
>* 对监听器的划分
>  *   按监听的对象划分，监听器可以分为：ServletContext对象监听器、HttpSession对象监听器、ServletRequest对象监听器。
>  *  安监听的事件划分，监听器可以分为：对象自身的创建和销毁的监听器、对象中属性的创建和消除的监听器、session中的某个对象的状态变化的监听器。
>* 注册监听器
>
>```xml
>
>#通过注解的方式注册监听器
>@WebListener
>
>#在web.xml中注册监听器   
><listener>
><listener-class>com.mrlang.listener.MyContextListener</listener-class>
></listener>
>
>```

	2、上下文监听ServletContext、Session监听、session中属性监听

```java
//上下文监听ServletContext
public class MyContextListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        //获取到上下文对象
        ServletContext application = servletContextEvent.getServletContext();
        System.out.println("上下文初始化"+application);
    }

    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        System.out.println("上下文销毁");
    }
}

//session监听
public class MySessionListener implements HttpSessionListener {
    @Override
    public void sessionCreated(HttpSessionEvent httpSessionEvent) {
        System.out.println("session创建"+httpSessionEvent.getSession().getId());
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent httpSessionEvent) {
        System.out.println("session销毁"+httpSessionEvent.getSession());
    }
}

//session中属性监听
public class MySessionAttributeListener implements HttpSessionAttributeListener {
    @Override
    public void attributeAdded(HttpSessionBindingEvent httpSessionBindingEvent) {
        System.out.println("session属性添加："+httpSessionBindingEvent.getName());
    }

    @Override
    public void attributeRemoved(HttpSessionBindingEvent httpSessionBindingEvent) {
        System.out.println("session属性移除："+httpSessionBindingEvent.getName());
    }

    @Override
    public void attributeReplaced(HttpSessionBindingEvent httpSessionBindingEvent) {
        System.out.println("session属性替换："+httpSessionBindingEvent.getName());
    }
}
```



**三、监听器的应用**

在线人数的统计：

1、先获取容器中的数量计数器，如果没有，创建后存进去

2、每创建一个session，计数器加1

3、每销毁一个session，计数器减1

```java
//应用程序、网站监听器：统计在线人数
@WebListener
public class OnlineListener implements ServletContextListener, HttpSessionListener {
    ServletContext appliction;
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        //获取上下文对象
        appliction = servletContextEvent.getServletContext();
        //是否有计数器
        if(appliction.getAttribute("count")==null){
            //存进去，只执行一次
            appliction.setAttribute("count",0);
        }
    }

    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
    }

    @Override
    public void sessionCreated(HttpSessionEvent httpSessionEvent) {
        //获取原来的计数器的值
        Integer count=Integer.valueOf(appliction.getAttribute("count").toString());
        count++;
        //存进去
        appliction.setAttribute("count",count);
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent httpSessionEvent) {
        Integer count=Integer.valueOf(appliction.getAttribute("count").toString());
        count--;
        appliction.setAttribute("count",count);
    }
}

```

# 21.Cookie & Session

>cookie就是存储在用户本地终端上的数据，也可以说**cookie就是表示存储数据的一种格式。**
>
>那我先说说cooike出现的原因吧：
>
>> 在网站中**，HTTP请求是无状态的**，也就是说即使第一次与服务器连接后并且用户登录成功后，第二次访问访问服务器时，服务器依然不知道是哪个用户发过来的请求，而**cookie的请求就会为了解决这一问题**，当用户第一次登录时服务器会给浏览器返回一个cookie，浏览器将此保存在本地，当用户第二次发出请求时，就会自动的把上次存储的cookie发送给服务器，服务器通过浏览器携带的数据来判断用户。
>
>### cookie的机制：
>
>> cookie是由服务端生成的，发送给客户端的特殊信息(cookie信息存放在http的响应头中)，当客户端收到服务端发送来的信息时，浏览器将会把cookie的(key/value)保存到本地的一个文件当中，而客户端每次向服务端发送请求时都会带上这些特殊的信息。
>
>这里要强调一点
>
>**cookie是由服务端产生的，而是保存在客户端的**
>
>### cookie的主要用途：
>
>> 1.cookie的最典型的用途是判定用户是否已经登陆过此网站，或者是提示用户是否下次登录时保存某些信息以便于下次登录时方便使用。
>>
>> 2.类似于"购物车"类型，用户在一段时间内在一家网站的不同页面选择不同的商品，这些信息都会保存在cookie中，以便于付款时使用。
>
>### cookie的生命周期：
>
>> cookie的生命的周期是可以自己设置的如果没有去设置cookie的生命周期的话一般浏览器会默认为浏览器的会话期间，浏览器关闭cookie以将消失
>>
>
>### cookie的功能特点：
>
>> cookie必须在HTML文件的内容输出之前设置在客户端，一个浏览器能创建的cookie数不能超过300个，并且每个不能超过4kb，每个web站点能设置的cookie总数不能超过20个。
>
>session:
>--------
>
>session称为“会话控制”和cookie的作用有点类似，都是为了存储用户的相关信息。也可以说是一种存储方式
>
>**session 是基于cookie实现的**
>
>### session的机制：
>
>> 一般来说有session的地方一定有cookie，用户通过浏览器讲信息发送到服务器中，当拿到用户的id时，并不是马上的将这些信息存在服务器当中，而是经过加密以后将这些信息存到session中，并且随机产生一个唯一的session\_id,然后将session\_id返回到浏览器，而这session\_id将会保存在cookie当中。下一次浏览器访问服务器是浏览器会吧cookie信息发送到服务器，然后拿到cookie以后从cookie中找到session\_id,通过session\_id找到对应的session信息。这样可以达到识别用户的功能。
>
>### 使用session的好处：
>
>> 1.敏感的数据不是直接发送给浏览器而是发送一个session\_id,服务器将session\_id和敏感数据映射存储在session中，更加安全
>>
>> 2.session可以设置过期时间，可以保证用户的安全
>
>### cookie与session的区别：
>
>> cookie数据是存放在客户端浏览器上 ，session是存放在服务器上
>>
>> cookie是不安全的，别人可以进行cookie进行cookie诈骗，考虑到安全应该使用session
>>
>> session会保存在服务器上的。当访问量变大时，会占用服务器的性能，考虑到减轻服务器的性能应该使用cookie



