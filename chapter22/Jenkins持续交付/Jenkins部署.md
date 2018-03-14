# Jenkins 部署

---

Lusifer 123456

## docker-compose.yml

```
version: '3.1'
services:
  jenkins:
    restart: always
    image: jenkins
    container_name: jenkins
    ports:
      - 8080:8080
      - 50000:50000
    environment:
      TZ: Asia/Shanghai
    volumes:
      - /usr/local/docker/jenkins/data:/var/jenkins_home
```

## 设置数据卷目录权限

```
chown -R 1000 /usr/local/docker/jenkins/data
```

## 部署成功效果图

![](/assets/Lusifer1521047141.png)