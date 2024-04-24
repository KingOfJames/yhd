#  JavaWeb基础知识
## 架构模式
* CS：客户端服务器架构模式
    * 优点：充分利用客户端机器的资源，减轻服务器的负荷
    (一部分安全要求不高的计算任务和存储任务放在客户端执行，不需要把所有的计算和存储都放在服务器进行，从而减轻服务器的压力，也能减轻网络负荷)
    * 缺点：需要安装；升级维护成本较高



* BS：浏览器服务器架构模式
    * 优点：客户端不需要安装；维护成本较低
    * 缺点：所有的计算和存储任务都是放在服务器端的，服务器的负荷较重；在服务端计算完成之后把结果再传输给客户端，因此客户端和服务器端会进行非常频繁的数据通信，从而网络负荷较重

## Servlet的生命周期
* 生命周期：从出生到死亡的过程就是生命周期。对应Servlet中的三个方法：init()：初始化时期、service()：服务时期、destroy()：销毁时期



* 导入Servlet的依赖坐标：

  ```xml
  <!--    导入servlet依赖坐标-->
      <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
      </dependency>
  ```

  



* 默认情况下：
  * 第一次接收请求时，这个Servlet会进行实例化(调用构造方法)、初始化(调用init()方法)、然后服务(调用service()方法)、最后销毁(调用destroy()方法)
  * 从第二次请求开始，每一次都是服务
  * 当容器关闭时，其中所有的Servlet实例都会被销毁，调用销毁方法
  * 第一次请求时，tomcat才会去实例化，初始化，然后才服务；好处：提高系统的启动速度；缺点：第一次请求时启动速度慢
  * 如果要提高系统的启动速度，当前就用默认情况，如果要提高响应速度，应该设置Servlet的初始化时机



* Servlet的初始化时机：
  * 默认是第一次接收请求时，实例化、初始化
  * 可以通过在web.xml文件中设置<load-on-startup>来设置Servlet启动的先后顺序，数字越小，启动越靠前，最小值为0



* Servlet在容器中是：单例的、线程不安全的
  * 单例：所有的请求都是一个实例去响应
  * 线程不安全的：有事两个不同线程会去修改同一个成员变量，导致某一个线程的动作改变，导致数据安全问题
  * 启发：尽量的不要在Servlet中定义成员变量

## HTTP状态码
    * 100(Continue)：只有请求的一部分已经被服务器接收，但只要该请求没有被拒绝，客户端就应该继续这个请求；
    * 101(Switching Protocols)：服务器切换协议；
    * 200(OK)：请求成功；
    * 201(Created)：该请求时完整的，并创建一个新的资源；
    * 202(Accepted)：该请求被接受处理，但是该处理是不完整的；
    * 300(Multiple Choices)：链接列表，用户可以选择一个链接，进入到该位置。最多五个地址；
    * 301(Moved Permanently)：所有请求的页面已经转移到一个新的URL；
    * 302(Found)：所有请求的页面已经临时转移到一个新的URL；
    * 404(Not Found)：服务器无法找到所请求的页面；
    * 405(Method Not Allowed)：在请求中指定的方法是不允许的；
    * 505(HTTP Version Not Supported)：服务器不支持"HTTP协议"版本；

## HTTP协议
概况：Hyper Text Transfer Protocol<span style="color:red">(超文本传输协议)</span>。HTTP最大的作用就是确定了请求和
响应数据的格式。浏览器发送给服务器的数据：请求报文；服务器返回给浏览器的数据：响应报文。



* HTTP是无状态的：服务器无法判断两次请求是同一客户端发送过来的，还是不同客户端发送过来的；
  * 会话跟踪技术：客户端第一次发请求给服务器，服务器会获取HttpSession，如果获取不到，则会创建新的HttpSession，然后响应(能在Response Header中看见)给客户端；
  下次客户端给服务器发送请求时，会把HttpSessionID(在Request Header中可以看见)带给服务器，服务器就能获取到了，之后服务器再去判断是否是同一个客户端；
  * 会话的常用API：
    * HttpServletRequest类的方法：
      * getSession()：获取当前的会话，没有则创建一个新的，底层会自动给一个ID记录；创建一个HttpSession对象
      * getSession(boolean flag)：获取当前的会话，如果参数为true，则与无参的getSession方法一样，如果参数为false，就获取当前会话，没有会话就返回null，不会创建新的会话
      * getServletContext()：获取当前应用程序的上下文
    * HttpSession类的方法：
      * getId()：获取会话的SessionId；
      * isNew()：判断当前SessionId是否是新的；
      * getMaxInactiveInterval()：获取会话的非激活间隔时长，默认1800秒(即半个小时)；
      * setMaxInactiveInterval()：设置会话的非激活间隔时长；
      * invalidate()：强制性让会话立即失效
      * getAttribute(String key)：获取当前Session作用域里的指定key的value；
      * setAttribute(String key,String value)：设置当前Session作用域的键值对；
      * removeAttribute(String key)：删除当前Session的指定key对应的值；
  * Session保存作用域：Session保存作用域是和某一个Session对应的；



### 保存作用域

概念：原始情况下，保存作用域我们可以认为有四个：

* page：页面级别
* request：一次请求响应范围(HttpServletRequest)
* session：一次会话范围(HttpSession)
* application：一次应用程序范围(ServletContext)



