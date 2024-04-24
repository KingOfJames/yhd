# Spring

* Sping框架的核心特性是可以用于开发任何Java应用程序，但是在JavaEE平台上构建web应用程序是需要扩展的。Spring框架的目标是使J2EE开发变得更容易使用，通过启用基于POJO变成模型来促进良好的编程实践；



## Spring Framework

* Spring Framework特性：
  * 非侵入式：使用Spring Framework开发应用程序时，Spring对应用程序本身的结构影响非常小。对领域模型可以做到零污染；对功能性组件也只需要使用几个简单的注解进行标记，完全不会破坏原有的结构，反而能将组件结构进一步简化。这就使得基于Spring Framework开发应用程序时结构清晰、简洁优雅；
  * 控制反转：IOC--Inversion of Control，翻转资源获取方向。把自己创建资源、向环境索取资源变成环境将资源准备好，我们享受资源注入；
  * 面向切面编程：AOP--Aspect Oriented Programming，在不修改源代码的基础上增强代码功能；
  * 容器：Spring IOC是一个容器，因为它包含并且管理组件对象的生命周期。组件享受到了容器化的管理，替程序员屏蔽了组件创建过程中的大量细节，极大的降低了使用门槛，大幅度提高了开发效率；
  * 组件化：Spring实现了使用简单的组件配置组合成一个复杂的应用。在Spring中可以使用XML和Java注解组合这些对象。这使得我么可以基于一个个功能明确、边界清晰的组件有条不紊的搭建超大型复杂应用系统；
  * 声明式：很多以前需要编写代码才能实现的功能，现在只需要声明需求即可由框架代为实现；
  * 一站式：在IOC和AOP的基础上可以整合各种企业应用的开源框架和优秀的第三方类库。并且Spring旗下的项目已经覆盖了广泛领域，很多方面的功能性需求可以在Spring Framework的基础上全部使用Spring来实现；
* Spring Framework五大功能模块：
  * Core Container：核心容器，在Spring环境下使用任何功能都必须基于IOC容器；
  * AOP&Aspects：面向切面编程；
  * Testing：提供了对junit或TestNG测试框架的整合；
  * Data Access/Integration：提供了对数据访问/集成的功能；
  * Spring MVC：提供了面向Web应用程序的集成功能；



## IOC

* 传统方式获取资源：在应用程序中的组件需要获取资源时，传统的方式是组件主动的从容器中获取所需要的资源，在这样的模式下开发人员往往需要知道在具体容器中特定资源的获取方式，增加了学习成本，同时降低了开发效率；
* 反转控制方式获取资源：反转控制的思想完全颠覆了应用程序组件获取资源的传统方式：反转了资源的获取方向--改由容器主动的获取资源推送给需要的组件，开发人员不需要知道容器是如何创建资源对象的，只需要提供接收资源的方式即可，极大的降低了学习成本，提高了开发的效率。这种行为也称为查找的被动形式；
* DI：Dependency Injection--依赖注入；DI是IOC的另一种表述方式：即组件以一些预先定义好的方式(例如：setter方法)接受来自于容器的资源注入。相对于IOC而言，这种表述更直接；IOC就是一种反转控制的思想，二DI是对IOC的一种具体实现；
* IOC容器在Spring中的实现：IOC容器中管理的组件也叫做bean。在创建bean之前，首先需要创建IOC容器
  * BeanFactory接口：这是IOC容器的基本实现，是Spring内部使用的接口。面向Spring本身，不提供给开发人员使用；
  * ApplicationContext接口：BeanFactory的子接口，提供了更多的高级特性。面向Spring的使用者，几乎所有场合都是使用ApplicationContext接口而不是BeanFactory接口；
  * ApplicationContext的主要实现类：![image-20240226000045789](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240226000045789.png)
    * ClassPathXmlApplicationContext：通过读取类路径下的XML配置文件创建IOC容器对象（相对路径）；
    * FileSystemXmlApplicationContext：通过文件系统路径读取XML配置文件创建IOC容器对象（绝对路径）；
    * ConfigurableApplicationContext：ApplicationContext的子接口，包含一些扩展方法refresh()和close()，让ApplicationContext具有启动、关闭和刷新上下文的能力；
    * WebApplicationContext：专门为Web应用准备，基于Web环境创建IOC容器对象，并将对象引入存入ServletContext域中；



### 基于XML管理bean

* 引入依赖：

  ```xml
  <dependencies>
  	<!-- 基于Maven依赖传递性，导入spring-context依赖即可导入当前所需所有jar包 -->
      <dependency>
      	<groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.3.1</version>
      </dependency>
      <!-- junit测试 -->
      <dependency>
      	<groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
          <scope>test</scope>
      </dependency>
  </dependencies>
  ```

* applicationContext.xml配置文件解析：

  * `<bean>`标签：配置一个bean对象，将对象交给IOC容器管理；
    * id属性：bean的唯一标识，不能重复；
    * class属性：设置bean对象所对应全类名；
    * scope属性：设置bean的作用域；
      * singleton（单例）：表示获取该bean所对应的对象都是同一个；
      * prototype（多例）：表示获取该bean所对应的对象都不是同一个；
      * request：在一个请求范围内有效（只在WebApplicationContext环境下才有）；
      * session：在一个会话范围内有效（只在WebApplicationContext环境下才有）；
  * `<property>`标签：通过bean对象成员变量的set方法进行依赖注入；
    * name属性：填写该bean对象的属性名称；
    * value属性：设置为属性赋值；
    * ref属性：引用IOC容器中的某个bean的id；
  * `<constructor-arg>`标签：通过bean对象的构造器进行依赖注入；
    * value属性：为匹配的构造器的某一个参数赋值；
    * name：指定参数名；
    * index属性：指定参数所在位置的索引（从0开始）；
    * ref属性：引用IOC容器中的某个bean的id；
  * `<null />`标签：为某个依赖注入设置为null；
  
* 写一些特殊符号时，可以使用XML实体来替代，例如：'<'可表示为`&lt;`。也可以使用CDATA节(CDATA节中的内容会原样解析)，例如：`<![CDATA[值]]>`

* 内部bean，只能在当前bean的内部使用，不能直接通过IOC容器获取；

* 配置一个集合类型的bean，需要使用util的约束：![image-20240228000453811](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240228000453811.png)

* 引入jdbc.properties文件，需要使用context约束：可以通过${key}的方式访问value；
  * ![image-20240228002059474](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240228002059474.png)



### bean的生命周期

* 实例化
* 依赖注入
* 初始化之前：后置处理器的postProcessBeforeInitialzation
* 初始化(依赖init-method属性指定初始化的方法)
* 初始化之后：后置处理器的postProcessAfterInitialzation
* 销毁(依赖destory-method属性指定销毁的方法)：在IOC容器关闭时销毁



* 若bean的作用域为单例时，生命周期的前三个步骤会在获取IOC容器时执行；若bean的作用域为多例时，生命周期的前三个步骤会在获取bean时执行；

