---
title: 撰写post代码
top_img: false
sticky: 1
date: 2021-12-01 20:52:31
tags:
 - hexo
 - butterfly
categories:
 - [学习]
 - [工具]
---

为了更好地上手，这里放一些关于hexo的命令、markdown简单语法

# markdown简单语法

### 标题

``` bash
'#' 加标题 为一级标题
'##' 加标题 为二级标题
'###' 加标题 为三级标题
```

### 代码引用

{% blockquote %}
``` bash
here is code home
```
{% endblockquote %}

# 引用块

{% blockquote %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}

### 代码块

{% codeblock [title] [lang:language] [url] [link text] [additional options] %}
code snippet
{% endcodeblock %}

### 图片

{% img [class names] /img/bg.jpg [auto] [auto] '"图片描述" "alt text"' %}

### sm.ms图床

![bg.jpg](https://i.loli.net/2021/12/02/ap9thsPx1cW57yw.jpg)

### 视频

<video src='/video/test.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>
</video>



