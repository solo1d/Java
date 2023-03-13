## 目录

- [ZonedDateTime](#ZonedDateTime)
- [时区转换](#时区转换)
- [练习](#练习)
- [小结](#小结)



## ZonedDateTime

`LocalDateTime`总是表示本地日期和时间，要表示一个带时区的日期和时间，我们就需要`ZonedDateTime`。

可以简单地把`ZonedDateTime`理解成`LocalDateTime`加`ZoneId`。`ZoneId`是`java.time`引入的新的时区类，注意和旧的`java.util.TimeZone`区别。

要创建一个`ZonedDateTime`对象，有以下几种方法，一种是通过`now()`方法返回当前时间：

```java
package myZonedDateTime;

import java.time.ZoneId;
import java.time.ZonedDateTime;

public class myZonedDateTime {
    public static void main(String[] args) {
        ZonedDateTime zbj = ZonedDateTime.now(); // 默认时区
        ZonedDateTime zny = ZonedDateTime.now(ZoneId.of("America/New_York"));// 用指定时区获取当前时间
      
      // 观察打印的两个ZonedDateTime，发现它们时区不同，但表示的时间都是同一时刻（毫秒数不同是执行语句时的时间差）：
        System.out.println(zbj); // 2023-03-10T13:52:45.662782+08:00[Asia/Shanghai]
        System.out.println(zny); // 2023-03-10T00:52:45.663613-05:00[America/New_York]
    }
}
```

另一种方式是通过给一个`LocalDateTime`附加一个`ZoneId`，就可以变成`ZonedDateTime`：

```java
package myZonedDateTime;

import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;

public class myZonedDateTime {
    public static void main(String[] args) {
        LocalDateTime ldt = LocalDateTime.of(2019,9,15,15,16,17);
        ZonedDateTime zbj1 = ldt.atZone(ZoneId.systemDefault());
        ZonedDateTime zny1 = ldt.atZone(ZoneId.of("America/New_York"));
        System.out.println(zbj1); // 2019-09-15T15:16:17+08:00[Asia/Shanghai]
        System.out.println(zny1);//  2019-09-15T15:16:17-04:00[America/New_York]
// 以这种方式创建的ZonedDateTime，它的日期和时间与LocalDateTime相同，但附加的时区不同，因此是两个不同的时刻
    }
}

```



## 时区转换

要转换时区，首先我们需要有一个`ZonedDateTime`对象，然后，通过`withZoneSameInstant()`将关联时区转换到另一个时区，转换后日期和时间都会相应调整。

下面的代码演示了如何将北京时间转换为纽约时间：

```java
package myZonedDateTime;

import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;

public class myZonedDateTime {
    public static void main(String[] args) {
        // 以中国时区获取当前时间:
        ZonedDateTime zbj2 = ZonedDateTime.now(ZoneId.of("Asia/Shanghai"));
        // 转换为纽约时间:
        ZonedDateTime zny2 = zbj2.withZoneSameInstant(ZoneId.of("America/New_York"));
        System.out.println(zbj2); // 2023-03-10T14:04:39.234498+08:00[Asia/Shanghai]
        System.out.println(zny2); // 2023-03-10T01:04:39.234498-05:00[America/New_York]
    }
}
```

要特别注意，时区转换的时候，由于夏令时的存在，不同的日期转换的结果很可能是不同的。这是北京时间9月15日的转换结果：

```
2019-09-15T21:05:50.187697+08:00[Asia/Shanghai]
2019-09-15T09:05:50.187697-04:00[America/New_York]
```

这是北京时间11月15日的转换结果：

```
2019-11-15T21:05:50.187697+08:00[Asia/Shanghai]
2019-11-15T08:05:50.187697-05:00[America/New_York]
```

两次转换后的纽约时间有1小时的夏令时时差。

> 涉及到时区时，千万不要自己计算时差，否则难以正确处理夏令时。

有了`ZonedDateTime`，将其转换为本地时间就非常简单：

```
ZonedDateTime zdt = ...
LocalDateTime ldt = zdt.toLocalDateTime();
```

转换为`LocalDateTime`时，直接丢弃了时区信息。

## 练习

某航线从北京飞到纽约需要13小时20分钟，请根据北京起飞日期和时间计算到达纽约的当地日期和时间。

提示：`ZonedDateTime`仍然提供了`plusDays()`等加减操作。

```java
package myZonedDateTime;

import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;

public class myZonedDateTime {
    public static void main(String[] args) {
        LocalDateTime departureAtBeijing = LocalDateTime.of(2019, 9, 15, 13, 0, 0);
        int hours = 13;
        int minutes = 20;
        LocalDateTime arrivalAtNewYork = calculateArrivalAtNY(departureAtBeijing, hours, minutes);
        System.out.println(departureAtBeijing + " -> " + arrivalAtNewYork);
        // test:
        if (!LocalDateTime.of(2019, 10, 15, 14, 20, 0)
                .equals(calculateArrivalAtNY(LocalDateTime.of(2019, 10, 15, 13, 0, 0), 13, 20))) {
            System.err.println("测试失败!");
        } else if (!LocalDateTime.of(2019, 11, 15, 13, 20, 0)
                .equals(calculateArrivalAtNY(LocalDateTime.of(2019, 11, 15, 13, 0, 0), 13, 20))) {
            System.err.println("测试失败!");
        }
    }
    static LocalDateTime calculateArrivalAtNY(LocalDateTime bj, int h, int m) {
        ZonedDateTime zbj = bj.atZone(ZoneId.of("Asia/Shanghai"));
        ZonedDateTime zdt = zbj.plusHours(h).plusMinutes(m);
        ZonedDateTime zny = zdt.withZoneSameInstant(ZoneId.of("America/New_York"));
        System.out.println( " calculateArrivalAtNY : " + zbj);
        bj = zny.toLocalDateTime();
        return bj;
    }
}
```

## 小结

`ZonedDateTime`是带时区的日期和时间，可用于时区转换；

`ZonedDateTime`和`LocalDateTime`可以相互转换。
