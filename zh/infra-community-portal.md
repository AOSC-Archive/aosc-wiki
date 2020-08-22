---
title: 门户网站
description: 门户网站维护说明
published: true
date: 2020-08-22T09:26:40.507Z
tags: infra
editor: markdown
---

# 目录结构

- 基本文件
    - `layouts/index.html` 站点首页
    - `layouts/404.html` 404 页面
    - `build.sh` 构建脚本
    - `config.yml` Hugo 配置文件

- 放置有 HTML 的目录
    - `contents/about`
    - `contents/downloads`
    - `contents/mail`
    - `contents/news`
    - `contents/people` 贡献者页面
    - `contents/repo`

- Hugo 数据文件目录
    - `data` 数据集
    - `assets/css` SCSS 样式文件
    - `layouts` 页面模板
    - `contents/news/post` 新闻文章

- 实用工具目录
    - `tools` 放置站点迁移使用的工具
    - `daemon` 镜像站信息生成器，提供了查询镜像站信息的 API

- 自动生成的目录
    - `public` 最终生成的目录
    - `assets/img/de-preview` 最终生成的略缩图
    - `resources/_gen` Hugo Pipe 生成的文件

# 在本地构建页面

安装 Hugo（如已安装可跳过此步）：

  - 如果你在使用 AOSC OS，你可以直接从软件仓库安装 Hugo：`sudo apt install hugo`。
  - 你也可以从 https://github.com/gohugoio/hugo/releases 获取编译好的版本。请务必下载增强版 `hugo_extended`。

站点构建与预览：

  - 执行 `build.sh` 生成站点。如果你对一些页面做了修改并希望预览效果，你可以执行 `hugo server` 并根据屏幕指示操作。
  
镜像站信息生成：

  - 首先确保你已经安装了 Python 3，接下来进入 `daemon/` 并在 `venv` 执行 `pip install -r requirements.txt`，然后执行 `python3 watcher.py`。

# 添加新闻页

## 使用 `hugo new`（推荐）

执行：

```hugo new -k posts content/news/posts/YYYY-mm-dd-title.md```

接下来使用你偏好的编辑器打开 `content/news/posts/YYYY-mm-dd-title.md` 并填入新闻内容。

## 手动添加

只需在 `content/news/posts` 目录下创建 `YYYY-mm-dd-title.md` 文件。

在填入新闻内容前，你需要参照下面的示例添加一个文件头：

```
---
categories:
  - news
title: "title"
date: 2006-01-02
important: false
---
```

请注意 `categories` 应该填写 `news` 和（或） `community`。

# 添加个人页

## 使用 `hugo new`（推荐）

如果你希望使用 Markdown 写作（请注意这种情况下在文件中添加的 HTML 行将不会生效）：

```hugo new -k people content/people/<preferred_name>.md```

如果你希望使用 HTML 写作：

```hugo new -k people content/people/<preferred_name>.html```

# Caveats for Hugo

1. The posts/personal pages cannot contain raw HTML code, the raw HTML code will be stripped away by Hugo's Markdown renderer as a security precaution. If you want to embed something, check out [Hugo's builtin shortcode explainer](https://gohugo.io/content-management/shortcodes/#use-hugos-built-in-shortcodes).
1. The posts/personal pages cannot contain templating syntax like `{{ $something }}`, they will be escaped automatically. If you want to use templating syntax, you probably need to use shortcodes, see the documentation above.

# Publishing Your Changes

Simply push your commits to `master` branch. The deployment of the website is automated, you can see [the process here](https://dev.azure.com/AOSC-Dev/aosc-portal-kiss.github.io/_build?definitionId=1&_a=summary). If you don't have the permission to do so, you may open a new PR instead.
