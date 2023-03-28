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
    - [空值注入](#空值注入)
    - [配置类注入](#配置类注入)
    - [拓展方式注入](#拓展方式注入)
- [bean的作用域](#bean的作用域)
- [bean的自动装配](#bean的自动装配)
  - [使用注解实现自动装配](#使用注解实现自动装配)

- [使用注解开发](#使用注解开发)
  - [bean注入](#bean注入)
  - [属性注入](#属性注入)
  - [衍生的注解](#衍生的注解)
  - [自动装配配置](#自动装配配置)
  - [作用域](#作用域)
- [使用Java的方式配置Spring](#使用Java的方式配置Spring)


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



- xml配置

  - ```xml
    <bean id="user" class="com.kuang.pojo.User">
        <property name="name" value="秦疆"/>
    </bean>
    ```

- User.java

  - ```java
    package com.kuang.pojo;
    public class User {
        private String name;
    
        public void setName(String name) {
            this.name = name;
        }
    
        public String getName() {
            return name;
        }
    
        @Override
        public String toString() {
            return "User{" +
                    "name='" + name + '\'' +
                    '}';
        }
    }
    ```



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

### 拓展方式注入

c 和 p 命名空间注入, 在使用前需要两个约束  

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"  <!-- 这里导入了p -->
       xmlns:c="http://www.springframework.org/schema/c"  <!-- 这里导入了c -->
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
```



- userbeans.xml

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:p="http://www.springframework.org/schema/p"
         xmlns:c="http://www.springframework.org/schema/c"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <!-- p 命名空间注入， 可以直接注入属性的值 : property -->
      <bean id="user" class="com.kuang.pojo.User"  p:name="p命名name" p:age="18"></bean>
  
      <!-- c命名空间注入, 通过有参构造器注入: constructs-args -->
      <bean id="userC" class="com.kuang.pojo.User" c:name="c命名name" c:age="20">
  
      </bean>
  </beans>
  ```

- User.java 

  ```java
  package com.kuang.pojo;
  
  public class User {
      private String name;
      private int   age;
  
      public User() {
          name = "";
          age = 0;
      }
  
      public User(String name, int age) {
          this.name = name;
          this.age = age;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public int getAge() {
          return age;
      }
  
      public void setAge(int age) {
          this.age = age;
      }
  
      @Override
      public String toString() {
          return "User{" +
                  "name='" + name + '\'' +
                  ", age=" + age +
                  '}';
      }
  }
  
  ```

- Test.java

  ```java
  import com.kuang.pojo.User;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  public class MyTest {
      public static void main(String[] args) {
          test2();
      }
  
      public static void test2()
      {
          ApplicationContext context = new  ClassPathXmlApplicationContext("userbeans.xml");
          User user = context.getBean("userC",User.class);
          System.out.println("userC.toString() = " + user.toString());
      }
  }
  ```

  

### bean的作用域

| Scope                                                        | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [singleton](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-singleton) | (Default) Scopes a single bean definition to a single object instance per Spring IoC container. |
| [prototype](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-prototype) | Scopes a single bean definition to any number of object instances. |
| [request](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-request) | Scopes a single bean definition to the lifecycle of a single HTTP request; that is, each HTTP request has its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [session](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-session) | Scopes a single bean definition to the lifecycle of an HTTP `Session`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [application](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-application) | Scopes a single bean definition to the lifecycle of a `ServletContext`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [websocket](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/spring-framework-reference/web.html#websocket-stomp-websocket-scope) | Scopes a single bean definition to the lifecycle of a `WebSocket`. Only valid in the context of a web-aware Spring `ApplicationContext`. |



1. 单例模式 (Spring默认机制)， 只创建一个对象，且所有地方共用这一个对象。 适合单线程

   1. ```xml
      <bean id="user" class="com.kuang.pojo.User"  p:name="p命名name" p:age="18" scope="singleton"></bean>
      ```

2. 原型模式 ： 每次从容器中get 的时候，都会产生一个新的对象。 适合多线程

   1. ```xml
       <bean id="userC" class="com.kuang.pojo.User" c:name="c命名name" c:age="20" scope="prototype"></bean>
      ```

3. 其余的 `request、session、application、websocket` 这些只能在web开发中用到。



- userbeans.xml

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- p 命名空间注入， 可以直接注入属性的值 : property -->
    <bean id="user" class="com.kuang.pojo.User"  p:name="p命名name" p:age="18" scope="singleton"></bean>

    <!-- c命名空间注入, 通过有参构造器注入: constructs-args -->
    <bean id="userC" class="com.kuang.pojo.User" c:name="c命名name" c:age="20" scope="prototype">

    </bean>
</beans>
```



## bean的自动装配

- 自动装配为 `bean` 标签的 `autowire` 属性
  - byName 的时候，需要保证所有的bean 的 id 唯一，并且这个 bean 需要和自动注入的属性的 set 方法后面的参数 保持一致。
  - byType  的时候，需要保证所有的bean 的 class 唯一，并且这个 bean 需要和自动注入的属性的 类型 保持一致。



- beanAuto.xml   实现了自动 名称装配

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="cat" class="com.kuang.pojo.Cat" />
    <bean id="dog" class="com.kuang.pojo.Dog" />
	<!-- 
		byName : 会在容器上下文中查找，和自己set方法后面的值对应的 beanid! (也就是 setDog(Dog dog) 最后的 dog 参数名称 )
	-->
  
    <bean id="peopel" class="com.kuang.pojo.Peopel" autowire="byName">
        <property name="name" value="小张"/>
    </bean>
</beans>
```

- beanAuto.xml  实现了自动 类型装配

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="cat" class="com.kuang.pojo.Cat" />  <!-- 里面的id 都可以不写 -->
    <bean id="dog" class="com.kuang.pojo.Dog" />

  <!-- 
		byType : 会在容器上下文中查找，和自己set方法后面的值对应的 参数属性相同的 bean, 而不看名称
	-->
    <bean id="peopel" class="com.kuang.pojo.Peopel" autowire="byType">
        <property name="name" value="小张"/>
    </bean>
</beans>
```





三个类

1. Dog.java

   1. ```java
      package com.kuang.pojo;
      
      public class Dog {
          public void shout()
          {
              System.out.println("wang～");
          }
      }
      ```

2. Cat.java

   1. ```java
      package com.kuang.pojo;
      
      public class Cat {
          public void shout()
          {
              System.out.println("miao～");
          }
      }
      ```

3. MyTest.java

   1. ```java
      import com.kuang.pojo.Peopel;
      import com.kuang.pojo.User;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;
      public class MyTest {
          public static void main(String[] args) {
              ApplicationContext context = new  ClassPathXmlApplicationContext("beanAuto.xml");
              Peopel peopel = context.getBean("peopel", Peopel.class);
              System.out.println(peopel.toString());
          }
      }
      ```



### 使用注解实现自动装配

需要在 xml 文件中添加支持后，才可以使用注解实现自动装配。

- annotation.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 上面的 xmlns:context 以及 这里 -->
    <context:annotation-config/>

    <bean id="cat" class="com.kuang.pojo.Cat" />
    <bean id="dog" class="com.kuang.pojo.Dog" />

    <bean id="peopel" class="com.kuang.pojo.Peopel" autowire="byName">
    </bean>
</beans>
```

- Peopel.java

  - ```java
    package com.kuang.pojo;
    
    import org.springframework.beans.factory.annotation.Autowired;
    
    public class Peopel {
    
        /*
            @Autowired
            注释实现自动装配。写在所需要的对象上即可
            但是也需要 set 方法,,
    	      该注释也可以放到 set方法上面。 与放到对象上有相同的结果
    	      
    	      @Qualifier(value = "cat")
    	      	显示的指定在配置.xml文件中，使用哪个名字的对象(id)来进行注册。 配合Autowired使用
         */
        @Autowired
    	  @Qualifier(value = "cat")
        private Cat cat;
      
        /*
         @Autowired(required =false) 显示定义了  @Autowired 的required 属性为 false ，
            说明这个对象可以为null， 否则不允许为空(默认不能为空)
         */
        @Autowired(required =false)
        private Dog dog;
      
        private String name;
    
      		/* @Nullable 注释说明 name 为空值不报错 */
    //    public Peopel( @Nullable String name) {
    //        this.name = name;
    //    }
      
        public void setCat(Cat cat) {
            this.cat = cat;
        }
    	  
        @Autowired
        public void setDog(Dog dog) {
            this.dog = dog;
        }
    
        public void setName(String name) {
            this.name = name;
        }
    
        @Override
        public String toString() {
           cat.shout();
            dog.shout();
            return "Peopel{ 'name='" + name + '\'' + '}';
        }
    }
    

- Cat.java

  - ```java
    package com.kuang.pojo;
    
    public class Cat {
        public void shout()
        {
            System.out.println("miao～");
        }
    }
  
- Dog.java

  - ```java
    package com.kuang.pojo;
    
    public class Dog {
        public void shout()
        {
            System.out.println("wang～");
        }
    }
    ```

  

- 注释：@Autowired 
  - 直接在属性上使用即可， 也可以在set方法上使用。 （都需要写set方法）
    - 先匹配类型，再匹配名字
  - 注释：@Qualifier(value = "cat")
    - 显示的指定在配置.xml文件中，使用哪个名字的对象(id)来进行注册。 配合Autowired使用
  - 注释：@Resource(name = "cat")
    - 在有参数的情况下与 `@Autowired 和 @Qualifier(value = "cat")` 一起组合的作用相同。
    - 在没有参数的情况下，与 @Autowired 作用相同。
- 注释：@Nullable
  - 字段标记了这个注解，说明这个字段可以为null



## 使用注解开发

@Component 注解(bean注入) 等价于  `<bean id="User" class="com.kuang.pojo.User"  />`



#### bean注入

- bean

  - MyTest.java

    - ```java
      import com.kuang.pojo.Peopel;
      import com.kuang.pojo.User;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;
      public class MyTest {
          public static void main(String[] args) {
              // 获取 Spring 的上下文对象（也就是容器）
      				ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
              User user = context.getBean("user", User.class);
              System.out.println(user.name);
          }
      }
      ```

  - applicationContext.xml

    - ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:context="http://www.springframework.org/schema/context"
             xsi:schemaLocation="http://www.springframework.org/schema/beans
              http://www.springframework.org/schema/beans/spring-beans.xsd
              http://www.springframework.org/schema/context
              http://www.springframework.org/schema/context/spring-context.xsd">
      
          <!-- 指定要扫描的包，这个包下的注释就会生效 -->
          <context:component-scan base-package="com.kuang"/>
          <context:annotation-config/>
      </beans>
      ```

  - User.java

    - ```java
      package com.kuang.pojo;
      
      import org.springframework.beans.factory.annotation.Value;
      import org.springframework.stereotype.Component;
      
      /*
          等价于  <bean id="User" class="com.kuang.pojo.User"  />
          @Component 组件
       */
      @Component
      public class User {
          public String name ="直接赋予参数值";
      }
      ```



#### 属性注入

@Value("kuangshen") 注解（属性注入） 等价于  bean 下的  property标签。(但是只适用于简单类型) `<bean xxxx > <property  name="name" value="kuangshen" /></bean>`



- 属性注入

  - applicationContext.xml 和 MyTest.java 不变

  - User.java

    - ```java
      package com.kuang.pojo;
      
      import org.springframework.beans.factory.annotation.Value;
      import org.springframework.stereotype.Component;
      
      /*
          等价于  <bean id="User" class="com.kuang.pojo.User"  />
          @Component 组件
       */
      @Component
      public class User {
          /*
           @Value("kuangshen")  等价于  bean 下的  property标签。(但是只适用于简单类型)
              <bean id="User" class="com.kuang.pojo.User">
                  <property  name="name" value="kuangshen" />
              </bean>
           */
          @Value("kuangshen")
          public String name ;
      }
      ```



#### 衍生的注解

@Component 有几个衍生注解，在web开发中，会按照mvc 三层架构分层

- dao	使用 @Repository 注解。 仓储层
- service    使用 @Service 注解。 业务层
- controller  使用 @Controller 注解。 代表被 Spring托管的组件

> 这四个注解的功能都是一样的(@Component、@Repository、@Service、@Controller)，都代表将某个类注册到 Spring 中，装配bean。



#### 自动装配配置

注释：@Autowired 

- 直接在属性上使用即可， 也可以在set方法上使用。 （都需要写set方法）
  - 先匹配类型，再匹配名字
- 注释：@Qualifier(value = "cat")
  - 显示的指定在配置.xml文件中，使用哪个名字的对象(id)来进行注册。 配合Autowired使用
- 注释：@Resource(name = "cat")
  - 在有参数的情况下与 `@Autowired 和 @Qualifier(value = "cat")` 一起组合的作用相同。
  - 在没有参数的情况下，与 @Autowired 作用相同。

```java
    /*
        @Autowired
        注释实现自动装配。写在所需要的对象上即可
        但是也需要 set 方法,,
	      该注释也可以放到 set方法上面。 与放到对象上有相同的结果
	      
	      @Qualifier(value = "cat")
	      	显示的指定在配置.xml文件中，使用哪个名字的对象(id)来进行注册。 配合Autowired使用
     */
    @Autowired
	  @Qualifier(value = "cat")
```



#### 作用域

就是替代 bean 标签下的 scope 

@Scope("prototype")     原型模式(每次都是新的)

@Scope("singleton")     单例模式(共用一个)

- UserService.java

  - ```java
    package com.kuang.service;
    
    import org.springframework.context.annotation.Scope;
    import org.springframework.stereotype.Controller;
    
    @Controller
    @Scope("prototype")    // 原型模式
    public class UserService {
    }
    ```



## 使用Java的方式配置Spring

配置类(也就是配置文件)

```java
package com.kuang.config;

import com.kuang.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@ComponentScan("com.kuang")
@Import(KuangConfig.class)
public class KuangConfig {
    @Bean
    public User getUser()
    {
        return new User();
    }
}
```

实体类

```java
package com.kuang.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class User {
    private String name ;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    @Value("QUANG")
    public void setName(String name) {
        this.name = name;
    }
}
```

测试类

```java
import com.kuang.config.KuangConfig;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyTest
{
    public static void main(String[] args) {
        ApplicationContext context =  new AnnotationConfigApplicationContext(KuangConfig.class);
        User user = (User) context.getBean("getUser");
        user.toString();
    }
}
```

在SpringBoot中随处可见











