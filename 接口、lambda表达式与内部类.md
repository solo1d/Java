### 目录

- [接口](#接口)
  - [ActionListener回调定时器和监听器操作](#ActionListener回调定时器和监听器操作)
  - [Comparator比较器接口](#Comparator比较器接口)
  - [Cloneable对象克隆接口](#Cloneable对象克隆接口)
- [lambda表达式](#lambda表达式)
  - [lamdba多种样式组合](#lamdba多种样式组合)
- [内部类](#内部类)
  - [局部内部类](#局部内部类)
  - [静态内部类](#静态内部类)
  - 











## 接口

Arrays 类中的 sort 方法承诺可以对对象数组进行排序， 但要求足下列前提: 对象所属的类必须实现了 Comparable 接口。

`public interface Comparable<T> {int compareTo(T other); }  /* parameter has type T */ `

- **接口中的所有方法自动地属于 public**
- 



==**为了让类实现一个接口， 通常需要下面两个步骤:**==

1. 将类声明为实现给定的接口。
2. 对接口中的所有方法进行定义。

**要将类声明为实现某个接口， 需要使用关键字 `implements`**



```java
假设希望使用 Arrays 类的 sort 方法对 Employee 对象数组进行排序， Employee 类 就必须实现 Comparable 接口。
  

/* EmployeeSortTest.java 文件*/ 
package interfaces;

import java.util.*;
public class EmployeeSortTest {
    public static void main(String[] args)
    {
        Employee[] staff = new Employee[3];

        staff[0] = new Employee("Harr", 35000);
        staff[1] = new Employee("Carl", 75000);
        staff[2] = new Employee("Tony", 38000);
        Arrays.sort(staff);  // 这里面会调用  compareTo 方法

        for (Employee e:staff){
            System.out.println("name=" + e.getName() + ",salary=" + e.getSalary());
            e.compareTo(e);
        }
    }
}


/* Employee.java文件  */
package interfaces;

public class Employee implements Comparable<Employee>
{
    private String name;
    private double salary;

    public Employee(String name, double salary )
    {
        this.name = name;
        this.salary = salary;
    }

    public String getName()
    {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public void raiseSalary(double byPercent)
    {
        double raise = salary * byPercent /100;
        salary += raise;
    }

    public int compareTo(Employee other)
    {
        return Double.compare(salary, other.salary);
    }
}



Comparable 相关API:    (这是个类)  import java.lang.Comparable<T>;
		int  compareTo (T other)
      /* 用这个对象与 other 进行比较。如果这个对象小于 other 则返回负值; 如果相等则返回 0; 否则返回正值 */

Arrays 相关API:    (这是个类)  import java.util.Arrays;
		static void sort (Object[] a)
      /* 使用 mergesort 算法对数组 a 中的元素进行排序。 
        要求数组中的元素必须属于实现了 Comparable 接口的类， 并且元素之间必须是可比较的。 */

Integer 相关API:    (这是个类)  import java.lang.Integer;
		static int compare (int x, int y)
      /* 如果 x < y 返回一个负整数; 如果x和y相等，则返回0; 否则返回一个负整数 */

Double 相关API:    (这是个类)  import java.lang.Double;
		static int compare (double x, double y)
      /* 如果 x < y 返回一个负整数; 如果x和y相等，则返回0; 否则返回一个负整数 */
```



### ActionListener回调定时器和监听器操作

```java
package timer;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.IOException;
import java.util.Date;

public class TimerTest {
    public static void main(String[] args) throws IOException {
        ActionListener listener = new TimePrinter();
        Timer t = new Timer(5000, listener);
        t.start();
        JOptionPane.showMessageDialog(null, "Quit program?");
        System.exit(0);
    }
}

// 定时器需要知道调用哪一个方法， 并要求传递的对象所属的类实现了 java.awt.event 包的 ActionListener 接口
class TimePrinter implements ActionListener {
    public void actionPerformed (ActionEvent event)
    {
        System.out.println("At the tone the time is "  +  new Date());
        Toolkit.getDefaultToolkit().beep();
    }
}


JOptionPane 相关API:    (这是个类)  import javax.swing.JOptionPane
		static void  showMessageDialog (Component parent, Object message)
      /* 显示一个包含一条消息和 OK 按钮的对话框。 这个对话框将位于其 parent 组件的中央。null, 对话框将显示在屏幕的中央 */
  
Timer 相关API:    (这是个类)  import javax.swing.Timer
		Timer (int interval, ActionListener listener)
      /* 构造一个定时器， 每隔 interval 毫秒通告 listener —次 */
		void start ()
      /* 启动定时器， 一旦启动成功， 定时器将调用监听器的 actionPerformed */
		void stop ()
      /* 停止定时器，一旦停止成功，定时器将不再调用监听器的 actionPerformed */
  
Toolkit 相关API:    (这是个类)  import java.awt.Toolkit
		static Toolkit getDefaultToolkit()
      /* 获得默认的工具箱， 工具箱包含 GUI 环境的信息  */

		void  beep ()
      /* 发出一声铃响  */
```



### Comparator比较器接口

```java
package comparator;

import java.util.Arrays;
import java.util.Comparator;

public class ComparatorTest {
    public static void main(String[] args) {
        String [] word = {"Peter", "Pau", "Mary"};
        Comparator<String > comp = new LengthComparator();

        // 比较大小
        if(comp.compare(word[0], word[1]) > 0)
        {
            System.out.println(word[0]);
        }
        else
        {
            System.out.println(word[1]);
        }

        // 对数组排序
        Arrays.sort(word, new LengthComparator());
        for(String s : word)
        {
            System.out.println(s);
        }
    }
}

class LengthComparator implements Comparator<String>
{
    public int compare(String first, String second) {
//        System.out.println(first + ", "+ second );
        return first.length() - second.length();
    }
}
```





### Cloneable对象克隆接口

- 对于每一个类， 需要确定:
  - 1 ) 默认的 clone 方法是否满足要求;
  - 2 ) 是否可以在可变的子对象上调用 clone 来修补默认的 clone 方法; 3 ) 是否不该使用 clone0
- 实际上第 3 个选项是默认选项。 如果选择第 1 项或第 2 项， 类必须:
  - 1 ) 实现 Cloneable 接口;
  -  2 ) 重新定义 clone 方法， 并指定 public 访问修饰符。



如果在一个对象上调用clone, 但这个对象的类并没有实现Cloneable接口，Object类 的clone方法就会拋出一个CloneNotSupportedException

```java
package clone;

import java.time.LocalDate;
import java.util.Date;
import java.util.GregorianCalendar;

public class Employee implements Cloneable
{
    private String name;
    private double salary;
    private Date hireDay;

    public Employee(String name, double salary)
    {
        this.name = name;
        this.salary = salary;
        hireDay = new Date();
    }

    public Employee clone() throws CloneNotSupportedException {
        Employee cloned = (Employee) super.clone();
        cloned.hireDay = (Date) hireDay.clone();
        return cloned;
    }

    public void raiseSalary(double byPercent)
    {
        double raise = salary * byPercent / 100;
        salary += raise;
    }

    public void setHireDay(int year, int month, int day)
    {
        Date newHireDay = new GregorianCalendar(year, month-1, day).getTime();
        hireDay.setTime(newHireDay.getTime());;
    }

    public String toString()
    {
        return "Employee[name=" + name + ",salary=" +salary + ",hireDay=" + hireDay + "]";
    }
}





package clone;

public class CloneTest {
    public static void main(String[] args) {
        try {
            Employee orginal = new Employee("John Q. public", 50000);
            orginal.setHireDay(2000,1,1);
            Employee copy = orginal.clone();
            copy.raiseSalary(10);
            copy.setHireDay(2002,12,31);
            System.out.println("original= " + orginal);
            System.out.println("copy= " + copy);
        }catch (CloneNotSupportedException e)
        {
            e.printStackTrace();
        }
    }
}

```



## lambda表达式

lambda 表达式是一个可传递的代码块， 可以在以后执行一次或多次。

**lambda 返回值无需指定，总是会由上下文推导得出。**

**lambda 表达式中捕获的变量必须实际上是最终变量 ( effectivelyfinal ) ，最终变量是指 这个变量初始化之后就不会再为它赋新值。**

```java
lambda 表达式具体方式：
			(参数1,参数2) -> { 语句 ;}
		  参数1 -> 语句;
        
	// 如果方法只有一 参数， 而且这个参数的类型可以推导得出， 那么甚至还可以省略小括号
			 ActionListener listener = event -> System.out.println("The time is " + new Date());
			 // 展开后就是下面的样子。
			 ActionListener listener = new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("The actionPerformed is " + new Date());
            }
        };
	// 将 lambda 表达式保存在这个类型的变量中
BiFunction<String ,String,Integer> comp = (first, second) -> first.length() -second.length();

// 这种也是 lamdba 表达式
ArrayList<String> name = new ArrayList<>(); name.add("askdas");
Stream<Person> strea = name.stream().map(Person::new);
Person[] people = stream.toArray(Person[]::new):  	// 最后的 Person[]::new 就是lambda




package lambda;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Arrays;
import java.util.Date;
import javax.swing.*;

public class LambdaTest {
    public static void main(String[] args) {
        String[] planets = new String[] {"Mercury", "Venus", "Earth", "Mars","Jupiter", "Saturn","Uranus", "Neptune"};
        System.out.println(Arrays.toString(planets));
        System.out.println("Sorted in dictionary order:");
        Arrays.sort(planets);
        System.out.println(Arrays.toString(planets));
        System.out.println("sorted by length");
        Arrays.sort(planets, (first,second)-> first.length() - second.length());
        System.out.println(Arrays.toString(planets));

        Timer t =  null;
        String lambda = new String("lambda");
        if(lambda.equals("lambda"))
        {
           t = new Timer(1000, event -> System.out.println("The time is " + new Date()));
        }
        else  {
            //            ActionListener listener = event -> System.out.println("The time is " + new Date());
            ActionListener listener = new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    System.out.println("The actionPerformed is " + new Date());
                }
            };
            t = new Timer(1000, listener);
        }

        t.start();

        JOptionPane.showMessageDialog(null, "Quit program?");
        System.exit(0);
    }
}
```



```java
// 使用方法2和使用方法3

import javax.swing.*;
import java.awt.event.ActionEvent;

public class Termc {
    public void greet(ActionEvent event)
    {
        System.out.println("Hello wordl");
    }
}


class TimedGree extends Termc{  // 继承 Termc 类
    public void greet()
    {
        javax.swing.Timer t = new Timer(1000, super::greet);
        t.start();
    }

    public static void main(String[] args) {
        TimedGree t = new TimedGree();
        t.greet();
        JOptionPane.showMessageDialog(null, "Quit program?");
        System.exit(0);
      
      	// lambda 第三种用法
        ArrayList<String> name = new ArrayList<>();
        name.add("askdas");
        Stream<String> strea = name.stream().map(String::new);
				String[] pre = strea.toArray(String[]::new);
    }
}


```

### lamdba多种样式组合

```java
import javax.swing.*;
import java.awt.event.ActionListener;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;



public class main {

    interface  If1{
        void test();  // 无参数
    }

    @FunctionalInterface
    interface  If2{
        int test(int t1);  // lambda有参数 有返回值
    }

    public int mytestLamdbaFunc(int m1)
    {
        return m1-1;
    }

    @FunctionalInterface   // 说明该接口是给 lambda 使用的。 且里面只能有一个抽象方法
    interface DogService{
        Dog getDog();  // 通过构造方法来返回 Dog 对象。
    }

    @FunctionalInterface   // 说明该接口是给 lambda 使用的。 且里面只能有一个抽象方法
    interface DogService2{
        Dog getDog(String _name, int _age);  // 通过构造方法来返回 Dog 对象。
    }

     static public int mytestLamdbaFuncStatic(int m1) {
        return m1-1;
    }

    public static void main(String[] args) throws IOException {
        If1  if1 = ()->{ System.out.println("lambda无参数 无返回值");};
        if1.test();

        If2 if2 = (t1)->{ return t1; };
        If2 if2_1 = (t1)->t1-4;   // 这个就是上面的简化 省略了 return . 如果想要省略return, 那么必须取消大括号{}
        System.out.println("lambda有参数 有返回值:" + if2.test(2)  + ",更加简化:" + if2_1.test(3) );

        main mm = new main();
        If2 if2_main = mm::mytestLamdbaFunc;
        System.out.println("lambda有参数 有返回值,使用对象中的函数方法:" + if2_main.test(123));

        If2 if2_static = main::mytestLamdbaFuncStatic;
        System.out.println("lambda有参数 有返回值,使用类中的静态函数方法:" + if2_static.test(24));


        DogService dogs1 = Dog::new;  // 无参构造方法引用
        Dog dog1 = dogs1.getDog();
        System.out.println("lambda无参构造方法引用: dog1="+ dog1);

        DogService2 dogs2 = Dog::new;
        Dog dog2 = dogs2.getDog("有参数",2);
        System.out.println("lambda有参构造方法引用: dog2="+ dog2);


        List<Dog> list = new ArrayList<>();
        list.add(new Dog("aa", 1));
        list.add(new Dog("bb", 3));
        list.add(new Dog("cc", 5));
        list.add(new Dog("dd", 2));
        list.add(new Dog("ee", 4));

        System.out.println("lamdba 表达式集合排序");
        list.sort((o1,o2)->{return o1.getAge() - o2.getAge();});

        System.out.println(list);

        System.out.println("lamdba 表达式遍历集合");
        list.forEach(System.out::println);
    }
}

class Dog {
    private String name;
    private int age;
    public Dog(String _name ,int _age){
        name = _name;
        age = _age;
    }

    public Dog(){
        name = "无姓名";
        age = -1;
    }
    public int getAge(){
        return age;
    }

    public String toString()
    {
        return "[name=" + name + ",age=" + age + "]";
    }

}
```





## 内部类

内部类可以访问外围类的私有数据

```java
package innerClass;

import javax.swing.*;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Date;

public class InnerClassTest {
    public static void main(String[] args) {
        TalkingClock clock = new TalkingClock(1000,true);
        clock.start();

        TalkingClock.TimePrinter tp =  clock.new TimePrinter();  // 这样来创建内部类对象。
        System.out.println(tp.getChild());

        JOptionPane.showMessageDialog(null, "Quit program?");
        System.exit(0);
    }
}


class TalkingClock
{
    private  int interval;
    private  boolean beep;

    public TalkingClock(int interval, boolean beep)
    {
        this.interval = interval;
        this.beep = beep;
    }

    public void start ()
    {
        ActionListener listener = this.new TimePrinter();
        Timer t = new Timer(interval, listener);
        t.start();
    }
    public int getInterval()
    {
        return  interval;
    }

    public class TimePrinter implements ActionListener
    {
        public int child = 0;
        public int getChild()
        {
            return  child + TalkingClock.this.getInterval();
        }
        public void actionPerformed(ActionEvent event)
        {
            System.out.println( TalkingClock.this.getInterval());  // 访问外部类的内容。
            System.out.println("At the tone the time is "+new Date());
            if(beep)
                Toolkit.getDefaultToolkit().beep();
        }
    }
}
```



### 局部内部类

 局部类不能用 public 或 private 访问说明符进行声明。它的作用域被限定在声明这个局部类的块中。

局部内部类对外部世界可以完全地隐藏起来。

```java
class TalkingClock
{
  private  int interval;
  
 	public void start()
  {
    // 局部类不能用 public 或 private 访问说明符进行声明。它的作用域被限定在声明这个局部类的块中。
    class TimerPr implements ActionListener 
    {
      	public void actionPerformed(ActionEvent event)
        {
            System.out.println("局部内部类"+new Date());
        }
      	
      ActionListener listner = new  new TimerPr();
	    Timer t = new Timer(interval listner);
		  t.start();
		}
  }  
}
```

```java
package innerClass;

import javax.swing.*;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;
import java.util.Date;

public class InnerClassTest {
    public static void main(String[] args) {
        TalkingClock clock = new TalkingClock(1000,true);
        clock.start();

        TalkingClock.TimePrinter tp =  clock.new TimePrinter();  // 这样来创建内部类对象。
        System.out.println(tp.getChild());

        // 创建匿名类并使用
        System.out.println( new ArrayList<String>() {{add("Harry"); add("Tony");}}  ) ;
        System.out.println("获得外围类类型：" + new Object(){}.getClass().getEnclosingClass() ) ; // InnerClassTest

        JOptionPane.showMessageDialog(null, "Quit program?");
        System.exit(0);
    }
}


class TalkingClock
{
    private  int interval;
    private  boolean beep;

    public TalkingClock(int interval, boolean beep)
    {
        this.interval = interval;
        this.beep = beep;
    }

    public boolean getbeep(){
        return beep;
    }
    public void start ()
    {
        ActionListener listener = this.new TimePrinter();
        Timer t = new Timer(interval, listener);
        t.start();

        class Timep {
            public boolean get()
            {
                return getbeep();
            }
        }

    }
    public int getInterval()
    {
        return  interval;
    }

    public class TimePrinter implements ActionListener
    {
        public int child = 0;
        public int getChild()
        {
            return  child + TalkingClock.this.getInterval();
        }
        public void actionPerformed(ActionEvent event)
        {
            System.out.println( TalkingClock.this.getInterval());  // 访问外部类的内容。
            System.out.println("At the tone the time is "+new Date());
            if(beep)
                Toolkit.getDefaultToolkit().beep();
        }
    }
}
```



### 静态内部类

这样就会产生名字冲突。解决这个问题的办法 是将 Pair 定义为 ArrayAlg 的内部公有类

在 Pair 对象中不需要引用任何其他的对象， 为此， 可以将这个内部类声明为 static

```java
package staticInnerClass;

public class StaticInnerClassTest {
    public static void main(String[] args) {
        double[] d = new double[20];
        for (int i= 0; i < d.length ; i++)
        {
            d[i] = 100 * Math.random();
        }
        ArrayAlg.Pair p = ArrayAlg.minmax(d);
        System.out.println("min = " + p.getFirst());
        System.out.println("max = " + p.getSecond());
    }
}

class ArrayAlg
{
    public static class Pair
    {
        private double first;
        private double second;

        public Pair(double f, double s)
        {
            first = f;
            second = s;
        }

        public double getFirst()
        {
            return  first;
        }

        public double getSecond() {
            return second;
        }
    }

    public static Pair minmax (double[] values)
    {
        double min = Double.POSITIVE_INFINITY;
        double max = Double.NEGATIVE_INFINITY;
        System.out.println("min=" + min + ", max=" + max );
        for (double v : values)
        {
            if (min > v) min = v;
            if (max < v) max = v;
        }
        return new Pair(min, max);
    }
}


```









