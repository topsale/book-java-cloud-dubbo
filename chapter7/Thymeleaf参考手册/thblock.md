# th:block

---

```
<table>
    <th:block th:each="user : ${users}">
    <tr>
        <td th:text="${user.login}">...</td> <td th:text="${user.name}">...</td>
    </tr>
    <tr>
        <td colspan="2" th:text="${user.address}">...</td> 
    </tr>
    </th:block>
</table>
```

推荐下面写法（编译前看不见）

```
<table>
    <tr>
        <td th:text="${user.login}">...</td>
        <td th:text="${user.name}">...</td> </tr>
        <tr>
        <td colspan="2" th:text="${user.address}">...</td>
    </tr>
    <!--/*/ </th:block> /*/--> 
</table>
```