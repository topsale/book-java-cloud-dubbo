# 重启 WEB 应用容器

---

## 停止WEB应用容器

```
docker stop amazing_archimedes
```

## 已经停止的容器，我们可以使用命令 `docker start` 来启动

```
docker start amazing_archimedes
```

## 重启WEB应用容器

```
docker restart amazing_archimedes
```

## 查询全部容器

```
docker ps -a
```

## 查询最后一次创建的容器

```
docker ps -l
```