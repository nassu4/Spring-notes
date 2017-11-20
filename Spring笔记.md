# Spring 笔记 #

标签（空格分隔）： Spring IOC

---

## 入门视频记录 ##

### 1. 第一天 ###

#### 1.1 Spring 的概念 ####

1. **Spring 是开源的轻量级框架**

2. **Spring 核心主要有两部分**
- AOP：面向切面编程，扩展功能不是修改源代码实现
- IOC：控制反转

3. **Spring 是一站式框架：Spring 在 javaee 三层结构中每层都提供不同的解决技术**
- web 层：SpringMVC
- service 层：Spring 的 IOC
- DAO 层：Spring 的 JDBCTemplate

#### 1.2 Spring 的 IOC 操作 ####

1. **IOC 底层原理使用技术**
- XML 配置文件
- dom4j
- 工厂设计模式
- 反射

2. **IOC 入门**
- 导入 jar 包
- 创建 Spring 配置文件
     - 核心配置文件位置和名称不是固定的，建议放到 src 下面，官方建议名称为 applicationContext.xml
     - 引入 schema 约束

#### 1.3 Spring 的 bean 管理（XML 方式） ####

1. **准备工作**
- 导入基本 jar 包
     - spring-beans-5.0.0.RELEASE.jar
     - spring-context-5.0.0.RELEASE.jar
     - spring-core-5.0.0.RELEASE.jar
     - spring-expression-5.0.0.RELEASE.jar
- 导入提供日志记录功能 jar 包
     - commons-logging-1.2.jar
     - log4j-1.2.17.jar
- 引入 schema 约束
  ```
  <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  ```

2. **bean 实例化的三种方式**
- 使用类的无参构造方法
 ```
   <bean id="person" class="com.nassu.bean.Person"></bean>
 ```
- 使用静态工厂创建
 ```
   <bean id="personFactory" class="com.nassu.factory.PersonFactory" factory-method="getPerson"></bean>
 ```
- 使用实例工厂创建
 ```
   <bean id="personFactory" class="com.nassu.factory.PersonFactory"></bean>
  <bean id="factory" factory-bean="personFactory" factory-method="getPerson"></bean>
 ```

3. **bean 标签的常用属性**
- id 属性：任意命名，不能包含特殊符号，根据 id 值得到对象
- class 属性：创建对象所在的类全路径
- name 属性：功能和 id 属性一样，但可以包含特殊符号（已被 id 属性取代）
- scope 属性：bean 的作用范围
     - singleton：单例（默认值）
     - prototype：多例
     - reqeust：创建对象并放入 request 域中
     - session：创建对象并放入 session 域中
     - globalSession：创建对象并放入 globalSession 域中

4. **属性注入的三种方式**
- set 方法注入
- 有参方法构造注入
- 接口注入

5. **Spring 中属性的注入**
- Spring 支持前两种注入方式
 ```
    <!-- 注入对象类型属性使用 ref 属性 -->
    <!-- set 方法注入  -->
  <bean id="person" class="com.nassu.bean.Person">
    <property name="name" value="nassu"></property>
    <property name="gender" value="male"></property>
  </bean>

  <!-- 有参构造方法注入  -->
  <bean id="person" class="com.nassu.bean.Person">
    <constructor-arg index="0" value="nassu4"></constructor-arg>
    <constructor-arg index="1" value="male"></constructor-arg>
  </bean>
 ```
- P 名称空间注入（基于 set 方法）
 ```
    <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:p="http://www.springframework.org/schema/p"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- P 名称空间注入  -->
    <bean id="person" class="com.nassu.bean.Person" p:name="nassu" p:gender="male4"></bean>
</beans>
 ```
