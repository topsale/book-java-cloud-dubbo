# IDEA 安装 Lombok 插件

---

## 安装

* 点击：File --> Settings --> Plugins
* 搜索：Lombok
* 安装完成后重启 IDEA

![](/assets/Lusifer1512345603.png)

* 查看是否安装成功

![](/assets/Lusifer1512345786.png)

## 使用

### 引入 Lombok 依赖

```
<dependency>
 <groupId>org.projectlombok</groupId>
 <artifactId>lombok</artifactId>
 <version>1.16.18</version>
</dependency>
```

### 使用 @Data 注解简化 POJO

`@Data` 包含了 `@ToString`，`@EqualsAndHashCode`，`@Getter/@Setter` 和 `@RequiredArgsConstructor` 的功能

其他相关注解请自行查阅：http://jnb.ociweb.com/jnb/jnbJan2010.html

### 使用案例

```
@Data
public class ItemCatNode implements Serializable {
    @JsonProperty(value = "u")
    private String url;
    @JsonProperty(value = "n")
    private String name;
    @JsonProperty(value = "i")
    private List<?> item;
}
```

![](/assets/Lusifer1512346835.png)