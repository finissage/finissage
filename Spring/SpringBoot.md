
## SpringBoot 面试题

### 1. 什么是SpringBoot
	SpringBoot 是Spring开源组织的一个子项目

### 2. 为什么要用 SpringBoot
	快速开发
	开箱即用
	简化配置
	自动装配
	简单上手

### 3. SpringBoot 核心配置文件有哪些，他们的区别
	bootstrap
	application

### 4. SpringBoot 配置文件格式，他们的区别
	yml
	properties
	yml: 多行配置，层侧分明，看上去很直观「不支持@PropertySrouce读取」
	properties：单行配置，等号区别key和value

### 5. SprintBoot 核心注解。都是干什么用的
	1.@SpringBootApplication
	2.@SpringBootConfig
	3.@EnableConfigction
	4.@CompentScan

	SpringBootApplication 启动类，也是springboot 最核心的注解，包含了 EnableConfigcation 和 CompentScan 
	SpringBootConfig SpringBoot 的配置类
	EnableConfigcation 自动配置类，自动配置原理。。。
	CompentScan 组件扫描

### 6. 开启SpringBoot 的方式
	1. maven 继承 spring-boot-starter-parent
	2. maven 引入 spring-boot-dependenies
	
### 7. SpringBoot 是否可以独立运行。 如果可以，怎么运行
	可以 因为SpringBoot 默认继承了 Tomcat 和Jetty 等web容器
	1. maven 命令运行：mvn spring-boot:run
	2. 打成jar包运行：java -jar XXX.jar
	3. main方法运行

### 8. SpringBoot 自动装配原理
	核心是类 AutoConfigcationImportSelect.java
	核心配置是 Spring-boot-autoconfig 下 WETI-INF/spring.properties

### 9. 如何在SpringBoot 启动的时候加入代码
	1. 继承 ApplicationRunner 类（应用级别）
	2. 继承 CommendLineRunner 类 (命令级别)

### 10. SpringBoot 如何读取配置文件
	1. @ConfigcationProperties
	2. @Value
	3. @PropertySource
	4. @Env..

### 11. SpringBoot 支持哪些日志框架
	1. java unit log
	2. log4j
	3. log4j2
	4. lockback
	默认是 lockback 推荐使用xml log文件，灵活配置


### 12. SpringBoot 如何区分环境
	配置文件区分
	application.yml (主配置)
	application-dev.yml (开发环境)
	application-test.yml （测试环境）
	application-pord.yml （生产环境）
	指定配置：java -jar xxx.jar --spring.profiles.active=dev

### 13. SpringBoot 能不能导入 Spring项目，怎么操作
	可以 @ImportSource 导入spring 配置文件
### 14. 什么是 Starter， 