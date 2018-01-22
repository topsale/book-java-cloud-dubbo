# 查看 WEB 应用容器的进程

---

我们还可以使用 `docker top` 来查看容器内部运行的进程

```
lusifer@UbuntuBase:~$ docker top amazing_archimedes 
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                71917               71897               0                   14:13               ?                   00:00:00            python app.py
```