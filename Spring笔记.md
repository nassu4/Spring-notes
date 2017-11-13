# Spring 笔记 #

标签（空格分隔）： Spring

---

## 入门视频记录 ##

### 1. 第一天 ###

#### 1.1 Spring 概念 ####

 1. Spring 是开源的轻量级框架

 2. Spring 核心主要有两部分：
 - AOP：面向切面编程，扩展功能不是修改源代码实现
 - IOC：控制反转

 3. Spring 是一站式框架：Spring 在 javaee 三层结构中每层都提供不同的解决技术
 - web 层：SpringMVC
 - service 层：Spring 的 IOC
 - DAO 层：Spring 的 JDBCTemplate

#### 1.2 Spring 的 IOC 操作 ####

1. IOC 底层原理使用技术：

- XML 配置文件
- dom4j
- 工厂设计模式
- 反射

2. IOC 入门

 1. 导入 jar 包：
  - spring-beans-5.0.0.RELEASE.jar
  - spring-context-5.0.0.RELEASE.jar
  - spring-core-5.0.0.RELEASE.jar
  - spring-expression-5.0.0.RELEASE.jar
  - commons-logging-1.2.jar
  - log4j-1.2.17.jar

 2. 创建 Spring 配置文件
  - 核心配置文件位置和名称不是固定的，建议放到 src 下面，官方建议名称为 applicationContext.xml
  - 引入 schema 约束

#### 1.3 Spring 的 bean 管理（XML 方式） ####
 1. schema 约束：
  ```
  <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  ```

 2. bean 实例化的三种方式：
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

 3. bean 标签的常用属性：
  - id 属性：任意命名，不能包含特殊符号，根据 id 值得到对象
  - class 属性：创建对象所在的类全路径
  - name 属性：功能和 id 属性一样，但可以包含特殊符号（已被 id 属性取代）
  - scope 属性：bean 的作用范围
       1. singleton：默认值，单例
       2. prototype：多例
       3. reqeust：创建对象并放入 request 域中
       4. session：创建对象并放入 session 域中
       5. globalSession：创建对象并放入 globalSession 域中

 4. 属性注入的三种方式：
 - set 方法注入
 - 有参方法构造注入
 - 接口注入

 5. Spring 中属性的注入

 - Spring 支持前两种注入方式：
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
 - P 名称空间注入
  ```
    <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:p="http://www.springframework.org/schema/p"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- P 名称空间注入  -->
    <bean id="person" class="com.nassu.bean.Person" p:name="nassu" p:gender="male4"></bean>
</beans>
  ```
 - 复杂类型注入
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
