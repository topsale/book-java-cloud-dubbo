# Dockerfile

---

Dockerfile是一个包含用于组合映像的命令的文本文档。可以使用在命令行中调用任何命令。 Docker通过读取Dockerfile中的指令自动生成映像。

`docker build` 命令用于从Dockerfile构建映像。可以在 `docker build` 命令中使用 `-f` 标志指向文件系统中任何位置的Dockerfile。

```
docker build -f /path/to/a/Dockerfile
```

## Dockerfile文件说明

说明不区分大小写，但必须遵循建议使用大写字母的约定。

Docker 以从上到下的顺序运行 Dockerfile 的指令。为了指定基本映像，第一条指令必须是 `FROM`。

一个声明以`＃`字符开头则被视为注释。可以在Docker文件中使用`RUN`，`CMD`，`FROM`，`EXPOSE`，`ENV`等指令。

在这里列出了一些常用的说明。

### FROM

该指令用于设置后续指令的基本映像。有效的 Dockerfile 必须使用`FROM`作为其第一条指令。

```
FROM ubuntu
```

### MAINTAINER

指定镜像的作者

```
MAINTAINER <name>
```

### RUN

该指令用于执行当前映像的任何命令。

```
RUN /bin/bash -c 'echo "Hello World"'
```

### CMD

这用于执行映像的应用程序。应该以下列形式总是使用CMD

```
CMD ["executable", "param1", "param2"]
```

这是使用CMD的首选方法。Dockerfile文件中只能有一个CMD。如果使用多个CMD，则只会执行最后一个CMD。

例：`CMD [“/bin/echo”, “this is a echo test ”]`

### COPY

该指令用于将来自源的新文件或目录复制到目的地的容器的文件系统。

```
COPY abc/ /xyz
```

规则：

* `source`路径必须在构建的上下文之内。无法使用`COPY ../something /something`，因为docker构建的第一步是将上下文目录\(和子目录\)发送到 docker 守护程序。

* 如果`source`是目录，则会复制目录的全部内容，包括文件系统元数据。

### WORKDIR

_WORKDIR_用于为_Dockerfile_中的`RUN`，`CMD`和`COPY`指令设置工作目录。如果工作目录不存在，它默认将会创建。

我们可以在_Dockerfile_文件中多次使用`WORKDIR`。

备注：可以简单理解为 `cd` 命令，但是如果目录不存在它会自动创建。

## 构建镜像

我们使用命令 `docker build` ， 从零开始来创建一个新的镜像。为此，我们需要创建一个 Dockerfile 文件，其中包含一组指令来告诉 Docker 如何构建我们的镜像。

```
root@UbuntuBase:/usr/local/docker/ubuntu# cat Dockerfile 
FROM ubuntu
MAINTAINER Lusifer

RUN /bin/echo 'root:123456' |chpasswd
RUN useradd lusifer
RUN /bin/echo 'lusifer:123456' |chpasswd
RUN /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local

EXPOSE 22
EXPOSE 80

CMD /usr/sbin/sshd -D
```

每一个指令都会在镜像上创建一个新的层，每一个指令的前缀都必须是大写的。

第一条FROM，指定使用哪个镜像源

RUN 指令告诉docker 在镜像内执行命令，安装了什么。。。

然后，我们使用 Dockerfile 文件，通过 `docker build` 命令来构建一个镜像。

```
root@UbuntuBase:/usr/local/docker/ubuntu# docker build -t lusifer/ubuntu:latest .
Sending build context to Docker daemon  2.048kB
Step 1/9 : FROM ubuntu
latest: Pulling from library/ubuntu
ae79f2514705: Pull complete 
5ad56d5fc149: Pull complete 
170e558760e8: Pull complete 
395460e233f5: Pull complete 
6f01dc62e444: Pull complete 
Digest: sha256:506e2d5852de1d7c90d538c5332bd3cc33b9cbd26f6ca653875899c505c82687
Status: Downloaded newer image for ubuntu:latest
 ---> 747cb2d60bbe
Step 2/9 : MAINTAINER Lusifer
 ---> Running in bdfec0486691
 ---> 40da56c4c0ad
Step 3/9 : RUN /bin/echo 'root:123456' |chpasswd
 ---> Running in 5baae880ffce
 ---> bc898d1fc903
Step 4/9 : RUN useradd lusifer
 ---> Running in 83b9ca8c7e59
 ---> 0e52bfd4fdec
Step 5/9 : RUN /bin/echo 'lusifer:123456' |chpasswd
 ---> Running in 8e04b80de324
 ---> 229189d4fce2
Step 6/9 : RUN /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
 ---> Running in 36c09a3d36f4
 ---> dc2b33c0216e
Step 7/9 : EXPOSE 22
 ---> Running in 19b391acb5d8
 ---> 7a9a0480fb19
Step 8/9 : EXPOSE 80
 ---> Running in fed8dad6ac5f
 ---> 70ff9da03753
Step 9/9 : CMD /usr/sbin/sshd -D
 ---> Running in fa89f7d2f1dc
 ---> 6cdd9c6b840d
Removing intermediate container 8e04b80de324
Removing intermediate container 36c09a3d36f4
Removing intermediate container 19b391acb5d8
Removing intermediate container fed8dad6ac5f
Removing intermediate container fa89f7d2f1dc
Removing intermediate container bdfec0486691
Removing intermediate container 5baae880ffce
Removing intermediate container 83b9ca8c7e59
Successfully built 6cdd9c6b840d
Successfully tagged lusifer/ubuntu:latest
```

参数说明：

* -t：指定要创建的目标镜像名
* .：Dockerfile 文件所在目录，可以指定 Dockerfile 的绝对路径

使用 `docker images` 查看创建的镜像已经在列表中存在,镜像ID为 6cdd9c6b840d

```
root@UbuntuBase:/usr/local/docker/ubuntu# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
lusifer/ubuntu      latest              6cdd9c6b840d        3 minutes ago       122MB
lusifer/ubuntu      v2                  2642b4944b28        About an hour ago   137MB
ubuntu              latest              747cb2d60bbe        3 weeks ago         122MB
ubuntu              14.04               dea1945146b9        7 weeks ago         188MB
ubuntu              15.10               9b9cb95443b5        15 months ago       137MB
training/webapp     latest              6fae60ef3446        2 years ago         349MB
```

我们可以使用新的镜像来创建容器

```
root@UbuntuBase:/usr/local/docker/ubuntu# docker run -it lusifer/ubuntu /bin/bash
root@f68cc2d65679:/# id lusifer
uid=1000(lusifer) gid=1000(lusifer) groups=1000(lusifer)
```

从上面看到新镜像已经包含我们创建的用户 lusifer