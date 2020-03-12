
[TOC]

---

# `Java` 面试题

## Java 中的集中基本类型,各占了多少字节
### 基本数据类型
* 基本数据类型  
     * 数值型  
          * 整数类型  
          * byte  
          * short  
          * int  
          * long  
     * 浮点类型  
          *  float  
          * double  
     * 字符型  
          * char  
     * 布尔型  
          * boolean  
* 引用数据类型  
     * 类     class  
     * 接口     interface  
     * 数组  

|数据类型|大小|范围|默认值|  
|:---:|:---:|:---:|:---:|  
|byte(字节)|8|-128 ~ 127|0|  
|shot(短整型)|16|-32768 ~ 32768|0|  
|int(整型)|32|-2147483648 ~ 217483648|0|  
|long(长整型)|64|-9233372036854477808 ~ 9233372036854477808|0|  
|float(浮点型)|32|-3.40292347E+38 ~ 3.40292347E+38|0.0f|  
|double(双精度)|64|-1.79769313486231570E+308 ~ 1.79769313486231850E+308|0.0d|  
|char(字符型)|16|\u0000 ~ \uffff|\u0000|  
|boolean(布尔型)|1|true/false|false|

--- 

### String 能不能被继承
`String` 不能被继承,因为 `String` 类有 `final` 修饰符,而 `final` 修饰的类是不能被继承的,实现细节不允许改变。
```java
// 平常我们定义的 
String str = "a"
// 其实和
String str = new String("a")   
// 还是有差异的
```
前者默认调用的是 
```java
String static String valueOf(int i){
     returninteger.toString(i);
}
```  
后者调用的是
```java
public String(String original){
     this.value = original.value;
     this.hash = original.hash;
}
```
最后我们的变量都存储在一个 `char` 数组中
```
private final char value[];
```

---

### `String`, `StringBuffer`, `StringBuilder` 的区别。
#### String
字符串常量(`final` 修饰,不可被继承),`String`是常量,当穿件后既不能更改。(可以通过`StringBuffer`和`StringBuilder`创建`String`对象"常用的两个字符串操作类")  

#### StringBuffer  
字符串变量,(线程安全),其也是`final`类别的,不允许被继承,其中的绝大多数方法都进行了同步处理,包括常用的`Append`方法也做了同步处理(`syncharonized`修饰).其自`jdk1.0`其就已经出现.其`tiString`方法会进行对象缓存,已减少元素复制开销。  
```java
public syncharonized String toString(){
     if(toStringCache == null) {
          toStringCache = Arrays.copyOfRange(value, 0, count);
     }
     return new String(toStringCache, true);
}
```  

#### StringBuilder
字符串变量(非线程安全)其自`jdk1.5`起开始出现.与`StringBuffer`一样都继承和实现了同样的接口和类,方法除了没使用`syncharonized`修饰意外基本一致,不同之处在于最后`toString`的时候,会直接返回一个新对象。  
```java
public String toString() {
     return new String(value, 0, count);
}
```
  
---

### `ArrayList` 和 `LinkedList` 有什么区别  
#### `ArrayList` 和 `LinkedList` 都实现了 `List` 接口,有一下的不同点:
1. `ArrayList`是基于索引的数据接口,它的底层是数组.它可以以0(1)时间复杂度对元素进行随机访问.与此对应,`LinkedList` 是已严肃列表的形式存储他的数据,每一个元素都和它的前一个和后一个元素链接在一起,在这个情况下,查找某个元素的时间复杂度0(n).
2. 相对于`ArrayList`, `LinkedList` 的插入, 添加,删除操作速度更快, 因为当元素被添加到集合任意位置的时候,不需要想数组那样重新计算大小或者是更新索引。
3. `LinkedList` 比 `ArrayList` 更占内存,因为`LinkedList`为每一个节点存储了两个引用,一个指向前一个元素,一个指向下一个元素。

---

### 讲讲类的实例化顺序,比如父类静态数据,构造函数,字段,子类静态数据,构造函数,字段,当New的时候他们的执行顺序, 他们的执行顺序。  
此题考察的是类加载器实例化是进行的操作步骤(加载 --> 链接 --> 初始化)
* 父类静态代码变量
* 父类静态代码块
* 子类静态变量
* 子类静态代码块
* 父类非静态变量(父类实例变量)
* 子类构造函数

---  

