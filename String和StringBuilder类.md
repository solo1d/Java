### 目录

- [String字符串API列表](#String字符串API列表)
- [构建String字符串的StringBuilder类](#构建String字符串的StringBuilder类)
- 





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

为了能高效拼接字符串，Java标准库提供了`StringBuilder`，它是一个可变对象，可以预分配缓冲区，这样，往`StringBuilder`中新增字符时，不会创建新的临时对象：



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

```java
测试代码
public class StringBuilderTest {
    public static void main(String[] args) {
        String[] fields = { "name", "position", "salary" };
        String table = "employee";
        String insert = buildInsertSql(table, fields);
        System.out.println(insert);
        String s = "INSERT INTO employee (name, position, salary) VALUES (?, ?, ?)";
        System.out.println(s);
        System.out.println(s.equals(insert) ? "测试成功" : "测试失败");
    }

    private static String buildInsertSql(String table, String[] fields) {
        StringBuilder sb = new StringBuilder();
        sb.append("INSERT INTO ")
                .append(table)
                .append(" (");
        for (String st : fields)
        {
            sb.append(st)
              .append(", ");
        }
        sb.delete( sb.length() - ", ".length(), sb.length());
        sb.append(") VALUES (")
                .append("?, ?, ?")
                .append(")");
        return  sb.toString();
    }
}
```

```java
测试代码2
  public class StringJoinerTest {
    public static void main(String[] args) {
        String[] fields = { "name", "position", "salary" };
        String table = "employee";
        String select = buildSelectSql(table, fields);
        System.out.println(select);
        System.out.println("SELECT name, position, salary FROM employee".equals(select) ? "测试成功" : "测试失败");
    }

    static String buildSelectSql(String table, String[] fields) {
        String ret = "SELECT ";
        ret += String.join(", ",fields);
        ret += " FROM " + table;
        return ret;
    }
}
```



