## 目录

- [使用List](#使用List)
- [List的特点和创建](#List的特点和创建)
- [遍历List](#遍历List)
- [List和Array转换](#List和Array转换)
- [编写equals方法](#编写equals方法)
- 







## 使用List

在集合类中，`List`是最基础的一种集合：它是一种有序列表。

`List`是一种有序链表：`List`内部按照放入元素的先后顺序存放，并且每个元素都可以通过索引确定自己的位置。

`List`的行为和数组几乎完全相同：`List`内部按照放入元素的先后顺序存放，每个元素都可以通过索引确定自己的位置，`List`的索引和数组一样，从`0`开始。

`ArrayList`把添加和删除的操作封装起来，让我们操作`List`类似于操作数组，却不用关心内部元素如何移动。

我们考察`List<E>`接口，可以看到几个主要的接口方法：

- 在末尾添加一个元素：`boolean add(E e)`
- 在指定索引添加一个元素：`boolean add(int index, E e)`
- 删除指定索引的元素：`E remove(int index)`
- 删除某个元素：`boolean remove(Object e)`
- 获取指定索引的元素：`E get(int index)`
- 获取链表大小（包含元素的个数）：`int size()`

但是，实现`List`接口并非只能通过数组（即`ArrayList`的实现方式）来实现，另一种`LinkedList`通过“链表”也实现了List接口。在`LinkedList`中，它的内部每个元素都指向下一个元素：

我们来比较一下`ArrayList`和`LinkedList`：

|                     | ArrayList    | LinkedList           |
| :------------------ | :----------- | :------------------- |
| 获取指定元素        | 速度很快     | 需要从头开始查找元素 |
| 添加元素到末尾      | 速度很快     | 速度很快             |
| 在指定位置添加/删除 | 需要移动元素 | 不需要移动元素       |
| 内存占用            | 少           | 较大                 |

通常情况下，我们总是优先使用`ArrayList`。



## List的特点和创建

使用`List`时，我们要关注`List`接口的规范。`List`接口允许我们添加重复的元素，即`List`内部的元素可以重复：

```java
package myList;

import java.util.ArrayList;
import java.util.List;

public class myList {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add(null); // size=2				List还允许添加null
        list.add("pear"); // size=3
        System.out.println(list.size());
      
     // 除了使用ArrayList和LinkedList，还可以通过List接口提供的of()方法，根据给定元素快速创建List：
     // 但是List.of()方法不接受null值，如果传入null，会抛出NullPointerException异常。
        List<Integer> listInt = List.of(1, 2, 5);
        System.out.println(listInt.size());
    }
}
```



## 遍历List

使用迭代器`Iterator`来访问`List`。`Iterator`本身也是一个对象，但它是由`List`的实例调用`iterator()`方法的时候创建的。`Iterator`对象知道如何遍历一个`List`，并且不同的`List`类型，返回的`Iterator`对象实现也是不同的，但总是具有最高的访问效率。

`Iterator`对象有两个方法：`boolean hasNext()`判断是否有下一个元素，`E next()`返回下一个元素。因此，使用`Iterator`遍历`List`代码如下：

```java
package myList;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class myList {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add(null); // size=2
        list.add("pear"); // size=3

        // 使用迭代器来遍历集合
        for (Iterator<String> it = list.iterator(); it.hasNext();){
            String s = it.next();
            System.out.println(s);
        }
        // 迭代器精简后的写法, Java的for each循环本身就可以帮我们使用Iterator遍历
        // Java编译器本身并不知道如何遍历集合对象，但它会自动把for each循环变成Iterator的调用，
        //    原因就在于Iterable接口定义了一个Iterator<E> iterator()方法，强迫集合类必须返回一个Iterator实例。
        for (String s : list){
            System.out.println(s);
        }

      
        // 继承并实现 Iterable 迭代器功能。 可以使用 for echo 检验
        ReverseList<String> rlist = new ReverseList<String>();
        rlist.add("Apple");
        rlist.add("Orange");
        rlist.add("Pear");
        for (String s : rlist) {
            System.out.println(s);
        }
    }
}

// 继承并实现 Iterable 迭代器功能。
class ReverseList<T> implements Iterable<T> {

    private List<T> list = new ArrayList<>();

    public void add(T t) {
        list.add(t);
    }

    @Override
    public Iterator<T> iterator() {
        return new ReverseIterator(list.size());
    }

    class ReverseIterator implements Iterator<T> {
        int index;

        ReverseIterator(int index) {
            this.index = index;
        }

        @Override
        public boolean hasNext() {
            return index > 0;
        }

        @Override
        public T next() {
            index--;
            return ReverseList.this.list.get(index);
        }
    }
}
```

实际上，只要实现了`Iterable`接口的集合类都可以直接用`for each`循环来遍历，Java编译器本身并不知道如何遍历集合对象，但它会自动把`for each`循环变成`Iterator`的调用，原因就在于`Iterable`接口定义了一个`Iterator<E> iterator()`方法，强迫集合类必须返回一个`Iterator`实例。



## List和Array转换

- 把`List`变为`Array`有三种方法

  - 第一种是调用`toArray()`方法直接返回一个`Object[]`数组

    - ```java
      package myList;
      
      import java.util.List;
      public class ListToArray {
          public static void main(String[] args) {
              List<String> list = List.of("apple", "pear", "banana");
              Object[] array = list.toArray();
              for (Object s : array) {
                  System.out.println(s);
              }
          }
      }
      ```

    - **这种方法会丢失类型信息，所以实际应用很少。**

  - 第二种方式是给`toArray(T[])`传入一个类型相同的`Array`，`List`内部自动把元素复制到传入的`Array`中

    - ```java
      package myList;
      
      import java.util.List;
      public class ListToArray {
          public static void main(String[] args) {
              //第二种方式
              List<Integer> list = List.of(12, 34, 56);
              Integer[] array = list.toArray(new Integer[list.size()]); // 这里和第一种有区别
            // 注意到这个toArray(T[])方法的泛型参数<T>并不是List接口定义的泛型参数<E>，所以，我们实际上可以传入其他类型的数组，例如我们传入Number类型的数组，返回的仍然是Number类型：
              for (Integer n : array) {
                  System.out.println(n);
              }
          }
      }
      ```

    - 注意到这个`toArray(T[])`方法的泛型参数`<T>`并不是`List`接口定义的泛型参数`<E>`，所以，我们实际上可以传入其他类型的数组，例如我们传入`Number`类型的数组，返回的仍然是`Number`类型。

  - 最后一种更简洁的写法是通过`List`接口定义的`T[] toArray(IntFunction<T[]> generator)`方法

    - ```java
      package myList;
      
      import java.util.List;
      public class ListToArray {
          public static void main(String[] args) {
              // List和Array转换
              //最后一种更写法： 函数式写法
              List<Integer> list = List.of(12, 34, 56);
              Integer[] arrayInt = list.toArray(Integer[]::new);
              for (Integer n : arrayInt) {
                  System.out.println(n);
              }
          }
      }
      ```

    - 



把`Array`变为`List`就简单多了，通过`List.of(T...)`方法最简单：要注意的是，返回的`List`不一定就是`ArrayList`或者`LinkedList`，因为`List`只是一个接口，如果我们调用`List.of()`，它返回的是一个只读`List`

```java
package myList;

import java.util.Arrays;
import java.util.List;
public class ListToArray {
    public static void main(String[] args) {
        // 和Array和List转换
        Integer[] array = {1,2,3,4};
        List<Integer> list = List.of(array);
        list.add(123);
//        对只读List调用add()、remove()方法会抛出UnsupportedOperationException。
    }
}

```





## 编写equals方法

`List`还提供了`boolean contains(Object o)`方法来判断`List`是否包含某个指定元素。此外，`int indexOf(Object o)`方法可以返回某个元素的索引，如果元素不存在，就返回`-1`。

```java
package myList;

import java.util.List;

public class myEquals {
    public static void main(String[] args) {
//        List还提供了boolean contains(Object o)方法来判断List是否包含某个指定元素。
//        int indexOf(Object o)方法可以返回某个元素的索引，如果元素不存在，就返回-1。
        List<String> list = List.of("A", "B", "C");
        System.out.println(list.contains(new String( "C"))); // true
        System.out.println(list.contains("X")); // false
        System.out.println(list.indexOf("C")); // 2
        System.out.println(list.indexOf("X")); // -1
    }
}
```

因为`List`内部并不是通过`==`判断两个元素是否相等，而是使用`equals()`方法判断两个元素是否相等。

要正确使用`List`的`contains()`、`indexOf()`这些方法，放入的实例必须正确覆写`equals()`方法，否则，放进去的实例，查找不到。我们之所以能正常放入`String`、`Integer`这些对象，是因为Java标准库定义的这些类已经正确实现了`equals()`方法。





如何正确编写`equals()`方法？`equals()`方法要求我们必须满足以下条件：

- 自反性（Reflexive）：对于非`null`的`x`来说，`x.equals(x)`必须返回`true`；
- 对称性（Symmetric）：对于非`null`的`x`和`y`来说，如果`x.equals(y)`为`true`，则`y.equals(x)`也必须为`true`；
- 传递性（Transitive）：对于非`null`的`x`、`y`和`z`来说，如果`x.equals(y)`为`true`，`y.equals(z)`也为`true`，那么`x.equals(z)`也必须为`true`；
- 一致性（Consistent）：对于非`null`的`x`和`y`来说，只要`x`和`y`状态不变，则`x.equals(y)`总是一致地返回`true`或者`false`；
- 对`null`的比较：即`x.equals(null)`永远返回`false`。

上述规则看上去似乎非常复杂，但其实代码实现`equals()`方法是很简单的，我们以`Person`类为例：



```java
package myList;

import java.util.List;
import java.util.Objects;

public class myEquals {
    public static void main(String[] args) {
//        List还提供了boolean contains(Object o)方法来判断List是否包含某个指定元素。
//        int indexOf(Object o)方法可以返回某个元素的索引，如果元素不存在，就返回-1。
//        List<String> list = List.of("A", "B", "C");
//        System.out.println(list.contains(new String( "C"))); // true
//        System.out.println(list.contains("X")); // false
//        System.out.println(list.indexOf("C")); // 2
//        System.out.println(list.indexOf("X")); // -1


        List<Person> list = List.of(
                new Person("Xiao Ming"),
                new Person("Xiao Hong"),
                new Person("Bod"),
                new Person(null)
        );
        System.out.println(list.contains(new Person(null)));

    }
}


/*
    我们要定义“相等”的逻辑含义。对于Person类，如果name相等，并且age相等，我们就认为两个Person实例相等。
 */
class Person{
    public String name;
    public int age;
    public Person(String name){
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

