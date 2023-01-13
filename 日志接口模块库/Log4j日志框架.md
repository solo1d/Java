前面介绍了Commons Logging，可以作为“日志接口”来使用。而真正的“日志实现”可以使用Log4j。

Log4j是一种非常流行的日志框架，最新版本是2.x。

Log4j是一个组件化设计的日志系统，它的架构大致如下：

```ascii
log.info("User signed in.");
 │
 │   ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
 ├──>│ Appender │───>│  Filter  │───>│  Layout  │───>│ Console  │
 │   └──────────┘    └──────────┘    └──────────┘    └──────────┘
 │
 │   ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
 ├──>│ Appender │───>│  Filter  │───>│  Layout  │───>│   File   │
 │   └──────────┘    └──────────┘    └──────────┘    └──────────┘
 │
 │   ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
 └──>│ Appender │───>│  Filter  │───>│  Layout  │───>│  Socket  │
     └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

当我们使用Log4j输出一条日志时，Log4j自动通过不同的Appender把同一条日志输出到不同的目的地。例如：

- console：输出到屏幕；
- file：输出到文件；
- socket：通过网络输出到远程计算机；
- jdbc：输出到数据库

在输出日志的过程中，通过Filter来过滤哪些log需要被输出，哪些log不需要被输出。例如，仅输出`ERROR`级别的日志。

最后，通过Layout来格式化日志信息，例如，自动添加日期、时间、方法名称等信息。

上述结构虽然复杂，但我们在实际使用的时候，并不需要关心Log4j的API，而是通过配置文件来配置它。

以XML配置为例，使用Log4j的时候，我们把一个`log4j2.xml`的文件放到`classpath`下就可以让Log4j读取配置文件并按照我们的配置来输出日志。下面是一个配置文件的例子：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
	<Properties>
        <!-- 定义日志格式 -->
		<Property name="log.pattern">%d{MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36}%n%msg%n%n</Property>
        <!-- 定义文件名变量 -->
		<Property name="file.err.filename">log/err.log</Property>
		<Property name="file.err.pattern">log/err.%i.log.gz</Property>
	</Properties>
    <!-- 定义Appender，即目的地 -->
	<Appenders>
        <!-- 定义输出到屏幕 -->
		<Console name="console" target="SYSTEM_OUT">
            <!-- 日志格式引用上面定义的log.pattern -->
			<PatternLayout pattern="${log.pattern}" />
		</Console>
        <!-- 定义输出到文件,文件名引用上面定义的file.err.filename -->
		<RollingFile name="err" bufferedIO="true" fileName="${file.err.filename}" filePattern="${file.err.pattern}">
			<PatternLayout pattern="${log.pattern}" />
			<Policies>
                <!-- 根据文件大小自动切割日志 -->
				<SizeBasedTriggeringPolicy size="1 MB" />
			</Policies>
            <!-- 保留最近10份 -->
			<DefaultRolloverStrategy max="10" />
		</RollingFile>
	</Appenders>
	<Loggers>
		<Root level="info">
            <!-- 对info级别的日志，输出到console -->
			<AppenderRef ref="console" level="info" />
            <!-- 对error级别的日志，输出到err，即上面定义的RollingFile -->
			<AppenderRef ref="err" level="error" />
		</Root>
	</Loggers>
</Configuration>
```

虽然配置Log4j比较繁琐，但一旦配置完成，使用起来就非常方便。对上面的配置文件，凡是`INFO`级别的日志，会自动输出到屏幕，而`ERROR`级别的日志，不但会输出到屏幕，还会同时输出到文件。并且，一旦日志文件达到指定大小（1MB），Log4j就会自动切割新的日志文件，并最多保留10份。

有了配置文件还不够，因为Log4j也是一个第三方库，我们需要从[这里](https://logging.apache.org/log4j/2.x/download.html)下载Log4j，解压后，把以下3个jar包放到`classpath`中：

- log4j-api-2.x.jar
- log4j-core-2.x.jar
- log4j-jcl-2.x.jar

因为Commons Logging会自动发现并使用Log4j，所以，把上一节下载的`commons-logging-1.2.jar`也放到`classpath`中。

要打印日志，只需要按Commons Logging的写法写，不需要改动任何代码，就可以得到Log4j的日志输出，类似：

```
03-03 12:09:45.880 [main] INFO  com.itranswarp.learnjava.Main
Start process...
```

### 最佳实践

在开发阶段，始终使用Commons Logging接口来写入日志，并且开发阶段无需引入Log4j。如果需要把日志写入文件， 只需要把正确的配置文件和Log4j相关的jar包放入`classpath`，就可以自动把日志切换成使用Log4j写入，无需修改任何代码。



## 练习1

将要执行的.java文件和Commons Logging（1个jar）、Log4j（1个xml+3个jar）文件放在同一个文件夹下，命令行运行以下代码：

```
javac -cp commons-logging-1.2.jar Main.java
java -cp .;commons-logging-1.2.jar;log4j-api-2.18.0.jar;log4j-core-2.18.0.jar;log4j-jcl-2.18.0.jar Main
```

输出结果如下：

```
07-08 14:44:01.908 [main] INFO Main
Start process...
07-08 14:44:01.916 [main] ERROR Main
Invalid encoding.
java.io.UnsupportedEncodingException: invalidCharsetName
```

​    `at java.lang.String.lookupCharset(String.java:819) ~[?:?]`

​    `at java.lang.String.getBytes(String.java:1758) ~[?:?]`

​    `at Main.main(Main.java:11) ~[work/:?]`

```
07-08 14:44:01.928 [main] INFO Main
Process end.
```





## 练习2

1.添加jar和log4j2.xml添加到classpath中（idea添加jar和配置文件到classpath方法可直接搜索到）

2.运行程序

3.观察结果

生成了log/all.log和log/err.log两个文件

all.log

```
08-10 09:57:48.927 [main] INFO  com.ccl.LogDemo
Start process...

08-10 09:57:48.933 [main] ERROR com.ccl.LogDemo
Invalid encoding.

java.io.UnsupportedEncodingException: invalidCharsetName
	at java.lang.StringCoding.encode(StringCoding.java:427) ~[?:?]
	at java.lang.String.getBytes(String.java:941) ~[?:?]
	at com.ccl.LogDemo.main(LogDemo.java:16) [java_base/:?]
08-10 09:57:48.943 [main] INFO  com.ccl.LogDemo
Process end.
```

err.log

```
08-10 09:57:48.933 [main] ERROR com.ccl.LogDemo
Invalid encoding.

java.io.UnsupportedEncodingException: invalidCharsetName
	at java.lang.StringCoding.encode(StringCoding.java:427) ~[?:?]
	at java.lang.String.getBytes(String.java:941) ~[?:?]
	at com.ccl.LogDemo.main(LogDemo.java:16) [java_base/:?]
```
