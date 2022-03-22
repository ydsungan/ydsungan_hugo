---
title: "使用Hugo平台搭建个人博客"
date: 2022-03-22T12:44:12+08:00
draft: false
tags:
  - 软件
categories:
  - Hugo
---

# 1 Windows 10 安装 Hugo

## 1.1 安装 Chocolatey 包管理器

windows 上需要使用 Chocolatey 作为包管理器，在安装 Hugo 之前需要在windows上安装 Chocolatey，以管理员方式运行PowerShell，输入以下指令自动安装 Chocolatey：

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

输入命令 “choco” 来检查安装是否成功。

## 1.2 安装 Hugo

接下来，输入以下命令来安装 Hugo：

```powershell
choco install hugo-extended -confirm
```

输入命令 “hugo help” 来检查安装是否成功。



# 2 搭建项目

## 2.1 建立项目

在 windows 的用户家目录下（C:\Users\username）输入以下命令（其中ydsungan为项目的名称）：

```powershell
hugo new site ydsungan
```

## 2.2 下载主题

在【https://themes.gohugo.io/】网站选择一个主题，进入主题的 Github 页面，拷贝地址，进入项目的themes目录，克隆该主题：

```
cd themes
git clone https://github.com/CaiJimmy/hugo-theme-stack.git
```

## 2.3 配置主题

在themes目录里有下载好的hugo-theme-stack文件夹，该文件夹里面的 exampleSite/config.ymal 就是配置文件，需要把该配置文件复制到项目的根目录下，直接覆盖根目录下的原有的配置文件（config.ymal 或者config.toml）。

如果根目录下有 config.toml 而没有 config.ymal 文件，那么把 config.toml 直接删除即可，把 exampleSite/config.ymal 直接黏贴在项目的根目录下。

## 2.4 添加内容

项目根目录下执行 *hugo new about.md* ，会在根目录下的 content 目录下生成一个about.md文件。该文件的默认内容为：

```
---
title: "About"
date: 2018-05-22T22:04:26+08:00
draft: true
---
```

draft：表示该文章是否为草稿，true表示为草稿状态，草稿状态下是对外不可见的，提交后需要改为true；

不过一般的博客文章都是放在根目录下的 */content/post/* 目录下的，执行 *hugo new post/test.md* 会创建一个test.md。

## 2.5 启动Hugo自带的服务器

使用以下命令即可启动一个Hugo服务器

```
 hugo server  --theme=hugo-theme-stack
```

启动后在浏览器输入 *http://127.0.0.1:1313* 即可。



## 2.6 生成静态网页

使用以下命令即可在根目录下的/public/里面生成静态的HTML

```
hugo --theme=hugo-theme-stack --baseUrl="http://ydsungan.com"
```



## 2.7 上传 Github 保存

在 Github 上新建一个仓库，把整个项目上传即可。然后在自己的服务器上拉取该项目即可。

## 2.8 配置自己服务器上的 Nginx

在自己的服务器上的 Nginx 的 conf/nginx.conf 文件里面配置如下即可：

```
location / {
            root   /usr/local/ydsungan_hugo/public;
            index  index.html index.htm;
        }
```

ydsungan_hugo 就是从 Github 拉取的项目，而静态HTML就生成在 public/ 目录下；输入命令 *./sbin/nginx -s reload* 重启 nginx 即可。现在可以在浏览器访问自己的服务器了。

