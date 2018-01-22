# Docker 安装 Tomcat

---

## 查找 Docker Hub 上的 Tomcat 镜像

```
root@UbuntuBase:/usr/local/docker/tomcat# docker search tomcat
NAME                           DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
tomcat                         Apache Tomcat is an open source implementa...   1550                [OK]                
dordoka/tomcat                 Ubuntu 14.04, Oracle JDK 8 and Tomcat 8 ba...   43                                      [OK]
tomee                          Apache TomEE is an all-Apache Java EE cert...   42                  [OK]                
davidcaste/alpine-tomcat       Apache Tomcat 7/8 using Oracle Java 7/8 wi...   21                                      [OK]
consol/tomcat-7.0              Tomcat 7.0.57, 8080, "admin/admin"              16                                      [OK]
cloudesire/tomcat              Tomcat server, 6/7/8                            15                                      [OK]
maluuba/tomcat7                                                                9                                       [OK]
tutum/tomcat                   Base docker image to run a Tomcat applicat...   8                                       
jeanblanchard/tomcat           Minimal Docker image with Apache Tomcat         8                                       
andreptb/tomcat                Debian Jessie based image with Apache Tomc...   7                                       [OK]
bitnami/tomcat                 Bitnami Tomcat Docker Image                     5                                       [OK]
aallam/tomcat-mysql            Debian, Oracle JDK, Tomcat & MySQL              4                                       [OK]
antoineco/tomcat-mod_cluster   Apache Tomcat with JBoss mod_cluster            1                                       [OK]
maluuba/tomcat7-java8          Tomcat7 with java8.                             1                                       
amd64/tomcat                   Apache Tomcat is an open source implementa...   1                                       
primetoninc/tomcat             Apache tomcat 8.5, 8.0, 7.0                     1                                       [OK]
trollin/tomcat                                                                 0                                       
fabric8/tomcat-8               Fabric8 Tomcat 8 Image                          0                                       [OK]
awscory/tomcat                 tomcat                                          0                                       
oobsri/tomcat8                 Testing CI Jobs with different names.           0                                       
hegand/tomcat                  docker-tomcat                                   0                                       [OK]
s390x/tomcat                   Apache Tomcat is an open source implementa...   0                                       
ppc64le/tomcat                 Apache Tomcat is an open source implementa...   0                                       
99taxis/tomcat7                Tomcat7                                         0                                       [OK]
qminderapp/tomcat7             Tomcat 7                                        0
```

这里我们拉取官方的镜像

```
docker pull tomcat
```

等待下载完成后，我们就可以在本地镜像列表里查到 REPOSITORY 为 tomcat 的镜像。


## 运行容器：

```
docker run --name tomcat -p 8080:8080 -v $PWD/test:/usr/local/tomcat/webapps/test -d tomcat
```

命令说明：

* -p 8080:8080：将容器的8080端口映射到主机的8080端口
* -v $PWD/test:/usr/local/tomcat/webapps/test：将主机中当前目录下的test挂载到容器的/test

查看容器启动情况

```
root@UbuntuBase:/usr/local/docker/tomcat/webapps# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
38498e53128c        tomcat              "catalina.sh run"   2 minutes ago       Up 2 minutes        0.0.0.0:8080->8080/tcp   tomcat
```

通过浏览器访问

![](/assets/微信截图_20171103174843.png)