* HTTP请求响应包含两个部分：请求和响应
  * 请求(包含三个部分)：
    * 请求行：有三个信息：
      * 请求的方式(一共有八种请求方式)；
      * 请求的URL；
      * 请求的协议(一般都是HTTP1.1)
    * 请求消息头：
      * 作用：通过具体的参数对本次请求进行详细的说明；
      * 格式：键值对形式，键和值之间使用冒号分隔；
      * 相对比较重要的请求消息头：
        * Host：服务器的主机地址；
        * Accept：声明当前请求能够接受的(媒体类型)；
        * Referer：当前请求来源页面的地址；
        * Content-Length：请求体内容的长度；
        * Content-Type：请求体的内容类型，这一项的具体值是媒体类型中的某一种；
        * Cookie：浏览器方位服务器时携带的Cookie数据；
    * 请求体：有三种情况：
      * get方式：没有请求体，但是有一个queryString
      * post方式：有请求体，form data
      * json格式：有请求体，request payload
  * 响应(包含三个部分)：
    * 响应状态行：有三个信息；
      * 协议；
      * 响应状态码(正常响应成功显示200)；
      * 响应状态(正常响应成功显示ok)；
    * 响应消息头：包含了服务器的信息；服务器发送给浏览器的信息(内容的媒体类型、编码、内容长度等)；
    * 响应体：响应的实际内容(比如请求index.html页面时，响应的内容就是<span style="color:red">&lt;html&gt;&lt;head&gt;&lt;body&gt;....</span>)



* 服务器内部转发以及客户端重定向：
  * 服务器内部转发：针对于HttpServletRequest收到的请求；<span style="color:green">getRequestDispatcher("...").forward(req,resp);</span>
    * 理解：就是服务器某一个组件接收到客户端发来的请求时，发现这个请求自己做不了，就转发给服务器的另一个组件去做，然后在响应给客户端；
    * 一次请求响应的过程，对于客户端而言，服务器内部经过多少次转发，它是不知道的；
  * 客户端重定向：针对于HttpServletResponse返回的响应；<span style="color:#008c8c">sendRedirect("...");</span>
    * 理解：客户端发送一个请求给服务器某一个组件，该组件接收到请求，但该组件不能完成此请求，响应时叫客户端去寻找某组件，客户端接到响应后，又发送请求给刚刚接收到的某组件的名称，又发送给该组件去执行，该组件执行好再响应给客户端；
    * 两次请求响应的过程，客户端是知道请求URL有变化；
    * **302：表示重定向；**



## Request获取请求数据

请求数据分为三部分：

* 获取请求行的数据：
  * String getMethod()：获取请求方式：GET、POST等
  * String getContextPath()：获取虚拟目录(项目访问路径)：/request-demo
  * StringBuffer getRequestURL()：获取URL(统一资源定位符)：http://localhost:8080/request-demo/req1
  * String getRequestURI()：获取URI(统一资源标识符)：/request-demo/req1
  * String getQueryString()：获取请求参数(GET方式)：username=zhangsan&password=123
* 获取请求头的数据：
  * String getHeader(String name)：根据请求头名称，获取值
* 获取请求体的数据：
  * ServletInputStream getInputStream()：获取字节输入流
  * BufferedReader getReader()：获取字符输入流
* Map<String String[]> getParameterMap()：获取所有参数的Map集合
* String[] getParameterValues(String name)：根据名称获取参数值(数组)
* String getParameter(String name)：根据名称获取参数值(单个值)
* post请求方式解决乱码问题：req.setCharacterEncoding(字符集);
* get请求方式解决乱码问题：
  * 第一步：先将读取出来的某中文参数用对应字符集解码变成字节数组，如username.getBytes("ISO-8859-1");
  * 第二步：将解码后的字节数组按照对应字符集进行编码，如new String(bytes,"UTF-8");

* URL编码实现方式：
  * 编码：URLEncoder.encode(String s,String character);
  * 解码：URLDecoder.decode(String s,String character);




## Response设置响应数据

响应数据分为三部分：

* 响应行：
  * void setStatus(int sc)：设置响应状态码
* 响应头：
  * void setHeader(String name,String value)：设置响应头键值对，例如name为Content-type，value为text/html
* 响应体：
  * PrintWriter getWriter()：获取字符输出流(可以传输一些html文本等)
  * ServletOutputStream getOutputStream()：获取字节输出流(可以用来传输一些图片、音频等)





# 基于springboot的请求参数

* 简单参数：参数名与形参变量名相同，定义形参即可接收参数

  * 如果方法形参名与请求参数名不匹配，可以使用“@RequestParam”注解来完成映射，例如：![image-20230522160337005](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230522160337005.png)
  * @RequestParam中的required属性默认为true，代表该请求参数必须传递，如果不传递将报错。如果该参数是可选的，可以将required属性的值设置为false

* 实体参数：编写一个实体对象，参数名要与实体对象的属性名相同

  * 简单实体对象：请求参数名与形参对象的属性名相同，定义一个对象即可接收

  * 复杂实体对象：请求参数名与形参对象的属性名相同，按照对象层次结构关系计科接收嵌套的对象属性参数，例如：![image-20230522160752184](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230522160752184.png)

    ![image-20230522160812052](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230522160812052.png)

* 数组集合参数：常用于复选框

  * 数组参数：请求参数名与形参数组名相同且请求参数为多个，定义数组类型形参即可接收参数
  * 集合参数：请求参数名与形参集合名相同且请求参数为多个，定义集合类型形参，并用“@RequestParam”注解绑定参数关系，例如：<img src="C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230522161141404.png" alt="image-20230522161141404" style="zoom:67%;" />

