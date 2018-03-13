# 创建 Solr 搜索实现项目

---

## 项目名称

gaming-server-service-search

## POM

增加 Solr 依赖

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-solr</artifactId>
</dependency>
```

完整 POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.ooqiu.gaming</groupId>
    <artifactId>gaming-server-service-search</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>gaming-server-service-search</name>
    <description></description>

    <parent>
        <groupId>com.ooqiu.gaming</groupId>
        <artifactId>gaming-server-dependencies</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <dependencies>
        <!-- Spring Boot Begin -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-solr</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!-- Spring Boot End -->

        <!-- Database Begin -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!-- Database End -->

        <!-- Alibaba Begin -->
        <dependency>
            <groupId>com.alibaba.boot</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
        </dependency>
        <!-- Alibaba End -->

        <!-- Zookeeper Begin -->
        <dependency>
            <groupId>com.101tec</groupId>
            <artifactId>zkclient</artifactId>
        </dependency>
        <!-- Zookeeper End -->

        <!-- Project Begin -->
        <dependency>
            <groupId>com.ooqiu.gaming</groupId>
            <artifactId>gaming-server-service-search-api</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>com.ooqiu.gaming</groupId>
            <artifactId>gaming-server-commons</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>
        <!-- Project End -->
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.ooqiu.gaming.service.</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

## Application

```
package com.ooqiu.gaming.service.search;

import com.alibaba.dubbo.container.Main;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan(basePackages = "com.ooqiu.gaming.service.search.mapper")
public class GamingServerServiceSearchApplication {
    public static void main(String[] args) {
        SpringApplication.run(GamingServerServiceSearchApplication.class, args);
        Main.main(args);
    }
}
```

## application.yml

```
spring:
  application:
    name: gaming-server-service-search
  data:
    solr:
      host: http://192.168.75.135:8983/solr
  datasource:
    druid:
      url: jdbc:mysql://192.168.75.132:3306/toutiao?useUnicode=true&characterEncoding=utf-8&useSSL=false
      username: root
      password: 123456
      initial-size: 1
      min-idle: 1
      max-active: 20
      test-on-borrow: true
      driver-class-name: com.mysql.jdbc.Driver

dubbo:
  scan:
    base-packages: com.ooqiu.gaming.service.search.api
  application:
    id: gaming-server-service-search
    name: gaming-server-service-search
  protocol:
    id: dubbo
    name: dubbo
  registry:
    id: zookeeper
    address: zookeeper://192.168.75.132:2181?backup=192.168.75.132:2182,192.168.75.132:2183
```

## application.properties

```
dubbo.protocol.port=20884
```

## Solr 测试类

主要用于初始化索引库

```
package com.ooqiu.gaming.service.search.test;

import com.ooqiu.gaming.server.domain.Article;
import com.ooqiu.gaming.service.search.mapper.ArticleMapper;
import org.apache.solr.client.solrj.SolrClient;
import org.apache.solr.client.solrj.SolrQuery;
import org.apache.solr.client.solrj.SolrServerException;
import org.apache.solr.client.solrj.response.QueryResponse;
import org.apache.solr.common.SolrDocument;
import org.apache.solr.common.SolrDocumentList;
import org.apache.solr.common.SolrInputDocument;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.io.IOException;
import java.util.List;
import java.util.Map;

@SpringBootTest
@RunWith(SpringRunner.class)
public class SolrTest {

    @Autowired
    private SolrClient solrClient;

    @Autowired
    private ArticleMapper articleMapper;

    /**
     * 添加索引库
     */
    @Test
    public void addDocument() throws IOException, SolrServerException {
        // 创建文档对象
        SolrInputDocument document = new SolrInputDocument();

        // 向文档中添加域
        document.addField("id", 152034783404107L);
        document.addField("article_title", "腾讯游戏安全中心：《绝地求生》《H1Z1》外挂举报已收到 静待国服上线");

        // 将文档添加到索引库
        solrClient.add(document);

        // 提交
        solrClient.commit("ik_core");
    }

    /**
     * 删除索引库
     */
    @Test
    public void delDocument() throws IOException, SolrServerException {
        solrClient.deleteByQuery("*:*");
        solrClient.commit("ik_core");
    }

