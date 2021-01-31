title: web核心-request&&response
date: 2021-01-31 10:49:14
tags:

- web核心

### request&&response

<!--more-->

### 请求对象

#### 请求对象的常用方法

| **返回值**   | **方法名**       | **说明**            |
| ------------ | ---------------- | ------------------- |
| String       | getContextPath() | 获取虚拟目录名称    |
| String       | getServletPath() | 获取Servlet映射路径 |
| String       | getRemoteAddr()  | 获取访问者ip地址    |
| String       | getQueryString() | 获取请求的消息数据  |
| String       | getRequestURI()  | 获取统一资源标识符  |
| StringBuffer | getRequestURL()  | 获取统一资源定位符  |

- 资源标识符>资源定位符

```java
package com.yishan.servlet.request;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 获取请求对象的各种信息
 *
 * @Author yishan
 * @Date 2021/1/30 0030 9:40
 * @Version 1.0
 */
@WebServlet("/ServletDemo01")
public class ServletDemo01 extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        //获取虚拟路径名称
        String contextPath = req.getContextPath();
        System.out.println("contextPath = " + contextPath);

        //获取Servlet映射路径名称
        String servletPath = req.getServletPath();
        System.out.println("servletPath = " + servletPath);

        //获取访问者的IP
        String ip = req.getRemoteAddr();
        System.out.println("ip = " + ip);

        //获取请求消息的数据
        String queryString = req.getQueryString();
        System.out.println("queryString = " + queryString);

        //获取统一资源标识符
        String requestURI = req.getRequestURI();
        System.out.println("requestURI = " + requestURI);

        //获取统一资源定位符
        StringBuffer requestURL = req.getRequestURL();
        System.out.println("requestURL = " + requestURL);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        doPost(req, resp);
    }
}
```

#### 获取请求头信息

| **返回值**          | **方法名**                | **说明**                 |
| ------------------- | ------------------------- | ------------------------ |
| String              | getHeader(String   name)  | 根据请求头名称获取一个值 |
| Enumeration<String> | getHeaders(String   name) | 根据请求头名称获取多个值 |
| Enumeration<String> | getHeaderNames()          | 获取所有请求头名称       |

```java
package com.yishan.servlet.request;

import java.io.IOException;
import java.util.Enumeration;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import sun.misc.Request;

/**
 * 获取请求头
 *
 * @Author yishan
 * @Date 2021/1/30 0030 9:40
 * @Version 1.0
 */
@WebServlet("/ServletDemo02")
public class ServletDemo02 extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        //根据名称获取头的值 一个头一个值
        String value = req.getHeader("Accept-Encoding");
        System.out.println(value);

        System.out.println("--------------------");

        //根据名称获取头的值 一个头多个值
        Enumeration<String> values = req.getHeaders("Accept");
        while (values.hasMoreElements()) {
            String value1 = values.nextElement();
            System.out.println(value1);
        }

        System.out.println("--------------------");

        //获取请求消息头的枚举
        Enumeration<String> keys = req.getHeaderNames();
        while (keys.hasMoreElements()) {

            String key = keys.nextElement();
            String value2 = req.getHeader(key);
            System.out.println(key + ":" + value2);
        }
    }
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        doPost(req, resp);
    }
}
```

#### 获取请求参数（重点）

| **返回值**           | **方法名**                        | **说明**             |
| -------------------- | --------------------------------- | -------------------- |
| String               | getParameter(String   name)       | 根据名称获取数据     |
| String[]             | getParameterValues(String   name) | 根据名称获取所有数据 |
| Enumeration<String>  | getParameterNames()               | 获取所有名称         |
| Map<String,String[]> | getParameterMap()                 | 获取所有参数的键值对 |

```java
package com.yishan.servlet.request;

import java.io.IOException;
import java.util.Enumeration;
import java.util.Map;
import java.util.Map.Entry;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 获取请求参数
 *
 * @Author yishan
 * @Date 2021/1/30 0030 9:40
 * @Version 1.0
 */
@WebServlet("/ServletDemo03")
public class ServletDemo03 extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        //根据名称获取数据
        String username = req.getParameter("username");
        System.out.println(username);

        String password = req.getParameter("password");
        System.out.println(password);

        //根据名称获取所有数据
        String[] hobbies = req.getParameterValues("hobby");
        for (String hobby : hobbies) {
            System.out.println(hobby);
        }

        //获取所有名称
        Enumeration<String> parameterNames = req.getParameterNames();
        while (parameterNames.hasMoreElements()) {
            String name = parameterNames.nextElement();
            System.out.println(name);
        }

        //获取所有参数的键值对
        Map<String, String[]> parameterMap = req.getParameterMap();
        for (Entry<String, String[]> entry : parameterMap.entrySet()) {
            String key = entry.getKey();
            System.out.print(key + ":");
            String[] values = entry.getValue();
            for (String value : values) {
                System.out.print(value + ",");
            }
            System.out.println();
        }
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        doPost(req, resp);
    }
}
```

