## 目录

- [读取classpath资源](#读取classpath资源)
- [小结](#小结)





## 读取classpath资源

很多Java程序启动的时候，都需要读取配置文件。例如，从一个`.properties`文件中读取配置：

```java
String conf = "C:\\conf\\default.properties";
try (InputStream input = new FileInputStream(conf)) {
    // TODO:
}
```

这段代码要正常执行，必须在C盘创建`conf`目录，然后在目录里创建`default.properties`文件。但是，在Linux系统上，路径和Windows的又不一样。

因此，从磁盘的固定目录读取配置文件，不是一个好的办法。



有没有路径无关的读取文件的方式呢？

我们知道，Java存放`.class`的目录或jar包也可以包含任意其他类型的文件，例如：

- 配置文件，例如`.properties`；
- 图片文件，例如`.jpg`；
- 文本文件，例如`.txt`，`.csv`；
- ……

从classpath读取文件就可以避免不同环境下文件路径不一致的问题：**如果我们把`default.properties`文件放到classpath中，就不用关心它的实际存放路径。**

在classpath中的资源文件，路径总是以`／`开头，我们先获取当前的`Class`对象，然后调用`getResourceAsStream()`就可以直接从classpath读取任意的资源文件：

```java
try (InputStream input = getClass().getResourceAsStream("/default.properties")) {
    if (input != null) {
        // TODO:
    }
}
```

调用`getResourceAsStream()`需要特别注意的一点是，如果资源文件不存在，它将返回`null`。因此，我们需要检查返回的`InputStream`是否为`null`，如果为`null`，表示资源文件在classpath中没有找到：

```java
package myReadClasspath;

import java.io.IOException;
import java.io.InputStream;
import java.util.BitSet;

public class myReadClasspath {
    public static void main(String[] args) throws IOException {
        myReadClasspathChile ccp = new myReadClasspathChile();
        BitSet  bitset = new BitSet();
        ccp.printFile(bitset);
    }
}

class myReadClasspathChile
{
    public void printFile( BitSet bitset ) throws IOException {
        try (InputStream input = bitset.getClass().getResourceAsStream("/default.properties"))
        {
            if(input != null){
                int n ;

                while ( (n = input.read()) != -1)
                {
                    System.out.print((char)(n));
                }
            }
        }
    }
}
```

如果我们把默认的配置放到jar包中，再从外部文件系统读取一个可选的配置文件，就可以做到既有默认的配置文件，又可以让用户自己修改配置：

```java
Properties props = new Properties();
props.load(inputStreamFromClassPath("/default.properties"));
props.load(inputStreamFromFile("./conf.properties"));
```

这样读取配置文件，应用程序启动就更加灵活。

```java
package myReadClasspath;

import java.io.IOException;
import java.io.InputStream;


/**
 * Learn Java from https://www.liaoxuefeng.com/
 *
 * @author liaoxuefeng
 *
 *         getResourceAsStream("/default.properties") 中 "/default.properties"
 *         判断规则，它是固定格式，与.class文件中配置的路径不一样 以 / 开头快速，在 classpath中，查找到该文件
 *         String baseName = c.getPackageName();
 *         if (baseName != null &&
 *         !baseName.isEmpty()) { name = baseName.replace('.', '/') + "/" +
 *         name; }
 */
public class myReadClasspath {

    public static void main(String[] args) throws IOException {
        // 当报main错误的时候，要注意 Main2.class 的引用，是否和你当前的 类名是否一致
        // .classpath 文件： https://blog.csdn.net/pengmm1990/article/details/68951389
        // 即path=”src”表示文件夹src与.classpath在同一个目录, src表示 而不是/src它是以自己文件的位置作为起点，且代表源文件。
        // <classpathentry kind="src" path="src"/>  kind="src" 代表类型，数据来源， path="src" 代表该项目下的路径，src所在位置就是项目的根目录
//		<classpathentry kind="output" path="bin"/> "output" 编译完输出到指定的目录， .class执行文件

        try (InputStream input = myReadClasspath.class.getResourceAsStream("default.properties")) {
            System.out.print(input == null ? "not find" : input.toString());
        }
    }
}
```



## 小结

把资源存储在classpath中可以避免文件路径依赖；

`Class`对象的`getResourceAsStream()`可以从classpath中读取指定资源；

根据classpath读取资源时，需要检查返回的`InputStream`是否为`null`。

