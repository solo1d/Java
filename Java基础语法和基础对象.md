## 目录

- [基础语法和关键字](#基础语法和关键字)
- [基础类型和类型转换](#基础类型和类型转换)
- [String字符串API列表](#String字符串API列表)
- [构建String字符串的StringBuilder类](#构建String字符串的StringBuilder类)
- 数组内容
  - [输出数组内的全部字符串](#输出数组内的全部字符串)
  - [数组深拷贝和浅拷贝](#数组深拷贝和浅拷贝)
  - [创建不规则数组](#创建不规则数组)

- [数学函数与常量](#数学函数与常量)
- [大数值](#大数值)
- [命名规则](#命名规则)
- [获取终止程序时返回的代码方法](#获取终止程序时返回的代码方法)
- [逻辑右移和算数右移](#逻辑右移和算数右移)
- [基础输入输出](#基础输入输出)
- [文件输入输出](#文件输入输出)
- [基础日期和时间类](#基础日期和时间类)
- [随机数生成器](#随机数生成器)
- 







## 基础语法和关键字

```java
/** 
* 范例程序，解释基础 
* This is the first sample program in Core Java Chapter 3
* Aversion 1.01 1997-03-22
* ©author Gary Cornell
*/
public class FirstSample
{
  public static final double CM=12;
  
	public static void main(String[] args)
	{
    final int FIN =0;
		System.out.println("We will not use 'Hello, world!'");
	}
  
  public  strictfp YA()
  {
    double a = 1.0 * 12.2;
  }
}


关键字： 
  	public  称为访问修饰符(assess modifier), 用于控制程序的其他部分对这段代码的访问级别。
    class   表明 Java 程序中的全部内容都包含在类中。
			      Java 应用程序中的全部内容都必须放置在类中。
		final   指示常量，类内的话就代表该值必须在类构造时初始化。
    strictfp  表示该方法内，所有的指令都将使用严格的浮点计算。
```



## String字符串API列表

```java
 一个 CharSequence 形参， 完全可以传入 String 类型的实参 。 

char charAt (int index);
			/* 返回给定位置的代码单元。 */

int codePointAt (int index)
  		/* 返回给定位置开始的码点   */

int offsetByCodePoints (int startIndex, int cpCount)
  		/* 返回从 startIndex 代码点开始， 位移 cpCount后的码点索引 */

int compareTo (String other)     *** 重要
	  	/* 按照字典顺序， 如果字符串位于 other 之前，返回一个负数; 如果字符串位于 other 之后， 返回一个正数; 如果两个字符串相等， 返回 0。 */

IntStream  codePoints()    			 *** 重要
  		/* 将这个字符串的码点作为一个流返回。 调用 toArray 将它们放在一个数组中。 */
  
new String (int[] codePoints, int offset, int count)     *** 重要
			/* 用数组中从 offset 开始的 count 个码点构造一个字符串。 */
  
boolean equals (Object other) 		*** 重要
			/* 如果字符串与 other 相等， 返回 true。 区分大小写  */

boolean equalsIgnoreCase (String other)	   *** 重要
			/* 如果字符串与 other 相等， 返回 true。 不区分大小写  */
  
boolean startsWith (String prefix)
boolean endsWith (String prefix)
			/* 如果字符串以 suffix 开头或结尾， 则返回 true。 */
  
int indexOf (String str)      *** 重要
int indexOf (String str, int fromIndex)
int indexOf (int cp)
int indexOf (int cp, int fromIndex)
			/* 返回与字符串str 或代码点cp 匹配的第一个子串的开始位置。 这个位置从索引0或fromIndex开始计算。 如果在原始串中不存在 str ,则返回 -1 */

int lastIndexOf (String str)      *** 重要
int lastIndexOf (String str, int fromIndex)
int lastIndexOf (int cp)
int lastIndexOf (int cp, int fromIndex)
			/* 返回与字符串str 或代码点cp 匹配的最后一个子串的开始位置。 这个位置从原始串尾端或fromIndex开始计算。 如果在原始串中不存在 str ,则返回 -1 */

int length ()      *** 重要
			/* 返回与字符串的长度 */
  
int codePointCount (int startIndex, int endIndex)
			/* 返回 startIndex 和 endIndex- 1 之间的代码点数量。 没有配成对的代用字符将计入代码点。 */
  
String replace (CharSequence oldString, CharSequence newString)      *** 重要
			/* 返回一个新字符串。 这个字符串用 newString 代替原始字符串中所有的 oldString。 可以用 String 或 StringBuffer 对象作为 CharSequence 参数。 */
  
String substring (int beginIndex)      *** 重要
String substring (int beginIndex, int endIndex)
			/* 返回一个新字符串，这个字符串包含原始字符从 beginIndex 到串尾或 endIndex -1 的所有代码单元 */
			
String toLowerCase ()			 *** 重要
String toUpperCase ()
			/* 返回一个新字符串， 这个字符串将原始字符串中的大写字母改为小写字母，或者将原始字符串中的所有小写字母改成了大写字母 */

String trim ()			 *** 重要
			/* 返回一个新字符串，这个字符串将删除了原始字符串头部和尾部的空格 */
  
String join (CharSequence delimiter, CharSequence... elements)			 *** 重要
			/* 返回一个新字符串， 用给定的定界符链接所有元素 */
```



## 构建String字符串的StringBuilder类

```java 
 按键或来自文件中的单词。 采用字符串连接的方式达到此目的效率比较低。 每次连接字符串，都会构建一个新的 String 对象，既耗时，又浪费空间。使用 StringBuilder 类就可以避免这个问题的发生。
   
如果需要用许多小段的字符串构建一个字符串， 那么应该按照下列步骤进行。 首先， 构建一个空的字符串构建器:
        StringBuilder builder = new StringBuilder();
				String str = "hello";
        builder.append("字符串内容");
        builder.append(str);
				String newStr = builder.toString();			// newStr 就是新的字符串。是拷贝出来的。


StringBuilder 相关API:    (这是个类)
	StringBuilder()
    		/* 构造一个空的字符串构建器 */
    
	int length ()
    		/* 返回构建器或缓冲器中的代码单元数量。 */

  StringBuilder append (String str)
    		/* 追加一个字符串并返回 this */

  StringBuilder append (char c)
    		/* 追加一个代码单元并返回 this。 */
    
  StringBuilder appendCodePoint (int cp)
     	/* 追加一个代码点， 并将其转换为一个或两个代码单元并返回 this	 */

	void setCharAt (int i, char c)
    	/* 将第 i 个代码单元设置为 c */

	StringBuilder insert (int offset, String str)
    	/* 在 offset 位置插入一个字符串并返回 this */

	StringBuilder insert (int offset, char c)
    	/* 在 offset 位置插入一个字符串并返回 this */

	StringBuilder delete (int startIndex, int endIndex)
     	/* 删除偏移量从 startIndex 到  endIndex -1 的代码单元并返回 this。 */
    
	StringBuilder toString ()
     	/* 返回一个与构建器或缓冲器内容相同的字符串 */
```





## 基础类型和类型转换

```java
 在Java中， 一共有8 种基本类型 ( primitive type ), 
		其中有 4 种整型、 2 种浮点类型、 1 种用于表示 Unicode 编码的字符单元的字符类型 char 和 1 种用于表示真值的 boolean 类型。
 Java 没有任何无符号(unsigned) 形式的 int、long、short 或 byte 类型。
      

 整数
			类型	存储需求		取值范围
			int			4字节			-2147483648 ~ 2147483647
			short		2字节			-32767 ~ 32767
			long 		8字节			-9223372036854775808 ~ 9223372036854775807
			byte		1字节			-128 ~ 127


 浮点
			类型		存储需求		取值范围
			float		4字节			大约 ± 3.402 823 47E+38F (有效位数为 6 ~ 7 位)
			double	2字节			大约 ± 1.797 693 134 862 315 70E+308 (有效位数为 15 位)
			
可以使用十六进制表示浮点数值。例如，0.125=2(—3次方)可以表示成0xl.0p-3。在十六 进制表示法中， 使用 p 表示指数， 而不是 e。 注意， 尾数采用十六进制， 指数采用十进制。 指数的基数是 2， 而不是 10。

三个特殊的浮点数值: 
			正整数除以 0 的结果为正无穷大。计算 0/0 或者负数的平方根结果为 NaN (不是一个数字）。
			

      
char类型
		Unicode 字符需要两个或一个 char值。 	
    		'\u0000' 到 '\Uffff' 是 Unciode编码。
	注意：Unicode 转义序列会在解析代码之前得到处理， 但也可以通过自增的char变量 来输出所有的 Unciode字符。
	 在 Java 中， char 类型描述了 UTF-16 编码中的一个代码单元。

      
boolean类型
	与C++不同，尽量不要使用类型转换。

枚举类型
	枚举类型的变量只能存储这个类型声明中给定的某个枚举值， 或者 null 值， null 表示这 个变量没有设置任何值。
	enum Size { A, B, C };
	使用 Size Tp  =  Size.A;
	
字符串
		Java 字符串由 char 值序列组成。
		Java 没有内置的字符串类型， 而是在标准 Java 类库中提供了 一个预定义类， 很自然地叫做 String。
			String e = ""; 		// an empty string 
			String greeting = "Hello";
		Java 字符串内容是无法修改，通过拼接来重新创建一个String 的字符串, 或者修改原字符串变量指向另一个新的字符串.
子串
		String 类的 substring 方法可以从一个较大的字符串提取出一个子串。 例如:
			String greeting = "Hello";
			String s = greeting.substring(0, 3);		// 从0开始到2 （不包括3）
拼接
		 Java 语言允许使用 + 号连接(拼接)两个字符串。
		 		String expletive = "Expletive"; 
		 		String PC13 = "deleted";
				String message = expletive + PC13;
		当将一个字符串与一个非字符串的值进行拼接时， 后者被转换成字符串(任何一个 Java 对象都可以转换成字符串):
				int age = 13;
				String rating = "PC" + age;
							// rating =  "PC13";
		如果需要把多个字符串放在一起， 用一个定界符分隔， 可以使用静态 join 方法:
				String all = String.join(" / ", "S", "M", "L", "XL");
                    // all is the string "S / H / L / XL"
检测字符串是否相等
		以使用 equals 方法检测两个字符串是否相等（区分大小写）。对于表达式:
					s.equals(t)；  // 字符串s与字符串t相等，则返回true; 否则，返回false
					"str".equals("对比字符串");   // 这样也是合法的.
		不区分大小写的字符串对比，可以使用 equalsIgnoreCase 方法。
				"Hello".equalsIgnoreCase("hel1o");   // 会返回 true

		Java 的 compareTo 方法与 strcmp() 完全类似， 因此， 可以这样使用:
					String str="hello";
					if (str.compareTo("Hello") === 0) { ... }

		一定不要使用 == 运算符检测两个字符串是否相等!这个运算符只能够确定两个字串 是否放置在同一个位置上。当然， 如果字符串放置在同一个位置上， 它们必然相等

检测字符串是否为空串
		先检查字符串是否为 null, 然后在检查字符串长度是否为0 (或者等于 "" 空串).
			if (str != null && str.length() != 0)
					/* 在字符串为 null 时，调用其他方法会出现错误。 */

码点与代码单元
		Java 字符串由 char 值序列组成。char 数据类型是一个采用 UTF-16 编码表示 Unicode 码点的代码单元。
				辅助字符需要一对代码单元表示。
			字符串.length() 方法返回采用 utf-16 编码表示的给定字符串所需要的代码单元数量。
					String str = "hello";
					int n = str.length();  // n = 5
			获得实际的大长度，即码点数量。
					int cpCount = str.codePointCount(0, str.length());
			调用 str.charAt(N) , 将返回位置 N 的代码单元， N 介于 0 ~ str.length()-1 之间。
					char frstr = str.charAt(0);   // frstr = 'h'
					char last  = str.charAt(4);   // last = 'o'
		如果 str 中第2个是需要两个代码单元表示的字符，那么 str.charAt(3)  会得到第2个字符的第二个代码单元，而不是第三个字符.
			如果想要遍历一个字符串， 并且依次査看每一个码点， 可以使用下列语句:
							int cp = str.codePointAt(i);
							if (Character.isSupplementaryCodePoint(cp))   i+=2 ;
							else  输出 ;  i++;  
				从末尾开始向前输出:
							i++;
							if (Character.isSurrogate(str.charAt(i)))  i--;
							int cp = str.codePointAt(i);

				 使用 codePoints 方法， 它会生成一个 int 值的“ 流”， 每个 int 值对应一个码点，转换为数组后再遍历.
							int[] codeP = str.codePoints().toArray();
				 反之， 要把一个码点数组转换为一个字符串， 可以使用构造函数。
                String str = new String(codeP, 0, codeP.length);
```

```java
不要在 boolean 类型与任何数值类型之间进行强制类型转换， 这样可以防止发生错误。 只有极少数的情况才需要将布尔类型转换为数值类型， 这时可以使用条件表 达式b? 1:0  
  
强制类型转换
  	double x = 9.1;
		int nx = (int) x;
```





## 输出数组内的全部字符串

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] a = {1, 2, 3, 4, 5, 6};
				
      // 第一种方式。 也是最为方便的。此方法会输出全部字符串。 
        System.out.println(Arrays.toString(a));
      		/// 输出  [1, 2, 3, 4, 5, 6]
      
      // 第二种方式。 使用 for 循环
      	for (int i : a)
        {
	          System.out.println(i);
        }
      
// 多维数组的输出方式:
        int[][][] a2 =
        {
          {
            {1,2,3,4},
            {5,6,7,8}
          },
          {
            {1,2,3,4},
            {5,6,7,8}
          }
        };			// int[2][2][4];
			  
      
      // 第一种方式。 也是最为方便的。此方法会输出全部字符串。 
      	   System.out.println(Arrays.deepToString(a2));
		      			// 输出： [[[1, 2, 3, 4], [5, 6, 7, 8]], [[1, 2, 3, 4], [5, 6, 7, 8]]]
      
      // 第二种方式。 使用 多重循环
        for (int row1[][] : a2) {
            for (int[] row : row1) {
                for (int value : row) {
                    System.out.println(value);
                }
            }
        }
    }
}
```



## 数组深拷贝和浅拷贝

```java
// 数组浅拷贝, 两个变量声明都指向同一个内存位置。
	int [] src = new int[] {1,2,3,4,5};
  int [] cpy = src;
  cpy [0] = 9;			// src 和 cpy 的 [0]  都会被修改为9


// 数组深拷贝, 需要使用方法来进行深拷贝。
  int [] src = new int[] {1,2,3,4,5};
  int [] cpy = Arrays.copyOf(src, src.length);		// 使用数组方法 Arrays.cpuyOf(); 进行深拷贝
	cpy [0] = 9;			// 只有 cpy 的[0] ，变成了 9
  

Arrarys 相关API:    java.util.Arrays
	static String toString (type a[])
    		/* 返回包含 a 中数据元素的字符串， 这些数据元素被放在括号内， 并用逗号分隔 */
  			  // type=int、double、float、long、boolean、char 等
	
  static type copyOf (type a[], int length)
  static type copyOfRang (type a[], int start, int end)
    		/*返回与 a 类型相同的一个数组 ,其长度为 length 或者 end - start， 数组元素为 a 的值 */
				   // type=int、double、float、long、boolean、char 等
  				 // start 起始下标， 包含这个值
  				 // end   终止下标， 不包含这个值， 如果该值大于a.length() 那么多余的内容会填充为0
  				 // length 拷贝的数据元素长度， 如果该值大于a.length() 那么多余的内容会填充为0
	

  static void sort (type a[])
  			/* 采用优化的快速排序算法对数组进行排序 */
  			 //type = int、double、float、long、boolean、char 等
  
	static int binarySearch (type a[], type v)
  static int binarySearch (type a[], int start, type v)
  			/* 采用二分搜索算法查找值 v。如果查找成功， 则返回相应的下标值; 否则， 返回一个 负数值r 。 r值为 搜索的元素数量 取反 ( -r ) */
  			   //type = int、double、float、long、boolean、char 等


  			/* 返回包含 a 中数据元素的字符串， 这些数据元素被放在括号内， 并用逗号分隔 */
  			   //type = int、double、float、long、boolean、char 等

  static void fill (type a[], type v)
  			/* 将数组的所有数据元素设置为 v  */
  			   //type = int、double、float、long、boolean、char 等

  static boolean equals (type a[], type v)
  			/* 如果两个数组大小相同， 并且下标相同的元素都对应相等， 返回 true */
  			   //type = int、double、float、long、boolean、char 等



  
```



## 创建不规则数组

```java
// 创建一个行数为10的数组。(java数组就是个表, 不是在内存中连续的,而是记录每个串所在的位置,并使用下标查找)
int [][] odds = new int [5][];		// 后面的空[] ,就相当于是个通配符号，代表后面的长度不固定。
for (int n = 0; n < 5; n++) {
  odds[n] = new int[n + 1];				// 给予了新的数组。 而且每个数组的长度不一样
}

System.out.println(Arrays.deepToString(odds));  //输出一次

输出内容：
 [[0], [0, 0], [0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0, 0]]
```





## 数学函数与常量

```java
import static java.1ang.Math.*;			/* 引用数学计算库 */

在 Math 类中， 包含了各种各样的数学函数
要想计算一个数值的平方根， 可以使用 sqrt 方法:
double x = 4;
double y = Math.sqrt(x); 
System.out.println(y); // prints 2.0


Math类提供了一些常用的三角角函数:
Math.sin
Math.cos 
Math.tan
Math.atan 
Math.atan2
  
还有指数函数以及它的反函数(自然对数以及以 10 为底的对数):
Math.exp
Math.log 
Math.loglO
  
最后， Java 还提供了两个用于表示 派(3.14...) 和 e 常量的近似值: 
Math.PI
Math.E
```



## 大数值

```java
import java.math.*    // 包中有大数值内容。
  //   BigInteger 和 BigDecimal  这两个类可以处理包含任意长度数字序列的数值。
  //      无法对 大数值内容 使用 + - * 等运算符， 需要用到特定的方法. add()  , multiply() 之类的。
  //   BigInteger 是 任意精度的整数运算。
  
	BigInteger big = BigInteger.valueOf(100);    // 静态方法 valueOf 将普通的数值转换成大数值
	BigInteger b = BigInteger.valueOf(123);    // 静态方法 valueOf 将普通的数值转换成大数值
	BigInteger c = big.add(b);									// c = big + b
	BigInteger e = c.multiply(b.add(BigInteger.valueOf(2)));   //  e = c * ( b + 2);


BigInteger 相关API:    (这是个类)  import java.math.BigInteger;
		BigInteger add (BigInteger other)
		BigInteger subtract (BigInteger other)
		BigInteger multiply (BigInteger other)
		BigInteger divide (BigInteger other)
		BigInteger mod (BigInteger other)
	      /* 返冋这个大整数和另一个大整数 other 的 和、差、积、商以及余数。 */

    int compareTo (BigInteger other)
	      /* 如果这个大整数与另一个大整数 other 相等，返回 0; 如果这个大整数小于另一个大整数other, 返回负数;  否则，返回正数。 */
      
    static  BigInteger valueOf (long x)
	      /* 返回值等于 x 的大整数 */

      
BigDecimal 相关API:    (这是个类)  import java.math.BigDecimal;
		BigDecimal add (BigDecimal other)
		BigDecimal subtract (BigDecimal other)
		BigDecimal multiply (BigDecimal other)
		BigDecimal divide (BigDecimal other , RoundingMode mode)
	      /* 返冋这个大整数和另一个大整数 other 的 和、差、积、商以及余数, mode参数事舍入方式（四舍五入） */
    int compareTo (BigDecimal other)
	      /* 如果这个大整数与另一个大整数 other 相等，返回 0; 如果这个大整数小于另一个大整数other, 返回负数;  否则，返回正数。 */
      
    static  BigDecimal valueOf (long x)
    static  BigDecimal valueOf (long x，int scale)
	      /* 返回值等于 x 的大整数 , 或者  x/(10^scale) 的一个大实数 */
```





## 命名规则

- **类名**
  - 是以大写字母开头的名词。如果名字由多个单词组成， 每个单词的第一个字母都应该大写 (这种在一个单词中间使 用大写字母的方式称为驼峰命名法。 以其自身为例， 应该写成 CamelCase )。
- **文件名**
  - **源代码的文件名必须与公共类的名字相同， 并用 .java 作为扩展名**
- 



## 获取终止程序时返回的代码方法

```java
/* 调用 System.exit 方法	*/
System.exit(1);
```



## 逻辑右移和算数右移

```javaa
>>>  运算符会用 0 填充高位, 逻辑右移
>>   它会用符号位填充高位,	 算数右移
```



## 基础输入输出

```java
基础输出：
  	   System.out.println("字符串");    /* 输出一行, 会自动换行 */
  	   System.out.print("字符串");      /* 输出一行, 不会自动换行 */
  	   System.out.printf("%s","字符串");      /* 与 C++ 中相同 */
						  	   System.out.printf("%tc", new Date() );      /* 输出日期和时间 */

基础输入：
  需要先添加头文件。
   import java.util.Scanner;				// 或者 import java.util.*;
			Scanner in = new Scanner(System.in);		// 先构造一个  Scanner
			String name = in.nextLine();	  // in.nextLine() 是从终端读取一行数据，并写入到 name
			String str = in.nextIn();				// in.nextIn() 读取一个单词  （空格分割）
			int Num = in.nextInt();					// in.nextInt() 读取一个 整数
			double dNum = in.nextDouble();					// in.nextDouble() 读取一个 浮点数 double
			
标准输人流 System.in 从控制台进行输入，首先需要构造一个Scanner对象，并与 标准输人流”System.in关联。
			
  Console 是不显示输入内容的标准输入， 适合密码；  （但有问题）
  		Console cons = System.console();
			if (cons == null) { System.exit(-1) ; return; }
			String username = cons.readline("User name:");
			char[] passwd = cons.readPassword("Password:");
  
  
  
Scanner 相关API:    (这是个类)  java.util.Scanner
		Scanner (InputStream in)
      /* 用给定的输入流创建一个 Scanner 对象 */

		String nextLine ()
      /* 读取输入的下一行内容。 */
      
		String next ()
      /* 读取输入的下一个单词 (以空格作为分隔符) */

		int nextInt ()
		double nextDouble ()
      /* 读取并转换下一个表示整数或浮点数的字符序列。 */
      
		boolean hasNext ()
     /* 检测输入中是否还有其他单词。  */

		boolean hasNextInt ()
		boolean hasNextDouble ()
     /*  检测是否还有表示整数或浮点数的下一个字符序列  */
  

Console 相关API:    (这是个类)  java.lang.System   java.io.Console
  	static Console consloe ()
			/* 不显示输入内容的标准输入， 适合密码 , 需要判断该对象返回值是否为 null*/
		
  	static char[] readPassword (String promt, Object... args)
  	static String readLine (String prompt, Object... args)
			/*  显示字符串 prompt 并且读取用户输入， 直到输入行结束。 args 参数可以用来提供输入格式。 */
```





## 文件输入输出

```java
// 读取文件内容 直接输出
	String dir = System.getProperty("user.dir");			// 获得当前工作路径
	Files.lines(Paths.get(dir + "/a.txt"), StandardCharsets.UTF_8).forEach(System.out::println);

// 打开文件，并读取一行内容
	Scanner in = new Scanner(Paths.get(dir + "/a.txt"), "UTF-8");
  String FileP =  in.nextLine();


// 打开文件并写入内容, 覆盖式写入
 PrintWriter fOut = new PrintWriter(dir + "/a.txt", StandardCharsets.UTF_8);
 fOut.println("hello");
 fOut.flush();				// 更新到文件



// 程序启动时 重定向文件成 System.in 或 System.out  ，也不必担心处理 IOException 异常了。
$	java MyProg < r.txt > o.txt				// 启动java程序 MyProg ，输入文件 r.txt ，输出文件 o.txt
  
  
  

String dir = System.getProperty("user.dir");			// 获得当前工作路径
import java.util.Scanner;
// 文件操作API
	Scanner (File f)
    	/* 构造一个从给定文件读取数据的 Scanner */
    
	Scanner (String data)
    	/* 构造一个从给定字符串读取数据的 Scanner , 把字符串当成输入 */

import java.io.PrintWriter;
	PrintWriter (String fileName)
    	/* 构造一个将数据写入文件的 PrintWriter 文件名由参数指定。 */
  
    
import java.nio.file.Paths;    // 使用的类是 Paths
		static Path get (String pathname)
    	/* 根据给定的路径名构造一个 Path。 */

```





## 基础日期和时间类

```java
用来表示时间点的 Date 类。
用来表示日期的  LocalDate 类 , 需要使用静态工厂方法。
  
  Data: 
        Date dt = new Date();
        System.out.println( dt.toString()); 
						// 输出内容 Sun Nov 13 18:11:53 CST 2022
	
	LocalDate:
				System.out.println(LocalDate.now() );		// 输出  2022-11-13
				System.out.println(LocalDate.now().getYear());  // 输出当前的年份  2022
				

LocalDate 相关API:    (这是个类)  import java.time.LocalDate;
		static LocalDate new ()
      /* 构造一个表示当前日期的对象 */
      
		static LocalDate of (int year, int month, int day)
      /* 构造一个给定日期的对象 */
      
		int getYear ()
		int getMonthValue ()
		int getDayOfMonth ()
      /* 获得当前日期的年、月和日 */

		DayOfWeek getDayOfWeek ()
      /* 获得当前日期是星期几。返回 DayOfWeek类的一个实例,该实例的 getValue() 方法可以返回星期的数字.*/
      
		LocalDate plusDays ( int n )
		LocalDate minusDays ( int n )
      /* 生成当前日期之后或之前 n 天的日期 */
```



## 随机数生成器

```java
Random 相关API:    (这是个类)  import java.util.Random;
  	Random ()
    	/* 构造一个新的随机数生成器 */
      
		int nextInt (int n )
      /* 返回一个 0 ~ n-1 之间的随机数 */

```

