# Docker Compose 使用

---

创建一个 `docker-compose.yml` 配置文件：

```
version: '3'
services:
  webapp:
    restart: always
    image: training/webapp
    container_name: webapp
    ports:
      - 5000:5000
```

参数说明：

* version：指定脚本语法解释器版本
* services：要启动的服务列表
  * webapp：服务名称，可以随便起名，不重复即可
    * restart：启动方式，这里的 `always` 表示总是启动，即使服务器重启了也会立即启动服务
    * image：镜像的名称，默认从 Docker Hub 下载
    * container\_name：容器名称，可以随便起名，不重复即可
    * ports：端口映射列列表，左边为宿主机端口，右边为容器端口

前台运行：

```
lusifer@UbuntuBase:/usr/local/docker/python$ docker-compose up
Creating network "python_default" with the default driver
Creating webapp ... 
Creating webapp ... done
Attaching to webapp
webapp    |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

后台运行：

```
lusifer@UbuntuBase:/usr/local/docker/python$ docker-compose up -d
Creating webapp ... 
Creating webapp ... done
lusifer@UbuntuBase:/usr/local/docker/python$
```

运行效果：

![](/assets/微信截图_20171104143438.png)