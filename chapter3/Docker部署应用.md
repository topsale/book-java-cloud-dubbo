# Docker 部署应用

---

* 上传应用到服务器
* 创建 Dockerfile

```
FROM tomcat
MAINTAINER Lusifer

ADD app.war /usr/local/tomcat/webapps/app.war
```

* 构建镜像

```
docker build -t lusifer/tomcat .
```

* 启动容器

```
docker run --name tomcat -p 8080:8080 lusifer/tomcat
```