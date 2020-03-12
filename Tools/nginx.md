# nginx

## 安装
1. 源码安装  
2. 源安装

### 目录结构
    conf: 配置文件
    html: 项目目录
    log: 日志文件
#### linux 下文件分布
配置文件：/etc/nginx/conf/
项目目录: /usr/share/nginx/html/
日志文件: /var/log/nginx/

## 配置文件
配置文件层级结构
main
    http
        server
            location
            location
        server
```
user nginx nginx; # nginx 运行用户和用户组
worker_precesses 1； # nginx 进程数，建议设置成CPU核心数。
error_log /var/log.nginx/error.log info; # 全局错误日志定义类型【debug|info|notice|warn|error】
pid /var/run/nginx.pid; # 进程文件
work_rlimit_nofile 1024; # 

events{
    use epoll; # 参考事件
}

http{
    i
}
```
## 