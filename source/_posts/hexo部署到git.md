---
title: hexo搭建博客(二) - 部署到github
date: 2020-10-09 16:09:18
tags:
    - 环境搭建
    - hexo
    - git
    - blog
toc: true
category: 
    - hexo
---
# 使用Travis CI自动部署Hexo
参考链接：
[hexo部署到git](https://hexo.io/zh-cn/docs/github-pages)

我们将会使用 Travis CI 将 Hexo 博客部署到 GitHub Pages 上。Travis CI 对于开源 repository 是免费的，但是这意味着你的站点文件将会是公开的。
<!--more-->

1. 新建一个 repository。如果你希望你的站点能通过 <你的 GitHub 用户名>.github.io 域名访问，你的 repository 应该直接命名为 <你的 GitHub 用户名>.github.io。
2. 将你的 Hexo 站点文件夹推送到 repository 中。默认情况下不应该 public 目录将不会被推送到 repository 中，你应该检查 .gitignore 文件中是否包含 public 一行，如果没有请加上。
3. 将 Travis CI 添加到你的 GitHub 账户中。
4. 前往 GitHub 的 Applications settings，配置 Travis CI 权限，使其能够访问你的 repository。
5. 你应该会被重定向到 Travis CI 的页面。如果没有，请 手动前往。
6. 在浏览器新建一个标签页，前往 GitHub 新建 Personal Access Token，只勾选 repo 的权限并生成一个新的 Token。Token 生成后请复制并保存好。
7. 回到 Travis CI，前往你的 repository 的设置页面，在 Environment Variables 下新建一个环境变量，Name 为 GH_TOKEN，Value 为刚才你在 GitHub 生成的 Token。确保 DISPLAY VALUE IN BUILD LOG 保持 不被勾选 避免你的 Token 泄漏。点击 Add 保存。
8. 在你的 Hexo 站点文件夹中新建一个 .travis.yml 文件：

```yaml
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - master # build master branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: master
  local-dir: public
```
9. 将 .travis.yml 推送到 repository 中。Travis CI 应该会自动开始运行，并将生成的文件推送到同一 repository 下的 gh-pages 分支下
10. 在 GitHub 中前往你的 repository 的设置页面，修改 GitHub Pages 的部署分支为 gh-pages。
11. 前往 https://<你的 GitHub 用户名>.github.io 查看你的站点是否可以访问。这可能需要一些时间。

# 将本地仓库推送到github.io
## 初始化本地仓库
```bash
cd <定位到本地站点文件夹>     #定位到本地文件夹
pwd                     #查看当前文件夹位置
git init        #初始化，将文件夹变成Git可管理的仓库
git add .       #将所有文件提交到暂存区，由于是第一次提交，需要将所有文件都进行提交，如果一个一个的提交太麻烦，通过. 命令可以将所有文件都进行提交。
git commit -m 'the initial edition' #提交到版本库
git remote add origin https://github.com/etwinnie/etwinnie.github.io.git #将本地仓库与Github仓库关联
```
Github仓库地址，在即为刚刚新建的Github仓库地址，可在此处找到

![image.png](https://i.loli.net/2020/10/10/CjdOv7ZBeFEtzgr.png)

## github推送
```bash
git pull --rebase origin master # 第一次推送，要与云端仓库合并
git push -u origin master #进行推送，-u指将master分支全部推送
```

```bash
#定期维护
git add -A               #将文件的修改上传到暂存区
git commit -m '说明'      #提交到本地仓库
git push origin master   #推送到GitHub网站上
```

### 可能出现的问题
#### 源引用表达式 master 没有匹配> 

```bash
$ git pull origin master    #源引用表达式 master 没有匹配

$ git push --set-upstream origin master    #当前分支 master 没有对应的上游分支
```
![image.png](https://cdn.nlark.com/yuque/0/2020/png/268532/1602158629415-5ed75d91-0d7c-443b-a1e5-558a86e8e6ed.png
)

#### 有些文件没有被提交到github
查看一下本地仓库的.gitignore文件中，是否设置对应被忽略的文件夹。
public文件夹一般情况下不会同步上传到github，它是生成的html静态文件。

#### 完成本地仓库推送后报错 - 
![image.png](https://i.loli.net/2020/10/09/3dOR6leFY2GEzTy.png)
> 参考链接：[hexo部署记录及采坑经验](https://blog.csdn.net/LSH_Blog/article/details/105208743)

**1. 邮箱提示：Page build failure**
hexo自带的主题 landscape 的 REDEME.md 文件里面有错误的标签，于是直接删除掉了这个 README 文件。

**2. 删除后邮箱提示：Page build warning**
提示说jekyll 主题不被github pages支持。可能是在git push是忽略了.deploy_git文件，该文件是被linux系统隐藏的。解决方案，修改项目的_config.yml配置，将.nojekyll添加在inclde中。
![image.png](https://i.loli.net/2020/10/09/UVWHZmKzXJtBk6v.png)

**3. 访问用户名.github.io不显示博客页面（问题一）**
（1）添加gh-pages分支
master分支存储代码，gh-pages分支用来存放hexo g出来的public文件夹。Travis CI即自动化部署master分支中的代码，将其生成的public文件夹部署到gh-pages分支中。
> 参考链接：[一文教你使用GitHub Pages部署静态网页](https://zhuanlan.zhihu.com/p/69592043)

```bash
# 列出所有本地分支和远程分支，仓库默认在 master 分支
git branch -a
# 新建并切换到 gh-pages 分支
git checkout -b gh-pages
# 显示有变更的文件
git status
git add .
# 提交暂存区到仓库区，并添加代码提交信息
git commit -m 'first commit'

# 添加远程仓库
git remote add origin git@github.com:DesertsX/yulequan-relations-graph.git
# 把本地的 gh-pages 分支推送到 origin 服务器上
git push origin gh-pages

# 切换分支命令
git checkout <想到切换的分支>
# 删除分支
git branch -d <想删除的分支名称>

```

注意：.travis.yml文件中，Travis CI默认部署的就是public文件夹了。
![image.png](https://i.loli.net/2020/10/09/nbaT2pUdDY8XjwH.png)

（2）在仓库setting中，修改仓库的默认Github pages显示页面。
![image.png](https://i.loli.net/2020/10/09/Qt6JZvpImjM8seW.png)

**4. 访问用户名.github.io不显示博客页面（问题一）**
问题描述：更改主题为Next后，git push页面为空白，Travis CI提示 WARN No layout
![](https://pic1.zhimg.com/v2-ce360b5ee451fcd9ad24d6b10935dde8_b.jpg)

原因：主题文件夹中有.git文件，导致上传git的时候被忽略了。
解决办法：

```bash
rm -rf themes/主题文件名
git add .git commit -m "fix"
git clone -b master 主题
git地址 themes/主题文件名rm -rf themes/主题文件名/.git
git add .git commit -m "fix"
git push
```
![image.png](https://i.loli.net/2020/10/12/O5zsnLMTBbmAC4l.png)
> 参考链接：[文件夹因存在.git而无法提交到git的解决办法](https://www.cnblogs.com/reboot777/p/11164193.html)


因为这个小问题，完全清空github和本地仓库重新设置了一遍~ 