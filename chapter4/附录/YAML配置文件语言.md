# YAML 配置文件语言

---

## 简介

YAML 是专门用来写配置文件的语言，非常简洁和强大，远比 JSON 格式方便。

YAML 语言的设计目标，就是方便人类读写。它实质上是一种通用的数据串行化格式。它的基本语法规则如下：

* 大小写敏感
* 使用缩进表示层级关系
* 缩进时不允许使用Tab键，只允许使用空格。
* 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可

```#``` 表示注释，从这个字符一直到行尾，都会被解析器忽略。

YAML 支持的数据结构有三种：

* 对象：键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）
* 数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）
* 纯量（scalars）：单个的、不可再分的值

## 对象

对象的一组键值对，使用冒号结构表示

```
animal: pets
```

## 数组

一组连词线开头的行，构成一个数组

```
- Cat
- Dog
- Goldfish
```

数据结构的子成员是一个数组，则可以在该项下面缩进一个空格

```
-
 - Cat
 - Dog
 - Goldfish
```

## 复合结构

对象和数组可以结合使用，形成复合结构

```
languages:
 - Ruby
 - Perl
 - Python 
websites:
 YAML: yaml.org 
 Ruby: ruby-lang.org 
 Python: python.org 
 Perl: use.perl.org 
```

## 纯量

纯量是最基本的、不可再分的值。以下数据类型都属于 JavaScript 的纯量

* 字符串
* 布尔值
* 整数
* 浮点数
* Null
* 时间
* 日期