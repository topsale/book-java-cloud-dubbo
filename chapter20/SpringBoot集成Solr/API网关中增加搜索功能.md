# API 网关中增加搜索功能

---

## 增加依赖

```
<dependency>
    <groupId>com.ooqiu.gaming</groupId>
    <artifactId>gaming-server-service-search-api</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

## 控制器代码

```
package com.ooqiu.gaming.server.api.controller.v1;

import com.alibaba.dubbo.config.annotation.Reference;
import com.ooqiu.gaming.server.api.dto.BaseResult;
import com.ooqiu.gaming.service.search.api.SearchService;
import com.ooqiu.gaming.service.search.domain.SearchResult;
import io.swagger.annotations.ApiImplicitParam;
import io.swagger.annotations.ApiOperation;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

/**
 * 搜索
 * <p>Title: SearchControllerV1</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/13 8:02
 */
@RestController
@RequestMapping(value = "${rest.path.api.v1}/search")
public class SearchControllerV1 {

    @Reference(version = DubboVersionConstant.DUBBO_VERSION_GAMING_SERVER_SERVICE_SEARCH)
    private SearchService searchService;

    /**
     * 搜索
     * @param query
     * @return
     */
    @ApiOperation(value = "搜索", notes = "搜索")
    @ApiImplicitParam(name = "query", value = "关键字", required = true, dataType = "String", paramType = "path")
    @RequestMapping(value = "{query}", method = RequestMethod.GET)
    public BaseResult search(@PathVariable(required = true) String query) {
        List<SearchResult> searchResults = searchService.search(query, 1, 10);
        return BaseResult.success(searchResults);
    }
}
```