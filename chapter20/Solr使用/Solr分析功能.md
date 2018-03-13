# Solr 分析功能

---

## 修改 managed-schema 配置业务系统字段

需要用到的业务字段如下：

* 文章 ID
* 文章来源
* 文章标题
* 文章简介
* 文章链接
* 文章图片

由于 Solr 中自带 id 字段所以无需添加，其它字段需要手动添加 Solr 字段

```
# 字段域
<field name="article_source" type="text_ik" indexed="true" stored="true"/>
<field name="article_title"  type="text_ik" indexed="true" stored="true"/>
<field name="article_introduction" type="text_ik" indexed="true" stored="true" />
<field name="article_url" type="string" indexed="false" stored="true" />
<field name="article_cover" type="string" indexed="false" stored="true" />

# 复制域（Solr 的搜索优化功能，将多个字段域复制到一个域里，提高查询效率）
<field name="article_keywords" type="text_ik" indexed="true" stored="false" multiValued="true"/>
<copyField source="article_source" dest="article_keywords"/>
<copyField source="article_title" dest="article_keywords"/>
<copyField source="article_introduction" dest="article_keywords"/>
```

![](/assets/Lusifer1520889778.png)

## 复制配置到容器并重启

```
# 复制到容器
docker cp managed-schema solr:/opt/solr/server/solr/ik_core/conf

# 重启容器
docker-compose restart
```

## 分词效果图

![](/assets/Lusifer1520889921.png)