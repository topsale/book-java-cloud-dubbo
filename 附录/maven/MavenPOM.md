# Maven POM

---

POM 代表项目对象模型。它是 Maven 中工作的基本单位，这是一个 XML 文件。它始终保存在该项目基本目录中的 pom.xml 文件。

POM 包含的项目是使用 Maven 来构建的，它用来包含各种配置信息。

POM 也包含了目标和插件。在执行任务或目标时，Maven 会使用当前目录中的 POM。它读取POM得到所需要的配置信息，然后执行目标。部分的配置可以在 POM 使用如下：

* project dependencies
* plugins
* goals
* build profiles
* project version
* developers
* mailing list

创建一个POM之前，应该要先决定项目组(groupId)，它的名字(artifactId)和版本，因为这些属性在项目仓库是唯一标识的。

## POM 的例子

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
   http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>
   <groupId>com.lusifer</groupId>
   <artifactId>project</artifactId>
   <version>1.0</version>
<project>
```

要注意的是，每个项目只有一个POM文件

* 所有的 POM 文件要项目元素必须有三个必填字段: groupId，artifactId，version
* 在库中的项目符号是：groupId:artifactId:version
* pom.xml 的根元素是 project，它有三个主要的子节点。

| 节点       | 描述                                                                                                                                                         |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| groupId    | 这是项目组的编号，这在组织或项目中通常是独一无二的。 例如，一家银行集团com.company.bank拥有所有银行相关项目。                                                |
| artifactId | 这是项目的ID。这通常是项目的名称。 例如，consumer-banking。 除了groupId之外，artifactId还定义了artifact在存储库中的位置。                                    |
| version    | 这是项目的版本。与groupId一起使用，artifact在存储库中用于将版本彼此分离。 例如：com.company.bank:consumer-banking:1.0，com.company.bank:consumer-banking:1.1 |