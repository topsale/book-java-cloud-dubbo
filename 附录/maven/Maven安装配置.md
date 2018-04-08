# Maven 安装配置

---

想要安装 Apache Maven 在 Windows 系统上, 需要下载 Maven 的 zip 文件，并将其解压到你想安装的目录，并配置 Windows 环境变量。

注意：请尽量使用 JDK 1.8 及以上版本

## JDK 和 JAVA_HOME

确保已安装JDK，并 “JAVA_HOME” 变量已加入到 Windows 环境变量。

![](/assets/Lusifer1511451715.png)

## 下载 Apache Mave

下载地址：http://maven.apache.org/download.cgi

![](/assets/Lusifer1511451890.png)

下载 Maven 的 zip 文件，例如： apache-maven-3.5.2-bin.zip，将它解压到你要安装 Maven 的文件夹。假设你解压缩到文件夹 –  D:\apache-maven-3.5.2

![](/assets/Lusifer1511452022.png)

注意：在这一步，只是文件夹和文件，安装不是必需的。

## 添加 MAVEN_HOME

添加 MAVEN_HOME 环境变量到 Windows 环境变量，并将其指向你的 Maven 文件夹。

![](/assets/Lusifer1511452135.png)

## 添加到环境变量 - PATH

![](/assets/Lusifer1511452190.png)

## 验证

使用命令：```mvn -version```

输出：

```
C:\Users\Lusifer>mvn -version
Apache Maven 3.5.2 (138edd61fd100ec658bfa2d307c43b76940a5d7d; 2017-10-18T15:58:13+08:00)
Maven home: D:\apache-maven-3.5.2\bin\..
Java version: 1.8.0_152, vendor: Oracle Corporation
Java home: C:\Program Files\Java\jdk1.8.0_152\jre
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```