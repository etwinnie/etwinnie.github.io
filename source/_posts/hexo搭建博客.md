---
title: hexo搭建博客(一) - 环境搭建
date: 2020-10-09 12:43:35
tags: 
    - 环境搭建
    - hexo
    - blog
    - 网站
toc: true
category: 
    - hexo
---

# 本地环境搭建
参考链接：
[Hexo官方文档](https://hexo.io/zh-cn/docs/)

## 安装前提
1. Node.js (Node.js 版本需不低于 10.13，建议使用 Node.js 12.0 及以上版本)
2. Git
<!--more-->
### 查看是否安装成功Node.js与Git
```
> $ node -v
> v13.8.0
> $ git version
> git version 2.25.0
```

### Git安装（Mac系统）
#### 使用homebrew安装
1. 安装homebrew [homebrew官网](https://brew.sh/)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

2. 使用homebrew安装git

```bash
$ brew install git
```

> Git安装依赖Xcode Command Line Tools，在安装时会默认安装该工具。如果出现报错，需要手动安装。

### Node.js安装（Mac系统）
1. 使用homebrew安装

```bash
$ brew install npm
```

> 如果下载速度较慢可以考虑使用[淘宝Node.js镜像](https://npm.taobao.org/mirrors/node)
> 如果homebrew下载过慢，可以参考修改brew修改下载源的方法。

### 安装Hexo
#### 使用npm安装hexo

```bash
$ npm install -g hexo-cli
```

# 使用Hexo建站
(1) 执行以下命令，Hexo会在指定文件夹中新建所需要的文件。(（该文件夹是未来放本地站点的位置，也是未来的github本地仓库）

```bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```
(2) 新建完成后，指定文件夹的目录如下：
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
(3) 启动本地服务器，访问本地站点：http://localhost:4000/，可查看到hexo默认站点样式。

```bash
$ hexo generate
$ hexo server
```

# 更改Hexo主题
1. 找到主题的github地址。
2. cd到本地的themes文件夹，使用git clone命令进行本地复制。
3. 修改项目config.yml文件关联主题。
4. hexo g生成，hexo s启动服务器并访问。

```bash
$ git clone https://github.com/litten/hexo-theme-yilia
```


```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: hexo-theme-Annie
```


> 有些主题应用访问时，会出现乱排的情况。使用主题时，要读清楚作者的说明。有些需要一些插件，或者配置。
更多主题可以查看：[更多主题](https://hexo.io/themes/)

# 可能的问题
## 更改npm的下载源
> 参考链接：[npm 切换镜像站点](https://www.runoob.com/w3cnote/npm-switch-repo.html)
## 下载hexo最新版本
### 执行如下命令

```bash
npm install hexo@5
```
### no such file or directory, open '/Users/anna/package.json'

```bash
#执行如下代码行即可，再重新安装包
npm init
```
### npmWARN
npm安装模块的 npm WARN root@1.0.0 No description 和 npm WARN root@1.0.0 No repository filed 


解决办法：修改根目录下的package.json文件

```json
'description': 'npm-install package'
'private' = true
```

![npm WARN](https://img-blog.csdnimg.cn/20200807164314974.png)
![增加discription与private](https://img-blog.csdnimg.cn/20200807163453649.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0paZXZpbg==,size_16,color_FFFFFF,t_70)
