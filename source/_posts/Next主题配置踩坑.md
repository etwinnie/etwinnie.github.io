---
title: Next主题配置踩坑
date: 2020-10-12 17:09:13
tags:
    - Next主题
    - Hexo
    - Git
categories:
    - Hexo
---

## git push到github后，访问主页为空白
**问题描述：**
- git push到github后，访问主页为空白
- Travis CI自动部署的log中提示WARN No layout
![](https://pic1.zhimg.com/v2-ce360b5ee451fcd9ad24d6b10935dde8_b.jpg)

**原因：**
因为这个问题，清空了一遍github和本地仓库，折腾了大半天。最后发现是因为next主题也是一个repo，下载的next主题中有.git文件，导致上传git的时候被忽略了。

**解决方案：**

```bash
rm -rf themes/主题文件名
git add .git commit -m "fix"
git clone -b master 主题
git地址 themes/主题文件名rm -rf themes/主题文件名/.git
git add .git commit -m "fix"
git push
```
![image.png](https://i.loli.net/2020/10/12/O5zsnLMTBbmAC4l.png)

> 参考链接：
> [文件夹因存在.git而无法提交到git的解决办法](https://www.cnblogs.com/reboot777/p/11164193.html)

## 集成Valine评论问题
Next官方文档相对来说已经比较详细了，此处记录一下遇到过的坑。
> [Next集成Valine评论系统](https://theme-next.js.org/docs/third-party-services/comments.html#Valine)
### 加载成功评论框，但无法提交
![image.png](https://i.loli.net/2020/10/12/y6eYsJk5MEx3fg9.png)

**解决方案：**
修改主题中的配置文件 _config.yml中Valine配置，增加url属性。

```yaml
# Valine
# For more information: https://valine.js.org, https://github.com/xCss/Valine
valine:
  enable: true
  appId: QDFGRy9PoFjzc6WqYi1aMvLw-gzGzoHsz # Your leancloud application appid
  appKey: RG34boH6ebkl76PpDLsYkyp7 # Your leancloud application appkey
  serverURLs: https://qdfgry9p.lc-cn-n1-shared.com  # When the custom domain name is enabled, fill it in here
  # 注意此处的url填写生成appId时候的url
  placeholder: Just go go # Comment box placeholder
  avatar: mm # Gravatar style
  meta: [nick, mail, link] # Custom comment header
  pageSize: 10 # Pagination size
  lang: 
    - en 
    - zh-CN 
    # Language, available values: en, zh-cn
  visitor: true # Article reading statistic
  comment_count: true # If false, comment count will only be displayed in post page, not in home page
  recordIP: false # Whether to record the commenter IP
  enableQQ: true # Whether to enable the Nickname box to automatically get QQ Nickname and QQ Avatar
  requiredFields: [] # Set required fields: [nick] | [nick, mail]

```
![image.png](https://i.loli.net/2020/10/12/94kdo6OqDWItGCA.png)