#### 将获取的请求参数封装到实体类中

##### **第一种：最简单直接的封装方式**

- 获取数据，手动创建对象

```java
package com.yishan.servlet.request;

import beans.Student;
import java.io.IOException;
import java.util.Enumeration;
import java.util.Map;
import java.util.Map.Entry;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 封装对象的第一种方式--手动封装对象
 *
 * @Author yishan
 * @Date 2021/1/30 0030 9:40
 * @Version 1.0
 */
@WebServlet("/ServletDemo04")
public class ServletDemo04 extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobbies = req.getParameterValues("hobby");

        Student student = new Student(username, password, hobbies);

        System.out.println(student);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        doPost(req, resp);
    }
}
```

##### 第二种：使用反射方式封装**

此种封装的使用要求：

​	表单`<input>`标签的name属性取值，必须和实体类中定义的属性名称一致。

​	注意什么是属性?

​	input的name值和javabean的实体类成员变量名必须一致，包括大小写

```java
 /**
 * 封装请求正文到javabean。使用的是反射+内省
 */
private void test7(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
    //1.获取请求正文的映射关系
    Map<String,String[]> map = request.getParameterMap();
    Users user = new Users();
    System.out.println("封装前："+user.toString());
    //2.遍历集合
    for(Map.Entry<String,String[]> me : map.entrySet()){
        String name = me.getKey();
        String[] value = me.getValue();
        try{
            //1.拿到User对象中的属性描述器。是谁的属性描述器：是由构造函数的第一个参数决定的。第二个参数是指定javabean的字节码
            PropertyDescriptor pd = new PropertyDescriptor(name, Users.class);//参数指的就是拿哪个类的哪个属性的描述器
            //2.设置javabean属性的值
            Method method = pd.getWriteMethod();
            //3.执行方法
            //判断参数到底是几个值
            if(value.length > 1){//最少有2个元素
                method.invoke(user, (Object)value);//第一个参数是指的给哪个对象，第二个参数指的是赋什么值
            }else{
                method.invoke(user, value);//第一个参数是指的给哪个对象，第二个参数指的是赋什么值
            }
        }catch(Exception e){
            e.printStackTrace();
        }
    }
    System.out.println("封装后："+user.toString());
}
```

##### 第三种：使用apache的commons-beanutils实现封装**

```java
package com.yishan.servlet.request;

import beans.Student;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.apache.commons.beanutils.BeanUtils;

/**
 * 封装对象的 借助工具类进行封装对象 -- apache的commons-beanutils实现封装
 *
 * @Author yishan
 * @Date 2021/1/30 0030 9:40
 * @Version 1.0
 */
@WebServlet("/ServletDemo06")
public class ServletDemo06 extends HttpServlet {

  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {

    Map<String, String[]> map = req.getParameterMap();

    Student student = new Student();

    try {
      BeanUtils.populate(student, map);
    } catch (IllegalAccessException e) {
      e.printStackTrace();
    } catch (InvocationTargetException e) {
      e.printStackTrace();
    }

    System.out.println(student);
  }

  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {
    doPost(req, resp);
  }
}
```

#### 请求正文中中文编码问题

请求中文乱码问题：

​	GET方式：TomCat8版本后已经解决。

​	POST方式：需要我们手动解决。通过setCharacterEncoding("UTF-8")解决

```java
/**
 * 请求正文的中文乱码问题
 */
public class RequestDemo5 extends HttpServlet {

    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        
        //核心控制代码
		request.setCharacterEncoding("UTF-8");
        
		String username = request.getParameter("username");
     
		System.out.println(username);
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        doGet(request, response);
    }
}
```

#### 请求域

- 可以在一次请求范围内进行数据共享
- 一般用于请求转发的多个资源中共享数据

- 

