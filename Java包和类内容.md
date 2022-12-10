## 目录

- [jar包内容](#jar包内容)
- [类的导入](#类的导入)
- [将类放入包中](#将类放入包中)
- [设置类路径](#设置类路径)
- [类、超类和子类](#类、超类和子类)
  - [子类](#子类)
  - [阻止继承final类和方法](#阻止继承final类和方法)
  - [抽象类](#抽象类)
  
- [反射](#反射)
  - [反射解析类内容](#反射解析类内容)
  - [运行时使用反射分析对象](#运行时使用反射分析对象)

- [异常](#异常)
  - [捕获异常](#捕获异常)

- [Object所有类的超类](#Object所有类的超类)
- 



## jar包内容

**jar包文件使用ZIP格式组织文件和子目录。**

> 为了使类能够被多个程序共享， 需要做到下面几点:
>
> 1. 把类放到一个目录中 例如 `/home/user/classdir` 需要注意 这个目录是包树状结构 ，。，
>
>    的基目录。 如果希望将 `com.horstmann.corejava.Employee` 类添加到其中， 这个 `Employee.class` 类文件就必须位于子目录 `/home/user/classdir/com/horstmann/corejava` 中。
>
> 2. 将jar文件放在一个目录中， 例如 `/home/user/archives`
>
> 3. 设置类路径(class  path)。类路径是所有包含类文件的路径的集合。
>
>    1. UNIX中， 类路径中的不同项目之间采用冒号 (:) 分隔:
>       1. `/home/user/classdir:.:/home/user/archives/archive.jar`





## 类的导入

```java
一般情况下类导入:
	import java.util.*;

静态导入：  静态方法和静态域
  import static java.lang.System.*;
			// 静态导入后，可这样使用
				out.println("Goog");
```







## 将类放入包中

```java
想要将一个类放入包中，就必须将包的名字放在源文件的开头，包中定义类的代码之前。
  	
  package com.horstmann.corejava;
	
// 该文件应该放到 com/horstmann/corejava/  目录中 ,该文件名为 Employee.java
//  而且 默认包和 com 是同目录。 (默认包是一个没有名字的包)
	public class Employee
  {
    .....
  }
  
```



## 设置类路径

采用 参数来指定类路径，  或者配置 `CLASSPATH` 环境变了来设置类路径。

```bash
# java 编译命令
#   -classpath  指定类路径 (或者 -cp)

Unix> $ java  -classpath /home/user/classdir:.:/home/user/archives/archive.jar MyProg
Windows> $ java -classpath c:\classdir;.;c:\archives\archive.jar MyProg


#配置环境变量
Unix> export CLASSPATH="/home/user/classdir:.:/home/user/archives/archive.jar"
Windows> set CLASSPATH=c:\classdir;.;c:\archives\archive.jar

```





# 类、超类和子类



## 子类

**继承（父类、超类、基类）的（子类、派生类、孩子类）, 关键字 `extends` 表示继承。**

==**Java中只有公有继承，不区分私有和保护继承。**==

与C++相同，子类无法直接修改父类中的私有内容，只能借助父类的公有接口来间接访问。

==**在子类中调用父类的方法时(尤其是覆盖同名的父类方法时)，需要指定 关键字`super` 来获得父类中的方法和内容。**==

**将一个父类的引用赋给一个子类变量， 必须进行类型转换。**

**在子类构造中可以通过 `super` 关键字来指定调用 父类中的构造器（必须是构造器内的第一条指令）。子类通过使用 `this` 关键字来调用自身的引用隐式参数 和 子类的其他构造器（必须是构造器内的第一条指令）。**

**继承后，复写父类中的方法时，子类的新方法可见性不能低于父类方法的可见性。（`public` 可见性大于 `private`）。**

**final可阻止类的继承和方法的拓展。**

**子类和父类转换之前，应该使用 `instanceof` 来进行检查, 具体参阅([Java基础语法和基础对象.md](Java基础语法和基础对象.md)）**

```java
/**
 *  父类、超类、基类
 */
public class Fund
{
    private String name;
    private double money;

    Fund( double aMoney, String aName )
    {
        aMoney = aMoney;
        name = new String(aName);
    }


    public  String getName()
    {
        String temp = new String(name);
        return temp;
    }

    public  void setMoney( double aMoney)
    {
        money = aMoney;
    }

}



/**
 * 	继承的子类、派生类、孩子类
 */

public class Child  extends Fund
{
    private  int workTime;

    Child(int aWorkTime)
    {
        this(0,"",aWorkTime);
    }

    /**
     *
     * @param aMory
     * @param aName
     * @param aWorkTime
     *
     *  super(aMory,aName); 指定调用父类的构造器
     */
    Child(double aMory, String aName, int aWorkTime)
    {
        super(0,"");
        workTime = aWorkTime;
    }
    
    /**
     * @param aMoney    设置money
     *  super 关键字。 指定调用父类的方法
     *  覆盖了父类中的  void setMoney( double aMoney) 方法
     */
    public  void setMoney( double aMoney)
    {
        super.setMoney(aMoney);     //  super 关键字。 指定调用父类的方法
    }

}
```



### 阻止继承final类和方法

**不允许扩展的类被称为 `final` 类。**

**`final` 类和 `final`域并不相关，不会相互影响。**

**在定义类的时候使用了 `final`修饰符就表明这个类是 `final` 类**

```java
public final class Exec 		// 该类不可以再次被继承
{
}

public class Exte
{
  	public final String getName()			// 该类可继承，但该方法不可以被继承的子类覆盖
    { return ""; }
}
```



### 抽象类

**使用 `abstract` 关键字来声明类 和类中的方法， 这样当前类就是个抽象类了。**

**直接给类使用 `abstract` 关键字来声明抽象类， 可以不需要抽象的方法。这样的类也是不可创建实体的。但子类可以。** 

```java
public abstract class Fund			  // abstract 关键字 声明该类是个抽象类。
{
    public void print()
    {
        System.out.println("Fund");
    }
     public abstract void retPrint();	  // 声明该方法是个抽象方法，没有实现，需要继承的子类来实现。
}

public class Child extends  Fund
{
    public void print()
    {
        System.out.println("Child");
    }

    public final  String getName()
    {
        return "";
    }

    public  void retPrint()
    {
        System.out.println("这里实现了 抽象类父类 中 的抽象方法");
    }
}
```









## 反射

**反射则是一开始并不知道我要初始化的类对象是什么，自然也无法使用 new 关键字来创建对象。**

**反射: 是指在程序运行期间发现更多的类及其属性的能力。**

**反射库(reflection library) 提供了一个非常丰富且精心设计的工具集， 以便编写能够动态操纵 Java 代码的程序。**

使用反射， Java 可以支持 Visual Basic 用户习惯 使用的工具。特别是在设计或运行中添加新类时， 能够快速地应用开发工具动态地查询新添 加类的能力。

- **能够分析类能力的程序称为反射 ( reflective )**
  - 反射机制的功能：
    - 运行时分析类的能力。
    - 在运行时查看对象， （例如， 编写一个 toString 方法供所有类使用）
    - 实现通用的数组操作代码。
    - 利用 Method 对象， 这个对象很像 C++ 中的函数指针。

```java
public class Main {
    public static void main(String[] args) throws ClassNotFoundException {
      	// Object 类中的 getClass( ) 方法将会返回一个 Class 类型的实例
       Employee e = new Employee("名称",1.0,2,3,4);
       System.out.println(e.getClass().getName());		// 输出： inheritance.Employee
			
      	// 如果类在一个包里， 包的名字也作为类名的一部分:
        Random generator = new Random();
        Class cl = generator.getClass();
        String name = cl.getName();
        System.out.println(name);  			// 输出： java.util.Random

      	// 还可以调用静态方法 forName 获得类名对应的 Class 对象。
        Class newcl = Class.forName(name);
        System.out.println(newcl.toString()); 		// 输出： class java.util.Random
      // 如果类名保存在字符串中， 并可在运行中改变，就可以使用这个方法。
      // 这个方法 只有在 className 是类名或接口名时才能够执行
      
     //  获得 Class 类对象的第三种方法非常简单。如果 T 是任意的 Java 类型(或 void 关键字，) T.class 将代表匹配的类对象。
        Class cl1 = Random.class;
        Class cl2 = int.class;
        Class cl3 = Double[].class;
      // 一个 Class 对象实际上表示的是一个类型， 而这个类型未必一定是一种类。 例如， int 不是类， 但 int.class 是一个 Class 类型的对象。
    }
}




Class 相关API:    (这是个类)  import java.lang.Class;
		static Class  forName (String className)
      /* 返回描述类名为 className 的 Class 对象 */

		Object newInstance ()
      /* 返回这个类的一个新实例, 需要调用无参数构造 */

Constructor 相关API:    (这是个类)  import java.lang.reflect.Constructor;
		Object newInstance (Object[] args)    // JDK 18 已弃用
      /* 构造一个这个构造器所属类的新实例。 带参数的
      	 参数： args   这是提供给构造器的参数。 	
      */
```

**Class类中的getFields、getMethods和getConstructors方法将分别返回类提供的 public 域、 方法和构造器数组， 其中包括超类的公有成员。 Class 类的 getDeclareFields、 getDeclareMethods 和getDeclaredConstructors 方法将分别返回类中声明的全部域、 方法和构 造器， 其中包括私有和受保护成员， 但不包括超类的成员。**

```java
/* 
 * 读取输入的类名，打印类的内容和方法内容
*/
public class ReflectionTest {
    public static void main(String[] args)  {
        // 从命令行 读取类名
        String name = new String("java.lang.Double");
//        if(args.length > 0)
//        {
//            name = args[0];
//        }
//        else
//        {
//            Scanner in = new Scanner(System.in);
//            System.out.println("Enter class name (e.g java.util.Date);");
//            name = in.next();
//        }
        try {
            // 输出类名和基类名 （if != Object)
            Class cl = Class.forName(name);  // 获得类名 class java.lang.Double
            Class supercl = cl.getSuperclass();  // 获得 父类 类名 class java.lang.Number
            String modifiers = Modifier.toString(cl.getModifiers()); // 获得 类的类型 public final
            if (modifiers.length() > 0)
            {
                System.out.print(modifiers + " ");
            }
            System.out.print("class " + name);
            if (supercl != null && supercl != Object.class)
            {
                System.out.print("extends " + supercl.getName() );
            }
            System.out.print("\n{\n");
            printConstructors(cl);
            System.out.println();
            printMethods(cl);
            System.out.println();
            printFields(cl);
            System.out.println("}");
        }
        catch (ClassNotFoundException e)
        {
            e.printStackTrace();
        }
        System.exit(0);
    }

    public static void printConstructors(Class cl)
    {
        Constructor[] constructors = cl.getDeclaredConstructors();

        for (Constructor c: constructors)
        {
            String name = c.getName();
            System.out.print("  ");
            String modifiers = Modifier.toString(c.getModifiers());
            if (modifiers.length() > 0)
            {
                System.out.print(modifiers + " ");
            }
            System.out.print(name + "(");

            // 输出参数类型
            Class[] paramTypes = c.getParameterTypes();
            for (int j = 0; j < paramTypes.length ; j++)
            {
                if (j > 0)
                {
                    System.out.print(", ");
                }
				System.out.print(paramTypes[j].getName());
            }
            System.out.println(");");
        }
    }

    public  static void printMethods(Class cl)
    {
        Method[] methods = cl.getDeclaredMethods();

        for (Method m : methods)
        {
            Class retType = m.getReturnType();
            String name = m.getName();

            System.out.print("  ");

            // 输出返回类型
            String modifiers = Modifier.toString(m.getModifiers());
            if(modifiers.length() > 0)
            {
                System.out.print(modifiers + " ");
                System.out.print(retType.getName() + " " + name  + "(");

                // 打印参数类型
                Class[] paramTypes = m.getParameterTypes();
                for(int j = 0; j< paramTypes.length ; j++)
                {
                    if ( j>0 )
                    {
                        System.out.print(", ");
                    }
                    System.out.print(paramTypes[j].getName());
                }
                System.out.println(");");
            }
        }
    }

    public  static void printFields(Class cl)
    {
        Field[] fields = cl.getDeclaredFields();

        for (Field f : fields)
        {
            Class type = f.getType();
            String name = f.getName();
            System.out.print("   ");
            String modifiers = Modifier.toString(f.getModifiers());
            if (modifiers.length() > 0)
                System.out.print(modifiers + " ");
            System.out.println(type.getName() + " " + name + ";");
        }
    }
}


/* 输出内容如下：
public final class java.lang.Doubleextends java.lang.Number
{
  public java.lang.Double(double);
  public java.lang.Double(java.lang.String);

  public boolean equals(java.lang.Object);
  public static java.lang.String toString(double);
  public java.lang.String toString();
  public int hashCode();
  public static int hashCode(double);
  public static double min(double, double);
  public static double max(double, double);
  public boolean isInfinite();
  public static boolean isInfinite(double);
  public static native long doubleToRawLongBits(double);
  public static long doubleToLongBits(double);
  public static native double longBitsToDouble(long);
  public int compareTo(java.lang.Double);
  public volatile int compareTo(java.lang.Object);
  public static int compare(double, double);
  public byte byteValue();
  public short shortValue();
  public int intValue();
  public long longValue();
  public float floatValue();
  public double doubleValue();
  public static java.lang.Double valueOf(double);
  public static java.lang.Double valueOf(java.lang.String);
  public static java.lang.String toHexString(double);
  public volatile java.lang.Object resolveConstantDesc(java.lang.invoke.MethodHandles$Lookup);
  public java.lang.Double resolveConstantDesc(java.lang.invoke.MethodHandles$Lookup);
  public java.util.Optional describeConstable();
  public boolean isNaN();
  public static boolean isNaN(double);
  public static double sum(double, double);
  public static boolean isFinite(double);
  public static double parseDouble(java.lang.String);

   public static final double POSITIVE_INFINITY;
   public static final double NEGATIVE_INFINITY;
   public static final double NaN;
   public static final double MAX_VALUE;
   public static final double MIN_NORMAL;
   public static final double MIN_VALUE;
   public static final int SIZE;
   public static final int PRECISION;
   public static final int MAX_EXPONENT;
   public static final int MIN_EXPONENT;
   public static final int BYTES;
   public static final java.lang.Class TYPE;
   private final double value;
   private static final long serialVersionUID;
}
*/

Class 相关API:    (这是个类)  import java.lang.Class;
		Field[]  getFields ()
		Field[]  getDeclaredFields ()
      /* getFields 方法将返回一个包含 Field 对象的数组， 这些对象记录了这个类或其超类的公有域。
      	 getDeclaredField 方法也将返回包含 Field 对象的数组， 这些对象记录了这个类的全部域。 
      	 如果类中没有域， 或者 Class 对象描述的是基本类型或数组类型，这些方法将返回一个长度为 0 的数组 */

		Method[] getMethods ()
		Method[] getCeclareMethods ()
      /* 返回包含 Method 对象的数组: getMethods 将返回所有的公有方法， 包括从超类继承 来的公有方法;
         getDeclaredMethods 返回这个类或接口的全部方法， 但不包括由超类继承了的方法。 */

    Constructor   getConstructor ()
    Constructor[] getConstructors ()
    Constructor[] getDeclaredConstructors ()
	    /* 	返回包含 Constructor 对象的数组， 其中包含了 Class 对象所描述的类的所有公有构造器(getConstructors) 或所有构造器(getDeclaredConstructors。) */

Field 相关API:    (这是个类)  import java.lang.reflect.Field;
Method 相关API:    (这是个类)  import java.lang.reflect.Method;
Constructor 相关API:    (这是个类)  import java.lang.reflect.Constructor;
		Class  getDeclaringClass ()
      /* 返回一个用于描述符类中定义的构造器、方法域的 Class 对象 */
      
		Class[]  getExceptionTypes ()    (在 Constructor 和 Method 类中)
      /* 返回一个用于描述方法抛出的异常类型的 Class 对象数组。 */
	
    int getModifiers ()
      /* 返回一个用于描述构造器、方法或域的修饰符的整型数值。使用 Modifier 类中的这个方法可以分析这个返回值。 */
    
    String getName()
      	/*  返冋一个用于描述构造器、 方法或域名的字符串。 */
      
    Class[]  getParameterTypes ()    (在 Constructor 和 Method 类中)
      	/*  返回一个用于描述参数类型的 Class 对象数组。 */

    String getReturnType()      (在 Method 类中)
      	/*  返回一个用于描述返回类型的 Class 对象。 */

Modifier 相关API:    (这是个类)  import java.lang.reflect.Modifier;
    static  String toString (int modifiers)
      	/*  返回对应 modifiers 中位设置的修饰符的字符串表示。*/

    static boolean isAbstract  (int modifiers)
    static boolean isFinal     (int modifiers)
    static boolean isInterface (int modifiers)
    static boolean isNative    (int modifiers)
    static boolean isPrivate   (int modifiers)
    static boolean isProtected (int modifiers)
    static boolean isPublic    (int modifiers)
    static boolean isStatic    (int modifiers)
    static boolean isstrict    (int modifiers)
    static boolean isSyncchronized (int modifiers)
    static boolean isVolatile  (int modifiers)
      	/*  这些方法将检测方法名中对应的修饰符在 modifiers 值中的位  */

```



### 运行时使用反射分析对象

**利用反射机制可以查看在编译时还不清楚的对象域。**

查看对象域的关键方法是 `Field` 类中的 get 方法。 (只允许査看任意对象有哪些域， 而不允许读取它们的值) ，如果 f 是一个 Field 类型的对象(例如， 通过 getDeclaredFields 得到的对象，) obj 是某个包含 f 域的类的对象， f.get(obj) 将返回一个 对象， 其值为 obj 域的当前值。

```java



AccessibleObject 相关API:    (这是个类)  import java.lang.reflect.AccessibleObject;
    void  setAccessible (boolean flag)
      	/*  为反射对象设置可访问标志。 flag 为 true 表明屏蔽 Java 语言的访问检查， 使得对象的 私有属性也可以被査询和设置。 */
      
    boolean  isAccessible ()
      	/*  返回反射对象的可访问标志的值。 */
      
    static void  setAccessible ( AccessibleObject[] array, boolean flag)
      	/*  是一种设置对象数组可访问标志的快捷方法。 */
      
      
Class 相关API:    (这是个类)  import java.lang.Class;
    Field    getField (String name)
    Field[]  getField ()
      	/* 返回指定名称的公有域， 或包含所有域的数组。 */
      
    Field    getDeclaredField (String name)
    Field[]  getDeclaredFields ()
      	/* 返回类中声明的给定名称的域， 或者包含声明的全部域的数组。 */
      
Field 相关API:    (这是个类)  import java.lang.reflect.Field;
    Object   get (Object obj)
      	/* 返回 obj 对象中用 Field 对象表示的域值。 */
      
    void  set (Object obj, Object newValue)
      	/* 用一个新值设置 Obj 对象中 Field 对象表示的域。 */
```





## 异常

### 捕获异常

```java
try 
{
  可能会抛出异常的代码段
} 
catch(Exception e)
{
  	处理异常代码
}

 Throwable 相关API:    (这是个类)  import java.lang.Throwable;  Exception 继承于 Throwable
		void  printStackTrace ()
      /* 将 Throwable 对象和栈的轨迹输出到标准错误流。 */
```









## Object所有类的超类

Object 类是 Java 中所有类的始祖， 在 Java 中每个类都是由它扩展而来的。 是默认开始类的默认父类。

可以使用 Object 类型的变量引用任何类型的对象。

只有基本类型 ( primitive types ) 不是对象， 例如， 数值、 字符和布尔类型的 值都不是对象。

所有的数组类塱， 不管是对象数组还是基本类型的数组都扩展了 Object 类。

**Object 类中的 `equals` 方法用于检测一个对象是否等于另外一个对象，也就是判断引用。该方法可以被覆盖。 如果该方法重写覆盖，那么 `hashCode` 方法也必须进行重定义。**

Equals 与 hashCode 的定义必须一致: 如果 x.equals(y) 返回 true, 那么 x.hashCode( ) 就必 须与 y.hashCode( ) 具有相同的值。

```java
Object obj = new 其他类();		// 这样是可以的。


Object objC = new Child();
Object objF = new Fund();
if (objC.equals(objF) == true)		// 检测对象是否等于另一个对象。
{
  System.out.println("true");
}
else
{
  System.out.println("false");			// 会执行这里
}


/**
*  编写 equals 方法的范例
*/
 class Fund
{
    Fund()
    {
        id =1;
        name = new String("对象值");
    }
    private int id;
    private String name;

    public void print()
    {
        System.out.println("Fund");
    }
     public  void retPrint(){

     }

    @Override   public boolean equals (Object otherObject)
    {
        if(otherObject == null)
            return false;

        if( this == otherObject )
            return true;

        // 比较 this 与 otherObject 是否属于同一个类。如果 equals 的语义在每个子类中有所改变， 就使用 getClass 检测:
        if(getClass() != otherObject.getClass())
            return false;

        // 如果所有的子类都拥有统一的语义， 就使用 instanceof 检测:
        if(! (otherObject instanceof Fund))
            return false;

        //对所有需要比较的域进行比较
        Fund other = (Fund)otherObject;
        // 开始对比里面详细的所有变量
        if(other.id == this.id && other.name.equals(this.name))
        {
            return true;
        }
        return false;
    }

    @Override  public int hashCode()
    {
        System.out.println(getClass().toString());

        return Objects.hash(name, id);
    }

    @Override public String toString()
    {
        return super.toString() + "[name=" + name.toString() + ", id=" + id + "]";
    }
}



Arrays 相关API:    (这是个类)   java.util.Arrays
		static Boolean equals (type[] a, type[] b)
      /* 如果两个数组长度相同， 并且在对应的位置上数据元素也均相同，将返回true。数组 的兀素类型可以是 Object、 int、 long、 short、 char、 byte、 boolean、 float 或 double */
		
  
Object 相关API:    (这是个类)   java.util.Object
		static boolean equals (Object a, Object b)
      /* 如果 a 和 b 都为 null， 返回 true ; 如果只有其中之一为 null， 则返回 false ; 否则返回 a.equals(b)  */
  
		boolean equals (Object otherObject)
      /* 比较两个对象是否相等， 如果两个对象指向同一块存储区域， 方法返回 true ;否则方法返回 false。在自定义的类中， 应该覆盖这个方法。  */

  	String toString ()
			/* 返冋描述该对象值的字符串。在自定义的类中， 应该覆盖这个方法。 */
  	

    Object 散列码内容
		int hashCode ()
      /* 返回对象的散列码。 散列码可以是任意的整数，包括正数或负数。两个相等的对象要求返回相等的散列码。*/
  
		static int hash (Object... Objects) 
      /* 返回一个散列码， 由提供的所有对象的散列码组合而得到。  */
  
		static int hashCode (Object a) 
      /* 如果 a 为 null 返回 0， 否则返回 a.hashCode() */
  
		static int hashCode ((int|long|short|byte|double|float|char|boolean) value) 
      /* 返回给定值的散列码。 */

		static int hashCode (type[] a) 
      /* 计算数组 a 的散列码 组成这个数组的元素类型可以是 object int long, short, char, byte, boolean, float 或 double。 */
  
  
获得类名字,需要在当前类内调用。
		Class getClass () 
      /* 返回包含对象信息的类对象。它的内容被封装在 Class 类中 */

  	Class getSuperclass ()
  		/* 以 Class 对象的形式返回这个类的超类信息。 */
  	
```





