## 目录

- [编写和编译模块](#编写和编译模块)
- [生成模块流程](#生成模块流程)
- [使用模块打包JRE](#使用模块打包JRE)
- 





## 编写和编译模块

从Java 9开始引入的模块，主要是为了解决“依赖”这个问题。如果`a.jar`必须依赖另一个`b.jar`才能运行，那我们应该给`a.jar`加点说明啥的，让程序在编译和运行的时候能自动定位到`b.jar`，这种自带“依赖关系”的class容器就是模块。

为了表明Java模块化的决心，从Java 9开始，原有的Java标准库已经由一个单一巨大的`rt.jar`分拆成了几十个模块，这些模块以`.jmod`扩展名标识，可以在`$JAVA_HOME/jmods`目录下找到它们：



这些`.jmod`文件每一个都是一个模块，模块名就是文件名。例如：模块`java.base`对应的文件就是`java.base.jmod`。模块之间的依赖关系已经被写入到模块内的`module-info.class`文件了。所有的模块都直接或间接地依赖`java.base`模块，只有`java.base`模块不依赖任何模块，它可以被看作是“根模块”，好比所有的类都是从`Object`直接或间接继承而来。



**把一堆class封装为jar仅仅是一个打包的过程，而把一堆class封装为模块则不但需要打包，还需要写入依赖关系，并且还可以包含二进制代码（通常是JNI扩展）。此外，模块支持多版本，即在同一个模块中可以为不同的JVM提供不同的版本。**





### 生成模块流程

```java
提供两个java文件 内容如下：

  
// Greeting.java 文件内容
package com.itranswarp.sample;

public class Greeting {
		public String hello ()
		{
			return "Greeting";
		}
}


// Main.java 文件内容
package com.itranswarp.sample;

// 必须引入java.xml模块后才能使用其中的类:
import javax.xml.XMLConstants;

public class Main {
	public static void main(String[] args) {
		Greeting g = new Greeting();
		System.out.println(g.hello());
	}
}
```





```java
1、 创建模块和原有的创建Java项目是完全一样的，以`oop-module`工程为例，它的目录结构如下：
oop-module
├── bin
├── build.sh
└── src
    ├── com
    │   └── itranswarp
    │       └── sample
    │           ├── Greeting.java
    │           └── Main.java
    └── module-info.java

// 其中，bin目录存放编译后的class文件，src目录存放源码，按包名的目录结构存放，仅仅在src目录下多了一个module-info.java这个文件，这就是模块的描述文件。在这个模块中，它长这样：
  
module hello.world {
	requires java.base; // 可不写，任何模块都会自动引入java.base
	requires java.xml;

  exports com.itranswarp.sample;  // 声明 导出模块中的 com.itranswarp.sample.Greeting类 ，让外界可以访问
}

其中，module是关键字，后面的hello.world是模块的名称，它的命名规范与包一致。
花括号的requires xxx;表示这个模块需要引用的其他模块名。除了java.base可以被自动引入外，这里我们引入了一个java.xml的模块。
```

```bash
# 2、把工作目录切换到 oop-module，在当前目录下编译所有的.java文件，并存放到bin目录下，命令如下：
$ javac -d bin src/module-info.java src/com/itranswarp/sample/*.java


如果编译成功，现在项目结构如下：
oop-module   
├── bin
│   ├── com
│   │   └── itranswarp
│   │       └── sample
│   │           ├── Greeting.class
│   │           └── Main.class
│   └── module-info.class
├── build.sh
└── src
    ├── com
    │   └── itranswarp
    │       └── sample
    │           ├── Greeting.java
    │           └── Main.java
    └── module-info.java
```

```bash
# 3、把bin目录下的所有class文件先打包成jar，在打包的时候，注意传入--main-class参数，让这个jar包能自己定位main方法所在的类：

$ jar --create --file hello.jar --main-class com.itranswarp.sample.Main -C bin .

# 就在当前目录下得到了hello.jar这个jar包，它和普通jar包并无区别，可以直接使用命令java -jar hello.jar来运行它
```

```bash
# 4、继续使用JDK自带的jmod命令把一个jar包转换成模块
$ jmod create --class-path hello.jar hello.jmod

# 在当前目录下我们又得到了hello.jmod这个模块文件，这就是最后打包出来的模块
```



### 使用模块打包JRE

创建的`hello.jmod` 可以用它来打包JRE

```bash
#“复制”一份JRE，但只带上用到的模块。为此，JDK提供了`jlink`命令来干这件事。命令如下：
$ jlink --module-path hello.jmod --add-modules java.base,java.xml,hello.world --output jre/

#在--module-path参数指定了我们自己的模块hello.jmod，
#然后，在--add-modules参数中指定了用到的3个模块java.base、java.xml和hello.world，用,分隔。
#最后，在--output参数指定输出目录。
```

```bash
现在，在当前目录下，我们可以找到jre目录，这是一个完整的并且带有我们自己hello.jmod模块的JRE。可以直接运行这个JRE：
$ jre/bin/java --module hello.world

#输出如下：
Greeting
```