* 日期参数：使用“@DateTimeFormat”注解完成日期参数格式转换，可以用Date类或者LocalDateTime类来接收参数

  * 注意：由于前端传递的日期格式有多种，所以可以指定传递过来的日期参数的格式，只要在注解中添加pattern属性来说明格式即可，例如：![image-20230522155945097](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230522155945097.png)

* JSON参数：JSON数据键名与形参对象属性名相同，定义对象形参即可接收参数，需要使用“@RequestBody”注解来标识

* 路径参数：通过请求URL直接传递参数，使用“{...}”的形式来标识该路径参数，需要使用“@PathVariable”获取路径参数，例如：![image-20230522161747249](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230522161747249.png)

  * 注意：也可以有多个路径参数，如：

  ![image-20230522161932632](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230522161932632.png)



# 基于springboot的响应

* @ResponseBody：是方法注解、类注解
  * 位置：Controller方法上或者类上
  * 作用：将方法返回值直接响应，如果返回值类型是实体对象或集合、数组，将会转为JSON格式响应
  * 注意：@RestController其实是由@Controller与@ResponseBody组合而成



## 三层架构

* controller层：控制层，接受前端发送的请求，对请求进行处理，并响应数据
* service层：业务逻辑层，处理具体的业务逻辑
* dao层：数据访问层(Data Access Object：持久层)，负责数据访问操作，包括数据的增、删、改、查

![image-20230523110548910](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230523110548910.png)



### 分层解耦

* 内聚：软件中各个功能模块内部的功能联系
* 耦合：衡量软件中各个层或模块之间的依赖、关联的程度
* 软件设计原则：高内聚低耦合
* 控制反转：Inversion Of Control，简称IOC。对象的创建控制权由程序自身转移到外部(容器)，这种思想称为控制反转
* 依赖注入：Dependency Injection，检查DI。容器为应用程序提供运行时，所依赖的资源，称之为依赖注入
* Bean对象：IOC容器中创建、管理的对象，称之为bean

Bean的声明：要把某个对象交给IOC容器管理，需要在对应的类上加上如下注解之一：

![image-20230523121804999](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230523121804999.png)

注意：

* 声明bean对象的时候，可以通过value属性指定bean的名字，如果没有指定，默认为类名首字母小写
* 使用以上四个注解都可以声明bean对象，但是在springboot集成web开发中，声明控制器bean要使用@Controller或者@RestController

Bean组件扫描：

* 前面声明bean的四大注解，要想生效，还需要被组件扫描注解@ComponentScan扫描
* @ComponentScan注解虽然没有显式配置，但是实际上已经包含在了启动类声明注解@SpringBootApplication中，默认扫描的范围是启动类所在包及其子包，如果某个bean的位置不在该范围内，可以在启动类里声明如下注解：@CompontentScan({"该bean所在包名","启动类所在包名"})

Bean注入：

* @Autowired注解，默认按照类型进行，如果存在多个相同类型的bean，将会报错：

![image-20230523123835711](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230523123835711.png)

* 可以通过下面的方案来解决：

![image-20230523123913013](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230523123913013.png)

![image-20230523124025260](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230523124025260.png)



# Maven概述

概述：它是一个项目管理和综合工具，Maven使用标准的目录结构和默认构建生命周期。提供了开发人员构建一个完整的生命周期框架，开发团队可以自动完成该项目的基础设施建设

* project标签：工程的根标签
* groupId标签： 这是工程组的标识。它在一个组织或者项目中通常是唯一的。例如，一个银行组织 com.companyname.project-group 拥有所有的和银行相关的项目
* artifactId标签：这是工程的标识。它通常是工程的名称。例如，消费者银行。groupId 和 artifactId 一起定义了 artifact 在仓库中的位置
* version标签： 这是工程的版本号。在 artifact 的仓库中，它用来区分不同的版本
* exclusions标签与exclusion标签：当计算传递依赖时， 从依赖构件列表里，列出被排除的依赖构件集。即告诉maven你只依赖指定的项目，不依赖项目的依赖。此元素主要用于解决版本冲突问题



### 依赖管理

* 通过设置坐标的依赖范围(scope标签)，可以设置对应jar包的作用范围：编译环境、测试环境、运行环境

![image-20230630113358097](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230630113358097.png)

* `<scope>`默认值：compile



### Maven的生命周期

概念：Maven的生命周期就是为了对所有的maven项目构建过程进行抽象和统一

maven中有三套相互独立的生命周期：

* clean：清理工作
* default：核心工作，如：编译、测试、打包、安装。部署等
* site：生成报告、发布站点等

![image-20230521125902522](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230521125902522.png)

常见的生命周期阶段：

* clean：移除上一次构建生成的文件
* compile：编译项目源代码
* test：使用合适的单元测试框架运行测试(junit)
* package：将编译后的文件打包，如jar、war等
* install：安装项目到本地仓库

注意：<span style="color:red;">在同一套生命周期中</span>，当运行后面的阶段时，前面的阶段都会运行



# Mybatis



持久层：

* 负责将数据保存到数据库的那一层代码
* JavaEE三层架构：表现层、业务层、持久层
* 导入Mybatis的依赖坐标：

  ```xml
  <!--    导入mybatis依赖坐标-->
      <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.13</version>
      </dependency>
  ```
* 导入MySQL依赖坐标：

```xml
<!--    导入mysql依赖坐标-->
    <dependency>
      <groupId>com.mysql</groupId>
      <artifactId>mysql-connector-j</artifactId>
      <version>8.0.33</version>
    </dependency>
```



