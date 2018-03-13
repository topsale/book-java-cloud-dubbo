# Nginx 负载均衡配置

---

## 什么是负载均衡

负载均衡建立在现有网络结构之上，它提供了一种廉价有效透明的方法扩展网络设备和服务器的带宽、增加吞吐量、加强网络数据处理能力、提高网络的灵活性和可用性。

负载均衡，英文名称为 Load Balance，其意思就是分摊到多个操作单元上进行执行，例如 Web 服务器、FTP 服务器、企业关键应用服务器和其它关键任务服务器等，从而共同完成工作任务。

## Nginx 实现负载均衡

* nginx 作为负载均衡服务器，用户请求先到达 nginx，再由 nginx 根据负载配置将请求转发至 tomcat 服务器
* nginx 负载均衡服务器：192.168.75.145:80
* tomcat1 服务器：192.168.75.145:9090
* tomcat2 服务器：192.168.75.145:9091

## Nginx 配置负载均衡

修改 `/usr/local/docker/nginx` 目录下的 nginx.conf 配置文件：

```
user  nginx;
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;

    keepalive_timeout  65;
	
	upstream tomcat_server_pool{
		server 192.168.75.145:9090 weight=10;
		server 192.168.75.145:9091 weight=10;
	}

	server {
		listen 80;
		server_name admin.ooqiu.com;
		location / {
			proxy_pass http://tomcat_server_pool;
			index index.jsp index.html index.htm;
		}
	}
}
```

## 相关配置说明

```
# 定义负载均衡设备的 Ip及设备状态 
upstream myServer {   
    server 127.0.0.1:9090 down; 
    server 127.0.0.1:8080 weight=2; 
    server 127.0.0.1:6060; 
    server 127.0.0.1:7070 backup; 
}
```

在需要使用负载的 Server 节点下添加

```
proxy_pass http://myServer;
```

* upstream：每个设备的状态:
* down：表示当前的 server 暂时不参与负载 
* weight：默认为 1.weight 越大，负载的权重就越大。 
* max_fails：允许请求失败的次数默认为1.当超过最大次数时，返回 proxy_next_upstream 模块定义的错误 
* fail_timeout:max_fails 次失败后，暂停的时间。 
* backup：其它所有的非 backup 机器 down 或者忙的时候，请求 backup 机器。所以这台机器压力会最轻