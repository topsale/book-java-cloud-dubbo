# 创建 FastDFS 控制器

---

## 项目名称

gaming-server-web-admin

## 增加 FastDFS 相关配置

在 application.yml 中增加 FastDFS 相关配置

```
# FastDFS Begin
fastdfs.base.url: http://192.168.75.132:8888/
storage:
  type: fastdfs
  fastdfs:
    tracker_server: 192.168.75.132:22122
# FastDFS End
```

## 控制器代码

```
package com.ooqiu.gaming.server.web.admin.controller;

import com.google.common.collect.Maps;
import com.ooqiu.gaming.server.web.admin.config.fastdfs.StorageService;
import com.ooqui.gaming.server.commons.utils.MapperUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

/**
 * 文件上传控制器
 * <p>Title: UploadController</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/4 21:44
 */
@Controller
public class UploadController {

    @Value("${fastdfs.base.url}")
    private String FASTDFS_BASE_URL;

    @Autowired
    private StorageService storageService;

    @ResponseBody
    @RequestMapping("upload")
    public String upload(MultipartFile uploadFile, MultipartFile wangEditorH5File) {
        if (uploadFile != null) {
            String oName = uploadFile.getOriginalFilename();
            String extName = oName.substring(oName.indexOf(".") + 1);
            HashMap<String, Object> map = new HashMap<>();

            try {
                String uploadUrl = storageService.upload(uploadFile.getBytes(), extName);
                map.put("success", "上传成功");
                map.put("url", FASTDFS_BASE_URL + uploadUrl);
            } catch (IOException e) {
                map.put("error", 1);
                map.put("message", "上传失败");
            }

            return MapperUtils.mapToJson(map);
        } else if (wangEditorH5File != null) {
            String oName = wangEditorH5File.getOriginalFilename();
            String extName = oName.substring(oName.indexOf(".") + 1);

            Map<String, Object> map = Maps.newHashMap();

            try {
                String uploadUrl = storageService.upload(wangEditorH5File.getBytes(), extName);
                String url = FASTDFS_BASE_URL + uploadUrl;

                // 上传成功
                map.put("errno", 0);
                map.put("data", new String[]{url});
            } catch (IOException e) {
                // 上传失败
                map.put("errno", 1);
                map.put("message", "服务端错误");
            }

            return MapperUtils.mapToJson(map);
        }

        return "";
    }
}
```