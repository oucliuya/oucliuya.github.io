---
title: vim 操作备忘
layout: post
date:   2019-05-29 10:08:00 +0800
categories: document
tag: 笔记
---
## 系统配置
配置完成之后，tab自动变成4个空格
```
" add by school1024.com
set ts=4
set softtabstop=4
set shiftwidth=4
set expandtab
set autoindent
```


## 旧文件的处理

### TAB 换成空格
```
:set ts=4
:set expandtab
:%retab!
```

### 空格换成 TAB
```
:set ts=4
:set noexpandtab
:%retab!
```