* bean的后置处理器会在生命周期的初始化前后添加额外的操作：
  * 需要实现BeanPostProcess接口且配置到IOC容器中
  * 注意：bean后置处理器不是单独针对某一个bean生效，而是针对这一个IOC容器生效



### 自动装配

* 根据指定的策略，在IOC容器中匹配某个bean，自动为bean中的类类型的属性或接口类型的属性赋值，可以通过bean标签中的**autowire**属性设置自动装配的策略；

* 自动装配的策略：
  * no与default策略：表示不装配，即bean中的属性不会自动匹配某个bean为属性赋值，此时属性使用默认值；
  * byType策略：根据需要赋值的属性的类型，在IOC容器中匹配某个bean为属性赋值；
    * 若通过类型没有找到任何一个类型匹配的bean，此时不装配，属性为默认值；
    * 若通过类型找到了多个类型匹配的bean，此时会抛出异常：NoUniqueBeanDefinitionException；
    * 当使用byType实现自动装配时，IOC容器中有且仅有一个类型匹配的bean能够为属性赋值；
    * byName策略：将需要赋值的属性的属性名作为bean的id在IOC容器中匹配某个bean为属性赋值；
      * 当类型匹配的bean有多个时，此时可以使用byName策略实现自动装配；



### 基于注解管理bean

* 注解：
  * @Componet：将类标识为普通组件；
  * @Controller：将类标识为控制层组件；
  * @Service：将类标识为业务层组件；
  * @Repository：将类标识为持久层组件；
  * @Autowired：实现自动装配功能；
    * 基于该注解标识的成员变量，不需要再设置set方法；注意：基于xml形式的自动装配是需要给成员变量设置get和set方法的；
    * 该注解还可以标注在set方法上；
    * 该注解还可以标注在为当前成员变量赋值的有参构造上；
    * 原理：
      * 默认通过byType的方式，在IOC容器中通过类型匹配某个bean为属性赋值；
      * 当有多个类型相同的bean时，会使用byName的方式（利用你将赋值的属性的属性名作为该bean的id），在IOC容器中通过id匹配某个bean为属性赋值；
      * 当byType和byName都不生效时，会报异常：NoUniqueBeanDefinitionException；
  * @Qualifier：通过该注解的value属性，指定某个bean的id，将这个bean为属性赋值；与@Autowired注解一起使用；
  * @Configuration：将当前类标识为配置类；
  * @ComponentScan：扫描指定包下的组件；
  * @Bean：可以将标识的方法的返回值作为bean进行管理；
  
* 扫描组件：

  ```xml
  <context:componet-scan base-package="包名"></context:componet-scan>
  <!--
  	base-package属性：写要扫描的包名；
  	user-default-filter属性：true（默认）/false；
  								为true时，所设置的包下所有的类都需要扫描，此时可以使用排除扫描；
  								为false时，所设置的包下所有的类都不需要扫描，此时可以使用只扫描；
  -->
  ```

  * 排除扫描：

    ```xml
    <context:exclude-filter type="?" expression="全类名"></context:exclude-filter>
    <!-- type(设置排除扫描的方式)：1.annotation：根据注解的类型进行排除；2.assignable：根据类的类型进行排除 -->
    ```

  * 只扫描：与context:componet-scan标签的use-default-filters属性一起使用；

    ```xml
    <context:include-filter type="?" expression="全类名"></context:include-filter>
    <!-- type(设置只扫描的方式)：1.annotation：根据注解的类型进行只扫描；2.assignable：根据类的类型进行只扫描 -->
    ```

* 通过注解+扫描所配置的bean的id，默认值为类的小驼峰，即类名的首字母小写的驼峰命名规则



## AOP

* 概述：AOP（Aspect Oriented Programming）是一种设计思想，是软件设计领域中的面向切面编程，它是面向对象编程的一种补充和完善，它可以通过预编译方式和运行期动态代理方式实现在不修改源代码的情况下给程序动态统一添加额外功能的一种技术；
* AOP的相关术语：
  * 横切关注点：从目标对象的每个方法中抽取出来的同一类非核心业务。在同一个项目中，可以使用多个横切关注点对相关方法进行多个不同方面的增强（类似于计算器中的加减乘除的日志功能）；
  * 通知：每一个横切关注点上要做的事情都需要写一个方法来实现，这样的方法就叫通知方法；
    * 前置通知：在被代理的目标方法前执行；
    * 返回通过：在被代理的目标方法成功结束后执行；
    * 异常通知：在被代理的目标方法异常结束后执行；
    * 后置通知：在被代理的目标方法最终结束后执行；
    * 环绕通知：使用try..catch..finally结构围绕整个被代理的目标方法，包括上面四种通知对应的所有位置；
  * 切面：封装通知方法的类；
  * 目标：被代理的目标对象；
  * 代理：向目标对象应用通知之后创建的代理对象；
  * 连接点：抽取横切关注点的位置；![image-20240305215732975](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240305215732975.png)
  * 切入点：定位连接点的方式；每个类的方法中都包含多个连接点，所以连接点是类中客观存在的事物。如果把连接点看做数据库中的记录，那么切入点就是查询记录的sql语句。Spring的AOP技术可以通过切入点定位到特定的连接点。切点通过**org.springframework.aop.Pointcut**接口进行描述，它使用类和方法作为连接点的查询条件；
* 作用：
  * 简化代码：把方法中固定位置的重复代码**抽取**出来，让被抽取的方法更专注于自己的核心功能，提高内聚性；
  * 代码增强：把特定的功能封装到切面类中，看哪里有需要，就往上套，被**套用**了切面逻辑的方法就被切面给增强了；



### 代理模式

* 代理模式：二十三种设计模式中的一种，属于结构型模式。它的作用就是通过提供一个代理类，在调用目标方法的时候，不再直接对目标方法进行调用，而是通过代理类间接调用。让不属于目标方法核心逻辑的代码从目标方法中剥离出来--**解耦**。调用目标方法时先调用代理对象的方法，减少对目标方法的调用和打扰，同时让附加功能能够集中在一起也有利于统一维护；

  * 动态代理：动态代理也叫**JDK代理**、**接口代理**。JDK原生的实现方式，需要被代理的目标类必须实现接口，要求代理对象和目标对象实现同样的接口，代理对象的生成，是利用JDK的API，动态的在内存中构建代理对象。即使用JDK包java.lang.reflect.Proxy中的newProxyInstance方法来动态的创建目标对象（被代理对象）：`static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces,InvocationHandler h )`       

    ```java
    public class ProxyFactory {
    	//维护一个目标对象 , Object
    	private Object target;
    	//构造器 ， 对target 进行初始化
    	public ProxyFactory(Object target) {
    		this.target = target;
    	}
    	//动态生成一个代理对象
    	public Object getProxy() {
            /**
            *	ClassLoader loader：指定加载动态生成的代理类的类加载器；
            *	Class<?> interfaces：获取目标对象实现的所有接口的class对象的数组；
            *	InvocationHandler h：设置代理类中抽象方法如何重写；
            */
            ClassLoader cl = this.class.getClassLoader();
            Class<?> interfaces = target.class.getInterfaces();
            InvocationHandler ih = new InvocationHandler(){
                @Override
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    /**
                    *	proxy表示代理对象；
                    *	method表示要执行的方法；
                    *	args表示要执行的方法的参数列表；
                    */
                    System.out.println("动态代理开始");
                    //反射机制调用目标对象的方法
                    Object result = method.invoke(target, args);
                    System.out.println("动态代理结束");
                    return result;
                }
            }
            return Proxy.newProxyInstance(cl,interfaces,ih);
        }
    }
    ```

  * cglib代理：通过继承被代理的目标类实现代理，所以不需要目标类实现接口；

  * AspectJ：本质上是静态代理，将代理逻辑"织入"被代理的目标类编译得到的字节码文件中，所以最终效果是动态的。weaver就是织入器。Spring只是借用了AspectJ中的注解；



