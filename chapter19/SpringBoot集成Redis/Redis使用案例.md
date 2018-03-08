# Redis 使用案例

---

为频道增加缓存功能

## 项目名称

gaming-server-service-channel

## 增加依赖

```
<dependency>
    <groupId>com.ooqiu.gaming</groupId>
    <artifactId>gaming-server-service-redis-api</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

## 关键代码

```
@Reference(version = DubboVersionConstant.DUBBO_VERSION_GAMING_SERVER_SERVICE_REDIS)
private RedisService redisService;

@Override
public List<Channel> selectAll() {
    try {
        // 查询缓存
        String json = (String) redisService.get("channelList");
        if (StringUtils.isNotBlank(json)) {
            return MapperUtils.json2list(json, Channel.class);
        }

        // 没有缓存数据则查询数据库
        else {
            List<Channel> list = channelMapper.selectAll();
            redisService.set("channelList", MapperUtils.obj2json(list), 3600);
            return list;
        }
    } catch (Exception e) {
        return null;
    }
}
```