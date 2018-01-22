# Docker Compose 安装

---

以下是在 Ubuntu 系统中安装 Docker Compose 的说明：

```
curl -L https://github.com/docker/compose/releases/download/1.17.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

以下是下载进度：

```
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   617    0   617    0     0    168      0 --:--:--  0:00:03 --:--:--   168
100 8649k  100 8649k    0     0  15651      0  0:09:25  0:09:25 --:--:-- 10383
```

给 `docker-compose` 添加执行的权限：

```
sudo chmod +x /usr/local/bin/docker-compose
```

查看 `docker-compose` 的版本

```
docker-compose version
```