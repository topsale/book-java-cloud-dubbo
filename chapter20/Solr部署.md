# Solr 部署

---

## docker-compose.yml

```
version: '3.1'
services:
  solr:
    image: solr
    restart: always
    container_name: solr
    ports:
      - 8983:8983
```

## 部署成功效果图

访问地址：http://192.168.75.135:8983/

![](/assets/Lusifer1512700142.png)