- 复杂类型注入（基于 set 方法）
 ```
  <bean id="person" class="com.nassu.bean.Person">
    <!-- 1. 数组 -->
    <property name="arrs">
      <list>
        <value>一</value>
        <value>二</value>
        <value>三</value>
      </list>
    </property>
    <!-- 2. list 集合 -->
    <property name="list">
      <list>
        <value>one</value>
        <value>two</value>
        <value>three</value>
      </list>
    </property>
    <!-- 3. map 集合 -->
    <property name="map">
      <map>
        <entry key="a" value="啊"></entry>
        <entry key="b" value="吧"></entry>
        <entry key="c" value="才"></entry>
      </map>
    </property>
    <!-- 4. properties 类型 -->
    <property name="properties">
      <props>
        <prop key="username">root</prop>
        <prop key="password">root</prop>
      </props>
    </property>
  </bean>
 ```

### 2. 第二天 ###

#### 2.1 Spring 的 bean 管理（注解方式） ####

1. **准备工作**
- 基本 jar 包和支持日志记录 jar 包（同 XML 方式）
- 导入用于注解的 jar 包：spring-aop-5.0.0.RELEASE.jar
- 引入 schema 约束
  ```
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:context="http://www.springframework.org/schema/context"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context
                http://www.springframework.org/schema/context/spring-context.xsd">
  ```

2. **配置注解**
- 核心配置文件中
  ```
 <!-- 开启注解扫描（到包里面扫描类、方法、属性上面是否有注解） -->
  <context:component-scan base-package="com.nassu"></context:component-scan>

  <!-- 只扫描属性上面的注解 -->
  <context:annotation-config />
  ```
- 实体类中
 ```
 @Component(value = "person")
 @Scope(value = "prototype")
    public class Person {
        //...
    }
 ```

3. **常用的四个用来实例化的注解**
- @Component
- @Controller：WEB 层
- @Service： 业务层
- @Repository：持久层

 其他三个注解是 @Component 的三个衍生注解，目的是为了让标注类本身的用途更清晰。

4. **属性的注入**
 ```
    @Component(value = "book")
    public class Book {
      @Value(value = "Thinking in Java")
      private String name;

      @Override
        public String toString() {
          return "Book [name=" + name + "]";
        }
    }

    @Component(value = "person")
    public class Person {
      @Value(value = "张三")
      private String name;

      @Autowired
      private Book book;

      @Override
      public String toString() {
        return "Person [name=" + name + ", book=" + book + "]";
      }
    }
 ```

 除了 @Autowired 注解还有另外一个注解，使用 @Resource(name = "book") 可以达到相同效果，这两种注入方式不需要写 set 方法。

5. **注解和配置文件的混合使用**
 - 创建对象操作使用配置文件方式实现
 - 注入属性操作使用注解方式实现

#### 2.2 AOP ####

1. **AOP 概念**
 Aspect Oriented Programing，面向切面（方面）编程。AOP 采取横向抽取机制，取代了传统纵向继承体系重复性代码。

2. **AOP 原理**
 底层使用动态代理方式实现。
- 使用 jdk 动态代理，针对有接口情况
- 使用 cglib 动态代理，追对没有接口情况

3. **AOP 操作术语**
- Joinpoint（连接点）：类里面可以被增强的方法
- Pointcut（切入点）：实际增强的方法
- Advice（通知/增强）：增强的逻辑
     - 前置通知：在方法之前执行
     - 后置通知：在方法之后执行
     - 异常通知：方法出现异常后执行
     - 最终通知：在后置通知之后执行
     - 环绕通知：在方法前后都执行
- Aspect（切面）：把增强的逻辑应用到具体方法上面的过程

#### 2.3 Spring 的 AOP 操作（XML 方式） ####

1. **使用 aspectj 实现**
- aspectj 不是 Spring 的一部分
- Spring2.0 之后增加了对 aspectj 的支持

2. **使用 aspectj 实现 AOP 有两种方式**
- 基于 aspectj 的 xml 配置
- 基于 aspectj 的注解

3. **准备工作**
- 基本 jar 包和支持日志记录 jar 包
- 导入用于 AOP 操作的 jar 包：
     - spring-aop-5.0.0.RELEASE.jar
     - spring-aspects-5.0.0.RELEASE.jar
     - aopalliance-1.0.jar
     - aspectjweaver.jar
