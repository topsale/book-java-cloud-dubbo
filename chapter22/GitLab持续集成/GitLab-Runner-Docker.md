# GitLab Runner Docker

---

为了配置方便，我们使用 `docker` 来部署 `GitLab Runner`

## 环境准备

* 创建工作目录 `/usr/local/docker/runner`
* 创建构建目录 `/usr/local/docker/runner/environment`
* 下载 `jdk-8u152-linux-x64.tar.gz` 并复制到 `/usr/local/docker/runner/environment`

## Dockerfile

在 `/usr/local/docker/runner/environment` 目录下创建 `Dockerfile`

```
FROM gitlab/gitlab-runner
MAINTAINER Lusifer <topsale@vip.qq.com>

# 修改软件源
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse' > /etc/apt/sources.list && \
    echo 'deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse' >> /etc/apt/sources.list && \
    echo 'deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse' >> /etc/apt/sources.list && \
    echo 'deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse' >> /etc/apt/sources.list && \
    apt-get update -y && \
    apt-get clean

# 安装 Docker
RUN apt-get -y install apt-transport-https ca-certificates curl software-properties-common && \
    curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add - && \
    add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" && \
    apt-get update -y && \
    apt-get install -y docker-ce
COPY docker.service /lib/systemd/system/docker.service

# 安装 Docker Compose
WORKDIR /usr/local/bin
RUN wget https://raw.githubusercontent.com/topsale/resources/master/docker/docker-compose
RUN chmod +x docker-compose

# 安装 Java
RUN mkdir -p /usr/local/java
WORKDIR /usr/local/java
COPY jdk-8u152-linux-x64.tar.gz /usr/local/java
RUN tar -zxvf jdk-8u152-linux-x64.tar.gz && \
    rm -fr jdk-8u152-linux-x64.tar.gz

# 安装 Maven
RUN mkdir -p /usr/local/maven
WORKDIR /usr/local/maven
RUN wget https://raw.githubusercontent.com/topsale/resources/master/maven/apache-maven-3.5.3-bin.tar.gz
# COPY apache-maven-3.5.3-bin.tar.gz /usr/local/maven
RUN tar -zxvf apache-maven-3.5.3-bin.tar.gz && \
    rm -fr apache-maven-3.5.3-bin.tar.gz
COPY settings.xml /usr/local/maven/apache-maven-3.5.3/conf/settings.xml

# 配置环境变量
ENV JAVA_HOME /usr/local/java/jdk1.8.0_152
ENV MAVEN_HOME /usr/local/maven/apache-maven-3.5.3
ENV PATH $PATH:$JAVA_HOME/bin:$MAVEN_HOME/bin

WORKDIR /
```

## docker.service

在 `/usr/local/docker/runner/environment` 目录下创建 `docker.service`，用于配置加速器和仓库地址

* --registry-mirror=加速器地址
* --insecure-registry 仓库地址

```
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target docker.socket firewalld.service
Wants=network-online.target
Requires=docker.socket

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd -H fd:// --registry-mirror=https://jxus37ac.mirror.aliyuncs.com --insecure-registry 192.168.75.128:5000
ExecReload=/bin/kill -s HUP $MAINPID
LimitNOFILE=1048576
# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNPROC=infinity
LimitCORE=infinity
# Uncomment TasksMax if your systemd version supports it.
# Only systemd 226 and above support this version.
#TasksMax=infinity
TimeoutStartSec=0
# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
# kill only the docker process, not all processes in the cgroup
KillMode=process
# restart the docker process if it exits prematurely
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target
```

## settings.xml

在 `/usr/local/docker/runner/environment` 目录下创建 `settings.xml`，用于配置 `maven` 仓库地址

