# Solr 维护功能

---

维护功能即对数据库的 CRUD 操作

## 添加索引库

```
{"id":152034732415540, "article_source":"我的头条", "article_title":"《H1Z1》在Steam正式发行后突遭大量差评 锁IP旧事再被提起"}
```

![](/assets/Lusifer1520891737.png)

## 测试查询

![](/assets/Lusifer1520891788.png)

## 删除索引库

设置文档类型为 XML

![](/assets/Lusifer1520892036.png)

### 根据 ID 删除

```
<delete>
    <id>152034732415540</id>
</delete>
<commit />
```

### 根据查询删除

```
<delete>
    <query>*:*</query>
</delete>
<commit/>
```