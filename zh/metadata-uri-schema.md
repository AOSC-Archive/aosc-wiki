---
title: 元数据 URI 规范
description: 
published: true
date: 2020-06-03T16:50:59.089Z
tags: 
editor: undefined
---

GitHub
======
下载文件阶段需求分析
------

### 例子
https://github.com/jgraph/drawio/archive/master.zip
https://github.com/liberationfonts/liberation-fonts/files/4178407/liberation-fonts-ttf-2.1.0.tar.gz
https://github.com/jgraph/drawio/releases/download/v13.1.14/draw.war
https://github.com/jgraph/drawio/archive/v13.1.14.zip
https://github.com/jgraph/drawio/archive/v13.1.14.tar.gz

### 场景分类
不支持：
1. `https://github.com/xxx/yyy/files/${FILE_ID}/${FILE_NAME}` ，由于 `${FILE_ID}` 不可预测，视作版本间不稳定的 URL。

支持：
1. `https://github.com/xxx/yyy/archive/${CHECKOUT_ID}.tar.gz` archive 类型
2. `https://github.com/xxx/yyy/releases/download/${CHECKOUT_ID}/${FILE_NAME}` releases 类型

#### 数据条目
- `${VER}`:存储于元数据中。
- `${CHECKOUT_ID}`: checkout-mangle 规则 `${VER}` => `${CHECKOUT_ID}`（选用，默认相等）
- `${FILE_NAME}`: filename-pattern 规则 `${VER}` => `${FILE_NAME}`（releases 类型必填）

checkout-mangle 用于下载文件，可以考虑实现反向转换 checkout-demangle，用于检查更新。
也可进一步改进，设计一个表达式内描述双向转换（构造、解析）的方法。

URI 规范
-------
### 本体部分
方案一：
1. `github+archive:owner/repository`
2. `github+releases:owner/repository`

方案二：
1. `github:owner/repository/archive`
2. `github:owner/repository/releases`

### 参数部分
方案一：作为 URI 参数
- `github...:owner/repository...?checkout=v${VER}`
- `github...:owner/repository...?filename=repo-${VER}-linux.deb`
- `github...:owner/repository...?filename=repo-${VER}-linux.deb&checkout=v${VER}`

方案二：额外设置一个元数据，以某种方式拼接，例如沿用上述参数风格
- `DOWNLOAD_PARAM="checkout=v${VER}"`
- `DOWNLOAD_PARAM="filename=repo-${VER}-linux.deb&checkout=v${VER}"`

方案三：额外设置多个特定于 schema 的元数据
```
DOWNLOAD_CHECKOUT="v${VER}"
DOWNLOAD_GITHUB_FILENAME="repo-${VER}-linux.deb"
```
