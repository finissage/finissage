# spring aware 接口解析

## Aware 接口介绍
    Spring 的依赖注入的最大亮点是所有的 Bean 对 Spring 容器的存在是没有意识的，我们可以将 Spring 容器换成其他的容器，Spring 容器中的 Bean 的耦合度因此也是极低的。
    但是我们实现开发中，我们缺经常要用到 Spring 容器本身的功能资源，所以 Spring 容器中的 Bean 此时就要意识到 Spring 容器的存在才能调用 Spring 锁提供的资源。我们通过 Spring 提供的已一系列 Spring Aware 来实现具体的功能。

### 常用的 Aware
BeanNameAware 获得到容器中的 Bean 的名称
BeanFactoryAware 获得当前 Bean Factory, 从而调用容器的服务
ApplicationContextAware 获取当前的 application contxt 从而调用容器的服务
MessageSourceAware 得到 message source 从而得到文本信息。
ApplicationEventPublisherAware 应用实践发布器，用于发布事件
ResourceLoaderAware 获取资源加载器，可以获得外部资源文件