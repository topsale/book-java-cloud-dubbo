# PageHelper 分页查询

---

## 定义接口

```
/**
 * 分页查询
 * @param page 页码（第几页）
 * @param pageSize 每页显示的笔数
 * @return
 */
public PageInfo<Article> page(int page, int pageSize);
```

## 实现接口

```
@Override
public PageInfo<Article> page(int page, int pageSize) {
    Example example = new Example(Article.class);
    PageHelper.startPage(page, pageSize);
    PageInfo<Article> pageInfo = new PageInfo<>(articleMapper.selectByExample(example));
    return pageInfo;
}
```

## 控制器代码

```
/**
 * 分页查询
 *
 * @return
 */
@ResponseBody
@RequestMapping(value = "data")
public DataTable<Article> data(HttpServletRequest request) {
    String strPage = request.getParameter("datatable[pagination][page]");
    String strPageSize = request.getParameter("datatable[pagination][perpage]");

    // 遍历客户端 POST 请求的参数
//        Enumeration<String> parameterNames = request.getParameterNames();
//        while (parameterNames.hasMoreElements()) {
//            String name = parameterNames.nextElement();
//            String value = request.getParameter(name);
//            System.out.println("name=" + name + "     value=" + value);
//        }

    int page = 1;
    int pageSize = 10;

    if (!StringUtils.isBlank(strPage)) {
        page = Integer.parseInt(strPage);
    }
    if (!StringUtils.isBlank(strPageSize)) {
        pageSize = Integer.parseInt(strPageSize);
    }

    PageInfo<Article> pageInfo = articleService.page(page, pageSize);
    return new DataTable<Article>(pageInfo);
}
```