- 引入 schema 约束
 ```
<beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:aop="http://www.springframework.org/schema/aop"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/aop
                http://www.springframework.org/schema/aop/spring-aop.xsd">
 ```

4. **通过 execution 函数定义切点方法**
语法：execution(<访问修饰符>?<返回类型><方法名>(<参数>)<异常>)，常用表达式：
- execution(* com.nassu.person.add(..))
- execution(* com.nassu.Person.*(..))
- execution(* *.*(..))

5. **配置文件和类中方法的书写**
- 配置文件
 ```
 <!-- 配置对象 -->
  <bean id="person" class="com.nassu.bean.Person"></bean>
  <bean id="book" class="com.nassu.bean.Book"></bean>

  <!-- 配置AOP操作 -->
  <aop:config>
    <!-- 配置切入点 -->
    <aop:pointcut expression="execution(* com.nassu.bean.Person.*(..))" id="pointcut" />

    <!-- 配置切面 -->
    <aop:aspect ref="book">
      <!-- 前置通知 -->
      <aop:before method="before" pointcut-ref="pointcut" />
      <!-- 后置通知 -->
      <aop:after method="after" pointcut-ref="pointcut" />
      <!-- 环绕通知 -->
      <aop:around method="around" pointcut-ref="pointcut" />
    </aop:aspect>
  </aop:config>
 ```

- 增强方法
 ```
 public void before() {
    System.out.println("前置通知......");
  }

  public void after() {
    System.out.println("后置通知......");
  }

  public void around(ProceedingJoinPoint pjp) throws Throwable {
    System.out.println("before...");
    pjp.proceed();
    System.out.println("after...");
  }
 ```

#### 2.4 log4j 介绍 ####

1. **通过 log4j 可以看到程序运行过程中更详细的信息**

2. **引入方式**
 - 导入 log4j 的 jar 包
 - 在 src 下创建配置文件

#### 2.5 服务器启动时加载配置文件 ####

1. 导入 jar 包：spring-web-5.0.0.RELEASE.jar
2. 在 web.xml 中进行配置
 ```
 <!-- 指定 Spirng 配置文件的位置（如果名称为 applicationContext.xml,则不用配置） -->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>

  <!-- 配置监听器 -->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
 ```

### 3. 第三天 ###

#### 3.1 Spring 的 AOP 操作（注解方式） ####

1. **核心配置文件中配置**
 ```
 <!-- 开启AOP操作 -->
  <aop:aspectj-autoproxy />

  <!-- 配置对象 -->
  <bean id="person" class="com.nassu.bean.Person"></bean>
  <bean id="book" class="com.nassu.bean.Book"></bean>
 ```

2. **增强方法上的注解**
 ```
 @Before(value = "execution(* com.nassu.bean.Person.method())")
  public void before() {
    System.out.println("前置通知......");
  }

  @AfterReturning(value = "execution(* com.nassu.bean.Person.method())")
  public void after() {
    System.out.println("后置通知......");
  }

  @Around(value = "execution(* com.nassu.bean.Person.method())")
  public void around(ProceedingJoinPoint pjp) throws Throwable {
    System.out.println("before...");
    pjp.proceed();
    System.out.println("after...");
  }
 ```

#### 3.2 Spring 的 jdbcTemplate 操作 ####

1. **准备工作**
 导入 jar 包
 - spring-jdbc-5.0.0.RELEASE.jar
 - spring-tx-5.0.0.RELEASE.jar
 - mysql-connector-java-5.1.42-bin.jar

