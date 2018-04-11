# 使用 Root 用户

---

在实际生产操作中，我们基本上都是使用超级管理员账户操作 Linux 系统，也就是 Root 用户，Linux 系统默认是关闭 Root 账户的，我们需要为 Root 用户设置一个初始密码以方便我们使用。

## 设置 Root 账户密码

```
sudo passwd root
```

## 切换到 Root

```
su
```

## 设置允许远程登录 Root

```
nano /etc/ssh/sshd_config

# Authentication:
LoginGraceTime 120
#PermitRootLogin without-password     //注释此行
PermitRootLogin yes                             //加入此行
StrictModes yes

重启服务
service ssh restart
```