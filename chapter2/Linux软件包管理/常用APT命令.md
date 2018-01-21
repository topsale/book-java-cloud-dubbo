# 常用 APT 命令

---

## 安装软件包

```
apt-get install packagename
```

## 删除软件包

```
apt-get remove packagename
```

## 更新软件包列表

```
apt-get update
```

## 升级有可用更新的系统（慎用）

```
apt-get upgrade
```

## 其它 APT 命令

### 搜索

```
apt-cache search package
```
### 获取包信息

```
apt-cache show package
```

### 删除包及配置文件

```
apt-get remove package --purge
```

### 了解使用依赖

```
apt-cache depends package
```

### 查看被哪些包依赖

```
apt-cache rdepends package
```

### 安装相关的编译环境

```
apt-get build-dep package
```

### 下载源代码

```
apt-get source package
```

### 清理无用的包

```
apt-get clean && apt-get autoclean
```

### 检查是否有损坏的依赖

```
apt-get check
```