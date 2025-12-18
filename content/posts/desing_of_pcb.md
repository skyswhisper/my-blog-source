---
title: "CH32v203f6p6开发板"
date: 2025-12-18T19:51:00+08:00
lastmod: 2025-12-18T19:51:00+08:00
draft: false                       # 是否草稿
author: "sky"                 # 优先使用此处的作者，覆盖全局配置
categories: ["项目作品"]
keywords: ["Hugo教程","博客搭建"]  # 针对搜索引擎的关键词
tags: ["开发板","pcb","ch32v203f6p6"]
weight: 0                         # 置顶权重：数字越小越靠前，0 为默认排序
description: "ch32v203f6p6开发板"
summary: "ch32v203f6p6开发板"

featuredImage: "/images/workplace.jpg" 
# 首页列表中显示的预览图 (如果不填，默认用上面的 featuredImage)
featuredImagePreview: "/images/design-of-pcb.png" 
# 封面图的标题/版权文字
featuredImageCaption: "图片来源:电脑截屏"

lightgallery: true                # 开启文章内图片的灯箱效果 (点击放大)
comment: true                     # 是否开启评论 (覆盖全局配置)
---
> 我的工位，我的制作大部分诞生于这里

- CH32v203f6p6是WCH(南京沁恒)基于青稞 32 位 RISC-V 设计的单片机（对，就是做CH340那家），较为准确的内部晶振和较小的体积并且采用TSSOP-20封装，使它可以优雅的放在较小的pcb板上，并且144mhz主频让它拥有了较强的处理能力，虽然引脚较少不过也算是可堪一用的水平，开发方式可采用官方SDK或者arduino等。

- 这单片机是我最近才发现的，当时想找一个**好焊接体积小价格低性能强开发简单**的单片机，于是找到了它，正好最近没什么事情就顺手画了个开发板准备测试一下。简单的电路，就是滤波电容重复了，摆放的位置也有点问题，不过都是小事，就不改了，不想焊16pinTYPE-C的可以直接不焊，用右侧排针直接供电也行，能用arduino编程非常的不错，据说官方sdk也很好用，有兴趣的可以试试。

- 以下是pcb资料，[详情请看这里](https://oshwhub.com/skywhispers/chv203f6p6_1)
![原理图](/images/design-of-pcb2.png "PCB原理图")
![pcb](/images/design-of-pcb1.png "pcb排线")
![pcb](/images/design-of-pcb.png "3D图")