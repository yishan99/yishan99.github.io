---
title: web核心-Servlet
date: 2021-01-31 10:48:36
tags:
- web核心
---
### Servlet

<!--more-->

#### Serevlet执行过程分析

- 通过浏览器发送请求，请求首先到达Tomcat服务器，由服务器解析请求URL，然后在部署的应用列表中找到对应的应用。

- 接下来，在找应用里的web.xml配置文件，在web.xml中找到Servlet的配置，找到后执行service方法，最后由Servlet响应客户浏览器。

#### Servlet编写方式

##### 实现Servlet功能时，可以选择以下三种方式：

- 第一种：实现Servlet接口，接口中的方法必须全部实现。

- 第二种：继承GenericServlet，service方法必须重写，其他方可根据需求，选择性重写。

​	**此种方式是和HTTP协议无关的。**

- 第三种：继承HttpServlet，它是javax.servlet.http包下的一个抽象类，是GenericServlet的子类。

​	<b><font color='red'>如果我们选择继承HttpServlet时，只需要重写doGet和doPost方法，不要覆盖service方法。</font></b>

​	**使用此种方式，表示我们的请求和响应需要和HTTP协议相关。**

#### Servlet使用细节

##### Servlet的生命周期

- init()方法
  - 在Servlet的生命周期中，仅执行一次init()方法，它是在服务器装入Servlet时执行的。
- service()方法
  - 它是Servlet的核心，每当一个客户请求一个HttpServlet对象，该对象的Service()方法就要调用。
  - 而且传递给这个方法一个“请求”（ServletRequest）对象和一个“响应”（ServletResponse）对象作为参数。
- destroy()方法
  - 仅执行一次，在服务器端停止且卸载Servlet时执行该方法。

##### Servlet的线程安全

- **Servlet不是线程安全的：**
  - Servlet类中尽量不要定义成员变量。
  - 同步代码会影响Servlet性能，尽量定义局部变量。

#### Servlet的注意事项

##### 映射Servlet的方式

- **第一种：指名道姓的方式**

  - 此种方式，只有和映射配置一模一样时，Servlet才会接收和响应来自客户端的请求。
  - 例如：映射为：/servletDemo5

