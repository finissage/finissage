# SpEL 表达式

`SpEL`表达式可基于上下文并通过使用缓存抽象，提供与`root`独享相关联的缓存特定的内置参数。
|名称|	位置|	描述|	示例|
|--|--|--|--|
|methodName	|root对象|	当前被调用的方法名|	`#root.methodname`|
|method	|root对象|	当前被调用的方法|	`#root.method.name`|
|target	|root对象|	当前被调用的目标对象实例|	`#root.target`|
|targetClass	|root对象|	当前被调用的目标对象的类|	`#root.targetClass`|
|args	|root对象|	当前被调用的方法的参数列表|	`#root.args[0]`|
|caches	|root对象|	当前方法调用使用的缓存列表|	`#root.caches[0].name`|
|Argument Name	|执行上下文|	当前被调用的方法的参数，如`findArtisan(Artisan artisan)`,可以通过`#artsian.id获得参数`|	`#artsian.id`|
|result	|执行上下文|	方法执行后的返回值（仅当方法执行后的判断有效，如 `unless cacheEvict`的`beforeInvocation=false）`|	`#result`|