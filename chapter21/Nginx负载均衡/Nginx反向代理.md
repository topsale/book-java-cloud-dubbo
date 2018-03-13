# Nginx 反向代理

---

## 什么是反向代理

通常的代理服务器，只用于代理内部网络对 Internet 的连接请求，客户机必须指定代理服务器,并将本来要直接发送到 Web 服务器上的 HTTP 请求发送到代理服务器中由代理服务器向 Internet 上的 Web 服务器发起请求，最终达到客户机上网的目的。

而反向代理（Reverse Proxy）方式是指以代理服务器来接受 Internet 上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给 Internet 上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。

![](/assets/nginx_proxy.png)

## Nginx 反向代理 Tomcat

### 需求

* 两个 tomcat 服务通过 nginx 反向代理
* nginx   服务器：192.168.75.145:80
* tomcat1 服务器：192.168.75.145:9090
* tomcat2 服务器：192.168.75.145:9091

### 启动 Tomcat 容器

启动两个 Tomcat 容器，映射端口为 9090 和 9091，docker-compose.yml 如下：

```
version: '3'
services:
  tomcat1:
    image: tomcat
    container_name: tomcat1
    ports:
      - 9090:8080

  tomcat2:
    image: tomcat
    container_name: tomcat2
    ports:
      - 9091:8080
```

### 配置 Nginx 反向代理

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
	
	# 配置一个代理即 tomcat1 服务器
	upstream tomcat_server1 {
		server 192.168.75.145:9090;
	}

	# 配置一个代理即 tomcat2 服务器
	upstream tomcat_server2 {
		server 192.168.75.145:9091;
	}

	# 配置一个虚拟主机
	server {
		listen 80;
		server_name admin.ooqiu.com;
		location / {
				# 域名 admin.ooqiu.com 的请求全部转发到 tomcat_server1 即 tomcat1 服务上
				proxy_pass http://tomcat_server1;
				# 欢迎页面，按照从左到右的顺序查找页面
				index index.jsp index.html index.htm;
		}
	}

	server {
		listen 80;
		server_name service.ooqiu.com;

		location / {
			# 域名 service.ooqiu.com 的请求全部转发到 tomcat_server2 即 tomcat2 服务上
			proxy_pass http://tomcat_server2;
			index index.jsp index.html index.htm;
		}
	}
}
```