# Dubbo Admin 管理控制台

---

管理控制台为内部裁剪版本，开源部分主要包含：路由规则，动态配置，服务降级，访问控制，权重调整，负载均衡，等管理功能。

## 克隆

```
git clone https://github.com/dubbo/dubbo-ops.git
```

## 打包

```
mvn clean package
```

## 配置

编辑 `dubbo-admin/WEB-INF/dubbo.properties` 配置文件

```
dubbo.registry.address=zookeeper://192.168.75.132:2181
dubbo.admin.root.password=root
dubbo.admin.guest.password=guest
```

## 启动 Tomcat

## 访问 Tomcat

```
http://localhost:8080/dubbo-admin/
```

## 登录账号

* 账号：root，密码：root
* 账号：guest，密码：guest

## 服务页面

![](/assets/Lusifer1517526838.png)