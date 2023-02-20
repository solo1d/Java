## 目录

- [使用Stack](#使用Stack)
- [Stack的作用](#Stack的作用)
- [小结](#小结)



## 使用Stack

`Stack`是这样一种数据结构：只能不断地往`Stack`中压入（push）元素，最后进去的必须最早弹出（pop）来：



`Stack`只有入栈和出栈的操作：

- 把元素压栈：`push(E)`；
- 把栈顶的元素“弹出”：`pop()`；
- 取栈顶元素但不弹出：`peek()`。

在Java中，我们用`Deque`可以实现`Stack`的功能：

- 把元素压栈：`push(E)`/`addFirst(E)`；
- 把栈顶的元素“弹出”：`pop()`/`removeFirst()`；
- 取栈顶元素但不弹出：`peek()`/`peekFirst()`。

为什么Java的集合类没有单独的`Stack`接口呢？因为有个遗留类名字就叫`Stack`，出于兼容性考虑，所以没办法创建`Stack`接口，只能用`Deque`接口来“模拟”一个`Stack`了。

当我们把`Deque`作为`Stack`使用时，注意只调用`push()`/`pop()`/`peek()`方法，不要调用`addFirst()`/`removeFirst()`/`peekFirst()`方法，这样代码更加清晰。



## Stack的作用

Stack在计算机中使用非常广泛，JVM在处理Java方法调用的时候就会通过栈这种数据结构维护方法调用的层次。例如：

```java
static void main(String[] args) {
    foo(123);
}

static String foo(x) {
    return "F-" + bar(x + 1);
}

static int bar(int x) {
    return x << 2;
}
```

JVM会创建方法调用栈，每调用一个方法时，先将参数压栈，然后执行对应的方法；当方法返回时，返回值压栈，调用方法通过出栈操作获得方法返回值。

因为方法调用栈有容量限制，嵌套调用过多会造成栈溢出，即引发`StackOverflowError`

```java
package com.itranswarp.learnjava;



import java.util.*;



/**

 * Learn Java from https://www.liaoxuefeng.com/

 * 

 * @author liaoxuefeng

 */

public class Main {
	public static void main(String[] args) {
		String exp = "x + 2 * (y - 5)";
		SuffixExpression se = compile(exp);
		Map<String, Integer> env = Map.of("x", 1, "y", 9);
		int result = se.execute(env);
		System.out.println(env);
		System.out.println(exp + " = " + result + " " + (result == 1 + 2 * (9 - 5) ? "yes" : "no"));
	}

	static SuffixExpression compile(String exp) {
		// TODO:
		Deque<String> nums = new LinkedList<>();
    	Deque<String> symbols = new LinkedList<>();
    	for (int i = 0; i < exp.length(); i++) {
			String n = exp.charAt(i) + "";
			if (n.equals(" ") || n.equals("(") || n.equals(")")) continue; 
			if (n.equals("+") || n.equals("-") || n.equals("*") || n.equals("/")) {
				symbols.push(n);
				continue;
			}
			nums.push(n);
		}
        return new SuffixExpression(nums, symbols);
	}
}

class SuffixExpression {
	Deque<String> nums;
	Deque<String> symbols;

	public SuffixExpression(Deque<String> nums, Deque<String> symbols) {
		this.nums = nums;
		this.symbols = symbols;
	}

	int execute(Map<String, Integer> env) {
		// TODO:
		int res = 0;
		while (symbols.size() != 0) {
			String last = nums.pop();
			String pre = nums.pop();
			String symb = (String) symbols.pop();
		
			int lastV = env.get(last) == null ? Integer.parseInt(last): env.get(last);
			int preV = env.get(pre) == null ? Integer.parseInt(pre): env.get(pre);

			switch(symb) {
			case "+": 
				res = preV + lastV;
				break;
			case "-": 
				res = preV - lastV;
				break;
			case "*": 
				res = preV * lastV;
				break;
			case "/": 
				res = preV / lastV;
				break;
			}
			nums.push(res + "");
		}
		return res;
	}
}
```



## 小结

栈（Stack）是一种后进先出（LIFO）的数据结构，操作栈的元素的方法有：

- 把元素压栈：`push(E)`；
- 把栈顶的元素“弹出”：`pop(E)`；
- 取栈顶元素但不弹出：`peek(E)`。

在Java中，我们用`Deque`可以实现`Stack`的功能，注意只调用`push()`/`pop()`/`peek()`方法，避免调用`Deque`的其他方法。

最后，不要使用遗留类`Stack`。