用过哪些 `Map` 类, 都有什么区别, `HashMap` 是线程安全的么, 并发下使用的 `Map` 是什么,他们内部原理分别是什么,比如存储方式, `HashCode`, 扩容,默认容量的。
`HashMap` 是线程不安全的, `HashMap` 是数组+链表+红黑树(JDK1.8增加了红黑树部门)实现的,采用哈希表来存储的。
> 参照该链接: https://zhuanlan.zhihu.com/p/21673805

---  

### Java8 的 ConcurrentHashMap 为什么放弃了分段锁,有什么问题么？,如果你来设计,你如何设计。 有没有有顺序的  Map 实现类,如果有,他们是怎么保证有序的。
TreeMap 和 LinkedHashMap 是有序的(TreeMap 默认升序,LinkedHashMap 则记录了插入顺序)
> 参照: http://uule.iteye.com/blog/1522291

--- 

### 抽象类和接口的区别,类可以继承多个类么？接口可以继承多个接口么？类可以实现多个接口么？
1. 抽象类和接口都不能直接实例化,如果实例化,抽象类变量必须指向实现类所有抽象方法的子类对象,接口变量必须指向实现所有接方法的类对象。
2. 抽象类要被子类继承,接口要被类实现
3. 接口只能做方法申明,抽象类中可以做方法申明,也可以做方法实现
4. 接口里定义的变量只能是公共静态变量,抽象类中的变量是普通变量。
5. 抽象类里的抽象方法必须全部被子类所实现,如果子类不能全部实现父类抽象方法,那么该子类只能是抽象类。同样,一个类实现接口的时候,如果不能全部实现接口方法,那么该类只能为抽象。
6. 抽象方法只能申明,不能实现, `abstract void abc();` 不能写成 `abstract void abc(){}`
7. 抽象类里可以没有抽象方法
8. 如果一个雷利有抽象方法,那么这个类只能是抽象类
9. 抽象方法要被实现,所以不能是静态的,也不能是私有的。
10. 接口可以继承接口,并可多继承接口,单类只能单根继承。

---

### 继承和聚合的区别在哪?
#### 继承
继承值得是一个类(称为子类、子接口)继承另外的一个类(称为父类、父接口)的功能,并可以增加他自己的新功能的能力,继承是类与类或者接口与接口之间最常见的关系,在`Java`中此类关系通过关键字 `extends` 明确表示,在设计时一般没有争议性: 
> 类图(明天添加)

#### 聚合
聚合是关联关系的一种特例,他体现的是整体与部门、拥有的关系,即 `has-a` 的关系,此时整体与部门之间是可分离的,他们可以具有个字的声明周期,部门可以属于多个整体对象,也可以是为多个整体对象共享: 比如计算机与CPU、公司与员工的关系等。表现在代码层面,和关联关系是一致的,只能从语义级别来区别: 
> 类图(明天添加)

---

### 讲讲你理解的 `IO` 和 `NIO` 的区别是啥,谈谈 `reactor` 模型
`IO` 是面向流的,阻塞IO
`NIO` 是面向缓冲区的,非阻塞IO

---

### 反射的原理,反射创建类实例的三种方式是什么？
```java
1. 第一种方式 任何类都有一个隐含的静态成员变量 class
Class cla = Student.class

2. 第二种方式 已知道该类的对象,通过 getClass 方法
Student student = new Student();
Class cla1 = student.getClass();

3. 第三种方式 已知对象全路径名,通过Class 的静态方法 forName
Class cla2 = null
try {
     cla2 = Class.forName("com.anan.entity.Student");
} catch (ClassNotFoundException e) {
     e.printStackTrace();
}
```

--- 

### 反射中,`Class.forName` 和 `ClassLoader` 区别。
#### Class.forName(className)
其实调用的方法是  `Class.forName(className, true, classloader);` 注意看第二个 `boolean` 参数, 它表示的意思, 在 `loadClass` 后必须初始化。 我们可以清晰的看到在执行过此方法后,目标对象的 `static` 快代码已经被执行,`static`参数也已经被初始化。

#### ClassLoader.loadClass(className)
起始他调用的方法是 ClassLoader.loadClass(className, false); 还是注意 boolean 参数,该参数表示目标对象被加载后不进行连接,这就意味着不会去执行该类静态快中间的内容,因此两者的区别就显而易见了。

---

### 单例模式的五种写法
***懒汉***  
线程不安全. 这种写法能够在多线程中很好的工作,而且看起来它也具备很好的 `lazy loading`,但是,很遗憾的是,效率很低,99%情况下不需要同步。
```java
public class Singleton{
     private static Singleton instance;

     private Singleton(){}

     public static syncharonized Singleton getInstance() {
          if(instance == null) {
               instance = new Singleton();
          }
          return instance;
     }
}
```  

