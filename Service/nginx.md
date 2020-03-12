# Nginx 教程

## nginx 目录结构
1. `./conf` 配置文件
2. `./html` 网页文件
3. `./logs` 日志文件
4. `./sbin` 主要二进制文件

## 启动 `nginx`
```shell
cd /usr/local/nginx
./sbin/nginx
```
如果没有出现错误提示，则启动成功了，
如果有错误提示，则根据错误提示解决，
常见的是端口 80 被占用

## nginx 命令参数

kill -USR2 （进程号）

####  nginx 
> 优雅停止：当进程彻底结束后关闭  
> 立即停止：立刻切断当前进程  s
```
nginx -t  # 测试配置是否正确
nginx -s reload  # 加载新的配置文件
nginx -s stop  # 立即停止
nginx -s quit  # 优雅的停止
nginx -s reopen  # 重新打开日志
```

## nginx 配置文件

#### nginx 虚拟主机
> 基于域名的虚拟主机
```
server {
	listen 80; 
	server_name localhost;
	localhost / {  
		root html/ddd;
		index index.html;
	}
}
```