### 基于注解的AOP

![image-20240305222241514](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240305222241514.png)

* 导入AOP所需依赖（在IOC所需依赖的基础上再加）：

  ```xml
  <!-- spring-aspects会传递aspectjweaver -->
  <dependency>
  	<groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>5.3.1</version>
  </dependency>
  ```

* 基于注解的AOP：

  * @Aspect：将当前组件标识为切面；

  * @Before：将当前方法标识为某个目标对象的前置通知方法；

    * value属性（必须写）：书写切入点表达式；

    * 例如：

      ```java
      @Before("execution(* com.itkim.aop.annotation.*.*(..))")
      public void beforeAdviceMethod(JoinPoint joinPoint){
          //获取连接点的签名信息
          Signature signature = joinPoint.getSignature();
          //获取连接点的参数信息
          Object[] args = joinPoint.getArgs();
          System.out.println("Logger（前置通知）,方法:"+ signature.getName()+",参数:"+ Arrays.toString(args));
      }
      ```

    * 获取连接点信息：在该通知方法的参数位置，设置JoinPoint类型的参数，就可以获取连接点所对应方法的信息；

  * @PointCut：声明一个公共的切入点，可在@Before、@After等注解中使用该注解标注的方法名；

  * @After：将当前方法标识为某个目标对象的后置通知方法；

    * 例如：

      ```java
      @After("execution(* com.itkim.aop.annotation.*.*(..))")
      public void afterAdviceMethod(JoinPoint joinPoint){
          Signature signature = joinPoint.getSignature();
          System.out.println("Logger（后置通知）,方法:"+ signature.getName() +",执行完毕");
      }
      ```

  * @AfterReturning：将当前方法标识为某个目标对象的返回通知方法；

    * returning属性：指定该方法的某个参数为接收目标对象方法的返回值的参数；

    * 例如：

      ```java
      @AfterReturning(value = "execution(* com.itkim.aop.annotation.*.*(..))",returning = "result")
      public void afterReturningAdviceMethod(JoinPoint joinPoint,Object result){
          Signature signature = joinPoint.getSignature();
          System.out.println("Logger（返回通知）,方法:"+ signature.getName()+",结果:"+ result);
      }
      ```

  * @AfterThrowing：将当前方法标识为某个目标对象的异常通知方法；

    * throwing属性：指定该方法的某个参数为接收目标对象出现的异常的参数；

    * 例如：

      ```java
      @AfterThrowing(value = "execution(* com.itkim.aop.annotation.*.*(..))",throwing = "ex")
      public void afterThrowingAdviceMethod(JoinPoint joinPoint,Exception ex){
          Signature signature = joinPoint.getSignature();
          System.out.println("Logger（异常通知）,方法:"+ signature.getName() +",异常:"+ ex);
      }
      ```

  * @Around：将当前方法标识为某个目标对象的环绕通知方法，类似于动态代理的过程；

    * 例如：

      ```java
      @Around("execution(* com.itkim.aop.annotation.*.*(..))")
      public Object aroundAdviceMethod(ProceedingJoinPoint proceedingJoinPoint){
          Object result = null;
          try {
              System.out.println("环绕通知--前置通知");
              //proceed()方法就是动态代理中利用反射执行目标对象方法的步骤
              result = proceedingJoinPoint.proceed();
              System.out.println("环绕通知--返回通知");
          } catch (Throwable e) {
              e.printStackTrace();
              System.out.println("环绕通知--异常通知");
          }finally {
              System.out.println("环绕通知--后置通知");
          }
          return result;
      }
      ```

  * @Order：设置切面的优先级；默认值为Integer的最大值，值越小，优先级越高；

* 切入点表达式：`execution(* 包名.*.*(..));`

  * 第一个*表示任意的访问修饰符和返回值类型；

  * 第二个*表示该包下任意的类；

  * 第三个*表示该类中任意的方法；

  * ..表示任意的参数列表；

  * 切入点表达式的复用：

    ```java
    //单独写一个任意名字的方法，但最好用pointCut的名字，然后使用@PointCut注解
    @PointCut("execution(* com.itkim.aop.annotation.*.*(..))")
    public void pointCut(){}
    
    @Before("pointCut()")
    public void beforeAdviceMethod(JoinPoint joinPoint){
        //获取连接点的签名信息
        Signature signature = joinPoint.getSignature();
        //获取连接点的参数信息
        Object[] args = joinPoint.getArgs();
        System.out.println("Logger（前置通知）,方法:"+ signature.getName()+",参数:"+ Arrays.toString(args));
    }
    ```

    

* AOP的注意事项：

  * 切面类和目标类都要交给IOC容器管理；

  * 切面类必须通过@Aspect注解标识为一个切面类；

  * 必须在spring的配置文件中开启基于注解的AOP功能：

    ```xml
    <aop:aspectj-autoproxy />
    ```




### 基于xml的AOP

* xml文件解析：
  * `<aop:config>`标签：为当前项目设置aop切面的配置；
    * `<aop:pointcut>`标签：设置一个公共的切入点；
      * id属性：设置该切入点表达式的唯一id标识；
      * expression属性：切入点；
    * `<aop:aspect>`标签：将IOC容器中的某个bean设置为切面类；
      * ref属性：引用IOC容器中的某个bean；
      * order属性：为该切面类设置优先级；
      * `<aop:before>`标签：将某个方法标识为该切面类的前置通知方法；
        * method属性：标识该切面类中的某个方法为该标签所代表的通知方法；
        * pointcut属性：切入点；
        * pointcut-ref属性：引入公共的切入点；
      * `<aop:after>`标签：将某个方法标识为该切面类的后置通知方法；
      * `<aop:after-returning>`标签：将某个方法标识为该切面类的返回通知方法；
        * returning属性：指定该方法的某个参数为接收目标对象方法的返回值的参数；
      * `<aop:after-throwing>`标签：将某个方法标识为该切面类的异常通知方法；
        * throwing属性：指定该方法的某个参数为接收目标对象出现的异常的参数；
      * `<aop:around>`标签：将某个方法标识为该切面类的环绕通知方法；
    * `<aop:advisor>`标签：设置一个声明式事务的切入点及事务通知；
      * advice-ref属性：引用指定id的事务通知；
      * pointcut属性：指定切入点；



