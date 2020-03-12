# Java 中的浅拷贝与深拷贝

## 什么是拷贝？
首先，我们队引用拷贝和对象拷贝进行一下区分。  

**引用拷贝**  正如它的名称所表述的意思，就是创建一个指向对象的引用变量的拷贝。如果我们有一个`Car`对象,而且让`MyCar` 变量指向这个变量，这时候当我们做引用拷贝，那么现在就会有两个`MyCar`变量，但是对象任然只存在一个。  

![](https://github.com/finissage/study_nodes/blob/master/img/1001.jpg?raw=true)


**对象拷贝**  
对象拷贝会创建对象本身的一个副本。因此如果我们再一次服务我们 car 对象，就会创建一个对象本身的一个副本，同时还会有第二个引用变量指向这个被复制出来的对象

![](https://github.com/finissage/study_nodes/blob/master/img/1002.jpg?raw=true)


## 什么是对象？
深拷贝和浅拷贝都是对象拷贝，但是一个对象市级是什么呢？当我们谈论到对象时，我们经常会说它就像一粒浑圆的咖啡豆，已经是一个不能够被进一步分解的单位了，单这种说法太过简化了。  

![](https://github.com/finissage/study_nodes/blob/master/img/1003.jpg?raw=true)


比如说我们有一个`Person`对象，这个`Person`对象实际上是由其他的对象组合而成的，如下图，`Person`对象包含了一个Name对象和一个`Address`对象，`Name`对象又包含了一个`FirstName`对象和一个`LastName`对象；`Address`对象又是由一个`Street`对象一级一个`City`组合而成的。那么当我们谈论本文中的`Person`时，实际上我是在讨论这些个对象所组成的整合的对象联系网络。

![](https://github.com/finissage/study_nodes/blob/master/img/1004.jpg?raw=true)


那么我为什么要对这个Person对象进行拷贝呢？对象复制，经常也会被称作克隆，它是在我们想要修改或者移除某个对象。单仍然要保留原来那个对象时要进行的操作。

---

## 浅拷贝
首先让我们来说说浅拷贝。对象的浅拷贝会对"主"对象进行拷贝，单不会复制主对象里面的对象，"里边的对象"会在原来的对象和它的副本之间共享，例如，我们会为一个Person对象创建第二个`Person`对象，而两个`Person`会共享相同的`Name`和`Address`对象。

我们有一个类`Person`，类里边包含一个`Name`和`Address`对象，拷贝构造器会拿到`OriginalPerson`对象，然后对其应用变量进行复制。
```java
public class Person {
	private Name name;

	private Address address;

	public Person(Person originalPerson) {
		this.name = originalPerson.name;
		this.address = originalPerson.address;
	}
	...
}
```
浅拷贝的问题就是两个对象并非独立的，如果你修改其中一个`Person`对象的`Name`对象，name这次修改也会影响到另外一个`Person`对象。假如我们有一个`Person`对象，然后也有一个引用变量`monther`来指向它，然后当我们队`mother`进行拷贝时，创建第二个`Person`对象`son`，如果在此后的代码中，`son`尝试用`moveOut()`来修改他的Address对象，那么`mother`也会跟着他一起搬走！
```java
Person mother = new Person(new Name(...), new Address(...))
[...]
Person son = new Person(mother);
[...]
son.moveOut(new Street(...), new City(...));
```
这种现象之所以会发生，是因为`mother`和`son`对象共享了相同的`Address`对象，当我们在一个对象中修改了`Address`对象，那么也就表示两个对象总是`Address`都被修改了

![](https://github.com/finissage/study_nodes/blob/master/img/1005.jpg?raw=true)


---

## 深拷贝
不同于浅拷贝，深拷贝是一个**整个独立的对象拷贝**。如果我们队整个Person对象进行深拷贝，我们队整个对象的机构进行拷贝。

![](https://github.com/finissage/study_nodes/blob/master/img/1006.jpg?raw=true)


对一个`Person`的`Address`对象进行了修改并不会对另一个对象造成影响。单对`Person`对象使用了拷贝构造器。同时也会对立面的对象使用拷贝构造器。
```java
public class Person {
	private Name name;

	private Address address;

	public Person(Person otherPerson) {
		this.name = new Name(otherPerson.name);
		this.address = new Address(otherPerson.address);
	}
}
```
不过故事到这还没有结束，要创建一个真正的深拷贝，就需要我们一直这样拷贝下去，一直覆盖到`Person`对象所有的内部元素，最后只剩下原始的类型以及"不可变对象(`Immutables`)"。
```java 
public class Street {
	private String name;

	private int number;

	public Street(Street otherStreet) {
		this.name = otherStreet.name;
		this.number = otherStreet.number;
	}
}
```
`Street`对象又两个实体变量组成，`String`类型的`Name`以及`int`类型的`number`。`int`类型的`number`是一个原始类型，并非对象，它只是一个简单的值，不能共享，因此在创建第二个实体变量时，我们可以自动创建一个独立的拷贝，String是一个不可变对象(`Immutable`)。简言之，不可变对象也是对象，可一旦创建好了以后就再也不能修改了，因此，你可以不用为其创建深拷贝就能对其进行共享。


> 本文引用：http://www.oschina.net/translate/java-copy-shallow-vs-deep-in-which-you-will-swim