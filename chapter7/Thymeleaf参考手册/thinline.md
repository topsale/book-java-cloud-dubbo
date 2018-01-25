# th:inline

---

th:inline 可以等于 text , javascript(dart) , none

## `text: [[…]]`

```
<p th:inline="text">Hello, [[#{test}]]</p>
```

## `javascript: /[[…]]/`

```
<script th:inline="javascript">
    var username = /*[[
        #{test}
    ]]*/;
    var name = /*[[
        ${param.name[0]}+${execInfo.templateName}+'-'+${#dates.createNow()}+'-'+${#locale}
    ]]*/;
</script>
```

```
<script th:inline="javascript">

/*<![CDATA[*/

    var username = [[#{test}]];

    var name = [[${param.name[0]}+${execInfo.templateName}+'-'+${#dates.createNow()}+'-'+${#locale}]];

/*]]>*/

</script>
```

## `adding code: /* [+…+]*/`

```
var x = 23;
/*[+
var msg = 'Hello, ' + [[${session.user.name}]]; +]*/
var f = function() {
...
```

## `removind code: /[- / and /* -]*/`

```
var x = 23;
/*[- */
var msg = 'This is a non-working template'; /* -]*/
var f = function() {
...
```