## 声明式事务

### JdbcTemplate

* Spring框架对JDBC进行封装，使用JdbcTemplate方便实现对数据库操作；

* 导入依赖（在IOC所需依赖上加入）：

  ```xml
  <!-- Spring持久层支持jar包 -->
  <!-- Spring在执行持久化层操作、与持久化层技术进行整合过程中，需要使用orm、jdbc、tx三个jar包 -->
  <!-- 导入orm包就可以通过maven的依赖传递性把其他两个也导入 -->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
      <version>5.3.1</version>
  </dependency>
  <!-- Spring测试相关 -->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.3.1</version>
  </dependency>
  <!-- MySQL驱动 -->
  <dependency>
      <groupId>com.mysql</groupId>
      <artifactId>mysql-connector-j</artifactId>
      <version>8.1.0</version>
  </dependency>
  <!-- 数据源 -->
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.2.8</version>
  </dependency>
  ```

* 配置xml文件：![image-20240306231705885](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240306231705885.png)

* spring-test中需要的注解：

  * @RunWith注解：指定当前测试类在Spring的测试环境中执行，此时就可以通过依赖注入的方式直接获取IOC容器中的bean；
    * 需要填入SpringJUnit4ClassRunner.class；
  * @ContextConfiguration注解：设置Spring测试环境的配置文件；
    * 需要填入你所需要的配置文件的目录，例如：classpath:applicationContext.xml；



### 声明式事务

* 编程式事务：事务功能的相关操作全部通过自己编写代码来实现，例如：![image-20240306231805894](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240306231805894.png)
  * 存在的缺陷：
    * 细节没有被屏蔽：具体操作过程中，所有细节都需要自己完成，比较繁琐；
    * 代码复用性不高：如果没有有效抽取出来，每次实现功能都需要自己编写代码，代码没有得到复用；

* 声明式事务：由于事务控制的代码有迹可循，代码的结构基本是确定的，所以框架就可以将固定模式的代码抽取出来，进行相关的封装，封装后只需要在配置文件中进行简单的配置即可完成操作；
  * 好处：
    * 提高开发效率；
    * 消除了冗余代码；
    * 框架会综合考虑相关领域中在实际开发环境下有可能遇到的各种问题，进行了健壮性、性能等各个方面的优化；



### 基于注解的声明式事务

* 基于注解的声明式事务的配置步骤：
  * 在Spring的配置文件中配置事务管理器；
  * 开启事务的注解驱动；
  * 在需要被事务管理的方法或类上，添加@Transactional注解，该方法或类中所有方法就会被事务管理；

* 基于注解的声明式事务：
  * @Transactional：标识的方法或类中所有方法使用事务进行管理

* 在配置配置文件：![](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240306235658024.png)
  * `<tx:annotation-driven />`标签：用来开启事务的注解驱动
    * transaction-manager属性：用来设置事务管理器的id；若事务管理器的id为transactionManager，则该属性可以不写；

* 事务属性：

  * 只读：将@Transactional注解中的**readOnly(默认值为false)属性设置为true**；对一个查询操作来说，如果设置成只读，就能够明确告诉数据库，这个操作不涉及写操作。这样数据库就能针对查询操作来进行优化；**注意**：如果对增删改操作设置只读就会抛出SQLException异常；

  * 超时：将@Transactional注解中的**timeout(默认值为-1)属性设置为你设置的时间(以秒为单位)**；超时强制回滚，抛出异常，释放资源；事务在执行过程中，有可能因为遇到某些问题，导致程序卡住，从而长时间占用数据库资源。而长时间占用数据库资源，大概率是因为程序运行出现了问题。此时这个很可能出现问题的程序应该被回滚，撤销它已做的操作，事务结束，把资源让出来，让其他正常程序可以执行；如果超时还未完成事务将会抛出TransactionTimeException异常；
  * 回滚策略：声明式事务**默认只针对运行时异常回滚**，编译时异常不回滚；
    * 通过@Transactional注解中的rollbackFor属性：需要设置一个Class类型的对象，针对某些抛出的异常回滚；
    * 通过@Transactional注解中的rollbackForClassName属性：需要设置一个字符串类型的全类名；
    * 通过@Transactional注解中的noRollbackFor属性：需要设置一个Class类型的对象，不管某些抛出的异常不回滚；
    * 通过@Transactional注解中的noRollbackForClassName属性：需要设置一个字符串类型的全类名；
  * 事务的隔离级别：将@Transactional注解中的**isolation属性设置为你想设置的隔离级别(Isolation是一个枚举类)**；数据库系统必须具有隔离并发运行各个事务的能力，使它们不会相互影响，避免各种并发问题。一个事务与其他事务隔离的程度称为隔离级别。SQL标准中规定了多种事务隔离级别，不同隔离级别对应不同的干扰程度，隔离级别越高，数据一致性越好，但并发性越弱；
    * 读未提交(READ UNCOMMITED)：允许事务1读取事务2中未提交的修改（会造成脏读、不可重复读、幻读）；
    * 读已提交(READ COMMITED)：要求事务1只能读取事务2中已提交的修改（会造成不可重复读、幻读）；
    * 可重复读(REPEATABLE READ)：确保事务1可以多次从一个字段中读取到相同的值，即事务1执行期间禁止其它事务对这个字段进行更新(相当于加了锁)（会造成幻读）；
    * 串行化(SERIALIZABLE)：确保事务1可以多次从一个表中读取到相同的行，在事务1执行期间，禁止其它事务对这个表进行添加、更新、删除操作。可以避免任何并发问题，但性能低下；
    * 事务的传播行为：将@Transactional注解中的**propagation属性设置事务传播行为**；当事务方法被另一个事务方法调用时，必须指定事务应该如何传播。



### 基于xml的声明式事务

* xml的声明式事务配置步骤：
  * 配置事务管理器；
  * 配置事务通知：![image-20240307005736518](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240307005736518.png)
    * `<tx:attributes>`标签：与事务通知一起使用，用来指定某个方法的事务属性，其内部自带的属性与基于注解的声明式事务用法相同；![image-20240307010439533](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240307010439533.png)
      * name属性：指定方法名，可以使用*号代表多个字符
  * 配置事务连接点：![image-20240307010114974](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240307010114974.png)
* 注意：使用基于xml的声明式事务时，是基于AOP思想使用的，所以要引入AOP所需要的依赖，否则会抛出异常；

![image-20240307010932217](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240307010932217.png)



# Spring MVC

## MVC

* MVC是一种软件架构思想，将软件按照模型、视图、控制器来划分；
  * M：Model，模型层，指工程中的Javabean，作用是处理数据；
  * Javabean分为两类：
    * 一类是实体类bean：专门存储业务数据，如Student、User等；
    * 一类是业务处理bean：指Service或Dao对象，专门用于处理业务逻辑和数据访问；
  * V：View，视图层，指工程中的heml或jsp页面等，作用是与用户进行交互，展示数据；
  * C：Controller，控制层，指工程中的servlet，作用是接收请求和响应浏览器；
