# 使用 iframe 展示功能页

## 引入选项卡插件

需要使用 `jquery-scrollbar` 和 `nth-tabs` 两个插件

![](/assets/Lusifer1519851345.png)

## 引用 CSS

```
<link href="/assets/plugins/jquery-scrollbar/jquery.scrollbar.css" rel="stylesheet" type="text/css" />
<link href="/assets/plugins/nth-tabs/css/nth.tabs.css" rel="stylesheet" type="text/css" />
```

## 引用 JS

```
<script src="/assets/plugins/jquery-scrollbar/jquery.scrollbar.js" type="text/javascript"></script>
<script src="/assets/plugins/nth-tabs/js/nth.tabs.js" type="text/javascript"></script>
<script src="/assets/plugins/nth-tabs.js" type="text/javascript"></script>
```

## 添加页签包装节点

```
<div id="editor-tabs" class="nth-tabs"></div>
```

注：本项目中是添加在页面的 `<div class="m-grid__item m-grid__item--fluid m-wrapper"></div>` 节点中

## 初始化 JS 代码

```
<script>
    $(function () {
        NthTabs.home("/home");
    });
</script>
```

## 调用 JS 代码

```
NthTabs.addTab('a', '频道管理', '/channel/list', true, true);
```

## 效果图

![](/assets/Lusifer1519851549.png)