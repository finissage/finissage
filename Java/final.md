# 深入理解 Java 中的 final 关键字
##### Java 中的final关键字非常重要，它可以应用于类、方法以及变量。本文将详细介绍 final 关键字的作用

### final 关键字的含义？
`final` 在 Java 中是一个保留的关键字，可以声明成员变量，方法，类以及本地变量。 一旦你讲引用申明作为`final`, 你讲不能改变这个引用了，编译器检查代码，如果你试图将变量再次初始化的话，编译器会报编译错误。

### 什么是 final 变量？
凡是对陈冠变量或者本地变量(在方法中的或者代码块中的变量成为本地变量)申明为 `final`的都叫做`final`变量。`final`变量经常和`static`关键字一起使用，作为常量，下面是`final`变量的例子；
```java
public static final String LOAN = "loan"
LOAN = new String("loan") //invalid compilation error
```
> final 变量是只读的。

### 什么是 final 方法？
`final`也可以申明方法。方法前面加上`final`关键字，代表这个方法不可以被子类的方法重写，如果你认为一个方法的功能已经足够完整了，子类中不需要改变的话，你可以申明此方法为`final`，`final`方法比非`final`方法要快，因为在编译的时候已经静态绑定了，不需要在运行时在运行时再动态绑定，下面是`final`方法的例子；
```java
class Person {
	public final String getName() {
		return "Person name";
	}
}

class Student extends Person {
	@Overide
	public final String getName() {
		return "Student name"; // compilation error: overridded method is final 
	}
}
```

### 什么是 final 类？
使用`final`来修饰的类叫做`final`类，`final` 类通常功能是完整的，他们不能被继承。`Java`中有许多类是`final`的，比如`String`,`integer`一级其他包装类。下面是`final`类的实例；
```java
final class Person {

}

class Student extends Person { // compilation error：cannot inherit form final class

}
```

### final 关键字的好处
#### 下面总结了一些使用 final 关键字的好处
- `final`关键字提高了性能，`JVM`和`Java`应用都会缓存`final`变量。
- `final`变量可以安全的在多线程环境下进行共享，而不需要额外的同步开销。
- 使用`final`关键字，`JVM`会对方法，变量及类进行优化。  

#### 不可变类
创建不可变类要使用`final`关键字。不可变类是指他的对象一旦被创建了就不能被更改了，`String`是不可变类的代表。不可变类有很多好处，比如他们的对象是只读的。可以再多个线程环境下安全的共享，不用额外的同步开销等等。

### 关于 final 的重要知识点
1. `final`关键字可以用于成员变量、本地变量、方法以及类。
2. `final`成员变量必须在申明的时候初始化或者在构造中初始化，否则汇报编译错误。
3. 本地变量必须在申明时赋值
4. 你不能够对`final`变量在此赋值
5. 在匿名类中所有变量都必须是`final`变量。
6. `final` 方法不能被重写
7. `final` 类不能被继承
8. `final` 关键字不能与`finally`关键字，后者用于异常处理
9. `final`关键字容易与`finalize()`方法搞混，后者是`Object`类中定义的方法，是在垃圾回收之前被`JVM`调用的方法。
10. 接口中声明的所有变量本身是`final`的。
11. `final`和`abstract`这两个关键字是相反的，`final`类就不可能是`abstract`的。
12. `final`方法在编译阶段绑定，成为静态绑定(`static bindding`)。
13. 没有在声明时初始化`final`变量的成为空白`final`变量(`blank final variable`),他们必须在构造器中初始化，或者调用`this()`初始化。不这么做的话，编译器会报错(`final`变量需要进行初始化)。
14. 将类、方法、变量申明为`final`能够提高性能，这样`JVM`就会有机会进行估计，然后优化。
15. 按照`Java`代码惯例，`final`变量是常量，而且通常常量名要大写；
```java
private final int PI = "3.14";
```
16. 对于集合对象申明为`final`指的是引用不能被更改，但是你可以向其中增加、删除或者改变内容，比如；
```java
private final List list = new ArrayList();
list.add("java");
list.add("java script");
list = new Vector();
```

> 我们已经知道`final`变量、`final`方法以及`final`类是什么了。必要的时候使用`final`,能写出更快、更好的代码。