* MVC的工作流程：用户通过视图层发送请求到服务器，在服务器中请求被Controller接收，Controller调用相应的Model层处理请求，处理完毕将结果返回到Controller，Controller再根据请求处理的结果找到相应的View视图，渲染数据后最终响应给浏览器；



### SpringMVC的使用

* 注解：

  * @RequestMapping：将请求和处理请求的控制器方法关联起来，建立映射关系；SpringMVC接收到指定的请求，就会来找到在映射关系中对应的控制器方法来处理该请求；
    * 该注解标注在类上时：设置映射请求的请求路径的初始信息；
    * 该注解标注在方法上时：设置映射请求的请求路径的具体信息；
    * 该注解可以有多个请求路径：当**value(String数组类型)**的值有多个时，那么这些请求路径都可以映射到该控制器方法或类上，以此来减少重复代码的书写；
    * 该注解可以有多个请求方式：当**method(RequestMethod数组类型)**的值有多个时，浏览器所发送的请求的请求方式满足其中任意一种，都会被该注解标识的方法处理；
    * **params(String数组类型)**属性：通过请求的请求参数匹配请求，即浏览器发送的请求的请求参数必须满足params属性的设置；有四种方式：
      * "param"：表示当前所匹配请求的请求参数中必须携带param参数；
      * "!param"：表示当前所匹配请求的请求参数中不能携带param参数；
      * "param=value"：表示当前所匹配请求的请求参数中必须携带param参数且值必须为value；
      * "param!=value"：表示当前所匹配请求的请求参数中可以不携带param，但携带了值就不能为value；
    * **headers(String数组类型)**属性：通过请求的请求头信息匹配请求，即浏览器发送的请求的请求头信息必须满足headers属性的设置。用法与params属性相同；
  * 在@RequestMapping的基础上，结合请求方式的一些派生注解：
    * @GetMapping：get请求，用来获取资源；
    * @PostMapping：post请求，用来新建资源；
    * @DeleteMapping：delete请求，用来删除资源；
    * @PutMapping：put请求，用来更新资源；
  * @RequestBody：用于接收json数据；（用于有请求体时使用），在SpringMVC中要导入jackson依赖，也要在配置文件中开启SpringMVC的注解驱动；
  * @RequestParam：用于接收url地址传参或表单传参；（用于发送非json格式数据时使用）；
  * @PathVariable：用于接收路径参数，使用{参数名称}描述路径参数；（采用RESTful进行开发时，参数数量较少时使用）；
  * @RequestHeader：用于获取此次发送过来的请求的请求头信息；
  * @ResponseBody：用来将返回的数据转为JSON格式，在SpringMVC中要导入jackson依赖，也要在配置文件中开启SpringMVC的注解驱动；
  * @RestController：是@Controller和@ResponseBody的组合，具有两个都有的功能；
  * @EnableWebMvc：开启MVC的注解驱动；
  * @RequestAttribute：获取一个请求作用域中的数据；

* 引入依赖：

  ```xml
  <!-- SpringMVC依赖 -->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.1</version>
  </dependency>
  
  <!-- 必须引入logback依赖 -->
  <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.3</version>
  </dependency>
  
  <!-- 引入servlet依赖 -->
  <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
  </dependency>
  
  <!-- 引入thymeleaf页面渲染框架依赖 -->
  <dependency>
      <groupId>org.thymeleaf</groupId>
      <artifactId>thymeleaf-spring5</artifactId>
      <version>3.0.12.RELEASE</version>
  </dependency>
  ```

* 配置web.xml文件：

  ```xml
  <servlet>
      <servlet-name>SpringMVC</servlet-name>
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      <!--            自定义springMVC的配置文件目录-->
      <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:springmvc.xml</param-value>
      </init-param>
      <!--            设置初始化DispatcherServlet的时间，1表示在服务器开始时-->
      <load-on-startup>1</load-on-startup>
  </servlet>
  
  <servlet-mapping>
      <servlet-name>SpringMVC</servlet-name>
      <url-pattern>/</url-pattern>
  </servlet-mapping>
  ```

* 配置springMVC.xml文件：

  ```xml
  <context:component-scan base-package="com.itkim"></context:component-scan>
  
  <!--    配置thymeleaf视图解析器-->
  <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
      <property name="order" value="1"></property>
      <property name="characterEncoding" value="UTF-8"></property>
      <property name="templateEngine">
          <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
              <property name="templateResolver">
                  <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                      <!--                        视图前缀-->
                      <property name="prefix" value="/WEB-INF/templates/" />
                      <!--                        视图后缀-->
                      <property name="suffix" value=".html"></property>
                      <property name="templateMode" value="HTML5" />
                      <property name="characterEncoding" value="UTF-8"></property>
                  </bean>
              </property>
          </bean>
      </property>
  </bean>
  
  <!-- 设置SpringMVC开启注解驱动 -->
  <mvc:annotation-driven></mvc:annotation-driven>
  ```



### SpringMVC获取请求参数

* 通过原生的ServletAPI获取：在控制器方法的形参上设置HttpServletRequest、HttpServletResponse等原生的ServletAPI；
* 通过控制器方法的形参获取：设置控制器方法的形参的参数名和请求路径的参数名相同即可；如果名字不同就会默认为null；



### 域共享数据

* 通过ModelAndView像请求域共享数据：使用ModeAndView时，可以使用其model功能向请求域共享数据，使用View功能设置逻辑视图，但是控制器方法的返回值必须是ModelAndView类型；
* 使用Model向请求域共享数据；
* 使用ModelMap向请求域共享数据；
* 使用Map向请求域共享数据；
* Model、ModelMap和Map的关系：在底层，都是通过BindingAwareModelMap创建![image-20240310215524763](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240310215524763.png)

* 向Session域共享数据：控制器方法的形参上必须有HttpSession类型的参数，然后向这个session中添加要共享的数据；
* 向application域共享数据：控制器方法的形参上必须有HttpSession类型的参数，用该session获取出ServletContext对象，这个对象就是应用域对象，然后向这个application中添加要共享的数据；



## REST开发

REST(Representational State Transfer)，表现层资源状态转移；

* 传统风格资源描述形式：
  * http://localhost/user/getById?id=1
  * http://localhost/user/saveUser
* REST风格描述形式：
  * http://localhost/user/1
  * http://localhost/user
* 优点：
  * 隐藏资源的访问行为，无法通过地址得知对资源是何种操作；
  * 简化书写；



## 文件上传和下载

### 文件下载

* ResponseEntity用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文，使用ResponseEntity实现下载文件功能；

![image-20240312211554480](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240312211554480.png)



### 文件上传

* 导入依赖：

  ```xml
  <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.1</version>
  </dependency>
  ```

