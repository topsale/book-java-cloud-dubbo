# 查看 WEB 容器

---

使用 `docker ps` 来查看我们正在运行的容器

```
lusifer@UbuntuBase:~$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                     NAMES
d89a78578846        training/webapp     "python app.py"     2 minutes ago       Up 2 minutes        0.0.0.0:32771->5000/tcp   laughing_cori
```

这里多了端口信息

```
PORTS
0.0.0.0:32771->5000/tcp
```

Docker 开放了 `5000` 端口（默认 Python Flask 端口）映射到主机端口 `32771` 上。

这时我们可以通过浏览器访问WEB应用

![](/assets/微信截图_20171103141022.png)

我们也可以指定 `-p` 标识来绑定指定端口

```
lusifer@UbuntuBase:~$ docker run -d -p 5000:5000 training/webapp python app.py
9490b017fdf6b8c6415d5d01b925413c6ed2dd782566d251ad2bdad90263c3cb
lusifer@UbuntuBase:~$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
9490b017fdf6        training/webapp     "python app.py"     3 seconds ago       Up 2 seconds        0.0.0.0:5000->5000/tcp   amazing_archimedes
```