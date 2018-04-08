# Maven 常用命令

---

## 清除产生的项目

```
mvn clean
```

## 编译源代码

```
mvn compile
```

## 打包

```
mvn package
```

## 只打包不测试（跳过测试）

```
mvn -dmaven.test.skip=true
```

## 安装到本地仓库

```
mvn install
```

## 源码打包

```
mvn source:jar
或
mvn source:jar-no-fork
```