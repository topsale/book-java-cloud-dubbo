# Thymeleaf 自定义标签

---

我们知道在 JSP 中，可以使用 JSTL 标签简化开发，并且 JSTL 还具备自定义标签的功能，那 Thymeleaf 如何实现自定义标签呢

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

方言类需要继承 Thymeleaf 的 `AbstractProcessorDialect`

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

        // 添加自定义标签处理器
        processors.add(new SysDictTagProcessor(dialectPrefix));
        processors.add(new StandardXmlNsTagProcessor(TemplateMode.HTML, dialectPrefix));
        return processors;
    }
}
```

## 创建标签处理器

标签处理器需要继承 Thymeleaf 的 `AbstractElementTagProcessor`

```
package com.ooqiu.gaming.server.web.admin.config.thymeleaf.tag;

import org.thymeleaf.context.ITemplateContext;
import org.thymeleaf.model.IModel;
import org.thymeleaf.model.IModelFactory;
import org.thymeleaf.model.IProcessableElementTag;
import org.thymeleaf.processor.element.AbstractElementTagProcessor;
import org.thymeleaf.processor.element.IElementTagStructureHandler;
import org.thymeleaf.templatemode.TemplateMode;

/**
 * 自定义标签
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
        // 创建将替换自定义标签的 DOM 结构
        IModelFactory modelFactory = iTemplateContext.getModelFactory();
        IModel model = modelFactory.createModel();

        // 需要替换的页面元素
        model.add(modelFactory.createOpenElementTag("div"));
        model.add(modelFactory.createText("Hello Thymeleaf Dialect"));
        model.add(modelFactory.createCloseElementTag("div"));

        // 利用引擎替换整个标签
        iElementTagStructureHandler.replaceWith(model, false);
    }
}
```

## 注入方言

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
     *
     * @return
     */
    @Bean
    public SysDialect sysDialect() {
        return new SysDialect();
    }
}
```

## 前台 HTML 中声明使用

增加命名空间配置：`xmlns:sys=""`

```
<!DOCTYPE html SYSTEM "http://www.thymeleaf.org/dtd/xhtml1-strict-thymeleaf-spring4-4.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org" xmlns:sys="">
```

## 使用标签

```
<sys:dict />
```

## 实例用法

详见：[项目实战-实现文章管理功能-自定义Thymeleaf字典标签](/chapter15/自定义Thymeleaf字典标签.md)