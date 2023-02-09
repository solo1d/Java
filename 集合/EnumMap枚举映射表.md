## 目录

- [使用EnumMap](#使用EnumMap)
- [小结](#小结)



## 使用EnumMap

如果作为key的对象是`enum`类型，那么，还可以使用Java集合库提供的一种`EnumMap`，它在内部以一个非常紧凑的数组存储value，并且根据`enum`类型的key直接定位到内部数组的索引，并不需要计算`hashCode()`，不但效率最高，而且没有额外的空间浪费。

```java
package myMap;

import java.time.DayOfWeek;
import java.util.EnumMap;
import java.util.Map;

public class myEnumMap {
    public static void main(String[] args) {
            Map<DayOfWeek, String> map = new EnumMap<DayOfWeek, String>(DayOfWeek.class);
            map.put(DayOfWeek.MONDAY, "星期一");
            map.put(DayOfWeek.TUESDAY, "星期二");
            map.put(DayOfWeek.WEDNESDAY, "星期三");
            map.put(DayOfWeek.THURSDAY, "星期四");
            map.put(DayOfWeek.FRIDAY, "星期五");
            map.put(DayOfWeek.SATURDAY, "星期六");
            map.put(DayOfWeek.SUNDAY, "星期日");
            System.out.println(map); 
      // 输出： {MONDAY=星期一, TUESDAY=星期二, WEDNESDAY=星期三, THURSDAY=星期四, FRIDAY=星期五, SATURDAY=星期六, SUNDAY=星期日}
            System.out.println(map.get(DayOfWeek.MONDAY)); // 输出： 星期一
    }
}
```

使用`EnumMap`的时候，我们总是用`Map`接口来引用它，因此，实际上把`HashMap`和`EnumMap`互换，在客户端看来没有任何区别。



## 小结

如果`Map`的key是`enum`类型，推荐使用`EnumMap`，既保证速度，也不浪费空间。

使用`EnumMap`的时候，根据面向抽象编程的原则，应持有`Map`接口。
