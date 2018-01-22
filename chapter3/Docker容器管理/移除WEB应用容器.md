# 移除 WEB 应用容器

---

我们可以使用 `docker rm` 命令来删除不需要的容器：

```
lusifer@UbuntuBase:~$ docker rm 9490b017fdf6 5198a90e2406 d89a78578846 435db4b99464 76aafa3bb20d 0977407542d1
9490b017fdf6
5198a90e2406
d89a78578846
435db4b99464
76aafa3bb20d
0977407542d1
```

注意：删除容器时，容器必须是停止状态，否则会报错