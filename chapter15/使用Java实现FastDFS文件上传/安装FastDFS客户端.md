# 安装 FastDFS 客户端

---

## 克隆源码

```
git clone https://github.com/happyfish100/fastdfs-client-java.git
```

## 从源码安装

```
mvn clean install
```

## 在项目中添加依赖

### gaming-server-dependencies

```
<!-- FastDFS Begin -->
<dependency>
    <groupId>org.csource</groupId>
    <artifactId>fastdfs-client-java</artifactId>
    <version>${fastdfs.version}</version>
</dependency>
<!-- FastDFS End -->
```

### gaming-server-web-admin

```
<!-- FastDFS Begin -->
<dependency>
    <groupId>org.csource</groupId>
    <artifactId>fastdfs-client-java</artifactId>
</dependency>
<!-- FastDFS End -->
```