| **返回值** | **方法名**                             | **说明**                       |
| ---------- | -------------------------------------- | ------------------------------ |
| void       | setAttribute(String name,Object value) | 向请求域对象中存储数据         |
| Object     | getAttribute(String name)              | 通过名称获取请求域对象中的数据 |
| void       | removeAttribute(String name)           | 通过名称删除请求域对象中的数据 |

#### 请求转发

- 请求转发：客户端的一次请求到达后，发现需要借助其他 Servlet 来实现功能。
- 特点：
  - 浏览器地址栏不变
  - 域对象中的数据不丢失
  - 负责转发的 Servlet 转发前后的响应正文会丢失
  - 由转发的目的地来响应客户端

| **返回值**        | **方法名**                          | **说明**         |
| ----------------- | ----------------------------------- | ---------------- |
| RequestDispatcher | getRequestDispatcher(String   name) | 获取请求调度对象 |

| **返回值** | **方法名**                                        | **说明** |
| ---------- | ------------------------------------------------- | -------- |
| void       | forward(ServletRequest req, ServletResponse resp) | 转发     |

```java
package com.yishan.servlet.request;

import beans.Student;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.apache.commons.beanutils.BeanUtils;

/**
 * 请求转发
 *
 * @Author yishan
 * @Date 2021/1/30 0030 9:40
 * @Version 1.0
 */
@WebServlet("/ServletDemo09")
public class ServletDemo09 extends HttpServlet {

  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {

    //浏览器响应语句
    resp.getWriter().write("ServletDemo09");

    //控制台输出语句
    System.out.println("ServletDemo09执行了");

    //设置共享数据
    req.setAttribute("encoding","GBK");

    //获取请求转发对象
    RequestDispatcher requestDispatcher = req.getRequestDispatcher("/ServletDemo09bak");

    //转发--核心代码
    requestDispatcher.forward(req,resp);
  }

  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {
    doPost(req, resp);
  }
}
```

```java
package com.yishan.servlet.request;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 请求转发至此Servlet进行处理
 *
 * @Author yishan
 * @Date 2021/1/30 0030 9:40
 * @Version 1.0
 */
@WebServlet("/ServletDemo09bak")
public class ServletDemo09bak extends HttpServlet {

  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {

    resp.getWriter().write("ServletDemo09bak");

    //获取共享数据 encoding=GBK
    Object encoding = req.getAttribute("encoding");
    System.out.println(encoding);

    System.out.println("ServletDemo09bak执行了");
  }

  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {
    doPost(req, resp);
  }
}
```

#### 请求包含

- 请求包含：可以合并其他 Servlet 中的功能一起响应给客户端。
- 特点：
  - 浏览器地址栏不变
  - 域对象中的数据不丢失
  - 被包含的 Servlet 响应头会丢失

| **返回值** | **方法名**                                           | **说明** |
| ---------- | ---------------------------------------------------- | -------- |
| void       | include(ServletRequest   req,ServletResponse   resp) | 实现包含 |

##### 请求包含注意

 * 把两个Servlet的响应内容**合并输出**。
 * 这种包含是**动态包含**。
 * 动态包含的特点：**各编译各的，只是最后合并输出**。

#####  细节问题

- 请求转发的注意事项：负责转发的Servlet，转发前后的响应正文丢失，由转发目的地来响应浏览器。
- 请求包含的注意事项：被包含者的响应消息头丢失。因为它被包含起来了。

```java
package com.yishan.servlet.request;

import java.io.IOException;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 请求包含发起者
 *
 * @Author yishan
 * @Date 2021/1/30 0030 9:40
 * @Version 1.0
 */
@WebServlet("/ServletDemo08")
public class ServletDemo08 extends HttpServlet {

  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {

    resp.setContentType("text/html;charset=UTF-8");
    resp.getWriter().write("我是请求者");

    //请求拿到包含对象
    RequestDispatcher rd = req.getRequestDispatcher("/ServletDemo08bak");

    //实现包含的操作
    rd.include(req,resp);
  }

  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {
    doPost(req, resp);
  }
}
```



```java
package com.yishan.servlet.request;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 请求包含被包含者
 *
 * @Author yishan
 * @Date 2021/1/30 0030 9:40
 * @Version 1.0
 */
@WebServlet("/ServletDemo08bak")
public class ServletDemo08bak extends HttpServlet {

  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {

    resp.setContentType("text/html;charset=UTF-8");
    resp.getWriter().write("我是被包含者");
  }

  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {
    doPost(req, resp);
  }
}
```

### 响应对象

#### 关于响应

