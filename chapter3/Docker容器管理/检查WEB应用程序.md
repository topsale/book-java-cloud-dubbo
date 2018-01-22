# 检查 WEB 应用程序

---

使用 `docker inspect` 来查看Docker的底层信息。它会返回一个 JSON 文件记录着 Docker 容器的配置和状态信息：

```
lusifer@UbuntuBase:~$ docker inspect amazing_archimedes 
[
    {
        "Id": "9490b017fdf6b8c6415d5d01b925413c6ed2dd782566d251ad2bdad90263c3cb",
        "Created": "2017-11-03T06:13:13.053498761Z",
        "Path": "python",
        "Args": [
            "app.py"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 71917,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2017-11-03T06:13:13.374235386Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:6fae60ef344644649a39240b94d73b8ba9c67f898ede85cf8e947a887b3e6557",
        "ResolvConfPath": "/var/lib/docker/containers/9490b017fdf6b8c6415d5d01b925413c6ed2dd782566d251ad2bdad90263c3cb/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/9490b017fdf6b8c6415d5d01b925413c6ed2dd782566d251ad2bdad90263c3cb/hostname",
        "HostsPath": "/var/lib/docker/containers/9490b017fdf6b8c6415d5d01b925413c6ed2dd782566d251ad2bdad90263c3cb/hosts",
        "LogPath": "/var/lib/docker/containers/9490b017fdf6b8c6415d5d01b925413c6ed2dd782566d251ad2bdad90263c3cb/9490b017fdf6b8c6415d5d01b925413c6ed2dd782566d251ad2bdad90263c3cb-json.log",
        "Name": "/amazing_archimedes",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {
                "5000/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "5000"
                    }
                ]
            },
...
```