* 配置文件上传解析器：

  ```xml
  <!--    配置文件上传解析器-->
  <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
      <property name="defaultEncoding" value="UTF-8"></property>
  </bean>
  ```

  ![image-20240312220941149](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240312220941149.png)

  

## 拦截器

* 拦截器的配置：
  * SpringMVC中的拦截器用于拦截控制器方法的执行；
  * SpringMVC中的拦截器需要实现HandlerInterceptor；
    * preHandler方法：目标方法执行之前；返回true为放行，返回false为不放行；
    * postHandler方法：目标方法执行之后；
    * afterHandler方法：页面渲染之后；
  * SpringMVC的拦截器必须在SpringMVC的配置文件中进行配置：![image-20240312222112824](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240312222112824.png)



## 异常处理

* 通过注解配置异常处理：
  * @ControllerAdvice：将当前类标识为异常处理的组件；
  * @ExceptionHandler：设置要处理的异常信息；



## SpringMVC常用组件

* DispatcherServlet：前端控制器，由框架提供；
  * 作用：统一处理请求和响应，整个流程控制的中心，由它调用其他组件处理用户的请求；
* HandlerMapping：处理器映射器，由框架提供；
  * 作用：根据请求的url、method等信息查找Handler，即控制器方法；
* Handler：处理器
  * 作用：在DispatcherServlet的控制下Handler对具体的用户请求进行处理；
* HandlerAdapter：处理器适配器，由框架提供；
  * 作用：通过HandlerAdapter对处理器(控制器方法)进行执行；
* ViewResolver：视图解析器，由框架提供；
  * 作用：进行视图解析，得到相应的视图，例如：ThymeleafView、InternalResourceView、RedirectView等；
* View：视图；
  * 作用：将模型数据通过页面展示给用户； 



# SpringBoot2

* SpringBoot是Spring提供的一个子项目，用于快速构建Spring应用程序；
* 特性：
  * 起步依赖：本质上就是一个Maven坐标，整合完成一个功能需要的所有坐标；
  * 自动配置：**遵循约定大约配置的原则，在boot程序启动后，一些bean对象会自动注入到ioc容器，不需要手动声明，简化开发；**
  * 其它特性：
    * 内嵌的tomcat、jetty（无需部署war文件）；
    * 外部化配置；
    * 不需要xml配置（properties/yml）；



## pom文件解析

* boot工程的父工程，用于管理起步依赖的版本：

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.0</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```



* web的起步依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```



* 想要改变扫描路径：
  * 第一种：在启动类的@SpringBootApplication(scanBasePackages="com.itkim")；
  * 第二种：在启动类上使用@ComponentScan指定扫描路径；



## SpringBoot2注解

* @SpringBootApplication：springboot的启动注解；
  * scanBasePackage属性：指定扫描的包路径；
* @ComponentScan：指定扫描的包路径；
* @Configuration：告诉Springboot这是一个配置类（本身也是一个组件） == 配置文件；
  * proxyBeanMethods属性：代理bean的方法，可以处理组件依赖，有两个值：
    * true：表示为Full模式，该模式下，如果某个方法返回值为一个对象，并使用了@Bean（默认是单实例对象）注解，在调用这个方法时，SpringBoot会先从IOC容器中查找；
    * false：表示为Lite模式，该模式下，如果某个方法返回值为一个对象，并使用了@Bean注解，在调用这个方法时，SpringBoot不会从IOC容器中查找；
  * 配置类组件之间无依赖关系用Lite模式加速容器启动过程，减少判断；
  * 配置类组件之间有依赖关系，方法会被调用得到之前的单实例组件，用Full模式；
* @Import：指定类型引入组件，放入IOC容器中，默认id为全类名；
* @Conditional：条件装配，满足Conditional指定的条件，则进行组件注入；![image-20240316222613087](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240316222613087.png)
* @ImportResouce：指定导入某个spring.xml的配置文件；
* @ConfigurationProperties：可以与yml配置文件对应的前缀、对应的属性进行值绑定，要是组件；
  * prefix属性：指定yml配置文件中要获取的前缀名；
* @EnableConfigurationProperties：开启某个类型的配置绑定功能，把某个类型自动注册到容器中；
  * 该类型必须要有@ConfigurationProperties注解标识；
* @CookieValue：用于获取此次发送过来的请求的Cookie值；
  * 如果页面开发中，cookie被禁用了，session里面的内容怎么使用：
    * 使用url重写+矩阵变量的方式进行传递cookie的值，例如：/abc;jsessionid=xxxx
* @MatrixVariable：矩阵变量，SpringBoot默认禁用了矩阵变量功能；
  * 矩阵变量的书写：![image-20240317144257814](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240317144257814.png)
  * 在获取时，要如下书写控制器方法，才能访问到：![image-20240317145444347](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240317145444347.png)
  * pathVar属性：指定获取restful风格下该值的对应的矩阵变量的值；



## 注意

* 开发SpringBoot程序要继承spring-boot-starter-parent；
* spring-boot-starter-parent中定义了若干个依赖管理；
* 继承parent模块可以避免多个依赖使用相同技术时出现依赖版本冲突；
* 继承parent的形式也可以采用引入依赖的形式实现效果；
* 开发SpringBoot程序需要导入坐标时通常导入对应的starter；
* 每个不同的starter根据功能不同，通常包含多个依赖坐标；
* 使用starter可以实现快速配置的效果，达到简化配置的目的；
* SpringB哦哦他的引导类是Boot工程的执行入口，运行main方法就可以启动项目；
* SpringBoot工程运行后初始化Spring容器，扫描引导类所在包加载bean；
* 各种配置拥有默认值：配置文件的值最终都会绑定到需要的每一个类上，这个类会在IOC容器中创建对象；
* SpringBoot所有的自动配置功能都在spring-boot-autoconfigure包中



## 跨域问题的解决方法

* 使用@CrossOrigin注解：
  * origins属性：用来配置将要跨域的地址；
  * 例如：@CrossOrigin(origins = "http://127.0.0.1:5500")



## SpringBoot提供的配置文件格式

SpringBoot提供了三种配置文件格式

* properties：传统格式/默认格式

  * 例如：server.port=80

* yml：主流格式

  * 例如：

    ```yml
    server:
    	port: 80
    ```

* yaml

  * 例如：

    ```yaml
    server:
    	port: 80
    ```
  
  * yaml语法规则：
    * 大小写敏感；
    * 属性层级关系使用多行描述，每行结尾使用冒号结束；
    * 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）；
    * 属性值前面添加空格（属性名与属性值之间使用冒号+空格作为分隔）；
    * #：表示注释；![image-20240109141851690](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240109141851690.png)
  
  * yaml数据读取：
    * 在某个属性使用@Value注解；
    * 封装全部数据到Environment对象：使用@Autowired注解自动装配数据到Environment对象中；
    * 自定义对象封装指定数据：![image-20240109145400106](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240109145400106.png)

## 整合第三方技术

### 整合JUnit

* 名称：@SpringBootTest

* 类型：测试类注解

* 位置：测试类定义上方

* 作用：设置JUnit加载的SpringBoot启动类