```
<?xml version="1.0" encoding="UTF-8"?>

<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  
  <pluginGroups>
    <!-- pluginGroup
     | Specifies a further group identifier to use for plugin lookup.
    <pluginGroup>com.your.plugins</pluginGroup>
    -->
  </pluginGroups>

  <proxies>
    <!-- proxy
     | Specification for one proxy, to be used in connecting to the network.
     |
    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>proxyuser</username>
      <password>proxypass</password>
      <host>proxy.host.net</host>
      <port>80</port>
      <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>
    -->
  </proxies>

  <servers>
    <!-- server
     | Specifies the authentication information to use when connecting to a particular server, identified by
     | a unique name within the system (referred to by the 'id' attribute below).
     |
     | NOTE: You should either specify username/password OR privateKey/passphrase, since these pairings are
     |       used together.
     |
    <server>
      <id>deploymentRepo</id>
      <username>repouser</username>
      <password>repopwd</password>
    </server>
    -->

    <!-- Another sample, using keys to authenticate.
    <server>
      <id>siteServer</id>
      <privateKey>/path/to/private/key</privateKey>
      <passphrase>optional; leave empty if not used.</passphrase>
    </server>
    -->
  </servers>

  <mirrors>
    <mirror>
      <id>maven-snapshots</id>
      <name>maven-snapshots</name>
      <url>http://192.168.75.128:8081/repository/maven-snapshots/</url>
      <mirrorOf>maven-snapshots</mirrorOf>
    </mirror>

    <mirror>
      <id>maven-public</id>
      <name>maven-public</name>
      <url>http://192.168.75.128:8081/repository/maven-public/</url>
      <mirrorOf>central</mirrorOf>
    </mirror>
  </mirrors>

  <profiles>
   <profile> 
      <id>maven-snapshots</id>  
      <repositories> 
        <repository> 
          <id>maven-snapshots</id>  
          <url>http://192.168.75.128:8081/repository/maven-snapshots/</url>  
          <releases> 
            <enabled>false</enabled> 
          </releases>  
          <snapshots> 
            <enabled>true</enabled> 
          </snapshots> 
        </repository> 
      </repositories> 
    </profile> 
    <profile> 
      <id>maven-public</id>  
      <repositories> 
        <repository> 
          <id>maven-public</id>  
          <url>http://192.168.75.128:8081/repository/maven-public/</url>  
          <releases> 
            <enabled>true</enabled> 
          </releases>  
          <snapshots> 
            <enabled>false</enabled> 
          </snapshots> 
        </repository> 
      </repositories> 
    </profile> 
  </profiles>
  
  <activeProfiles>
    <activeProfile>maven-snapshots</activeProfile>
    <activeProfile>maven-public</activeProfile>
  </activeProfiles>
</settings>
```

## docker-compose.yml

在 `/usr/local/docker/runner` 目录下创建 `docker-compose.yml`

```
version: '3.1'
services:
  gitlab-runner:
    build: environment
    restart: always
    container_name: gitlab-runner
    privileged: true
    volumes:
      - /usr/local/docker/runner/config:/etc/gitlab-runner
      - /var/run/docker.sock:/var/run/docker.sock
```

## 注册 Runner

```
docker exec -it gitlab-runner gitlab-runner register

# 输入 GitLab 地址
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://192.168.75.146:8080/

# 输入 GitLab Token
Please enter the gitlab-ci token for this runner:
1Lxq_f1NRfCfeNbE5WRh

# 输入 Runner 的说明
Please enter the gitlab-ci description for this runner:
可以为空

# 设置 Tag，可以用于指定在构建规定的 tag 时触发 ci
Please enter the gitlab-ci tags for this runner (comma separated):
deploy

# 这里选择 true ，可以用于代码上传后直接执行
Whether to run untagged builds [true/false]:
true

# 这里选择 false，可以直接回车，默认为 false
Whether to lock Runner to current project [true/false]:
false

# 选择 runner 执行器，这里我们选择的是 shell
Please enter the executor: virtualbox, docker+machine, parallels, shell, ssh, docker-ssh+machine, kubernetes, docker, docker-ssh:
shell
```

## 项目配置

### Dockerfile

```
FROM openjdk:8-jre

MAINTAINER Lusifer <topsale@vip.qq.com>

ENV APP_VERSION 1.0.0-SNAPSHOT
ENV DOCKERIZE_VERSION v0.6.0
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

RUN mkdir /app
WORKDIR /app
COPY leeshop-server-service-admin-$APP_VERSION.jar /app/app.jar
ENTRYPOINT ["dockerize", "-timeout", "5m", "-wait", "tcp://192.168.75.128:3306", "java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/app/app.jar"]
EXPOSE 20880
```

### docker-compose.yml

```
version: '3.1'
services:
  leeshop-server-service-admin:
    restart: always
    image: 192.168.75.128:5000/leeshop-server-service-admin
    container_name: leeshop-server-service-admin
    network_mode: "host"
```

### .gitlab-ci.yml

```
stages:
  - build
  - push
  - deploy
  - clean

build:
  stage: build
  script:
    - $MAVEN_HOME/bin/mvn clean package
    - cp target/leeshop-server-service-admin-1.0.0-SNAPSHOT.jar docker/
    - cd docker
    - docker build -t 192.168.75.128:5000/leeshop-server-service-admin .

push:
  stage: push
  script:
    - docker push 192.168.75.128:5000/leeshop-server-service-admin

deploy:
  stage: deploy
  script:
    - cd docker
    - docker-compose down
    - docker pull 192.168.75.128:5000/leeshop-server-service-admin
    - docker-compose up -d

clean:
  stage: clean
  script:
    - docker rmi $(docker images -q -f dangling=true)
```