## Mybatis快速入门

* 第一步：加载mybatis的核心配置文件，获取SqlSessionFactory对象

  ![image-20230808150633391](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230808150633391.png)

  * 其中mybatis-config.xml文件中的信息如下：

  ![image-20230808151059212](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230808151059212.png)

* 第二步：获取SqlSession对象，并用其来执行sql语句

  ![image-20230808150729400](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230808150729400.png)

  * 其中test.selectAll来自于：自定义的Mapper配置文件，如下：

  ![image-20230808151229046](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230808151229046.png)

* 第三步：释放资源(只用释放SqlSession对象的资源即可)



## Mapper代理开发

* 定义与SQL映射文件同名的Mapper接口，并且将Mapper接口和SQL映射文件放置在同一目录下；
* 设置SQL映射文件的namespace属性为Mapper接口的全限定名，如com.itkim.mapper.UserMapper；
* 在Mapper接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致；
* 将mybatis-config.xml文件中的映射语句的路径修改成UserMapper.xml现在的路径；
* 编码
  * 通过SqlSession的getMapper方法获取Mapper接口的代理对象；
  * 调用对应方法完成sql的执行

包扫描SQL映射文件：

```xml
<mappers>
    <!-- 加载sql映射文件 -->
    <!-- 包扫描方式:扫描该包下所有映射文件，在多个映射文件的情况下就省时 -->
    <package name="com.itkim.mapper"/>
</mappers>
```


### Mapper中SQL语句高亮显示

应修改以下代码：

```xml
//修改前
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
    
//修改后,就是将https修改为http
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">	
```





## Mybatis核心配置文件(mybatis-config.xml)

* `<properties>`标签：引入properties文件，在当前xml文件中使用${key}的方式访问
  * resource属性：填写要引入的properties文件的文件名及后缀名的字符串；
* mybatis核心配置文件中的标签必须要按照指定的顺序配置
  * properties、settings、typeAliases、typeHandlers、objectFactory、objectWrapperFactory、reflectorFactory、plugins、environments、databaseIdProvider、mappers
* ![image-20240219225141595](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240219225141595.png)

解读：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <typeAliases>
        <typeAliases type="com.itkim.pojo.User" aliase="abc"></typeAliases>
<!--        使用包扫描方式去简化映射文件时的resultType类型，默认不区分大小写-->
<!--        例如：com.itkim.pojo.User => user -->
        <package name="com.itkim.pojo"/>
    </typeAliases>

<!-- environments：配置数据库连接环境信息。可以配置多个environment，通过default属性切换不同的environment -->
    <environments default="development">

        <!--    开发过程使用的-->
        <environment id="development">
            <transactionManager type="JDBC"/> <!-- type：设置事务管理的方式，有两种方式：1.JDBC：表示使用JDBC中原生的事务管理方式；2.MANAGED：被管理，例如Spring -->
            <dataSource type="POOLED"> <!-- 数据源：type：设置数据源的类型，通常有三种方式：1.POOLED：表示使用数据库连接池；2.UNPOOLED：表示不使用数据库连接池；3.JNDI：表示使用上下文中的数据源 -->
<!--                数据库的链接信息-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///mybatis?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>

        <!--    测试过程使用的-->
        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
<!--                数据库的链接信息-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///mybatis?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    
    <mappers>
<!--        映射sql语句，也可以直接使用package标签去扫描某个包-->
        <mapper resource="com/itkim/mapper/UserMapper.xml"/>
    </mappers>
