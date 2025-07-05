---
title: "Github Pages设置Jekyll过程"
date: 2025-07-04 19:43:00 +0800
top: false
cover: false
password:
toc: true
mathjax: true
summary:
tags:
categories: github_page
---

### 配置Ruby 

因为Jekyll是用Ruby语言写的。参考[Jekyll官网教程](https://jekyllrb.com/docs/installation/)

---

### 安装jekyll

```bash
gem install jekyll bundler
```

打开githubio本地文件夹，在此路径下打开命令行执行如下命令：

```bash
jekyll new .
```

---

### 博客个性化配置
   通过修改_config.yml文件

   博客名称->titile

   你的邮箱->email

   你的博客地址-> url

   ...

   其余根据自己个人需求自主更改吧！

---

### 移除RSS
这个是通过自定义重载主题文件实现的，即把修改后的文件放在项目文件夹中

1. 找到原来的 home.html 文件
  
    Minima 是一个 gem-based 主题，默认主题文件在 gem 中。为了修改它，你需要复制并覆盖默认文件：

    在命令行中执行：

    ```bash
    bundle show minima
    ```
    你会看到类似这样的路径，例如：

    ```bash
    /your-path/.rbenv/versions/2.7.0/lib/ruby/gems/2.7.0/gems/minima-2.5.1
    ```

2. 复制并修改home.xml文件
  
    然后你可以把 **_layouts/home.html** 文件复制到你的githubpages项目文件夹中：

    找到类似以下内容并移除它：

    ```javascript
    <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | relative_url }}">via RSS</a></p>
    ```

3. 重新构建站点

    保存修改后，运行：
    ```bash
    bundle exec jekyll serve
    ```

