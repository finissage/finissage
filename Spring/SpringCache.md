# SpringCache 缓存

## 作用
当我们在调用一个缓存方法时，会根据相关信息和返回结果作为一个键值对存放在缓冲中，等到下次利用利用同样的参数来调用该方法是将不再执行该方法，而是直接从缓存中获取结果进行返回。

## 启用方法(基于SpringBoot)
###  添加依赖  
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```  
###  添加一个配置类
* 使用`@EnableCaching`和`@Configuration`注解 
```java
@Configuration
@EnableCaching
public class CacheConfiguration {
    //...
}
```  
* 配置缓存数据源（Redis，Ehcache）
```java
@Bean
public CacheManager cacheManager() {
    SimpleCacheManager cacheManager = new SimpleCacheManager();
    cacheManager.setCaches(Collections.singletonList(new ConcurrentMapCache("models")));
    return cacheManager;
}
```
### 使用方式
三个主要的注解 
* `Cacheable` 最常用的注解，用户标注需要缓存的方法
* `CacheEcict` 用于清除缓存
* `CachePut` 用于仅存缓存

##### `Cachebale`
```java
@Cacheable(value = "models", key = "#testModel.name", condition = "#testModel.address !=  '' ")
public TestModel getFromMem(TestModel testModel) throws InterruptedException {
    TimeUnit.SECONDS.sleep(1);
    testModel.setName(testModel.getName().toUpperCase());
    return testModel;
}
```
* `value` (也可以使用`cacheNames`)：命名空间，表示存到那个缓存里。
* `key`:表示命名空间下唯一的`key`，使用[`SpEL`](SpEL)生成。
* `condition`: 表示在那种情况下才缓存结果(对应的还有`unless`，那种情况不缓存),同样使用[`SpEL`](SpEL)

##### `ChcheEcict`
```java
@CacheEvict(value = "models", allEntries = true)
@Scheduled(fixedDelay = 10000)
public void deleteFromRedis() {
}

@CacheEvict(value = "models", key = "#name")
public void deleteFromRedis(String name) {
}
```
* `value`(`cacheNames`): 同`Cacheable`,命名空间，表示存到那个缓存里。
* `allEntries`: 标记是否删除命名空间下所有缓存，默认为`false`
* `key`: 同`Cacheable`注解，代表需要删除的命名空间下唯一的缓存`key`

##### `CachePut`
```java
@CachePut(value = "models", key = "#name")
public TestModel saveModel(String name, String address) {
return new TestModel(name, address);
}
```

* `value`: 同上
* `key`: 
* `condition`(`unless`): 同上