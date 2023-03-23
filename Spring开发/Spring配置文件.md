## 目录

- [Spring配置文件说明](#Spring配置文件说明)
- [依赖注入](#依赖注入)
  - [构造器注入](#构造器注入)
  - [set方式注入](#set方式注入)
    - [数组注入](#数组注入)
    - [Map注入](#Map注入)
    - [List注入](#List注入)
    - [Set注入](#Set注入)
    - [null注入](#null注入)
  - [拓展方式注入](#拓展方式注入)

## Spring配置文件说明

- beans.xml    Spring的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

<!-- 使用 Spring 来创建对象， 在Spring 中这些都称为 Bean
        只能写实现类，不能写接口
    类型 交量名 = new 类型 ()；
        Hello hello = new Hello ();
-->
    <bean id="hello" class="com.kuang.pojo.Hello" name="hello1,hello2,hello3" >
<!--    使用无参构造来创建对象。
				id = 变量名(可以用来获取对应的类对象),唯一标识符
        class = new 的对象, 是对象所对应的全限定名: 包名 + 类型
				name = 别名, 可以通过逗号或空格来同时取多个别名
        property = 相当于给对象中的属性设置一个值！
        value =  具体的值，基本数据类型
        ref = 引用 Spring 容器中已经创建好的对象 (该属性会放在 property 尖括号中)
-->
        <property name="str" value="Spring"/>
    </bean>

    <bean id="user" class ="com.kuang.pojo.User">
        <!-- 有参构造的方式1  ： 下标赋值 -->
        <!-- <constructor-arg index="0" value="房山"/>-->

        <!-- 有参构造的方式2  ： 类型赋值 (不建议使用) -->
        <!-- <constructor-arg type="java.lang.String" value="天下"/> -->

        <!-- 有参构造的方式3  ： 参数名称赋值  (方便)-->
        <constructor-arg name="name"  value="名称"/>
    </bean>
</beans>
```

- bean 配置

- import 配置

  - ```xml
    就是把别的配置文件中的内容包含进来。 相当于导入头文件的作用
    一般是将团队开发时 每个人不同的配置都包含到一个公共配置中 所使用的
    如果两个配置文件中有相同的配置 也不会发生冲突
    <import resurce="beans.xml"/>
    ```

  - 

- alias  别名配置

  - ```xml
    <alias name="user" alias="user1"/>   别名和原名 都可以使用
    ```



## 依赖注入

DI依赖注入

- 构造器注入
- set方式注入:
  - 依赖： bean对象的创建依赖于容器
  - 注入： bean对象中的所有属性，由容器来注入
- 拓展方式注入

## 构造器注入



## set方式注入

- 依赖： bean对象的创建依赖于容器
- 注入： bean对象中的所有属性，由容器来注入

- 下面是基础环境
- beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

<!-- 使用 Spring 来创建对象， 在Spring 中这些都称为 Bean
        只能写实现类，不能写接口
    类型 交量名 = new 类型 ()；
        Hello hello = new Hello ();
-->


    <bean id="address" class="com.kuang.pojo.Address">
        <property name="address" value="Address参数"/>
    </bean>

    <bean id="student" class="com.kuang.pojo.Student">
        <!-- 第一种，普通注入， value -->
        <property name="name" value="单个参数"/>
        <!-- 第二种，bean 引用注入， ref -->
        <property name="address" ref="address"/>
        <!-- String[] 数组参数初始化，数组注入， String[] ,  array , value -->
        <property name="books">
            <array>
                <value>数学书</value>  <!-- 里面是赋给 Stirng[] 数组中的值 -->
                <value>语文书</value>
                <value>英语书</value>
                <value>化学书</value>
            </array>
        </property>
        <!-- List 初始化，数组注入， List<String>  , value -->
        <property name="hobbys">
            <list>
                <value>听歌</value>
                <value>敲代码</value>
                <value>跑步</value>
            </list>
        </property>
        <!-- Map 初始化，Map注入， Map<String,String ,entry ,key , value  -->
        <property name="card">
            <map>
                <entry key="one" value="123456"/>
                <entry key="tow" value="567890"/>
            </map>
        </property>
        <!-- Set 初始化，Set 注入， Set<String> , value  -->
        <property name="games" >
            <set>
                <value>paly</value>
                <value>ovew</value>
                <value>EVE</value>
            </set>
        </property>
        <!-- null 空值初始化 ，String  -->
        <property name="wife">
            <null/>
        </property>

        <!-- 设置为空字符串 初始化 ，String  (是空, 而不是null)-->
        <property name="car" value=""/>

        <!-- Properties 第三方类 注入 -->
        <property name="info">
            <props>
                <prop key="url">本地.com</prop>
                <prop key="name">小明</prop>
                <prop key="pass">12345</prop>
            </props>
        </property>
    </bean>

</beans>
```

- MyTest.java

```java
import com.kuang.pojo.Address;
import com.kuang.pojo.Hello;
import com.kuang.pojo.Student;
import com.kuang.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        // 获取 Spring 的上下文对象（也就是容器）
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Address address = (Address) context.getBean("address");
        System.out.println(address.toString());

        Student student = (Student)context.getBean("student");
        System.out.println("student.toString() = " + student.toString());
    }
}

```

- Address.java

```java
package com.kuang.pojo;

import java.util.*;

public class Address {
    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "Address{" +
                "address='" + address + '\'' +
                '}';
    }
}
```

- Stduent.java

```java
package com.kuang.pojo;

import java.util.*;

public class Student {
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbys;
    private Map<String,String> card;
    private Set<String> games;
    private Properties info;
    private String wife;
    private String car;

    public String getCar() {
        return car;
    }

    public void setCar(String car) {
        this.car = car;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String[] getBooks() {
        return books;
    }

    public void setBooks(String[] books) {
        this.books = books;
    }

    public List<String> getHobbys() {
        return hobbys;
    }

    public void setHobbys(List<String> hobbys) {
        this.hobbys = hobbys;
    }

    public Map<String, String> getCard() {
        return card;
    }

    public void setCard(Map<String, String> card) {
        this.card = card;
    }

    public Set<String> getGames() {
        return games;
    }

    public void setGames(Set<String> games) {
        this.games = games;
    }

    public Properties getInfo() {
        return info;
    }

    public void setInfo(Properties info) {
        this.info = info;
    }

    public String getWife() {
        return wife;
    }

    public void setWife(String wife) {
        this.wife = wife;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", address=" + address +
                ", books=" + Arrays.toString(books) +
                ", hobbys=" + hobbys +
                ", card=" + card +
                ", games=" + games +
                ", info=" + info +
                ", wife='" + wife + '\'' +
                ", car='" + car + '\'' +
                '}';
    }
}

```

### 普通注入

```xml
<bean id="student" class="com.kuang.pojo.Student">
        <!-- 第一种，普通注入， value -->
        <property name="name" value="单个参数"/>
</bean>
```

### 引用注入

```xml
<bean id="address" class="com.kuang.pojo.Address">
      <property name="address" value="Address参数"/>
</bean>

<bean id="student" class="com.kuang.pojo.Student">
        <!-- 第二种，bean 引用注入， ref -->
        <property name="address" ref="address"/>
</bean>
```

### 数组注入

```xml
<bean id="student" class="com.kuang.pojo.Student">
        <!-- String[] 数组参数初始化，数组注入， String[] ,  array , value -->
        <property name="books">
            <array>
                <value>数学书</value>  <!-- 里面是赋给 Stirng[] 数组中的值 -->
                <value>语文书</value>
                <value>英语书</value>
                <value>化学书</value>
            </array>
        </property>
</bean>
```

### Map注入

```xml
<bean id="student" class="com.kuang.pojo.Student">
        <!-- Map 初始化，Map注入， Map<String,String ,entry ,key , value  -->
        <property name="card">
            <map>
                <entry key="one" value="123456"/>
                <entry key="tow" value="567890"/>
            </map>
        </property>
</bean>
```

### List注入

```xml
<bean id="student" class="com.kuang.pojo.Student">
        <!-- List 初始化，数组注入， List<String>  , value -->
        <property name="hobbys">
            <list>
                <value>听歌</value>
                <value>敲代码</value>
                <value>跑步</value>
            </list>
        </property>
</bean>
```

### Set注入

```xml
<bean id="student" class="com.kuang.pojo.Student">
        <!-- Set 初始化，Set 注入， Set<String> , value  -->
        <property name="games" >
            <set>
                <value>paly</value>
                <value>ovew</value>
                <value>EVE</value>
            </set>
        </property>
</bean>
```

### null注入

```xml
<bean id="student" class="com.kuang.pojo.Student">
        <!-- null 空值初始化 ，String  -->
        <property name="wife">
            <null/>
        </property>
</bean>
```

### 空值注入

```xml
<bean id="student" class="com.kuang.pojo.Student">
        <!-- 设置为空字符串 初始化 ，String  (是空, 而不是null)-->
        <property name="car" value=""/>
</bean>
```

### 配置类注入

`Properties` 是java的配置类

```xml
<bean id="student" class="com.kuang.pojo.Student">
        <!-- Properties 第三方类 注入 -->
        <property name="info">
            <props>
                <prop key="url">本地.com</prop>
                <prop key="name">小明</prop>
                <prop key="pass">12345</prop>
            </props>
        </property>
</bean>
```

## 拓展方式注入
