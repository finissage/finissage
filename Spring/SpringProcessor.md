# Spring PostProcessor

spring 框架提供了几种 PostProcessor 接口用户建模对容器或者 bean 的后置处理器，他们定义了一些方法，这些方法在特定的实际会被调用，通过这种机制，框架自身或者应用开发人员有机会在不入侵容器或者 bean 核心逻辑的情况下未容器或者 bean 做针对某些特定方面的定制或者扩展：能力增强，属性设置，内容修改，对象代理，甚至直接替换整个 bean。spring 提供的 PostProcessor 接口有一下几种
1. BeanDefinitionRegistryPostProcessor - BeanDefinitionRegistry 后置处理器 - 容器级别
2. BeanFactoryPostProcessor - BeanFacroty后置处理器 - 容器级别  
3. BeanPostprocessor - Bean后置处理器 - Bean实例级别  

### 实际使用中又可细分为一下几类：
1. InstantiationAwareBeanPostProcessor
2. MergedBeanDefinitionPostProcessor
3. DestructionAwareBeanPostProcessor
4. SmartInstantiationAwareBeanPostProcessor
5. 一般 BeanPostProcessor

spring 框架自身提供这些 PostProcessor 的实现类，每个 PostProcessor 实现类分别有不同的关注点， Spring 利用这个PostProcessor 实现类完成了很多框架自身的任务，主要在容器启动和 Bean 获取阶段。另外，开发人员也可以实现自己的 PostProcessor 来狂战 Spring 容器或者 Bean 的能力，这里边尤其是通过自定义实现 BeanPostProcessor， 开发人员有机会对容器中所有的 Bean 做定制。