</configuration>
```

* Mybatis核心配置文件的顶层结构如下：

![image-20230808230531085](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230808230531085.png)

* 注意：配置各个标签时，需要遵守前后顺序



## Mybatis中映射文件的详解

* @MapKey注解：将查询的某个字段的值作为大的map的键
  * value属性：指定查询出的某个字段名称为大的map的键的名称
  
* resultMap标签：
  * ![image-20240219230935335](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240219230935335.png)
  * ![image-20240219170442973](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240219170442973.png)

* 处理多对一的映射关系：

  * 级联方式处理（利用resultMap）

    * ![image-20240219230851039](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240219230851039.png)

  * association标签（属于resultMap的字标签，用于处理多对一的映射关系）：处理实体类类型的属性

    * ![image-20240219230914176](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240219230914176.png)

  * 分步查询

    * ```
      第一步：
      java代码：Emp getEmpAndDeptByStepOne(@Param("empId") Integer empId);
      Mapper的xml文件代码：
      	<resultMap id="empAndDeptResultMapByStep" type="Emp">
      		<id column="emp_id" property="empId"></id>
      		<result column="emp_name" property="empName"></result>
      		<result column="age" property="age"></result>
      		<result column="gender" property="gender"></result>
      		<association property="dept" select="com.itkim.mapper.xxxMapper.getEmpAndDeptByStepTwo" column="dept_id">
      		</association>
      	</resultMap>
      	<select id="getEmpAndDeptByStepOne" resultMap="empAndDeptResultMapByStep">
      		select * from emp where emp_id = #{empId}
      	</select>
      第二步：
      java代码：Dept getEmpAndDeptByStepTwo(@Param("deptId") Integer deptId);
      Mapper的xml文件代码：
      	<select id="getEmpAndDeptByStepTwo" resultType="Dept">
      		select * from dept where dept_id = #{deptId}
      	</select>
      ```

      

* 处理一对多的映射关系：

  * collection标签：处理集合类型的属性

    * ![image-20240219231259320](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240219231259320.png)

  * 分步查询:

    * ```
      第一步：
      java代码：Emp getDeptAndEmpByStepOne(@Param("deptId") Integer deptId);
      Mapper的xml文件代码：
      	<resultMap id="deptAndEmpResultMapByStep" type="Dept">
      		<id column="dept_id" property="deptId"></id>
      		<result column="dept_name" property="deptName"></result>
      		<collection property="emps" select="com.itkim.mapper.xxxMapper.getDepteAndEmpByStepTwo" column="dept_id"
      	</resultMap>
      	<select id="getDeptAndEmpByStepOne" resultMap="deptAndEmpResultMapByStep">
      		select * from dept where dept_id = #{deptId}
      	</select>
      第二步：
      java代码：Dept getDepteAndEmpByStepTwo(@Param("deptId") Integer deptId);
      Mapper的xml文件代码：
      	<select id="getDepteAndEmpByStepTwo" resultType="Emp">
      		select * from emp where dept_id = #{deptId}
      	</select>
      ```




## Mybatis的常见用法

`<package>`标签：包扫描，会将指定包下的Mapper文件扫描出来，简化了映射问题；

`<resultMap>`标签：当数据库里的字段名与实体类的属性名不一致时，用resultMap标签来映射其关系；

* ![image-20230825085649449](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230825085649449.png)

参数占位符：

* **#{}：会将其替换成 "?" ，再将参数放入，能防止sql注入问题；**
* **${}：会直接将参数拼接成sql语句，会有sql注入问题；**
* 特殊字符的处理：
  * 1.使用转义字符(与html的处理一样)；
  * 2.使用CDATA区，例如：**`<![CDATA[内容]]>`**。这种方法就是将内容转变成纯文本来处理；

多条件动态查询：

* 散装参数：需要使用@Param("SQL语句中的参数占位符名称")，例如：@Param("companyName")String companyName；
* 实体类封装参数：只需要保证SQL语句中的字段名和实体类属性名对应上
* map集合设置参数：只需要保证SQL语句中的字段名和map集合中键的名称对应上，例如：map.put("stautes",stautes);

添加：

* `<insert>`标签中的属性：
  * **useGeneratedKeys**：默认值为false。如果设置为true，MyBatis会使用JDBC的getGeneratedKeys方法取出由数据库内部生成的主键
  * **keyProperty**：MyBatis会使用JDBC的getGeneratedKeys方法获取主键值后将要赋值的属性名。如果希望得到多个数据库自动生成的列，属性值也可以是以逗号分隔的属性名称列表



## 动态sql语句

* `<where>`标签：where标签会自动判断是否需要where语句；

  * 若where标签中有条件成立，会自动生成where关键字
  * 会自动将where标签中内容多余的and去掉，但是其中内容后多余的and无法去掉
  * 若where标签中没有任何一个条件成立，则where没有任何功能

* `<trim>`标签：可以截取指定内容

  * 属性prefix、suffix：在标签中内容前面或后面添加指定内容
  * 属性prefixOverrides、suffixOverrides：在标签中内容前面或后面去掉指定内容

* `<set>`标签：set标签会自动判断是否需要set语句；
* `<if>`标签：if标签相当于Java中的if语句。通过test属性中的表达式判断标签中的内容是否有效（会拼接到sql语句中）

  * test属性：用来作为if标签的判断条件；
* `<choose>`标签：choose标签相当于Java中的switch语句 ；
* `<when>`标签：when标签相当于Java中带了break的case语句；

  * test属性：用来作为when标签的判断条件；
  * 注意：当满足其中一个when标签中的条件时，将不再执行其他的when标签和otherwise标签
* `<othewise>`标签：相当于Java中的default；
* `<foreach>`标签：相当于Java中的for语句；
  
  * 注意：mybatis会将数组参数、集合参数，封装到一个Map集合中(默认：array(键的名称) = 数组，list(键的名称) = 集合，也可以使用@Param注解去给键起别名)
  * collection属性：对应键的名称；
  * separator属性：分隔符，可以使用也可以不用，即每循环一个后面会不会加分隔符，加什么样的分隔符等等；
  
* `<sql>`标签：可以记录一段sql，在需要用的地方使用include标签进行引用

  * 例如：

    ```xml
    <sql id="empColumns">
    	emp_id,emp_name,age,gender,dept_id
    </sql>
    ```

  * `<include>`标签：可以在sql语句中引用sql片段标签

    * 例如：

      ```xml
      <include refid="empColumns"></include>
      ```




## MyBatis的缓存

* 一级缓存：一级缓存是SqlSession级别的，通过同一个SqlSession查询的数据会被缓存，下次该SqlSession查询同一条数据时，就会从缓存中直接获取，不会从数据库重新访问，MyBatis默认开启一级缓存；
* 使一级缓存失效的四种情况：
  * 不同的SqlSession对应不同的一级缓存；
  * 同一个SqlSession但是查询条件不同；
  * 同一个SqlSession两次查询期间执行了任意一次增删改操作(因为增删改会影响数据库数据，所以会清空缓存)；
  * 同一个SqlSession两次查询期间手动清空了缓存；
    * 使用SqlSession中的方法：clearCache()；



* 二级缓存：二级缓存是SqlSessionFactory级别的，通过同一个SqlSessionFactory创建的SqlSession对象查询出来的结果会被缓存，下次通过该SqlSessionFactory获取出来的SqlSession再次执行相同的查询语句时，就会从缓存中获取；
* 二级缓存开启的条件：
  * 在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true；
  * 在映射文件中设置标签`<cache />`；
  * 二级缓存必须在SqlSession关闭或提交后才有效；
  * 查询的数据所转换的实体类类型必须实现序列化接口
* 使二级缓存失效的情况：
  * 两次查询之间执行了任意的增删改，会使一级缓存和二级缓存同时失效；

* 二级缓存的相关配置：在映射文件中添加的cache标签可以设置一些属性
  * eviction属性：缓存回收策略，默认是LRU策略；
    * LRU(Least Recently Used)：最近最少使用，移除最长时间不被使用的对象；
    * FIFO(First in First out)：先进先出，按对象进入缓存的顺序来移除；
    * SOFT：软引用，移除基于垃圾回收器状态和软引用规则的对象；
    * WEAK：弱引用，更积极的移除基于垃圾收集器状态的弱引用规则的对象；
  * flushInterval属性：刷新间隔，单位为毫秒，即情况缓存；
    * 默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新；
  * size属性：引用数目，正整数；
    * 代表缓存最多可以存储多少个对象，太大容易导致内存溢出，最好用默认的数目；
  * readOnly属性：只读，只有两个值，分别为true或false；
    * true：只读缓存，会给所有调用者返回缓存对象的相同实例，因此这些对象是不能被修改的；
    * false：读写缓存，会返回缓存对象的拷贝(通过序列化来拷贝)，这样会慢一些，但是安全的，因此默认为false；



* MyBatis缓存查询的顺序：先查询二级缓存，因为二级缓存中可能会有其他SqlSession已经查询出来的数据，可以拿来直接使用。如果二级缓存没有命中，再查询一级缓存。如果一级缓存也没有命中，则查询数据库。SqlSession关闭之后，一级缓存中的数据会写入二级缓存；



## MyBatis的逆向工程

* 正向工程：先创建Java实体类，由框架负责根据实体类生产数据库表。Hibernate是支持正向工程的

* 逆向工程：先创建数据库表，由框架负责根据数据库表，反向生成如下资源：

  * Java实体类；
  * Mapper接口；
  * Mapper映射文件；

* 添加逆向工程所需的插件：

  ```xml
  <!-- 控制Maven在构建过程中相关配置 -->
  <build>
      <!-- 构建过程中用到的插件 -->
  	<plugins>
          <!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
      	<plugin>
          	<groupId>org.mybatis.generator</groupId>
              <artifactId>mybatis-generator-maven-plugin</artifactId>
              <version>1.3.0</version>
              
              <!-- 插件的依赖 -->
              <dependencies>
                  <!-- 逆向工程的核心依赖 -->
                  <dependency>
                      <groupId>org.mybatis.generator</groupId>
                      <artifactId>mybatis-generator-core</artifactId>
                      <version>1.3.2</version>
                  </dependency>
                  <!-- MySQL驱动 -->
                  <dependency>
                      <groupId>mysql</groupId>
                      <artifactId>mysql-connector-java</artifactId>
                      <version>8.0.16</version>
                  </dependency>
              </dependencies>
          </plugin>
      </plugins>
  </build>
  ```

* 添加逆向工程的核心配置文件：generatorConfig.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE generatorConfiguration
    PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
    "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
  
  <generatorConfiguration>
      <!-- targetRuntime有两种，分别为：
   		MyBatis3Simple:清晰简洁版，只有单纯的CURD；
  		MyBatis3：奢华版，相比于简洁版多了一些带条件的CURD
  	-->
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!-- 数据库的连接信息 -->
      <jdbcConnection 
          driverClass="com.mysql.cj.jdbc.Driver"
          connectionURL="jdbc:mysql://localhost:3306/数据库名?serverTimezone=UTC"
          userId="用户名"
          password="密码">
      </jdbcConnection>
        
  	<!-- Javabean的生成策略 -->
      <javaModelGenerator targetPackage="完整包名称" targetProject=".\src\main\java">
        <property name="enableSubPackages" value="true" />
        <property name="trimStrings" value="true" />
      </javaModelGenerator>
  
        <!-- SQL映射文件的生成策略 -->
      <sqlMapGenerator targetPackage="完整包名称"  targetProject=".\src\main\resources">
        <property name="enableSubPackages" value="true" />
      </sqlMapGenerator>
  
        <!-- Mapper接口的生成策略 -->
      <javaClientGenerator type="XMLMAPPER" targetPackage="完整包名称"  targetProject=".\src\main\java">
        <property name="enableSubPackages" value="true" />
      </javaClientGenerator>
  
        <!-- 逆向分析的表
   			tableName设置为*号，可以对应所有表，此时不写domainObjectName
  			domainObjectName属性指定生成出来的实体类的类名
  		-->
      <table schema="DB2ADMIN" tableName="ALLTYPES" domainObjectName="Customer" >
        <property name="useActualColumnNames" value="true"/>
        <generatedKey column="ID" sqlStatement="DB2" identity="true" />
        <columnOverride column="DATE_FIELD" property="startDate" />
        <ignoreColumn column="FRED" />
        <columnOverride column="LONG_VARCHAR_FIELD" jdbcType="VARCHAR" />
      </table>
    </context>
  </generatorConfiguration>
  ```

  

