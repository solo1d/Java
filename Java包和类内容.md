## 目录

- [jar包内容](#jar包内容)
- [类的导入](#类的导入)
- [将类放入包中](#将类放入包中)
- [设置类路径](#设置类路径)
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

