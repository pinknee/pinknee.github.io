---
title: markdown语法
top_img: false
cover: https://cdn.jsdelivr.net/gh/pinknee/img-bed/img/cover/markdown.jpg
sticky: 1
tags:
  - hexo
  - butterfly
  - markdown
categories:
  - [博客搭建]
abbrlink: markdown
date: 2021-12-01 20:52:31
---

为了更好地上手，这里放一些关于hexo的命令、markdown简单语法

## 编辑器选择

`Typora`

## 标题

```markdown
# 一级标题

## 二级标题

### 三级标题

注意！最多支持六级
```

## 粗体

**HelloWorld**

```markdown
**HelloWorld**
```

## 斜体

*HelloWorld*

```markdown
*HelloWorld*
```

## 粗体加斜体

***HelloWorld***

```markdown
***HelloWorld***
```

## 删除线

~~HelloWorld~~

```markdown
~~HelloWorld~~
```

## 引用

```markdown
>+空格
```

> 空格

## 图片

```markdown
![图片名](地址)
```

![示例](https://cdn.jsdelivr.net/gh/pinknee/img-bed/img/bg.jpg)
![示例](https://gitee.com/pinknee/img-bed/blob/master/favicon.png)
## 超链接

[Pink的博客](https://pinknee.cn/)

```markdown
[显示名称](实际地址)
```

## 表格

```markdown
|姓名|性别|生日|
|--|--|--|
|pinknee|男|1999.8.19|
|--|--|--|
```

| 姓名    | 性别 | 生日      |
| ------- | ---- | --------- |
| pinknee | 男   | 1999.8.19 |

## 代码

````markdown
```语言
代码区
```
````

```java
public class HelloWorld{
    public static void main(String[] args){
        System.out.println("HelloWorld");
    }
}
```

## 视频

```html
<video src='https://cdn.jsdelivr.net/gh/pinknee/img-bed/video/jinx.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>
```

<video src='https://cdn.jsdelivr.net/gh/pinknee/img-bed/video/jinx.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>









