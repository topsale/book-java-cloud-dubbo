# 查看 WEB 应用日志

---

docker logs \[ID或者名字\] 可以查看容器内部的标准输出：

```
lusifer@UbuntuBase:~$ docker logs -f amazing_archimedes 
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
192.168.75.1 - - [03/Nov/2017 06:14:05] "GET / HTTP/1.1" 200 -
192.168.75.1 - - [03/Nov/2017 06:14:05] "GET /favicon.ico HTTP/1.1" 404 -
192.168.75.1 - - [03/Nov/2017 06:16:57] "GET / HTTP/1.1" 200 -
192.168.75.1 - - [03/Nov/2017 06:16:57] "GET /favicon.ico HTTP/1.1" 404 -
```

参数说明：

* `-f`：让 dokcer logs 像使用 tail -f 一样来输出容器内部的标准输出

从上面，我们可以看到应用程序使用的是 5000 端口并且能够查看到应用程序的访问日志。