## MyBatis分页插件

* 添加依赖：

  ```xml
  <dependency>
  	<groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>5.2.0</version>
  </dependency>
  ```

* 配置分页插件：在MyBatis的核心配置文件中配置插件

  ```xml
  <plugins>
      <!-- 设置分页插件 -->
  	<plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
  </plugins>
  ```

* 分页插件的使用：

  * ![image-20240224235926794](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240224235926794.png)
  * 在查询功能之前使用PageHelper.startPage(int pageNum,int pageSize)开启分页功能
    * pageNum：当前页的页码；
    * pageSize：每页显示的条数；
  * 在查询获取list集合之后，使用PageInfo<T> pageInfo = new PageInfo<>(List<T> list,int navigatePages)获取分页相关数据
    * list：分页之后的数据；
    * navigatePages：导航分页的页码数；
  * 分页的相关数据：
    * pageNum：当前页的页码；
    * pageSize：每页显示的条数；
    * size：当前页显示的真实条数；
    * total：总记录数；
    * pages：总页数；
    * prePage：上一页的页码；
    * nextPage：下一页的页码；
    * isFirstPage/isLastPage：是否为第一页/最后一页；
    * hasPreviousPage/hasNextPage：是否存在上一页/下一页；
    * navigatePages：导航分页的页码数；
    * navigatePageNums：导航分页的页码，[1,2,3,4,5]



