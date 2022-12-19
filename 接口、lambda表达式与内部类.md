### 目录

- [接口](#接口)
  - [ActionListener回调定时器和监听器操作](#ActionListener回调定时器和监听器操作)
  - [Comparator比较器接口](#Comparator比较器接口)
  - [Cloneable对象克隆接口](#Cloneable对象克隆接口)
- [lambda表达式](#lambda表达式)
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


class TimedGree extends Termc{
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















