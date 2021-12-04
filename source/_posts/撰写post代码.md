---
title: 撰写post代码
top_img: false
sticky: 1
tags:
  - hexo
  - butterfly
categories:
  - - 学习
  - - 工具
abbrlink: hexo-tools
date: 2021-12-01 20:52:31
---

为了更好地上手，这里放一些关于hexo的命令、markdown简单语法

# markdown简单语法

### 标题

```
'#' 加标题 为一级标题
'##' 加标题 为二级标题
'###' 加标题 为三级标题
```

### 行内标签
<span class="inline-tag red">public.js</span>
```html
<span class="inline-tag red">public.js</span>
```

### 引用块

<div class="snote idea yellow"><p>如何按版本号引用</p></div>

<div class="note success icon flat"><p>魔改亮点</p></div>

<div class="checkbox green checked"><input type="checkbox" checked=""><p>🆙主题可升级：不改动主题源码。即便魔改，也不需要为升级主题而烦恼。</p></div>


```html
<div class="snote idea yellow"><p>如何按版本号引用</p></div>
```

### 引用块

``` markdown
{% blockquote %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}
```

{% blockquote %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}

### 代码块

`行内代码块` 此处高亮为两个`中间部分

``` Java
public class HelloWorld{
    public static void main(String[] args){
        System.out.println("HelloWorld");
    }
}
```

### 图片

``` markdown
{% img [class names] 'https://cdn.jsdelivr.net/gh/pinknee/img-bed/img/bg.jpg' [width] [height] '"在文章中插入指定大小的图片" "alt text"' %}

```
{% img [class names] 'https://cdn.jsdelivr.net/gh/pinknee/img-bed/img/bg.jpg' [width] [height] '"在文章中插入指定大小的图片" "alt text"' %}

### sm.ms图床

![bg.jpg](https://cdn.jsdelivr.net/gh/pinknee/img-bed/img/bg.jpg)

### 视频

<video src='https://cdn.jsdelivr.net/gh/pinknee/img-bed/video/test.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>
</video>

<video src='https://cdn.jsdelivr.net/gh/pinknee/img-bed/video/xinjiekou.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>
</video>

<video src='https://cdn.jsdelivr.net/gh/pinknee/img-bed/video/foreverlove.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>
</video>



