# 第一个 Docker 应用程序

---

Docker 允许你在容器内运行应用程序，使用 docker run 命令来在容器内运行一个应用程序。

输出 Hello Docker：

```
lusifer@UbuntuBase:~$ docker run ubuntu:15.10 /bin/echo "Hello Docker"
Hello Docker
```

参数解释：

* docker：Docker 的二进制执行文件
* run：与前面的 docker 组合来运行一个容器
* ubuntu:15.10：指定要运行的镜像，Docker首先从本地主机上查找镜像是否存在，如果不存在，Docker 就会从镜像仓库 Docker Hub 下载公共镜像
* /bin/echo "Hello Docker"：在启动的容器里执行的命令

以上命令完整的意思可以解释为：Docker 以 ubuntu15.10 镜像创建一个新容器，然后在容器里执行 bin/echo "Hello Docker"，然后输出结果。