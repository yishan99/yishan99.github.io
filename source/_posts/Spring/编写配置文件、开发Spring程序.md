---
title: 编写配置文件、开发Spring程序
date: 2020-05-21 09:22:36
tags: [Spring,IOC,AOP]
---
Spring   IOC    Aop
<!-- more -->
IOC:控制反转（DI：依赖注入），反转的是：获取对象的方式。
![理解控制反转](https://s1.ax1x.com/2020/05/21/YbpZfH.png)

    IOC(控制反转)也可以称之为DI（依赖注入）。
    控制反转：将 创造对象、属性值的方式进行了反转，从 new、setXxx（）反转为 从SpringIOC容器中getBean（）；
    依赖注入：将属性值注入给了属性，将属性注入给了bean,将bean注入给了IOC容器；
    总结：ioc/di，无论要什么对象，都可以直接去Springioc容器中获取，而不需要自己操作（new\setXxx()）；
    因此以后的ioc分为两步：
        1.先给SpringIOC中存放对象并复制
        2.拿取对象
#### 1.搭建Spring环境
下载jar网址：https://maven.springframework.org/release/org/springframework/spring/

spring-framework-4.3.9.RELEASE-dist.zip

    开发Spring至少需要使用的jar（5个+1个)：
    spring-aop.jar  开发AOP特性时需要的jar
    spring-beans.jar    处理Bean的jar        
    spring-context.jar  处理spring上下文的jar
    spring-core.jar spring的核心jar
    spring-expression.jar   spring表达式
    三方提供的日志.jar  
    commons-logging.jar     日志
#### 2.编写配置文件
>为了编写时有一些提示、自动生成一些配置信息

    方式一：
        可以给eclipse增加、支持spring的插件    spring tool suite(https://spring.io/tools/sts/all)
        下载spring-tool-suite-4-4.6.1.RELEASE-e4.15.0-win32.win32.x86_64.self-extracting.zip,
    方式二：
        直接下载sts工具（相当于一个Eclipse）:https://spring.io/tools/sts/
    新建：bean configuration .. --applicationContext.xml
#### 3.开发Spring程序（IOC）
        //spring上下文对象:context
		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		//直接从springIOC容器中获取一个id为student的对象
		Student student = (Student)context.getBean("student");
    可以发现，springioc容器帮助了我们new了对象，并且给对象赋了值
    IOC容器赋值：如果是简单类型（8个基本+String），用value;r如果是对象类型，用ref="需要引用的id值"，因此实现的对象与对象之间的依赖关系
    context.getBean(需要获取的bean的id值)
    依赖注入的3种方式：
        1.set方式的依赖注入：通过setXxx()赋值
            赋值，默认使用的是set方法（）；
            依赖注入底层是通过反射实现的
            <property name=""></property>进行赋值
        2.构造器注入：通过构造方法赋值
             <!-- 通过构造器赋值 
                <constructor-arg value="24" index="1"></constructor-arg>
                <constructor-arg value="ls" index="0"></constructor-arg>
                
                <constructor-arg value="24" name="age"></constructor-arg>
                <constructor-arg value="ls" name="name"></constructor-arg>
                -->

                <constructor-arg value="24" type="int"></constructor-arg>
                <constructor-arg value="ls" type="String"></constructor-arg>
                
                </bean>
            需要注意：如果<constructor-arg>的顺序与构造方法参数的顺序不一致，则需要通过type或者index或者name指定。
            一般建议使用name指定，见名知意。
        3.p命名空间注入：
            引入p命名空间：	xmlns:p="http://www.springframework.org/schema/p"
            <bean id="course" class="org.yishan.entity.Course" p:courseHour="300" p:courseName="hadoop" p:teacher-ref="teacher"> 
            简单类型：
                p:属性名=“属性值”、
            引用类型（除了String外）：
                p:属性名-ref=“引用的id”
            注意多个p赋值的时候要留空格。
        注意：
            无论是String还是Int/short/long,在赋值时都是value=“值”，因此建议此种情况需要配合name\type进行区分
        示例：
            注入各种集合数据类型：List,set,map,properties
                        <bean id="collectionDemo"
                    class="org.yishan.entity.AllCollectionType">
                    <!-- 通过set赋值 -->
                    <property name="list">
                        <list>
                            <value>足球</value>
                            <value>篮球</value>
                            <value>兵乓球</value>
                        </list>
                    </property>
                    <property name="array">
                        <array>
                            <value>足球1</value>
                            <value>篮球1</value>
                            <value>乒乓球1</value>
                        </array>
                    </property>
                    <property name="set">
                        <set>
                            <value>足球2</value>
                            <value>篮球2</value>
                            <value>乒乓球2</value>
                        </set>
                    </property>
                    <property name="map">
                        <map>
                            <entry>
                                <key>
                                    <value>foot</value>
                                </key>
                                <value>足球3</value>
                            </entry>
                            <entry>
                                <key>
                                    <value>basket</value>
                                </key>
                                <value>篮球3</value>
                            </entry>
                            <entry>
                                <key>
                                    <value>PP3</value>
                                </key>
                                <value>乒乓球3</value>
                            </entry>
                        </map>
                    </property>
                    <property name="props">
                        <props>
                            <prop key="fooot4">足球4</prop>
                            <prop key="basket4">篮球4</prop>
                            <prop key="pp4">乒乓球4</prop>
                        </props>
                    </property>
                </bean>
运行结果：

![运行结果](https://s1.ax1x.com/2020/05/27/tAppOs.png)
    set、list、数组，各自都有自己的标签<set><list><array>,但是也可以混着用，一般不建议。


    