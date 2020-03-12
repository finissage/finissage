# SSH 

## ssh 介绍

## 远程登录（默认端口22）
```shell
# 安装ssh
sudo apt-get install openssh-server

# 远程登录
ssh 用户名@ip -p 22
ssh root@localhost
```

## 文件传输（scp)
> 该命令的前提是目标主机必须安装 `openssh-server`
```shell
# 将本地文件传输到远程主机
scp 本地文件路径 登录账户@远程ip:远程存在路径
scp ~/demo.jar root@192.168.0.41:~/demo.jar

# 将远程的文件传输到本地
scp 登录账户@远程ip:远程存在路径 本地文件路径
scp root@192.168.0.41:~/demo.jar ~/demo.jar 

# 传输文件夹
scp -r 本地文件路径 登录账户@远程ip:远程存在路径
scp -r ~/demo.jar root@192.168.0.41:~/demo.jar
# -r 递归
```

# SSH key 的生成和使用

## 检测是否已经生成sshkey
> 通常`sshkey`会在`~/`用户家目录下生成`.ssh`文件夹，如果有通常说明已经生成`sshkey`

## 生成 sshkey
> 输入以下命令会生成两个文件`id_rsa`和`id_rsa.pub`两个文件  
> `id_rsa.pub`为公钥，`id_rsa`为私钥  
> 使用 `cat` 查看公钥并使用  
```shell
ssh-keygen -t rsa 
```