* 示例：

  ```java
  @SpringBootTest(classes = SpringbootApplication.class)
  class SpringBootApplicationTests{
      
  }
  ```

* 相关属性：classes：设置SpringBoot启动类

* 整合：

  * 导入测试对应的starter（springboot工程创建时就自动导入了）
  * 测试类使用@SpringBootTest修饰
  * 使用自动装配的形式添加要测试的对象



### 整合Mybatis

* 创建新模块，选择Spring初始化，并配置模块相关基础信息；

* 选择当前模块需要使用的技术集（Mybatis框架、MySQL驱动）；

* 设置数据源参数：

  ```yml
  spring:
    datasource:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/数据库名
      username: 用户名
      password: 密码
  ```

* 定义数据层接口与映射配置（用注解或配置文件）；

  * 用配置文件：

    * 创建mybatis全局配置文件

      ```xml
      <!-- 开启数据库中列名和pojp的驼峰命名映射 -->
          <settings>
              <setting name="mapUnderscoreToCamelCase" value="true"/>
          </settings>
      ```

      

    * 创建某个mapper的映射文件

    * 在yml中配置：

      ```yml
      # 加载mybatis的全局配置文件
      mybatis:
        config-location: classpath:mybatis/mybatis-config.xml
        mapper-locations: classpath:mybatis/mapper/*.xml
      ```

* 当数据库列名与实体类属性名不一致时，可以开启mybatis的驼峰命名配置：

  ```yml
  # 开启驼峰映射
  mybatis:
    configuration:
      map-underscore-to-camel-case: true
  ```

  



### 整合MyBatis-Plus

* 手动添加SpringBoot整合MyBatis-Plus的坐标，可以通过mvnrepository去获取；

  ```xml
  <dependency>
  	<groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>3.4.3</version>
  </dependency>
  ```

* 定义数据层接口与映射配置，继承BaseMapper

* 设置数据源参数；



### 整合Druid

* 导入Druid对应的starter：

  ```xml
  <dependency>
  	<groupId>com.alibaba</groupId>
      <artifactId>druid-spring-boot-starter</artifactId>
      <version>1.2.8</version>
  </dependency>
  ```

* 变更Druid的配置方式：

  ```yml
  spring:
    datasource:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/数据库名?serverTimezone=UTC
      username: 用户名
      password: 密码
      type: com.alibaba.druid.pool.DruidDataSource
  ```



### 整合lombok

* Lombok，一个Java类库，提供了一组注解，简化POJO实体类开发；

* 导入lombok依赖坐标：

  ```xml
  <dependency>
  	<groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
  </dependency>
  ```

* @Data注解：

  * 会自动生成get和set方法
  * 会自动生成toString、equals等方法



### 整合dev-tools

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>

<!-- 快捷键：Ctrl+F9(会自动重启SpringBoot) -->
```



### 自定义配置类在yml中提示

```xml
<!--        配置自定义提示-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <!--                在打包插件中配置，将下面的配置提示包排除，即打包时不需要打包此包-->
            <configuration>
                <excludes>
                    <exclude>
                        <groupId>org.springframework.boot</groupId>
                        <artifactId>spring-boot-configuration-processor</artifactId>
                    </exclude>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
