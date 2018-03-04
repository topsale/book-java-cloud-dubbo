# 自定义 Thymeleaf 字典标签

---

## Maven

```
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring4</artifactId>
    <version>3.0.9.RELEASE</version>
</dependency>
<dependency>
    <groupId>nz.net.ultraq.thymeleaf</groupId>
    <artifactId>thymeleaf-layout-dialect</artifactId>
    <version>2.3.0</version>
</dependency>
```

## 创建方言类

```
package com.ooqiu.gaming.server.web.admin.config.thymeleaf.dialect;

import com.ooqiu.gaming.server.web.admin.config.thymeleaf.tag.SysDictTagProcessor;
import org.thymeleaf.dialect.AbstractProcessorDialect;
import org.thymeleaf.processor.IProcessor;
import org.thymeleaf.standard.StandardDialect;
import org.thymeleaf.standard.processor.StandardXmlNsTagProcessor;
import org.thymeleaf.templatemode.TemplateMode;

import java.util.HashSet;
import java.util.Set;

/**
 * Thymeleaf 方言：系统用
 * <p>Title: SysDialect</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/4 9:34
 */
public class SysDialect extends AbstractProcessorDialect {
    // 定义方言名称
    private static final String DIALECT_NAME = "Sys Dialect";

    public SysDialect() {
        // 设置自定义方言与“方言处理器”优先级相同
        super(DIALECT_NAME, "sys", StandardDialect.PROCESSOR_PRECEDENCE);
    }

    /**
     * 元素处理器
     * @param dialectPrefix 方言前缀
     * @return
     */
    @Override
    public Set<IProcessor> getProcessors(String dialectPrefix) {
        Set<IProcessor> processors = new HashSet<IProcessor>();

        // 添加自定义标签
        processors.add(new SysDictTagProcessor(dialectPrefix));
        processors.add(new StandardXmlNsTagProcessor(TemplateMode.HTML, dialectPrefix));
        return processors;
    }
}
```

## 创建字典标签处理器

```
package com.ooqiu.gaming.server.web.admin.config.thymeleaf.tag;

import com.ooqiu.gaming.server.domain.Dict;
import com.ooqiu.gaming.server.web.admin.utils.DubboContextUtils;
import com.ooqiu.gaming.service.admin.api.DictService;
import org.springframework.context.ApplicationContext;
import org.thymeleaf.context.ITemplateContext;
import org.thymeleaf.model.IModel;
import org.thymeleaf.model.IModelFactory;
import org.thymeleaf.model.IProcessableElementTag;
import org.thymeleaf.processor.element.AbstractElementTagProcessor;
import org.thymeleaf.processor.element.IElementTagStructureHandler;
import org.thymeleaf.spring4.context.SpringContextUtils;
import org.thymeleaf.templatemode.TemplateMode;

import java.util.List;

/**
 * 自定义字典标签
 * <p>Title: SysDictTagProcessor</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/4 10:52
 */
public class SysDictTagProcessor extends AbstractElementTagProcessor {

    // 标签名
    private static final String TAG_NAME = "dict";

    // 优先级
    private static final int PRECEDENCE = 10000;

    public SysDictTagProcessor(String dialectPrefix) {
        super(
                // 此处理器将仅应用于HTML模式
                TemplateMode.HTML,

                // 要应用于名称的匹配前缀
                dialectPrefix,

                // 标签名称：匹配此名称的特定标签
                TAG_NAME,

                // 将标签前缀应用于标签名称
                true,

                // 无属性名称：将通过标签名称匹配
                null,

                // 没有要应用于属性名称的前缀
                false,

                // 优先(内部方言自己的优先)
                PRECEDENCE
        );
    }

    /**
     * 处理自定义标签 DOM 结构
     *
     * @param iTemplateContext            模板页上下文
     * @param iProcessableElementTag      待处理标签
     * @param iElementTagStructureHandler 元素标签结构处理器
     */
    @Override
    protected void doProcess(ITemplateContext iTemplateContext, IProcessableElementTag iProcessableElementTag, IElementTagStructureHandler iElementTagStructureHandler) {
        // 获取 Spring 上下文
        ApplicationContext applicationContext = SpringContextUtils.getApplicationContext(iTemplateContext);

        // 注入字典
        DictService dictService = applicationContext.getBean(DubboContextUtils.class).getDictService();

        // 从标签读取属性值，这里的值是用来作为字典的查询参数
        String dictType = iProcessableElementTag.getAttributeValue("type");

        // 提交表单时的 name
        String dictName = iProcessableElementTag.getAttributeValue("name");

        // 元素的 class 样式
        String dictClass = iProcessableElementTag.getAttributeValue("class");

        // 根据类型查询出字典列表
        List<Dict> dictList = dictService.selectByType(dictType);

        // 创建将替换自定义标签的 DOM 结构
        IModelFactory modelFactory = iTemplateContext.getModelFactory();
        IModel model = modelFactory.createModel();

        // 这里是将字典的内容拼装成一个下拉框
        model.add(modelFactory.createOpenElementTag(String.format("select name='%s' class='%s'", dictName, dictClass)));
        for (Dict dict : dictList) {
            model.add(modelFactory.createOpenElementTag(String.format("option value='%s'", dict.getValue())));
            model.add(modelFactory.createText(dict.getLabel()));
            model.add(modelFactory.createCloseElementTag("option"));
        }
        model.add(modelFactory.createCloseElementTag("select"));

        // 利用引擎替换整合标签
        iElementTagStructureHandler.replaceWith(model, false);
    }
}
```

## 注入方言类

```
package com.ooqiu.gaming.server.web.admin.config.thymeleaf;

import com.ooqiu.gaming.server.web.admin.config.thymeleaf.dialect.SysDialect;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * Thymeleaf 方言配置
 * <p>Title: ThymeleafDialectConfig</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/4 10:57
 */
@Configuration
public class ThymeleafDialectConfig {

    /**
     * 系统方言
     * 主要作用有：
     * 1. 处理字典数据展示
     *
     * @return
     */
    @Bean
    public SysDialect sysDialect() {
        return new SysDialect();
    }
}
```

## HTML 中声明使用

增加命名空间配置：`xmlns:sys=""`

```
<!DOCTYPE html SYSTEM "http://www.thymeleaf.org/dtd/xhtml1-strict-thymeleaf-spring4-4.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org" xmlns:sys="">
```

## 使用标签

```
<div class="form-group m-form__group row">
    <label class="col-lg-3 col-form-label"> 文章类型 </label>
    <div class="col-lg-6">
        <sys:dict type="article_type" name="type" class="form-control m-input" />
    </div>
</div>
```

## 关于无法在标签处理类中注入 Dubbo 服务的解决办法

我们可以使用“曲线救国”的方式创建一个 Spring 的 `@Component` 组件，通过该组件实例化 Dubbo 服务即可

```
package com.ooqiu.gaming.server.web.admin.utils;

import com.alibaba.dubbo.config.annotation.Reference;
import com.ooqiu.gaming.service.admin.api.DictService;
import com.ooqui.gaming.server.commons.constant.DubboVersionConstant;
import org.springframework.stereotype.Component;

/**
 * Dubbo 上下文工具
 * 主要作用是在其它类里无法注入 Dubbo 服务时使用
 * <p>Title: DubboContextUtils</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/4 13:10
 */
@Component
public class DubboContextUtils {
    @Reference(version = DubboVersionConstant.DUBBO_VERSION_GAMING_SERVER_SERVICE_ADMIN)
    private DictService dictService;

    public DictService getDictService() {
        return dictService;
    }
}
```