2. **CRUD 操作**
 1. CUD 操作
  ```
  public void cud() {
    //1. 设置数据库信息
    DriverManagerDataSource dataSource = new DriverManagerDataSource();
    dataSource.setDriverClassName("com.mysql.jdbc.Driver");
    dataSource.setUrl("jdbc:mysql://localhost:3306/spring?useSSL=true");
    dataSource.setUsername("root");
    dataSource.setPassword("123456");

    //2. 创建 JdbcTemplate 对象
    JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);

    //3. 进行CUD操作
    //3.1 create
    Object[] objects1 = {"小明", 101};
    String sql1 = "insert into person(name, age) values(?, ?)";
    jdbcTemplate.update(sql1, objects1);

    //3.2 update
    Object[] objects2 = {10, "小明"};
    String sql2 = "update person set age=? where name=?";
    jdbcTemplate.update(sql2, objects2);

    //3.3 delete
    Object[] objects3 = {"小明"};
    String sql3 = "delete from person where name=?";
    jdbcTemplate.update(sql3, objects3);
  }
  ```
 2. Retrieve 操作
  ```
  public void Retrieve() {
    //1. 设置数据库信息
    DriverManagerDataSource dataSource = new DriverManagerDataSource();
    dataSource.setDriverClassName("com.mysql.jdbc.Driver");
    dataSource.setUrl("jdbc:mysql://localhost:3306/spring?useSSL=true");
    dataSource.setUsername("root");
    dataSource.setPassword("123456");

    //2. 创建 JdbcTemplate 对象
    JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);

    //3. 进行 Retrieve 操作
    //3.1 查询返回某一个值
    String sql1 = "select count(*) from person";
    int count = jdbcTemplate.queryForObject(sql1, Integer.class);
    System.out.println(count);

    //3.2 查询返回对象
    String sql2 = "select * from person where id=?";
    Person person = jdbcTemplate.queryForObject(sql2, new RowMapper<Person>() {
      public Person mapRow(ResultSet rs, int rowNum) throws SQLException {
        Person person = new Person();
        person.setId(rs.getInt(1));
        person.setName(rs.getString(2));
        person.setAge(rs.getInt(3));
        return person;
      }
    }, 4);
    System.out.println(person);

    //3.3 查询返回 list 集合
    String sql3 = "select * from person";
    List<Person> list = jdbcTemplate.query(sql3, new RowMapper<Person>() {
      public Person mapRow(ResultSet rs, int rowNum) throws SQLException {
        Person person = new Person();
        person.setId(rs.getInt(1));
        person.setName(rs.getString(2));
        person.setAge(rs.getInt(3));
        return person;
      }
    });
    System.out.println(list);
  }
  ```

#### 3.3 Spring 的事务管理 API ####

1. **Spring 事务管理的两种方式**
- 编程式事务管理（不用）
- 声明式事务管理
     - 基于 XML 配置文件实现
     - 基于注解实现

2. **Spring 事务管理 API 介绍**
PlatFormTransactionManager：针对不同的 DAO 层框架提供不同的实现类

3. **声明式事务管理（XML 方式）**
  ```
  <!-- 1. 配置事务管理器 -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
      <!-- 注入 dataSource -->
      <property name="dataSource" ref="dataSource" />
  </bean>

  <!-- 2. 配置事务增强 -->
  <tx:advice id="txAdvice" transaction-manager="transactionManager">
      <tx:attributes>
        <!-- 设置进行事务操作的方法名 -->
        <tx:method name="transfer" />
      </tx:attributes>
  </tx:advice>

  <!-- 3. 配置切面 -->
  <aop:config>
      <aop:pointcut expression="execution(* com.nassu.service.MoneyService.*(..))" id="pointcut" />
      <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut" />
  </aop:config>
  ```

4. **声明式事务管理（注解方式）**
- 配置事务管理器
 ```
 <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
      <!-- 注入 dataSource -->
      <property name="dataSource" ref="dataSource" />
</bean>
 ```
- 开启事务注解
 ```
 <!-- 开启事务注解 -->
  <tx:annotation-driven transaction-manager="transactionManager" />
 ```
- 在要开启事务的方法的类上添加注解
 ```
 @Transactional
 public class Service {
        //...
 }
 ```