***恶汉***  
这种方式基于`classloder`机制避免了多线程的同步问题,`instance`在类装在时就实例化。目前`Java`单利是指一个虚拟机的范围,因为装载类的功能是虚拟机的,所以一个虚拟机在通过自己的`ClassLoader`装载恶汉式实现单利类的时候就创建一个类的实例,这就意味着一个虚拟机里面有很多`ClassLoader`,而这些`ClassLoader`都能装载某个类的话,就算这个类是单例,当然如果一台机器上有很多虚拟机,那么每个虚拟机中都有至少一个这个类的实例的话,那这样就更不会有单例了。
```java
public class Singleton {
     private static Singleton instance = new Singleton();
     private Singleton () {}

     public static Singleton getInstance() {
          return instance;
     }
}
```

***静态内部类***  
这种方式利用了`ClassLoader`的机制来保证初始化`instance`时只有一个线程,这种方式是`Singleton`类被装载了,`instance`不一定被初始化。因为`SingletonHolder`类没有被主动使用,只有现实通过调用`getInstance`方式时,才会装载`SingletonHolder`类,从而实例化`instance`。想象一下,如果实例化`instance`很消耗资源,我想让他先吃加载！这个时候,这种方式相比第二种方式就显得合理
```java
public class Singleton {
     private static class SingletonHolder {
          private static final Singleton INSTANCE = new Singleton();
     }
     private Singleton() {}

     public static final Singleton getInstance() {
          return SingletonHoler.INSTANCE;
     }
}
```

***枚举***  
这种方式是`Effective Java`作者`Josh Bloch`提倡的方式,它不仅能避免多线程的同步问题,而且还能防止反序列化重新创建新的对象,可谓是很坚强的壁垒,不过个人认为由于1.5中才加入enum特性,用这种方式不避免让人感觉生疏,在市级工作中很少有人这么写。
```java
public enum Singleton{
     INSTANCE;
     public void whateverMethod(){

     }
}
```

***双重校验锁(jdk1.5)***  
这种方式实现线程安全第创建实例,二有不会对性能造成太大影响,他只是第一次创建实例的时候同步,以后就不需要同步了,由于`Volatile`关键字屏蔽了虚拟机中一些必要的代码优化,所以运行效率并不是很高,因此建议没有特别的需要不要使用,双重校验锁方式的单例不建议大量使用,根据情况决定。
```java
public class Singleton {
     private valatile static Singleton singleton;
     private SingLeton() {}
     public static SingLeton getSingleton() {
          if (singleton == null) {
               synchronized (Singleton.class) {
                    if (singleton == null) {
                         singleton = new Singleton();
                    }
               }
          }
          return singleton;
     }
}
```

---

## final 关键字
- 类  `final`修饰的类不能被继承 
- 变量 `final`修修饰时的变量不能被修改
- 方法  `final`修饰的方式不能被重写  
[Final](final)

---

## 如果在父类中未子类自动完成所有的 HashCode 和 Equals 实现？ 这么做有何优劣。   

[Equals and HashCode](Equals_and_HashCode)

---

## 请结合 OO 设计理念，谈谈访问修饰符 public、 private、 protected、 default在应用设计中的作用
|  |同一个类|同一个包|不同包的子类|不同包的非子类|
|:---:|:---:|:---:|:---:|
|private| 有效 |  |  |  |
|default|有效|有效| | |
|protected|有效|有效|有效| |
|publicv|有效|有效|有效|有效|

**public**: `Java`语言中访问限制最宽的修饰符，一般称之为"公共的"。被其修饰的类、属性以及方法不仅可以夸类访问，而却允许挎包`package`访问。  
**private**: `Java`语言中对访问权限限制的最窄的修饰符，一般称之为"私有的"。被其修饰的类，属性以及方法只能被该类的对象访问，其子类不能访问，更不能允许跨包访问。  
**oritect**: 介于`public`和`private`之间的一种访问修饰符，一般称之为"保护形"。被其修饰的类、属性以及方法只能被类本身的方法及子类访问，即时子类在不同的包中也可以访问。   
**default**: 即不加任何访问修饰符，通常称为"默认访问模式",该模式下，只允许在同一个包中进行访问。

---

## Java 中浅拷贝和的深拷贝
[what is a copy](java_copy)