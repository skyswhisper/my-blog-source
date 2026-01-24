---
title: "CH32v203f6p6开发板"
date: 2025-12-18T19:51:00+08:00
lastmod: 2025-12-18T19:51:00+08:00
draft: false                       # 是否草稿
author: "sky"                 # 优先使用此处的作者，覆盖全局配置
#categories: ["项目作品"]
keywords: ["开发板","pcb","ch32v203f6p6"]  # 针对搜索引擎的关键词
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

- CH32v203f6p6是WCH(南京沁恒)基于青稞 32 位 **RISC-V** 设计的单片机（对，就是做**CH340**那家），较为准确的内部晶振和较小的体积并且采用TSSOP-20封装，使它可以优雅的放在较小的pcb板上，并且144mhz主频让它拥有了较强的处理能力，虽然引脚较少不过也算是可堪一用的水平，开发方式可采用官方SDK或者arduino等。

- 这单片机是我最近才发现的，当时想找一个**好焊接体积小价格低性能强开发简单**的单片机，于是找到了它，正好最近没什么事情就顺手画了个开发板准备测试一下。简单的电路，就是滤波电容重复了，摆放的位置也有点问题，不过都是小事，就不改了，不想焊16pinTYPE-C的可以直接不焊，用右侧排针直接供电也行，能用**arduino**编程非常的不错，据说官方sdk也很好用，有兴趣的可以试试。

- 以下是pcb资料，[详情请看这里](https://oshwhub.com/skywhispers/chv203f6p6_1)
![原理图](/images/design-of-pcb2.png "PCB原理图")
![pcb](/images/design-of-pcb1.png "pcb")


- 后来我准备用它做点东西，因为以前从来没接触过类似的，所以对我来说有点困难，一开始焊板子的时候发现电源灯死活不亮，ldo输出电压只有**1.05V**且发热巨大，但是用电压表量发现后面的电路并没有短路，以为是ldo的问题，连着换了三个都没解决，我赶紧停下把最后一个ldo输出引脚挑开了，发现输出电压正常，然后我通了个小电压让他一直发热用手摸，发现是芯片焊反了😅，整了一个多小时。

- 处理好之后准备用**mounriver stdio**写一个点灯，写好之后开始烧录，又他妈的烧录不上，检查确实是**RISC-V**模式，用电压表一测时钟线电压居然有**4.5V**😦~~不是哥们？~~，电压表一测是仿真器坏了😅，算了过几天再整吧。。。
![pcb](/images/design-of-pcb3.jpg "。。。")