# JSP

概念：Java Server Page，Java服务端页面（一种动态的网页技术，其中既可以定义HTML、CSS、JS等静态内容，还可以定义Java代码的动态内容），JSP=HTML+Java；

* 导入JSP坐标：

  ```xml
  <!-- 导入jsp依赖坐标-->
      <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
        <scope>provided</scope>
      </dependency>
  ```

* 创建JSP文件

* 编写HTML标签和Java代码

![image-20231118171413958](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20231118171413958.png)

## JSP脚本

* JSP脚本用于在JSP页面内定义Java代码
* JSP脚本分类：
  * <%…………%>：内容会直接放到_jspService()方法中；
  * <%=…………%>：内容会放到out.print()中，作为out.print()的参数（这里的out是输出流，该脚本也会出现在_jspService()方法中）；
  * <%!…………%>：内容会放到_jspService()方法之外，被类直接包含（即一些成员变量或方法）；



## EL表达式

* Expression Language 表达式语言，用于简化JSP页面内的Java代码

* 主要功能：获取数据

* 语法：${expression}，例如：${brands}：获取域中存储的key为brands的数据；
  * el表达式也可以使用cookie里的数据：${cookie.name.value}(name换成cookie中存储的数据名字)；	

* JavaWeb中的四大域对象：

  * page：当前页面有效；
  * request：当前请求有效；
  * session：当前会话有效；
  * application：当前应用有效；
  * ![image-20231119172506026](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20231119172506026.png)

  * el表达式获取数据，会依次从这四个域中寻找，直到找到为止；
    * 注意：servlet 3.0以上默认忽视el标签，可以通过**isELIgnored="false"**设置不忽视



## JSTL标签

* JSP标准标签库(Jsp Standarded Tag Library)，使用标签取代JSP页面上的Java代码

* 导入JSTL与standard的依赖坐标：

  ```xml
  <!-- 导入jstl依赖坐标 -->
      <dependency>
        <groupId>jstl</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
      </dependency>
      <dependency>
        <groupId>taglibs</groupId>
        <artifactId>standard</artifactId>
        <version>1.1.2</version>
      </dependency>
  ```

* 使用JSTL标签前，要在HTML文档之前添加下面的代码：**<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>**

* JSTL标签(可以和el表达式一起用)：
  * <c:if test=""></c:if >：相当于Java中的if...else...语句，test属性用来存放判断语句；
  * <c:forEach ></c:forEach >：相当于Java中的增强for循环；
    * items属性用来指出被遍历的容器；
    * var属性用来指出遍历产生的临时变量；
    * varStatus属性用来指出遍历状态对象的名称，可以用这个状态来获取遍历的下标；
    * begin属性可以设置开始数；
    * end属性可以设置结束数；
    * step属性可以设置变量的自增数；



# MVC模式

* MVC是一种分层开发的模式，其中：

  * M：Model，业务模型，处理业务；

  * V：View，视图，界面展示；

  * C：Controller，控制器，处理请求，调用模型和视图；

    ![image-20231119204814548](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20231119204814548.png)

​	

# 会话跟踪技术

* 会话：用户打开浏览器，访问web服务器资源，会话建立，直到有一方断开连接，会话结束。在一次会话中可以包含多次请求和响应；
* 会话跟踪：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间共享数据；
* HTTP协议是无状态的，每次浏览器向服务器请求时，服务器都会将该请求视为新的请求，因此需要会话跟踪技术来实现会话内数据共享；
* 实现方式：
  * 客户端会话跟踪技术：Cookie；
  * 服务端会话跟踪技术：Session；



## Cookie

* Cookie：客户端会话技术，将数据保存到客户端，以后每次请求都携带Cookie数据进行访问
* Cookie的基本使用：
  * 发送Cookie：
    * 创建Cookie对象，设置数据：Cookie cookie = new Cookie("key","value");
    * 发送Cookie到客户端，使用response对象：resp.addCookie(cookie);
  * 获取Cookie：
    * 获取客户端携带的所有Cookie，使用request对象：Cookie[] cookies = req.getCookies();
    * 遍历数组，获取每一个Cookie对象；
    * 使用Cookie对象方法获取数据：cookie.getName();   cookie.getValue();
* Cookie的实现是基于HTTP协议的；
  * 响应头：set-cookie；
  * 请求头：Cookie；