- 响应，它表示了服务器端收到请求，同时也已经处理完成，把处理的结果告知用户。简单来说，指的就是服务器把请求的处理结果告知客户端。
- 响应对象，顾名思义就是用于在JavaWeb工程中实现上述功能的对象。

#### 常用响应对象

- 响应对象也是是Servlet规范中定义的，它包括了协议无关的和协议相关的。
  - 协议无关的对象标准是：ServletResponse接口
  - 协议相关的对象标准是：HttpServletResponse接口

#### 常用状态码 

| 状态码 |                            说明                            |
| :----: | :--------------------------------------------------------: |
|  200   |                          执行成功                          |
|  302   | 它和307一样，都是用于重定向的状态码。只是307目前已不再使用 |
|  304   |                 请求资源未改变，使用缓存。                 |
|  400   |            请求错误。最常见的就是请求参数有问题            |
|  404   |                       请求资源未找到                       |
|  405   |                      请求方式不被支持                      |
|  500   |                     服务器运行内部错误                     |

#### 状态码首位含义

| 状态码 |    说明    |
| :----: | :--------: |
|  1xx   |    消息    |
|  2xx   |    成功    |
|  3xx   |   重定向   |
|  4xx   | 客户端错误 |
|  5xx   | 服务器错误 |

#### 字节流输出中文问题

| **返回值**          | **方法名**                                | **说明**                           |
| ------------------- | ----------------------------------------- | ---------------------------------- |
| ServletOutputStream | getOutputStream()                         | 获取响应字节输出流对象             |
| void                | setContentType(“text/html;charset=UTF-8”) | 设置响应内容类型，解决中文乱码问题 |

```java
package com.yishan.servlet.response;

import java.io.IOException;
import javax.print.DocFlavor.STRING;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 字节流响应-输出乱码问题
 *
 * @Author yishan
 * @Date 2021/1/30 0030 10:00
 * @Version 1.0
 */
@WebServlet("/ServletResponseDemo01")
public class ServletDemo01 extends HttpServlet {

  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {

    //设置MIME类型，并告知浏览器编码格式-核心代码
    resp.setContentType("text/html;charset=UTF-8");

    String str = "你好呀！！！";

    ServletOutputStream os = resp.getOutputStream();

    //指定输出流的编码格式-核心代码
    os.write(str.getBytes("UTF-8"));
  }

  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {
    doPost(req, resp);
  }
}
```

#### 响应图片

- **注意**：
  - 发布后图片真实路径的获取

```java
package com.yishan.servlet.response;

import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 字节流响应图片
 *
 * @Author yishan
 * @Date 2021/1/30 0030 10:00
 * @Version 1.0
 */
@WebServlet("/ServletResponseDemo02")
public class ServletDemo02 extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        //获取发布后的真实路径-核心代码
        String realPath = getServletContext().getRealPath("/img/computer.jpg");

        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(realPath));

        ServletOutputStream sos = resp.getOutputStream();

        byte[] bytes = new byte[1024];
        int len = -1;
        while ((len = bis.read(bytes)) != -1) {
            sos.write(bytes, 0, len);
        }

        bis.close();
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        doPost(req, resp);
    }
}
```

#### 设置响应消息头-控制缓存

- 缓存：
  - 对于不经常变化的数据，我们可以设置合理缓存时间，以避免浏览器频繁请求服务器。以此来提高效率！

| **返回值** | **方法名**                               | **说明**           |
| ---------- | ---------------------------------------- | ------------------ |
| void       | setDateHeader(String   name,long   time) | 设置消息头添加缓存 |

```java
package com.yishan.servlet.response;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 设置缓存
 *
 * @Author yishan
 * @Date 2021/1/30 0030 10:00
 * @Version 1.0
 */
@WebServlet("/ServletResponseDemo04")
public class ServletDemo04 extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        String str = "设置缓存时间";

        //设置缓存时间就是设置响应消息头：Expires 但是值是一个毫秒数。
        //使用resp.setDateHeader()方法;--核心代码
        resp.setDateHeader("Expires", System.currentTimeMillis() * 1 * 60 * 60 * 1000);

        resp.setContentType("text/html;charset=UTF-8");

        resp.getWriter().write(str);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        doPost(req, resp);
    }
}
```

#### 设置响应消息头定时刷新

- 定时刷新：过了指定时间后，页面自动进行跳转

| **返回值** | **方法名**                              | **说明**           |
| ---------- | --------------------------------------- | ------------------ |
| void       | setHeader(String   name,String   value) | 设置消息头定时刷新 |

