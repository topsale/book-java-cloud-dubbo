# Docker 私服搭建

---

## 简介

官方的 Docker Hub 是一个用于管理公共镜像的地方，我们可以在上面找到我们想要的镜像，也可以把我们自己的镜像推送上去。但是，有时候我们的服务器无法访问互联网，或者你不希望将自己的镜像放到公网当中，那么你就需要 Docker Registry，它可以用来存储和管理自己的镜像。

## 服务端

### docker-compose.yml

```
version: '3.1'
services:
  registry:
    image: registry
    restart: always
    container_name: registry
    ports:
      - 5000:5000
    volumes:
      - /usr/local/docker/registry/data:/var/lib/registry
```

### 测试服务端

浏览器访问：http://192.168.75.136:5000/v2/

![](/assets/Lusifer1520955730.png)

终端访问：`curl http://192.168.75.136:5000/v2/`

![](/assets/Lusifer1520955773.png)

## 客户端

### 修改配置文件

```
vi /lib/systemd/system/docker.service
```

增加配置属性

```
--insecure-registry 192.168.75.136:5000
```

![](/assets/Lusifer1520956575.png)

### 重启服务

```
systemctl daemon-reload
service docker restart
```

### 测试镜像上传

```
## 拉取一个镜像
docker pull nginx

## 查看全部镜像
docker images

## 标记本地镜像并指向目标仓库（ip:port/image_name:tag，该格式为标记版本号）
docker tag nginx 192.168.75.136:5000/nginx

## 提交镜像到仓库
docker push 192.168.75.136:5000/nginx
```

### 查看私服镜像

查看全部镜像

```
curl -XGET http://192.168.75.136:5000/v2/_catalog
```

![](/assets/Lusifer1520957219.png)

查看指定镜像

```
curl -XGET http://192.168.75.136:5000/v2/nginx/tags/list
```

![](/assets/Lusifer1520957242.png)

### 测试拉取私服镜像

先删除镜像

```
docker rmi nginx
docker rmi 192.168.75.136:5000/nginx
```

拉取镜像

```
docker pull 192.168.75.136:5000/nginx
```