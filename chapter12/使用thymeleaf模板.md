# 使用 thymeleaf 模板

---

我们可以使用 thymeleaf 的模板功能将顶部 CSS 与底部 JS 封装起来，方便重用

## 定义模板

在 `templates` 目录下新建 `includes` 目录，并创建需要的模板文件

定义模板：`<th:block th:fragment="header"></th:block>`

引用模板：`<th:block th:include="includes/header :: header"></th:block>`

## 顶部模板 header.html

语法：`<th:block th:fragment="header"></th:block>`

```
<!DOCTYPE html SYSTEM "http://www.thymeleaf.org/dtd/xhtml1-strict-thymeleaf-spring4-4.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org">
<th:block th:fragment="header">
    <meta charset="utf-8" />
    <meta name="description" content="Latest updates and statistic charts">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <link href="/assets/vendors/base/vendors.bundle.css" rel="stylesheet" type="text/css" />
    <link href="/assets/demo/default/base/style.bundle.css" rel="stylesheet" type="text/css" />
    <link href="/assets/custom/style.css" rel="stylesheet" type="text/css" />

    <link rel="shortcut icon" href="/assets/demo/default/media/img/logo/favicon.ico" />
</th:block>
```

## 底部模板 footer.html

```
<!DOCTYPE html SYSTEM "http://www.thymeleaf.org/dtd/xhtml1-strict-thymeleaf-spring4-4.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org">
<th:block th:fragment="footer">
<script src="/assets/vendors/base/vendors.bundle.js" type="text/javascript"></script>
<script src="/assets/demo/default/base/scripts.bundle.js" type="text/javascript"></script>
</th:block>
```