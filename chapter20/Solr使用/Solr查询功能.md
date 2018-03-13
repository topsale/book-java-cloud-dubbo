# Solr 查询功能

---

## 查询条件

![](/assets/Lusifer1520892126.png)

说明：

* q：查询条件，`*:*` 为查询所有域，单独查询某个域如：article_title:h1z1
* fq: 过滤条件
* sort：排序条件
* start,rows：分页条件
* fl：字段列表返回域，如只希望返回 id
* df：默认搜索域，如之前配置的复制域 article_keywords

## 高亮显示

![](/assets/Lusifer1520892901.png)

说明：上图意为在默认搜索域 `article_keywords` 中搜索关键字 `正式`  并指定需要高亮显示的结果域 `article_title` 以 `红色` 显示