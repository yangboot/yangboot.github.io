---
title: 【git学习笔记】git配置代理
categories: git学习笔记
tags:
  - git
  - proxy
excerpt: git配置代理
date: 2020-12-16 21:58:36
img: '/images/git/git.png'
---
## git配置代理
### http

```shell
git config --global http.proxy 'socks5://127.0.0.1:7890'
```

### https

```shell
git config --global https.proxy 'socks5://127.0.0.1:7890'
```

### 取消代理

```shell
git config --global --unset http.https://github.com.proxy
```

