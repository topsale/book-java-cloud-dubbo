# Maven 私服搭建

---

## 简介

在使用 Maven 打包项目时我们需要依赖自己或其他同事开发的项目，这些项目在 Maven 中央仓库里是不存在的，我们也不能每次都是以通过编译源码的方式将这些依赖安装到本地仓库中，此时就需要 Maven 私服来帮我解决项目依赖的管理问题

## docker-compose.yml

```
version: '3.1'
services:
  nexus:
    restart: always
    image: shifudao/nexus3
    container_name: nexus
    ports:
      - 8081:8081
    volumes:
      - /usr/local/docker/nexus/data:/nexus-data
```

## 登录控制台

地址：http://192.168.75.136:8081/

用户名：admin

密码：admin123

![](/assets/Lusifer1521047001.png)

## 设置代理仓库

pom.xml 中增加配置

```
<repositories>
    <repository>
        <id>maven-snapshots</id>
        <name>maven-snapshots</name>
        <url>http://192.168.75.136:8081/repository/maven-snapshots/</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <enabled>false</enabled>
        </releases>
    </repository>
</repositories>
```

## 部署管理

pom.xml 中增加配置

```
<distributionManagement>
    <snapshotRepository>
        <id>nexus</id>
        <name>Nexus Snapshot</name>
        <url>http://192.168.75.136:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
    <repository>
        <id>nexus</id>
        <name>Nexus Releases</name>
        <url>http://192.168.75.136:8081/repository/maven-releases/</url>
    </repository>
</distributionManagement>
```

## 配置 Maven 服务器认证

```
<server>
    <id>nexus</id>
    <username>admin</username>
    <password>admin123</password>
</server>
```

![](/assets/Lusifer1521052579.png)

## 部署工程到私服

```
mvn deploy
```

## 部署成功效果图

![](/assets/Lusifer1521052757.png)