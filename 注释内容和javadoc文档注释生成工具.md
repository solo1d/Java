## 目录

- [javadoc文档注释生成工具](#javadoc文档注释生成工具)
  - [注释的插入和类注释](#注释的插入和类注释)
  - [类中方法注释](#类中方法注释)
  - [通用注释](#通用注释)



## javadoc文档注释生成工具

JDK包含这个 `javadoc` 工具。 它可以由源文件生成一个 HTML 文档。



### 注释的插入和类注释

```java
注释应该放置在所描述特性的前面。
类注释必须放在 import 语句之后， 类定义之前。
  注释以 /** 开始， 并 以 */ 结束。
  
每个 /** ... */ 文档注释在标记之后紧跟着自由格式文本。 标记由 @开始， 如 @author 或 @param。

还可以添加 HTML 修饰符，  
	 <em>...</em>    				 用于强调
	 <strong>...</strong>    用于着重强调
   <img>								   包含图片 (将图片或其他的文件资源放到子目录 doc-files 中)
   {@code ...}					   键入等宽代码

有的标签是 不可以使用的 <h1> 或<hr> 之类的，会和文档起冲突。  
  
  标签范例：
	<img src="doc-files/uml.png"  alt="UML doagra">
  

  类注释范例：
  
  /**
  *	A @{code Card} object represennts a playing card, such
  * 这里面写的是类说明，只是描述这个类的作用的
	*	<em>用于强调</em>
	*	<strong>用于着重强调</strong>
	*	<img src="doc-files/uml.png"  alt="UML doagra">			
  */
  public class Card{
    ..... 
  }
```



### 类中方法注释

```java
每一个方法注释必须放在所描述的方法之前。 除了通用标记之外， 还可以使用下面的标记:

@param  变量描述
  				这个标记将对当前方法的“ param” (参数)部分添加一个条目。这个描述可以占据多行， 并可以使用 HTML 标记。一个方法的所有 @param 标记必须放在一起。
   /** @param  变量  描述内容 */
  
@return 描述
		 	  这个标记将对当前方法添加“ return” (返回)部分。这个描述可以跨越多行， 并可以使用 HTML 标记。
   /** @return  返回值描述内容 */
  
@throws 异常描述
  	这个标记将添加一个注释， 用于表示这个方法有可能抛出异常.
   /** @throws  会抛出的异常  异常描述内容 */
  
  
// 范例：
public class A
{
	/**
	 *	@param argv  说明 argv
	 *  @return      返回值说明
	 * 	@throws  IOException   异常说明
	 */
	public static int main(String[] argv) throws IOException
	{
		Files.getFileStore(Path.of("/"));
		System.out.println("hello");
		return 0;
	}
}
```





### 通用注释



- **类文档的注释**
  - @author 姓名
    - 这个标记将产生一个  author ( 作者)条目。可以使用多个 @author 标记， 每个 @author 标记对应一个作者
  - @version  文本
    - 这个标记将产生一个“ version ”(版本)条目。 这里的文本可以是对当前版本的任何描述。
- **所有的文档都可以注释**
  - @since 文本
    - 这个标记将产生一个“ since” (始于)条目。这里的 text 可以是对引人特性的版本描 述 例如 ©since version 1.7.10
  - @deprecated 文本
    - 这个标记将对类、方法或变量添加一个不再使用的注释。 文本中给出了取代的建议。 例如：
      - `@deprecated   方法 <code>方法名称</code> 不再使用的说明`
      - 通过 @see 和 @link 标记， 可以使用超级链接， 链接到 javadoc 文档的相关部分或外部文档。
  - @see  引用
    - 这个标记将在  `see also` 部分增加一个超级链接。它可以用于类中，也可以用于方法中。 这里的引用可以选择下列情形之一:
      - `@see com.horstraann.corejava.Employee#raiseSalary(double)`
      - `@see <a href="m«w.horstmann.com/corejava.htinl">The Core ]ava home page</a>`
  - @link  引用
    - 注释中的任何位置放置指向其他类或方法的超级链接， 以及插入一个专用的标记，和 @see 一样
      - `{@link package.class#方法}`