* Cookie使用细节：
  * Cookie存活时间：
    * 默认情况下，Cookie存储在浏览器内存中，当浏览器关闭，内存释放，则Cookie被销毁；
    * setMaxAge(int seconds)：设置Cookie存活时间；
      * 正数：将Cookie写入浏览器所在电脑的硬盘，持久化存储。到时间自动删除；
      * 负数：Cookie的默认值，Cookie在当前浏览器的内存中，当浏览器关闭，则Cookie被销毁；
      * 0：删除对应的Cookie；

  * Cookie存储中文：
    * Cookie不能直接存储中文；
    * 如果需要存储中文，则需要进行转码：URL编码；
      * URL编码：URLEncoder.encode(value,"字符集")；
      * URL解码：URLDecoder.decode(value,"字符集")；




## Session

* 服务器会话跟踪技术：将数据保存到服务器；
* JavaEE提供HttpSession接口，来实现一次会话的多次请求间数据共享功能；
* Session的基本使用：
  * 获取Session对象：HttpSession session = new HttpSession();
  * Session对象功能：
    * void setAttribute(String name,Object o)：存储数据到session域中；
    * Object getAttribute(String name)：根据key，获取值；
    * void removeAttribute(String name)：根据key，删除该键值对；
* Session钝化、活化：
  * 服务器重启后，Session中的数据是否还存在？
    * 钝化：在服务器正常关闭后，Tomcat会自动将Session数据写入硬盘的文件中；
    * 活化：再次启动服务器后，从文件中加载数据到Session中；
  
* Session销毁：

  * 默认情况下，无操作30分钟自动销毁；
  * 也可以在该项目的web.xml文件下配置：

  ```xml
  <session-config>
      <session-timeout>时间(以分钟为单位)</session-timeout>
  </session-config>
  ```

  * 调用Session对象的invalidate()方法；



# Filter

* Filter表示过滤器，是JavaWeb三大组件(Servlet、Filter、Listener)之一；
* 过滤器可以把对资源的请求拦截下来，从而实现一些特殊功能；
* 过滤器一般完成一些通用操作，比如：权限控制、统一编码处理、敏感字符处理等等；



## Filter的基本使用

* 定义一个类，实现Filter接口，并重写其所有方法；
* 配置Filter拦截资源的路径：在类上定义@WebFilter注解，里面写要拦截资源的路径；
* 在doFilter方法中实现其他功能，最后再放行；



### Filter拦截路径配置

* Filter可以根据需求，配置不同的拦截资源路径；
  * 拦截具体的资源：/index.jsp：只有访问index.jsp时才会被拦截；
  * 目录拦截：/user/*：访问/user下的所有资源，都会被拦截；
  * 后缀名拦截：*.jsp：访问后缀名为jsp的资源，都会被拦截；
  * 拦截所有：/*：访问所有资源，都会被拦截；



* 过滤器链：一个web应用，可以配置多个过滤器，这多个过滤器称为过滤器链；
* 注解配置的Filter，优先级按照过滤器类名(字符串)的自然排序；



# Listener

* Listener表示监听器，是javaWeb三大组件(Servlet、Filter、Listener)之一；
* 监听器可以监听就是在application，session，request三个对象创建、销毁或者往其中添加修改删除属性时自动执行代码的功能组件；
* Listener分类：javaWeb中提供8个监听器；

![image-20231127220240850](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20231127220240850.png)



# JSON数据和Java对象的转换

* Fastjson是阿里巴巴提供的一个Java语言编写的高性能功能完善的JSON库，是目前Java语言中最快的JSON库，可以实现Java对象和JSON字符串的相互转换；

* 使用：

  * 导入Fastjson依赖坐标：

  ```xml
  <!-- 导入Fastjson依赖坐标 -->
  <dependency>
  	<groupId>com.alibaba</groupId>
      <artifactId>fastjson</artifactId>
      <version>1.2.62</version>
  </dependency>
  ```

  * Java对象转为JSON格式：

  ```java
  String json = JSON.toJSONString(Java对象);
  ```

  * JSON字符串转为Java对象：

  ```java
  User user = JSON.parseObject(JSON字符串,Java对象.class);
  ```

  * 写入页面时，如果数据中有中文，注意字符集的转换；
  * 从页面不能直接用请求对象的getParameter方法获取，要获取页面请求体的数据再使用fastjson中的方法将json数据转为Java对象；



# lombok

![image-20230523223201949](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230523223201949.png)

* mybatis的动态sql语句：可以使用#{}的形式来声明，会在预编译的时候变成JDBC里的?占位符
* 日志输出：可以再properties配置文件中指定下面的话：<img src="C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230524125810645.png" alt="image-20230524125810645" style="zoom:67%;" />

主键返回：在数据添加成功后，需要获取插入数据库数据的主键

![image-20230524140239061](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230524140239061.png)

数据封装：

* 实体类属性名和数据库表查询返回的字段名一致，mybatis会自动封装
* 实体类属性名和数据库表查询返回的字段名不一致，mybatis不会自动封装
  * 解决方案一：查询时，给字段区别名，别名应于实体类属性名一致
  * 解决方案二：通过@Results与@Result注解手动映射封装，例如：![image-20230524155715197](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230524155715197.png)
  * 解决方案三：开启mybatis的驼峰命名自动映射开关，如：![image-20230524155843561](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20230524155843561.png)
* concat函数：可以在sql中拼接字符串，更安全，防止sql注入等问题，例如：concat('%',#{name},'%')







