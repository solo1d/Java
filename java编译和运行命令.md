- [编译指定目录下的所有文件](#编译指定目录下的所有文件)
- [指定编译的版本和兼容JDK版本](#指定编译的版本和兼容JDK版本)
- [运行时指定classpath路径](#运行时指定classpath路径)
- [直接运行jar包的内容](#直接运行jar包的内容)
- [编译和运行项目中携带第三方库时的命令](#编译和运行项目中携带第三方库时的命令)



### 编译指定目录下的所有文件

```bash
假设创建了如下的目录结构：
work
├── bin
└── src
    └── com
        └── itranswarp
            ├── sample
            │   └── Main.java
            └── world
                └── Person.java

# 所在位置是 work 目录
# 编译src目录下的所有Java文件：
$ javac -d ./bin src/**/*.java
		# 命令行-d指定输出的class文件存放bin目录，后面的参数src/**/*.java表示src目录下的所有.java文件，包括任意深度的子目录。
```





### 指定编译的版本和兼容JDK版本

```bash
$ java -version     # 查看当前的 JDK版本（就是 java.exe 这个程序的版本）。


第一种方法：
#指定编译输出有两种方式，一种是在javac命令行中用参数--release设置：
$ javac --release 11  Main.java   
		#参数 --release 11表示源码兼容Java 11，编译的class输出版本为Java 11兼容，即class版本55。
		
第二种方式 
# 是用参数--source指定源码版本，用参数--target指定输出class版本：
$ javac --source 9 --target 11 Main.java	
		#  上述命令如果使用Java 17的JDK编译，它会把源码视为Java 9兼容版本，并输出class为Java 11兼容版本。注意--release参数和--source --target参数只能二选一，不能同时设置。
```



### 运行时指定classpath路径

```bash
classpath是JVM用到的一个环境变量，它用来指示JVM如何搜索class。
classpath就是一组目录的集合，它设置的搜索路径与操作系统相关

# 参数 -classpath  可以简写为 -cp
$ java -classpath  /java_obj/T1  t1   
				# 路径为 /java_obj/T1  , 执行 t1.class 文件

$ java -cp  /java_obj/T1   
			 # 与上面相同， 只不过换成了简写

$ java -cp ./hello.jar abc.xyz.Hello
			 # 执行  hello.jar 文件下内的,  abc/xyz/Hello.class 文件

# 如果 .jar 包内有 /META-INF/MANIFEST.MF  文件的话， 就可以直接启动，而不需要再末尾指定启动类了。
# jar包还可以包含其它jar包，这个时候，就需要在MANIFEST.MF文件里配置classpath了。
$ java -jar hello.jar 

# 指定多个 jar ，互为依赖 , 后面是启动的类
$ java -jar app.jar:a.jar:b.jar:c.jar   com.liaoxuefeng.sample.Main
```



### 直接运行jar包的内容

```bash
jar只是用于存放class的容器，它并不关心class之间的依赖。


$ java -cp ./hello.jar abc.xyz.Hello
			 # 执行  hello.jar 文件下内的,  abc/xyz/Hello.class 文件
			 


# 如果 .jar 包内有 /META-INF/MANIFEST.MF  文件的话， 就可以直接启动，而不需要再末尾指定启动类了。
# jar包还可以包含其它jar包，这个时候，就需要在MANIFEST.MF文件里配置classpath了。
$ java -jar ./hello.jar

# 指定多个 jar ，互为依赖 , 后面是启动的类
$ java -jar app.jar:a.jar:b.jar:c.jar   com.liaoxuefeng.sample.Main
```



## 编译和运行项目中携带第三方库时的命令

```bash
编译前目录结构如下：
T2
└── src
    └── logging
        ├── CommonsTest.java
        ├── commons-logging-1.2-javadoc.jar
        ├── commons-logging-1.2.jar
        └── loggingTest.java


# 编译命令
cd T2			# 工作目录切换为T2
javac -d T2/bin -cp  T2/src/logging/commons-logging-1.2.jar  T2/src/**/*.java 
 

# 运行命令
cd T2			# 工作目录切换为T2
java  -Dfile.encoding=UTF-8 -Dsun.stdout.encoding=UTF-8 -Dsun.stderr.encoding=UTF-8 -classpath "T2/bin":"T2/src/logging/commons-logging-1.2.jar" logging.CommonsTest


#  -classpath      <目录和 zip/jar 文件的类搜索路径>


编译后目录结构如下：
T2
├── bin
│   └── logging
│       ├── CommonsTest.class
│       └── loggingTest.class
└── src
    └── logging
        ├── CommonsTest.java
        ├── commons-logging-1.2-javadoc.jar
        ├── commons-logging-1.2.jar
        └── loggingTest.java
```





