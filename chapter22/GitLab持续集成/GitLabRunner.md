# GitLab Runner

---

## 简介

理解了上面的基本概念之后，有没有觉得少了些什么东西 —— 由谁来执行这些构建任务呢？
答案就是 GitLab Runner 了！

想问为什么不是 GitLab CI 来运行那些构建任务？

一般来说，构建任务都会占用很多的系统资源 (譬如编译代码)，而 GitLab CI 又是 GitLab 的一部分，如果由 GitLab CI 来运行构建任务的话，在执行构建任务的时候，GitLab 的性能会大幅下降。

GitLab CI 最大的作用是管理各个项目的构建状态，因此，运行构建任务这种浪费资源的事情就交给 GitLab Runner 来做拉！

因为 GitLab Runner 可以安装到不同的机器上，所以在构建任务运行期间并不会影响到 GitLab 的性能

## 安装

* 在目标主机上安装 GitLab Runner，这里的目标主机指你要部署的服务器
* Ubuntu 安装脚本：

```
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-ci-multi-runner/script.deb.sh | sudo bash
sudo apt-get update
sudo apt-get install gitlab-ci-multi-runner
```

## 注册 Runner

安装好 GitLab Runner 之后，我们只要启动 Runner 然后和 GitLab CI 绑定：

```
[root@iZbp1fmnx8oyubksjdk7leZ gitbook]# gitlab-ci-multi-runner register
Running in system-mode.                            
                                                   
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://192.168.75.146:8080/
Please enter the gitlab-ci token for this runner:
1Lxq_f1NRfCfeNbE5WRh
Please enter the gitlab-ci description for this runner:
[iZbp1fmnx8oyubksjdk7leZ]: deploy-gaming
Please enter the gitlab-ci tags for this runner (comma separated):
deploy
Whether to run untagged builds [true/false]:
[false]: true
Whether to lock Runner to current project [true/false]:
[false]: 
Registering runner... succeeded                     runner=P_zfkhTb
Please enter the executor: virtualbox, docker+machine, parallels, shell, ssh, docker-ssh+machine, kubernetes, docker, docker-ssh:
shell
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded! 
```

说明：

* gitlab-ci-multi-runner register：执行注册命令
* Please enter the gitlab-ci coordinator URL：输入 ci 地址
* Please enter the gitlab-ci token for this runner：输入 ci token
* Please enter the gitlab-ci description for this runner：输入 runner 名称
* Please enter the gitlab-ci tags for this runner：设置 tag
* Whether to run untagged builds：这里选择 true ，代码上传后会能够直接执行
* Whether to lock Runner to current project：直接回车，不用输入任何口令
* Please enter the executor：选择 runner 类型，这里我们选择的是 shell

CI 的地址和令牌，在 项目 --> 设置 --> CI/CD --> Runner 设置：

![](/assets/Lusifer1521043282.png)

## .gitlab-ci.yml

在项目工程下编写 .gitlab-ci.yml 配置文件：

```
stages:
  - install_deps
  - test
  - build
  - deploy_test
  - deploy_production

cache:
  key: ${CI_BUILD_REF_NAME}
  paths:
    - node_modules/
    - dist/

# 安装依赖
install_deps:
  stage: install_deps
  only:
    - develop
    - master
  script:
    - npm install

# 运行测试用例
test:
  stage: test
  only:
    - develop
    - master
  script:
    - npm run test

# 编译
build:
  stage: build
  only:
    - develop
    - master
  script:
    - npm run clean
    - npm run build:client
    - npm run build:server

# 部署测试服务器
deploy_test:
  stage: deploy_test
  only:
    - develop
  script:
    - pm2 delete app || true
    - pm2 start app.js --name app

# 部署生产服务器
deploy_production:
  stage: deploy_production
  only:
    - master
  script:
    - bash scripts/deploy/deploy.sh
```

上面的配置把一次 Pipeline 分成五个阶段：

* 安装依赖(install_deps)
* 运行测试(test)
* 编译(build)
* 部署测试服务器(deploy_test)
* 部署生产服务器(deploy_production)

设置 Job.only 后，只有当 develop 分支和 master 分支有提交的时候才会触发相关的 Jobs。

节点说明：

* stages：定义构建阶段，这里只有一个阶段 deploy
* deploy：构建阶段 deploy 的详细配置也就是任务配置
* script：需要执行的 shell 脚本
* only：这里的 master 指在提交到 master 时执行
* tags：与注册 runner 时的 tag 匹配

## 其它配置

为保证能够正常集成，我们还需要一些其它配置：

* 安装完 GitLab Runner 后系统会增加一个 gitlab-runner 账户，我们将它加进 root 组：

```
gpasswd -a gitlab-runner root
```

* 配置需要操作目录的权限，比如你的 runner 要在 gaming 目录下操作：

```
chmod 775 gaming
```

* 由于我们的 shell 脚本中有执行 git pull 的命令，我们直接设置以 ssh 方式拉取代码：

```
su gitlab-runner
ssh-keygen -t rsa -C "你在 GitLab 上的邮箱地址"
cd 
cd .ssh
cat id_rsa.pub
```

* 复制 id_rsa.pub 中的秘钥到 GitLab：

![](/assets/Lusifer1521043534.png)

* 通过 ssh 的方式将代码拉取到本地

## 测试集成效果

所有操作完成后 push 代码到服务器，查看是否成功：

![](/assets/clipboard1.png)

passed 表示执行成功

## 其他命令

删除注册信息：

```
gitlab-ci-multi-runner unregister --name "名称"
```

查看注册列表：

```
gitlab-ci-multi-runner list
```