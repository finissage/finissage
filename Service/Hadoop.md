hsdooop
## 处理海量数据
1. 存储
	- 
2. 运算


## 安装hadoop
### hadoop 文件目录
`bin` 启动或停止`Hadoop`相关服务
bin 对hadoop相关服务 进行操作的脚本
etc hadoop的配置文件
share hadoop的依赖jar包和文档。文档可以删除
lib hadoo的本地库


## 配置hadoop伪分布式

1. 修改 hadoop.env.sh
expore JAVA_HOME=

2. 修改 core-site.xml
```
<configuration>

	<!-- 配置 hdfs 的 namenode (老大)的地址 -->
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://localhost:9000</value>
	</property>

	<!-- 配置 hadoop 运行时产生数据的存储目录，不是临时数据 -->
	<property>
		<name>hadoop.tmp.dir</name>
		<value>/bigdata/tmp</value>
	</property>
</configuration>
```

3. 修改 hdfs-site.xml
```
<configuration>
	<!-- 数据保存多少份 -->
	<property>
		<name>dfs.replication</name>
		<value>1</value>
	</property>
</configuration>
```

4. 修改 mapred-site.xml
```
<configuration>
	<!-- 指定 mapreduce 运行在什么环境下 (yarn) -->
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
</configuration>
```

5. 修改 yarn-site.xml
```
<configuration>
	<!-- 指定 yarn 的老大 ResourceManager 的地址 -->
	<property>
		<name>yarn.resourcemanager.hostname</name>
		<value>localhost</value>
	</property>

	<!-- reducer 获取数据的方式 -->
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mareduct_shuffle</value>
	</property>
</configuration>
```

## 初始化

`/bin/hdfs namenode -format`

## 启动
```
/sbin/start-all.sh
# 或者
/sbin/start-dfs.sh
/sbin/start-yarn.sh
```

## 访问
`ip:50070`


## hdfs comment 
```
# 修改环境变量
## 永久修改
vim /etc/profile
# 添加
export HADOOP_HOME=/home/james/document/hadoop

PATH:$HADOOP_HOME/bin

```


