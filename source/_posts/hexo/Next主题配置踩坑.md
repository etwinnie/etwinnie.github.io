---
title: Next主题配置踩坑
date: 2020-10-12 17:09:13
tags:
    - Next主题
    - hexo
    - Git
categories:
    - hexo
---

## git push到github后，访问主页为空白
**问题描述：**
- git push到github后，访问主页为空白
- Travis CI自动部署的log中提示WARN No layout


**原因：**
因为这个问题，清空了一遍github和本地仓库，折腾了大半天。最后发现是因为next主题也是一个repo，下载的next主题中有.git文件，导致上传git的时候被忽略了。
<!--more-->
![](https://pic1.zhimg.com/v2-ce360b5ee451fcd9ad24d6b10935dde8_b.jpg)
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
![image.png](https://i.loli.net/2020/10/12/1mcGCaKFXI93lQf.png)
**解决方案：**
修改主题中的配置文件 _config.yml中Valine配置，增加url属性。

![image.png](https://i.loli.net/2020/10/12/IEOtmFDQ2ReKo65.png)
![image.png](https://i.loli.net/2020/10/12/mNwuAjlSnvULGf4.png)

## 配置Gitalk
1. 参考官方文档进行配置[Gitalk - Next主题配置](https://theme-next.js.org/docs/third-party-services/comments.html#Gitalk)
2. Homepage URL与Authorization callback URL均为https不是http
3. 如果github.io绑定了域名的话，Authorization callback URL为自己申请的域名。
4. 要在线上环境才可以测试成功
5. 测试时，第一次需要登录一遍github授权后，再刷新后即可正常使用了。
6. 用来存储issue的repo需要在repo设置中打开issue功能。


## Next主题配置进阶教程
> [NexT主题进阶](https://blog.csdn.net/qq_41518277/article/details/101766036#_165)