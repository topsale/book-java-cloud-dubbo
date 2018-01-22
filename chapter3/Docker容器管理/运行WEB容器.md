# 运行 WEB 容器

---

前面我们运行的容器并没有一些什么特别的用处。

接下来让我们尝试使用 docker 构建一个 web 应用程序。

我们将在 docker 容器中运行一个 Python Flask 应用来运行一个web应用。

```
lusifer@UbuntuBase:~$ docker run -d -P training/webapp python app.py
d89a785788469da23529eebf70f1764611bb28818a8de7fa1ea85154327b807a
```

参数说明：

* `-d`：让容器在后台运行
* `-P`：将容器内部使用的网络端口映射到我们使用的主机上