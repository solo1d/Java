### 目录

- [标准日志库](#标准日志库)
  - [使用JDK_Logging日志](#使用JDK_Logging日志)
- [第三方日志库](#第三方日志库)
  - [Commons_Logging第三方日志库](#Commons_Logging第三方日志库)





## 标准日志库

由 Java 原生提供支持

#### 使用JDK_Logging日志

Java标准库内置了日志包`java.util.logging`。

日志的输出可以设定级别。JDK的Logging定义了7个日志级别，从严重到普通：

- SEVERE
- WARNING
- INFO
- CONFIG
- FINE   （默认不打印，以下的级别也不打印）
- FINER
- FINEST

因为默认级别是INFO，因此，INFO级别以下的日志，不会被打印出来。使用日志级别的好处在于，调整级别，就可以屏蔽掉很多调试相关的日志输出。

Logging系统在JVM启动时读取配置文件并完成初始化，一旦开始运行`main()`方法，就无法修改配置；

**配置不太方便，需要在JVM启动时传递参数`-Djava.util.logging.config.file=<config-file-name>`。**

因此，Java标准库内置的Logging使用并不是非常广泛。



```java
package logging;

import java.util.logging.Logger;

public class loggingTest {
    public static void main(String[] args) {
        Logger logger = Logger.getGlobal();
        logger.info("start Process...");
        /* 输出内容如下：
        12月 26, 2022 10:09:17 上午 logging.loggingTest main
        信息: start Process...
         */
      
        logger.warning("memory is running out...");
        /* 输出内容如下：
        12月 26, 2022 10:09:17 上午 logging.loggingTest main
        警告: memory is running out...
         */
      
        logger.fine("ignored");
        /*  默认不打印该等级的日志  */
      
        logger.severe("process will be terminated...");
        /* 输出内容如下：
        12月 26, 2022 10:09:17 上午 logging.loggingTest main
        严重: process will be terminated...
         */
    }
}
```



## 第三方日志库

和Java标准库提供的日志不同，Commons Logging是一个第三方日志库，它是由Apache创建的日志模块。

[点击查看导入第三方库到IDE项目的方法](./导入第三方库到IDE方法.md)



### Commons_Logging第三方日志库

Commons Logging的特色是，它可以挂接不同的日志系统，并通过配置文件指定挂接的日志系统。默认情况下，Commons Loggin自动搜索并使用Log4j（Log4j是另一个流行的日志系统），如果没有找到Log4j，再使用JDK Logging。

 

- Commons Logging定义了6个日志级别：
  - FATAL
  - ERROR
  - WARNING
  - INFO
  - DEBUG
  - TRACE





- 使用Commons Logging只需要和两个类打交道，并且只有两步：

  - 第一步，通过`LogFactory`获取`Log`类的实例； 第二步，使用`Log`实例的方法打日志。

    - ```java
      import org.apache.commons.logging.Log;
      import org.apache.commons.logging.LogFactory;
      public class Main {
          public static void main(String[] args) {
              Log log = LogFactory.getLog(Main.class);
              log.info("start...");
              log.warn("end.");
          }
      }
      
      // 输入如下
      12月 26, 2022 3:06:49 下午 logging.CommonsTest main
      信息: start...
      12月 26, 2022 3:06:50 下午 logging.CommonsTest main
      警告: end.
      ```

    - 

```java
package logging;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;

public class CommonsTest {
    // 这种方式有个非常大的好处，就是子类可以直接使用该log实例
    protected final  Log  log = LogFactory.getLog(getClass());

    public static void main(String[] args) {
//        Log log = LogFactory.getLog(CommonsTest.class);
        CommonsTest commonsTest = new Lct();
        commonsTest.printLog("Lct...");
        /* 输出信息
            12月 26, 2022 5:01:38 下午 logging.Lct printLog
            信息: Lct...
         */

        CommonsTest commp  = new CommonsTest();
        commp.printLog("commp...");
        /* 输出信息
            12月 26, 2022 5:01:38 下午 logging.CommonsTest printLog
            信息: commp...
         */

        Lct lct = new Lct();
        lct.printLog("lct...");
        /* 输出信息
            12月 26, 2022 5:01:38 下午 logging.Lct printLog
            信息: lct...
         */
    }

    public void printLog(String LogString)
    {
        log.info(LogString);
    }
}

class  Lct extends CommonsTest {
    void childPrLog(String  LogString){
        log.info(LogString);
    }
}
```