    /**
     * 初始化索引库
     */
    @Test
    public void initDocument() throws IOException, SolrServerException {
        List<Article> articles = articleMapper.selectAll();

        for (Article article : articles) {
            SolrInputDocument document = new SolrInputDocument();

            document.addField("id", article.getId());
            document.addField("article_title", article.getTitle());
            document.addField("article_source", article.getSource());
            document.addField("article_introduction", article.getIntroduction());
            document.addField("article_url", article.getUrl());
            document.addField("article_cover", article.getCover());

            solrClient.add(document);
            solrClient.commit("ik_core");
        }
    }

    /**
     * 查询索引库
     */
    @Test
    public void queryDocument() throws IOException, SolrServerException {
        // 创建查询对象
        SolrQuery query = new SolrQuery();

        // 设置查询条件
        query.setQuery("演示");

        // 设置分页
        query.setStart(0);
        query.setRows(10);

        // 设置默认搜索域
        query.set("df", "article_keywords");

        // 设置高亮显示
        query.setHighlight(true);
        query.addHighlightField("article_title");
        query.setHighlightSimplePre("<span style='color:red;'>");
        query.setHighlightSimplePost("</span>");

        // 执行查询操作
        QueryResponse queryResponse = solrClient.query("ik_core", query);

        // 查询结果集
        SolrDocumentList results = queryResponse.getResults();

        // 获取高亮显示
        Map<String, Map<String, List<String>>> highlighting = queryResponse.getHighlighting();

        // 遍历结果集
        for (SolrDocument result : results) {
            String articleTitle = "";
            
            // 获取高亮显示
            List<String> list = highlighting.get(result.get("id")).get("article_title");
            if (list != null && list.size() > 0) {
                articleTitle = list.get(0);
            }
            else {
                articleTitle = (String) result.get("article_title");
            }

            System.out.println(articleTitle);
        }
    }
}
```

## 实现接口

```
package com.ooqiu.gaming.service.search.api.impl;

import com.alibaba.dubbo.config.annotation.Service;
import com.google.common.collect.Lists;
import com.ooqiu.gaming.service.search.api.SearchService;
import com.ooqiu.gaming.service.search.domain.SearchResult;
import org.apache.solr.client.solrj.SolrClient;
import org.apache.solr.client.solrj.SolrQuery;
import org.apache.solr.client.solrj.SolrServerException;
import org.apache.solr.client.solrj.response.QueryResponse;
import org.apache.solr.common.SolrDocument;
import org.apache.solr.common.SolrDocumentList;
import org.springframework.beans.factory.annotation.Autowired;

import java.io.IOException;
import java.util.List;
import java.util.Map;

@Service(version = DubboVersionConstant.DUBBO_VERSION_GAMING_SERVER_SERVICE_SEARCH)
public class SearchServiceImpl implements SearchService {

    @Autowired
    private SolrClient solrClient;

    @Override
    public List<SearchResult> search(String queryString, int page, int rows) {
        List<SearchResult> searchResults = Lists.newArrayList();

        // 创建查询对象
        SolrQuery query = new SolrQuery();

        // 设置查询条件
        query.setQuery(queryString);

        // 设置分页条件
        query.setStart((page - 1) * rows);
        query.setRows(rows);

        // 设置默认搜索域
        query.set("df", "article_keywords");

        // 设置高亮显示
        query.setHighlight(true);
        query.addHighlightField("article_title");
        query.setHighlightSimplePre("<span style='color:red'>");
        query.setHighlightSimplePost("</span>");

        try {
            // 执行查询操作
            QueryResponse queryResponse = solrClient.query("ik_core", query);
            SolrDocumentList solrDocuments = queryResponse.getResults();
            Map<String, Map<String, List<String>>> highlighting = queryResponse.getHighlighting();
            for (SolrDocument solrDocument : solrDocuments) {
                SearchResult result = new SearchResult();

                result.setId((String) solrDocument.get("id"));
                result.setArticle_cover((String) solrDocument.get("article_cover"));
                result.setArticle_introduction((String) solrDocument.get("article_introduction"));
                result.setArticle_source((String) solrDocument.get("article_source"));
                result.setArticle_url((String) solrDocument.get("article_url"));

                String articleTile = "";
                List<String> list = highlighting.get(solrDocument.get("id")).get("article_title");
                if (list != null && list.size() > 0) {
                    articleTile = list.get(0);
                } else {
                    articleTile = (String) solrDocument.get("article_title");
                }
                result.setArticle_title(articleTile);

                searchResults.add(result);
            }
        } catch (SolrServerException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

        return searchResults;
    }
}
```