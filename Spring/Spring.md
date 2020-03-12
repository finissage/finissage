# Spring 常用注解

## Spring 名词解释
### id 和 name
    每个 Bean 在 Spring 容器中都有一个唯一的名字（beanName）和0个或多个别名（aliases）
### profile
    区分环境  
### BeanFactory
    特殊的 Bean
### FactoryBean
    静态工厂货实例工厂
### Bean 
> Spring Bean 在代码层其实是 BeanDefinition：
> BeanDefinition 中保存了我们的 Bean 信息，比如这个 Bean 指向的是哪个类、是否是单例的、是否懒加载、这个 Bean 依赖了哪些 Bean 等等。


## 容器

### Configuration 
```java
/**
 * spring容器 跟XML配置相同
 */
@Configuration()
public class DefaultConfig() {
    @Bean(name = "person")
    public Person person() {
        return new Person();
    }

}
```

### 加载Bean容器
> spring 常用加载容器有两种方式，xml方式和注解方式  
```java
public class DemoTest{
    public static void main(String[] args) {
        // 通过xml方式加载容器
        // ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("/application.xml")
        // 通过注解方式加载并获取容器
        ApplicationContext context = new AnnotationConfigApplicationContext();
        context.getBean("person");
        ...
    }
}
```
## 组件注解
### @Controller `接口层`
### @Service `业务层`
### @Rpository `数据层`
### @Component `自定义组件`
## 扫描包
### @ComponentScan
```java
// 单用注解会扫描该类所在所有字包
@ComponentScan
// 扫描指定路径下所有包
@ComponentScan("com.package.model")
public class ApplicationBase{

}
```

### @FactoryBean 自定义注入方式
```java
// 第一步 自定义一个工厂Bean
public class JamesFacotyBean implement FactoryBean<Target>{
    // 实现接口接口中的方法
    /**
     * 该方法是实例化Bean方法
     */
     @Override
    public Target getObject() throws Exception {
        return new Target();
    }
    /**
     * 返回对象的类对象
     */
    @Override
    public Class<?> getObjectType() {
        return Target.class;
    }
    /**
     * 返回对象是否是单例模式加载  
     */
    @Override
    public boolean isSingleton() {
        return true;
    }
}
// bean 工厂初始化对象
public class Target{
    public Target() {
        System.out.println("初始化 target Bean");
    }
}
// 容器配置对象
@Configuraction
public class ConfigurationTest{
    @Bean(name = "jamesFactoryBean")
    public JamesFactoryBean jamesFacotoryBean() {
        return new JamesFactoryBean();
    }
}
// 结果
public class DemoTest() {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigurationContext(ConfiguractionTest.class);
        // 获取 jamesFactoryBean 工厂中的Bean
        applicationContext.getBean("jamesFactoryBean");
        // 获取 jamesFactoryBean 对象
        applicationContext.getBean("&jamesFacotryBean");
    }
}
// 第二部
```

### @Conditional  Bean 注入过滤器
```java
@Bean(name = "person")
// WindowsConditional 是自定义的条件过滤器
@Conditional(WindowsConditional.class)
public Person person() {
    return new Person();
}

// 自定义条件过滤器
public class WindowsConditional implements Condition {
    /**
     * ConditionContext: 获取并注册的上下文
     * acotatedTypeMetadata: 注解的信息
     *
     */
    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
        ConfigurableListableBeanFactory beanFactory = conditionContext.getBeanFactory();
        // 获取系统信息
        Environment environment = conditionContext.getEnvironment();
        // 获取系统的名称
        String property = environment.getProperty("os.name");
        // 如果当前系统是 包含 mac 则注册这个Bean
        if(property.contains("Mac"))
            return true;
        return false;
    }
}
```

### ImportBeanDefinitionRegistrer 注册Bean的定义
```java
public class JamesImportBeanDefinitionRegistrer implements ImportBeanDefinitionRegistrer {
    @Override
    public void registerBeanDefinftions(AnnotationMetadata importingClassMetadata, BeanDefintionRegistry registry){
        RootBeanDefinition bean = new RootBeanDefinition();
        // ...
    }
 }
```

### Bean 的生命周期
> xml方式 <Bean id="beanName" class="com.**.**.BeanName"  initMethod = "初始化方法", destroyMethod = "销毁方法">  
> 注解方法  ``` @Bean(name="beanName", initMethod="初始化方法", destroyMethod="销毁方法")```

```java
// 注解方式2
public DemoBean {
    // 初始化方法
    @PostConstruct
    public void init() {
        System.out.printon("demo bean....@PostConstruct...")
    }

    // 销毁方法
    @PreDestory
    public void destory() {
        System.out.printon("demo bean....@PreDestory...")
    }
}

// 实现接口方式
@Component
public class DemoBean implements InitializingBean, DisposableBean {
    public DemoBean{
        System.out.println("demo bean....constructor....");
    }
    /**
     * Bean 销毁方法
     */
    @Override
    public void destroy() throws Exception {
        System.out.println("demo bean ..... destroy....");
    }

    /**
     * bean 初始化方法
     */
    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("demo bean .... init");
    }
}
```

### @Primary 和 @Autowired 优先级
> 如果@ComponentScan() 和 @Bean 两种注册方式 
> 未指定@Qualifile注解 @Qualifile注解 否则 @Primary 优先级高 

### @Autow
