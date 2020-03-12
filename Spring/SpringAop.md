# Spring Aop  
1. 传入配置类，创建 `ioc` 容器
    1. 注册配置类，调用 `refresh()`刷新容器；
    2. `registerBeanPostProcessors(BeanFactory)`；注册bean的后置处理器来方便拦截 `bean` 创建
        1. 先获取 `ioc` 容器已经定义了的需要创建对象的所有 `BeanPostProcessor`
        2. 给容器中加入别的 `BeanPostProcessor`
        3. 优先注册实现了 `ProorityOrdered` 接口的 `BeanPostProcessor`
        4. 在给容器中注册实现了 `Ordered` 接口的 `BeanPostProcessor`
        5. 注册没有实现优先接口的 `BeanPostProcessor`
        6. 注册 BeanPostProcessor，实际上就是创建 `BeanPostProcessor` 对象，保存在容器中，创建 `initernalAutoProxyCreator` 的 `BeanPostProcessor`[`AnnotationAwareAspectJAutoProxyCreator`]
            1. 创建 `Bean` 的实例
            2. `populateBean`； 给 `bean` 的各种属性赋值
            3. `initializeBean`： 初始化 `bean`
                1. `invokeAwareMethods()`;处理 `Aware` 接口方法的回调
                2. `applyBeanPostProcessorsBeforeInitialication()`；应用后置处理器的 `postProcessBeforeInitialization()`
                3. `invokeInitMethos()`；执行自定义的初始化方法
                4. `applyBeanPostProcessorsAfterInitialization()`； 执行后置处理器的 `postProcessAgterInitialization()`；
            4. `BeanPostProcessor(AnnotationAwareAspectJAutoProxyCreator)` 创建成功-> `aspectJAdvisorsBuilder`
        7. 把 `BeanPostProcessor` 注册到 `BeanFactory` 中；`beanFactory.addBeanPostProcessor(postProcessor)`;

---  
    
***以上是创建并注册 AnnotationAwareAspectJAutoProxyCreator 的过程  ***

---