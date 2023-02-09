## 目录

- [使用TreeMap](#使用TreeMap)
- [小结](#小结)



## 使用TreeMap

我们已经知道，`HashMap`是一种以空间换时间的映射表，它的实现原理决定了内部的Key是无序的，即遍历`HashMap`的Key时，其顺序是不可预测的（但每个Key都会遍历一次且仅遍历一次）。

还有一种`Map`，它在内部会对Key进行排序，这种`Map`就是`SortedMap`。注意到`SortedMap`是接口，它的实现类是`TreeMap`。

```ascii
       ┌───┐
       │Map│
       └───┘
         ▲
    ┌────┴─────┐
    │          │
┌───────┐ ┌─────────┐
│HashMap│ │SortedMap│
└───────┘ └─────────┘
               ▲
               │
          ┌─────────┐
          │ TreeMap │
          └─────────┘
```

`SortedMap`保证遍历时以Key的顺序来进行排序。例如，放入的Key是`"apple"`、`"pear"`、`"orange"`，遍历的顺序一定是`"apple"`、`"orange"`、`"pear"`，因为`String`默认按字母排序：

```java
package myMap;

import java.util.Map;
import java.util.TreeMap;

public class myTreeMap {
    public static void main(String[] args) {
        Map<String, Integer> map = new TreeMap<>();
        map.put("orange", 1);
        map.put("apple", 2);
        map.put("pear", 3);
        for (String key : map.keySet())
        {
            System.out.println(key);
        }
        // 输出： apple, orange, pear
    }
}
```

使用`TreeMap`时，放入的Key必须实现`Comparable`接口。`String`、`Integer`这些类已经实现了`Comparable`接口，因此可以直接作为Key使用。作为Value的对象则没有任何要求。



`TreeMap`不使用`equals()`和`hashCode()`。

如果作为Key的class没有实现`Comparable`接口，那么，必须在创建`TreeMap`时同时指定一个自定义排序算法：

```java
package myMap;

import java.util.Comparator;
import java.util.Map;
import java.util.TreeMap;

public class myTreeMap {
    public static void main(String[] args) {
        Map<Person, Integer> map = new TreeMap<Person, Integer>( new Comparator<Person>() {
            @Override
          // 注意到Comparator接口要求实现一个比较方法，它负责比较传入的两个元素a和b，如果a<b，则返回负数，通常是-1，如果a==b，则返回0，如果a>b，则返回正数，通常是1。TreeMap内部根据比较结果对Key进行排序。
            public int compare(Person p1, Person p2) {
                return p1.name.compareTo(p2.name);
            }
        });
        map.put(new Person("Tom"), 1);
        map.put(new Person("Bob"), 2);
        map.put(new Person("Lily"), 3);
        for (Person key : map.keySet()) {
            System.out.println(key);
        }
        // {Person: Bob}, {Person: Lily}, {Person: Tom}
        System.out.println(map.get(new Person("Bob"))); // 2
    }
}

// 注意到Person类并未覆写equals()和hashCode()，因为TreeMap不使用equals()和hashCode()。
class Person {
    public  String name;
    Person(String name){
        this.name =name;
    }
    public String toString(){
        return "{Person: " + name + "}";
    }
}
```

注意到`Comparator`接口要求实现一个比较方法，它负责比较传入的两个元素`a`和`b`，如果`a<b`，则返回负数，通常是`-1`，如果`a==b`，则返回`0`，如果`a>b`，则返回正数，通常是`1`。`TreeMap`内部根据比较结果对Key进行排序。

```java
package myMap;

import java.util.Comparator;
import java.util.Map;
import java.util.TreeMap;

public class myTreeMap {
    public static void main(String[] args) {
        Map<StudentMap, Integer> map = new TreeMap<StudentMap, Integer>(new Comparator<StudentMap>() {
            @Override
            public int compare(StudentMap o1, StudentMap o2) {
                // TreeMap在比较两个Key是否相等时，依赖Key的compareTo()方法或者Comparator.compare()方法。在两个Key相等时，必须返回0。
                if(o1.score ==  o2.score)
                    return 0;
                return  o1.score > o2.score ? -1 : 1;
            }
        });
        map.put(new StudentMap("Tom", 77), 1);
        map.put(new StudentMap("Bob", 66), 2);
        map.put(new StudentMap("Lily", 99), 3);
        for (StudentMap key : map.keySet())
        {
            System.out.println(key);
        }
        System.out.println(map.get(new StudentMap("Bob", 66)));;
    }
}


class StudentMap {
    public String name;
    public int score;
    StudentMap(String name, int score) {
        this.name = name;
        this.score = score;
    }
    public String toString() {
        return String.format("{%s: score=%d}", name, score);
    }
}
```





## 小结

`SortedMap`在遍历时严格按照Key的顺序遍历，最常用的实现类是`TreeMap`；

作为`SortedMap`的Key必须实现`Comparable`接口，或者传入`Comparator`；

要严格按照`compare()`规范实现比较逻辑，否则，`TreeMap`将不能正常工作。
