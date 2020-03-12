# Lambda 表达式
** -> **  
**左侧：**`Lambda` 表达式的参数列表  
**右侧：**`Lambda` 表达式中所执行的功能,即 `Lambda` 体  

语法格式一：无参数，无返回值
```java
() -> System.out.println("hello lambda!");
````

语法格式二：有一个参数，并且无返回值
```java
(x) -> System.out.println(x);  
// 如果只有一个参数,() 可以省略不谢·
x -> System.out.println(x);
```

语法格式四：有两个以上的参数，并且Lambda 内有多条语句
```java
(x, y) -> {
	System.out.println("这是多条语句");
	return "aaa";
}
```

语法格式五：若 Lambda 体中只有一条语句，return和大括号都可以省略不写
```java
Compartor<Integer> i = (x, y) -> Integer.compare(x , y);
```

语法格式六： Lambda 表达式的参数列表数据类型可以省略不写，因为 JVM 编译器的上下文推断出数据类型，即“类型推断”
```java
(Integer x, Integer y) -> Integer.compare(x, y)
```

## Lambda 表达式需要“函数式接口”的支持
**函数式接口** 接口中只有一个抽象方法的接口，成为函数式接口。可以使用 `@FunctionInterface` 修饰，可以检查是否是函数式接口


## Java8 四大内置核心函数式接口
### Consumer<T> : 消费型接口
	void accept(T t);

### Supplier<T>: 供给型接口
	T get();

### Function<T, R> : 函数型接口
	R apply(T t);

### Prodicate<T> : 段言型接口
	boolean test(T t)