- **第二种：/开头+通配符的方式**

  - 此种方式，只要符合目录结构即可，不用考虑结尾是什么。

  - 例如：映射为：/servlet/*

- **第三种：通配符+固定格式结尾**
  - 此种方式，只要符合固定结尾格式即可，其前面的访问URI无须关心（注意协议，主机和端口必须正确）
  - 例如：映射为：*.do
- **三种方式优先级**
  - 三种映射方式的优先级为：第一种>第二种>第三种。

```xml
 <!-- ServletDemo04的配置 -->
  <servlet>
    <servlet-name>ServletDemo04</servlet-name>
    <servlet-class>com.yishan.servlet.ServletDemo04</servlet-class>
  </servlet>

  <servlet-mapping>
    <servlet-name>ServletDemo04</servlet-name>
    <url-pattern>/ServletDemo04</url-pattern>
  </servlet-mapping>

  <!-- ServletDemo04的配置 /开头+通配符的方式 -->
  <servlet>
    <servlet-name>ServletDemo04</servlet-name>
    <servlet-class>com.yishan.servlet.ServletDemo04</servlet-class>
  </servlet>

  <servlet-mapping>
    <servlet-name>ServletDemo04</servlet-name>
    <url-pattern>/ServletDemo/*</url-pattern>
  </servlet-mapping>


  <!-- ServletDemo04的配置 通配符+固定格式结尾 -->
  <servlet>
    <servlet-name>ServletDemo04</servlet-name>
    <servlet-class>com.yishan.servlet.ServletDemo04</servlet-class>
  </servlet>

  <servlet-mapping>
    <servlet-name>ServletDemo04</servlet-name>
    <url-pattern>*.d</url-pattern>
  </servlet-mapping>
```

#### Servlet创建时机

- 第一种：应用加载时创建Servlet
  - 它的优势是在服务器启动时，就把需要的对象都创建完成了，从而在使用的时候减少了创建对象的时间，提高了首次执行的效率。
  - 它的弊端也同样明显，因为在应用加载时就创建了Servlet对象，因此，导致内存中充斥着大量用不上的Servlet对象，造成了内存的浪费。
- 第二种：请求第一次访问是创建Servlet
  - 它的优势就是减少了对服务器内存的浪费，因为那些一直没有被访问过的Servlet对象都没有创建，因此也提高了服务器的启动时间。
  - 而它的弊端就是，如果有一些要在应用加载时就做的初始化操作，它都没法完成，从而要考虑其他技术实现。
- 两种方式使用场景
  - 就是当需要在应用加载就要完成一些工作时，就需要选择第一种方式。
  - 当Servlet的使用时机并不确定是，就选择第二种方式。

- web.xml配置。
  - 正整数代表服务器加载时创建，值越小优先级越高。
  - 负整数或不写load-on-startup标签代表第一次访问时创建。

```xml
<load-on-startup>1</load-on-startup>
```

#### 默认Servlet

- 默认Servlet是由服务器提供的一个Servlet，它配置在Tomcat的conf目录下的web.xml中。

- 它的映射路径是<b><font color='red'>`<url-pattern>/<url-pattern>`</font></b>

- 我们在发送请求时，首先会在我们应用中的web.xml中查找映射配置，找到就执行。但是当找不到对应的Servlet路径时，就去找默认的Servlet，由默认Servlet处理。

#### ServletConfig

- 它是Servlet的配置参数对象，在Servlet规范中，允许为每个Servlet都提供一些初始化配置

作用：

- 在Servlet初始化期间，把一些配置信息传递给Servlet。
- 每一个Servlet都有自己的ServletConfig

##### 生命周期

- 在初始化阶段读取了web.xml中为Servlet准备的初始化配置，并把配置信息传递给Servlet，所以生命周期与Servlet相同。
- 这里需要注意的是，如果Servlet配置了`<load-on-startup>1</load-on-startup>`，那么ServletConfig也会在应用加载时创建。

``` xml
  <!-- ServletConfigDemo01的配置 -->
  <servlet>
    <servlet-name>ServletConfigDemo01</servlet-name>
    <servlet-class>com.yishan.servlet.ServletConfigDemo01</servlet-class>

    <!--配置初始化参数-->
    <init-param>
      <!--用于获取初始化参数的key-->
      <param-name>encoding</param-name>
      <!--初始化参数的值-->
      <param-value>UTF-8</param-value>
    </init-param>

    <!--每个初始化参数都需要用到init-param标签-->
    <init-param>
      <param-name>desc</param-name>
      <param-value>This is ServletConfigDemo01</param-value>
    </init-param>
  </servlet>

  <servlet-mapping>
    <servlet-name>ServletConfigDemo01</servlet-name>
    <url-pattern>/ServletConfigDemo01</url-pattern>
  </servlet-mapping>

```

##### 常用方法

| **返回值**          | **方法名**                      | **说明**                 |
| ------------------- | ------------------------------- | ------------------------ |
| String              | getInitParameter(String   name) | 根据参数名称获取参数的值 |
| Enumeration<String> | getInitParameterNames()         | 获取所有参数名称的枚举   |
| String              | getServletName()                | 获取Servlet的名称        |
| ServletContext      | getServletContext()             | 获取ServletContext对象   |

#### ServletContext

##### 配置

- ServletContext既然被称之为应用上下文对象，所以它的配置是针对整个应用的配置。

  配置的方式，需要在`<web-app>`标签中使用`<context-param>`来配置初始化参数。

```xml
<!-- ServletContextDemo01的配置 -->

  <servlet>
    <servlet-name>ServletContextDemo01</servlet-name>
    <servlet-class>com.yishan.servlet.ServletContextDemo01</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>ServletContextDemo01</servlet-name>
    <url-pattern>/ServletContextDemo01</url-pattern>
  </servlet-mapping>

  <!-- ServletContext的配置 -->

  <context-param>
    <param-name>globalEncoding</param-name>
    <param-value>UTF-8</param-value>
  </context-param>
  <context-param>
    <param-name>globalDesc</param-name>
    <param-value>This is ServletContextDemo01</param-value>
  </context-param>
```

##### 常用方法

| **返回值**          | **方法名**                                 | **说明**                               |
| ------------------- | ------------------------------------------ | -------------------------------------- |
| void                | setAttribute(String   name,Object   value) | 向域对象中存储数据                     |
| Object              | getAttribute(String   name)                | 通过名称获取域对象中的数据             |
| Enumeration<String> | getAttributeNames()                        | 获取域对象中所有数据的名称             |
| void                | removeAttribute(String   name)             | 通过名称移除域对象中的数据             |
| String              | getServletContextName()                    | 获取ServletContext的名称               |
| String              | getContextPath()                           | 获取当前应用的访问虚拟目录             |
| String              | getRealPath(String   path)                 | 根据虚拟目录获取应用部署的磁盘绝对路径 |
| String              | getServerInfo()                            | 获取服务器的名称和版本信息             |
| String              | getInitParameter(String   name)            | 根据名称获取全局配置的值               |
| Enumeration<String> | getInitParamterNames()                     | 获取全局配置的所有名称                 |

#### 注解

```java
@@WebServlet("/ServletDemo01")
```

##### 注意

- 括号内的虚拟访问路径必须加   **/**    ,还有双引号