```



## 表现层消息一致性处理

* 设计表现层返回结果的模型类，用于后端与前端进行数据格式统一，也称为前后端数据协议![image-20240110233459393](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240110233459393.png)

* 对异常进行统一处理，出现异常后，返回指定信息![image-20240113233921502](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240113233921502.png)
  * @ExceptionHandler：可以用来统一处理方法抛出的异常，被该注解处理的方法可以处理controller层其他方法抛出的异常；@ExceptionHandler注解是Spring框架提供的一种异常处理机制。通过在方法上添加@ExceptionHandler注解，可以指定该方法用于处理特定类型的异常。当程序中抛出指定类型的异常时，Spring会自动调用带有@ExceptionHandler注解的方法来处理异常。
  * @RestControllerAdvice：@RestControllerAdvice是一个注解，用于定义全局的异常处理器和全局数据绑定。它可以被用于一个类上，该类中的方法将会被应用到所有使用@RestController注解的控制器中。

* 可以在表现层Controller中进行消息统一处理；

![image-20240113234741768](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240113234741768.png)



## SpringBoot项目打包并启动

### Windows版

* 对SpringBoot项目打包(执行Maven构建指令：package)：mvn package

* 运行项目(执行启动指令，一般在该项目的target目录输入cmd进入，然后执行)：java -jar 项目名称.jar

* 注意：

  * jar支持命令行启动需要依赖maven插件支持，请确认打包时是否具有SpringBoot对应的maven插件

    ```xml
    <build>
    	<plugins>
        	<plugin>
            	<groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
    ```

![image-20240115211029995](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240115211029995.png)



### Linux版



### 配置文件分类

![image-20240115215229208](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240115215229208.png)



## 端口被占用时常用指令

![image-20240115211704147](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240115211704147.png)



## Spring Validation校验

* 引入Spring Validation起步依赖

  ```xml
  <dependency>
  	<groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-validation</artifactId>
  </dependency>
  ```

* 在参数前面添加@Pattern注解：例如：@Pattern(regexp = "正则表达式")String username；会去自动校验参数信息

* 在Controller类上添加@Validated注解



# Spring系列常用注解

## Spring常用注解

* **@Configuration**
  * 相当于传统的xml配置文件，如果有些第三方库需要用到xml文件，建议仍然通过@Configuration类作为项目的配置主类——可以使用@ImportResource注解加载xml配置文件。
  * 把一个类作为一个IoC容器，它的某个方法头上如果注册了@Bean，就会作为这个Spring容器中的Bean。



* **@Import**
  * 用来导入其他配置类。
  * 第一种用法：@Import（{ 要导入的容器中的组件 } ）：容器会自动注册这个组件，id默认是全类名
  * 第二种用法：ImportSelector：返回需要导入的组件的全类名数组，springboot底层用的特别多【重点 】
  * 第三种用法：ImportBeanDefinitionRegistrar：手动注册bean到容器



* **@ImportResource**
  * 用来加载xml配置文件。



* **@Bean**
  *  用@Bean标注方法等价于XML中配置的bean。



* **@Value**
  * @Value的值：
    * 使用@Value读取单个数据，属性名引用方式：${一级属性名.二级属性名......}



* **@Primary**
  * 自动装配时当出现多个Bean候选者时，被注解为@Primary的Bean将作为首选者，否则将抛出异常



* **@Autowired**
  * 自动导入依赖的bean。byType方式。把配置好的Bean拿来用，完成属性、方法的组装，它可以对类成员变量、方法及构造函数进行标注，完成自动装配的工作。当加上（required=false）时，就算找不到bean也不报错。
  * 默认按类型装配，如果我们想使用按名称装配，可以结合@Qualifier注解一起使用。如下：
    * @Autowired @Qualifier(“personDaoBean”) 存在多个实例配合使用



* **@Qualifier**
  * 当有多个同一类型的Bean时，可以用@Qualifier(“name”)来指定。与@Autowired配合使用。@Qualifier限定描述符除了能根据名字进行注入，但能进行更细粒度的控制如何选择候选者



* **@Resource**（J2EE里面的注解）

  * @Resource注解与@Autowired注解作用非常相似。

  * @Resource的装配顺序：
    * @Resource后面没有任何内容，默认通过name属性去匹配bean，找不到再按type去匹配
    * 指定了name或者type则根据指定的类型去匹配bean
    * 指定了name和type则根据指定的name和type去匹配bean，任何一个不匹配都将报错
  * @Autowired和@Resource两个注解的区别：
    * @Autowired默认按照byType方式进行bean匹配，@Resource默认按照byName方式进行bean匹配
    * @Autowired是Spring的注解，@Resource是J2EE的注解，这个看一下导入注解的时候这两个注解的包名就一清二楚了。 Spring属于第三方的，J2EE是Java自己的东西，因此，建议使用@Resource注解，以减少代码和Spring之间的耦合。
  



* **@controller**

  * @Controller对应表现层的Bean，使用@Controller注解标识UserAction之后，就表示要把UserAction交给Spring容器管理，在Spring容器中会存在一个名字为"userAction"的action，这个名字是根据UserAction类名来取的。注意：如果@Controller不指定其value【@Controller】，则默认的bean名字为这个类的类名首字母小写，如果指定value【@Controller(value=“UserAction”)】或者【@Controller(“UserAction”)】，则使用value作为bean的名字。

  * @Scope(“prototype”)表示将Action的范围声明为原型，可以利用容器的scope="prototype"来保证每一个请求有一个单独的Action来处理，避免线程安全问题。spring 默认scope 是单例模式(scope=“singleton”)，这样只会创建一个Action对象，每次访问都是同一Action对象，数据不安全，每次次访问都对应不同的Action，scope=“prototype” 可以保证当有请求的时候都创建一个Action对象



* **@Service**
  * @Service对应的是业务层Bean，@Service(“userService”)注解是告诉Spring，当Spring要创建UserServiceImpl的的实例时，bean的名字必须叫做"userService"，这样当Action需要使用UserServiceImpl的的实例时,就可以由Spring创建好的"userService"，然后注入给Action：在Action只需要声明一个名字叫"userService"的变量来接收由Spring注入的"userService"即可



* **@Repository**

  * @Repository对应数据访问层Bean ，

  * @Repository(value=“userDao”)注解是告诉Spring，让Spring创建一个名字叫"userDao"的UserDaoImpl实例。

  * 当Service需要使用Spring创建的名字叫"userDao"的UserDaoImpl实例时，就可以使用@Resource(name = “userDao”)注解告诉Spring，Spring把创建好的userDao注入给Service即可

  * 使用@Repository注解可以确保DAO或者repositories提供异常转译，这个注解修饰的DAO或者repositories类会被ComponetScan发现并配置，同时也不需要为它们提供XML配置项。



* **@Mapper**（Mybatis的注解）

  * 在用idea写一个实现类时引用了mapper类的来调用dao层的处理，使用@Autowired注解时被标红线，找不到bean。

  * 解决办法：在mapper加@mapper或者@repository注解。

  * 这两种注解的区别在于：
    * 使用@mapper后，不需要在spring配置中设置扫描地址，通过mapper.xml里面的namespace属性对应相关的mapper类，spring将动态的生成Bean后注入到ServiceImpl中。
    * @repository则需要在Spring中配置扫描包地址，然后生成dao层的bean，之后被注入到ServiceImpl中



* **@Component**
  * 把普通pojo实例化到spring容器中，相当于配置文件中的 
  * 泛指各种组件，就是说当我们的类不属于各种归类的时候（不属于@Controller、@Services等的时候），我们就可以使用@Component来标注这个类。



* **@Scope**
  * a.singleton单例模式 -- 全局有且仅有一个实例
  * b.prototype原型模式 -- 每次获取Bean的时候会有一个新的实例
  * c.request -- request表示该针对每一次HTTP请求都会产生一个新的bean，同时该bean仅在当前HTTP request内有效
  * d.session -- session作用域表示该针对每一次HTTP请求都会产生一个新的bean，同时该bean仅在当前HTTP session内有效
  * e.globalsession -- global session作用域类似于标准的HTTP Session作用域，不过它仅仅在基于portlet的web应用中才有意义



* **@Lazy(true)**

  * Spring IoC容器一般都会在启动的时候实例化所有单实例 bean 。如果我们想要 Spring 在启动的时候延迟加载 bean，即在调用某个 bean 的时候再去初始化，那么就可以使用 @Lazy 注解。

  * value 取值有 true 和 false 两个 默认值为 true，true 表示使用 延迟加载， false 表示不使用。

  * @Lazy注解注解的作用主要是减少springIOC容器启动的加载时间，当出现循环依赖时，也可以添加@Lazy



* **@PostConstruct**
  * 用于指定初始化方法（用在方法上）。
  * @PostConstruct该注解被用来修饰一个非静态的void（）方法。被@PostConstruct修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器执行一次。PostConstruct在构造函数之后执行，init（）方法之前执行。
  * 通常我们会是在Spring框架中使用到@PostConstruct注解 该注解的方法在整个Bean初始化中的执行顺序：
    * Constructor(构造方法) -> @Autowired(依赖注入) -> @PostConstruct(注释的方法)



* **@PreDestory**
  * 用于指定销毁方法（用在方法上）
  * 被@PreDestroy修饰的方法会在服务器卸载Servlet的时候运行，并且只会被服务器调用一次，类似于Servlet的destroy()方法。被@PreDestroy修饰的方法会在destroy()方法之后运行，在Servlet被彻底卸载之前。



* **@DependsOn**
  * 定义Bean初始化及销毁时的顺序
  * 有很多场景需要bean B应该被先于bean A被初始化，从而避免各种负面影响。我们可以在bean A上使用@DependsOn注解，告诉容器bean B应该先被初始化。
  * 如果我们注释掉@DependsOn("eventListener")，我们可能不确定获得相同结果。尝试多次运行main方法，偶尔我们将看到EventListenerBean 没有收到事件。为什么是偶尔呢？因为容器启动过程中，spring按任意顺序加载bean。
  * 那么当不使用@DependsOn可以让其100%确定吗？可以使用@Lazy注解放在eventListenerBean ()上。因为EventListenerBean在启动阶段不加载，当其他bean需要其时才加载。这次我们仅EventListenerBean被初始化。
  * 现在重新增加@DependsOn，也不删除@Lazy注解，输出结果和第一次一致，虽然我们使用了@Lazy注解，eventListenerBean在启动时仍然被加载，因为@DependsOn表明需要EventListenerBean。



* **@Async**
  * 创建独立的线程去完成相应的异步调用逻辑，通过主线程和不同的线程之间的执行流程，从而在启动独立的线程之后，主线程继续执行而不会产生停滞等待的情况。



## Spring MVC常用注解

