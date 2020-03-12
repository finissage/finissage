# Hadoop hdfs 命令
#### 格式化文件系统
```
hdfs namenode -format
```
#### 查看文件系统
```shell
hdfs dfs -ls -R / 
# -ls 查看
# / 是文件系统的根
# -R 是递归查看
```

#### 上传文件
```shell
hdfs dfs -put test.java /
# -put 上传文件
# test.java 要上传的物理文件
# / 要上传到虚拟文件系统中的路径
```

#### 下载文件
```shell
hdfs dfs -get /test.java /home/james/document/files/test.java
# -get 下载文件
# /test.java 是指分布式文件系统上的文件
# /home/java/document/files/ 是要保存在物理机上的位置,如果指定到文件是指定的文件名，否则跟分布式上文件名称相同
```

#### 重命名/剪切文件
```shell
hdfs dfs -mv /test.java /test1.java
# -mv 剪切、重命名文件
# /test.java 剪切、重命名前的文件路径及文件名
# /test1.java 剪切、重命名后的文件路径及文件名
```

#### 复制文件
```shell
hdfs dfs -cp /test.java /logs/test.java
# cp 复制文件
# /test.java 文在所在分布式文件系统中的路径
# /logs/test.java 文件拷贝到的位置：指定名称就是指定名称。如果不指定文件名称就是原文件名称
```

#### 合并多个文件到本地
```shell
hdfs dfs -getmeger /logs/test.java /logs/demo.java new.java
# getmeger 合并多个文件到本地
# /logs/test.java /logs/demo.java 分布式系统上的多个文件
# new.java 合成后保存到本地的文件
```