---
title: hexo搭建博客(三) - 新建文档Markdown
date: 2020-10-09 21:52:52
tags:
    - hexo
    - blog
    - markdown
toc: true
category: 
    - hexo
---

# 新建文章的方法
(1)通过命令新建
```bash
$ hexo new 'title' # 新建文章
$ hexo new draft 'draftname' #新建草稿
```
（2）编辑Markdown文档
执行上述命令后，会在仓库文件夹下的/source/_posts中找到新增的Markdown文档。
<!--more-->
（3）生成，push

```bash
$ hexo g
$ hexo s
$ git add -A
$ git commit -m '说明'
$ git push origin master
```

## Markdown中增加代码块等其他标签块
参考链接：[Hexo 标签插件（Tag Plugins）](https://hexo.io/zh-cn/docs/tag-plugins)

# Markdowm添加图片 - 图床功能
- sm.ms：免费、稳定，未来的稳定性不可控。
- 微博图床：国内、速度不错，有点麻烦
- 七牛云：国外，稳定，要收费
- imgur：老牌稳定图床，国外服务器，慢
- 腾讯云、阿里云
- 图床工具：Picgo