```java
package com.yishan.servlet.response;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 *设置响应消息头定时刷新
 *
 * @Author yishan
 * @Date 2021/1/30 0030 10:00
 * @Version 1.0
 */
@WebServlet("/ServletResponseDemo03")
public class ServletDemo03 extends HttpServlet {

  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {

    String str = "您的账号密码有误，3秒后重新跳转到登录页面···";

    resp.setContentType("text/html;charset=UTF-8");

    resp.getWriter().write(str);
	//设置响应消息头定时刷新--核心代码
    //Refresh设置的时间单位是秒，如果刷新到其他地址，需要在时间后面拼接上地址
    resp.setHeader("Refresh","3;URL="+req.getContextPath()+"/index.html");
  }

  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {
    doPost(req, resp);
  }
}
```

####  请求重定向：注意地址栏发生改变。

请求重定向：客户端的一次请求到达后，发现需要借助其他 Servlet 来实现功能。

特点：浏览器地址栏发生改变

| **返回值** | **方法名**                  | **说明**   |
| ---------- | --------------------------- | ---------- |
| void       | sendRedirect(String   name) | 设置重定向 |

- 重定向：资源跳转的方式
  - 方式一：设置头信息location
    - 设置状态码为302 
      - response.setStatus(302);
    - 设置响应头location
      - response.setHeader("location","/day15/responseDemo2");
  - 方式二：
    - 简单的重定向方法
      - response.sendRedirect("/day15/responseDemo2");
- 重定向的特点：redirect
  - 地址栏发生变化
  - 重定向可以访问其他站点(服务器)的资源
  - 重定向是两次请求。不能使用Request域对象来共享数据
- 转发的特点：forward
  - 转发地址栏路径不变
  - 转发只能访问当前服务器下的资源
  - 转发是一次请求，可以使用Request域对象来共享数据

```java
package com.yishan.servlet.response;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 重定向
 * 共享数据在重定向后获取不到 请求转发是可以获取到的
 *
 * @Author yishan
 * @Date 2021/1/30 0030 10:00
 * @Version 1.0
 */
@WebServlet("/ServletResponseDemo06")
public class ServletDemo06 extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        req.setAttribute("username", "zhangsan");

        //重定向路径--核心代码
        resp.sendRedirect(req.getContextPath() + "/ServletResponseDemo06bak");

        System.out.println("ServletResponseDemo06执行了");
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        doPost(req, resp);
    }
}
```

```java
package com.yishan.servlet.response;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 重定向目的地
 *
 * @Author yishan
 * @Date 2021/1/30 0030 10:00
 * @Version 1.0
 */
@WebServlet("/ServletResponseDemo06bak")
public class ServletDemo06bak extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        resp.getWriter().write("ServletResponse06bak");

        //获取共享数据，重定向之后获取不到数据，username=null
        Object username = req.getAttribute("username");
        System.out.println(username);
        System.out.println("ServletResponseDemo06bak");
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        doPost(req, resp);
    }
}
```

#### 响应和消息头组合应用-文件下载

```java
package com.yishan.servlet.response;

import java.io.FileInputStream;
import java.io.IOException;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 响应和消息头组合应用-文件下载
 *
 * @Author yishan
 * @Date 2021/1/30 0030 10:00
 * @Version 1.0
 */
@WebServlet("/ServletResponseDemo07")
public class ServletDemo07 extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        /*
        文件下载的思路：
           1.获取文件路径
            2.把文件读到字节输入流中
            3.告知浏览器，以下载的方式打开（告知浏览器下载文件的MIME类型）
           4.使用响应对象的字节输出流输出到浏览器上
        */
        //通过文件的绝对路径获取文件的虚拟路径进而得到发布后文件的真实路径
        FileInputStream fis = new FileInputStream(
                this.getServletContext().getRealPath("/uploads/girl.jpg"));

        //设置响应消息头，注意下载的时候，设置响应正文的MIME类型，用application/octet-stream
        resp.setHeader("Content-Type", "application/octet-stream");

        //告知浏览器以下载的方式
        resp.setHeader("Content-Disposition", "attachment;filename=1.jpg");

        //使用响应对象的字节输出流输出
        ServletOutputStream sos = resp.getOutputStream();
        byte[] bytes = new byte[1024];
        int len = -1;
        while ((len = fis.read()) != -1) {
            sos.write(bytes,0,len);
        }
        fis.close();
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        doPost(req, resp);
    }
}
```





















