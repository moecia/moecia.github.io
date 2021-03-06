---
layout:     post   				    # 使用的布局（不需要改）
title:      Dust & Dirt		   	# 标题 
subtitle:   本科毕业设计          # 副标题
date:       2021-05-02 				# 时间
author:     Nathan 				# 作者
header-img: img/post-bg-dustdirt.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Unity
    - 作品集
---

# Dust & Dirt

你可曾想象过，当地球奄奄一息，只剩满地的黄沙会怎么样？

Dust & Dirt是我2016年完成的大四毕业设计，游戏的背景来源于小时候看到北京沙尘暴相关的新闻，以及《星际穿越》的故事背景。

D&D构建了一个末世。此时地表已经不适合人类生活，人们不得不生活在海洋下的"方舟"里。可是若干年后，方舟的资源和食物均告急。此时方舟便派出一些地表侦察队，在搜集资源同时也希望能找到地球正在复苏的痕迹。

游戏的系统设计源于玩过的一些生存游戏。个人认为其中对我影响最为深远的便是《The Long Dark》(漫漫长夜)，其中诸如建造系统，合成系统，气候系统均参考了该游戏。

虽然总体只开发了4个月，但是这款游戏可以说是我个人作品中完成度最高的了。。（我目前似乎都没有**独立**做完过完成度如此之高的游戏)。~~可能这就是我在托福和GRE都很拉跨的情况还能申上UCSC的Master？~~

根据目前的记忆，游戏完成了这些系统：
- 装备系统：
    - 图形界面的背包，背包物品可以整理。
    - 设计了30多种物品。种类有食物，合成原料等。
    - 设计了头，身子，脚，手四个装备栏，玩家可以装备保暖衣服和如枪支等武器
- 收集系统：
    - 游戏中见到的大部分物件都可以交互。
- 玩家状态：
    - 有体力值，饥饿值，口渴值，生命值。体力值会随着时间流逝降低上限（所以太累了会跑不动）饥饿和口渴也会随着时间下降。如果长时间不吃不喝会降低生命值。
    - 疾病/心情系统：吃生肉喝脏水会拉肚子、晚上冻太久了会感冒发烧，方舟上的家人死了会悲伤。，这些debuff会带来一系列负面效果如走路变慢，状态数值条上限降低等。
- 天气系统：
    - 每天的不同时刻气温会有变化（晚上冷白天热）
    - 随机生成的恶劣天气，如沙尘暴，有毒雾霾。（玩家碰到沙尘暴要找掩体躲避，碰到有毒雾霾需要戴上防毒面具）
- 合成系统：
    - 玩家可以合成各种装备，也可以建造帐篷和火堆
    - 玩家在帐篷里可以恢复体力
    - 玩家在火堆边上可以烹饪，吃下烹饪过的食物和烧开的水会不容易生病，数值回复的也会更多。保持火堆燃烧需要加入燃料。玩家亦可以选择烹饪工具如空罐头或者铁锅，这将会影响到烹饪速度。
- 任务系统：
    - 游戏的核心任务是给方舟收集到足够多的资源，于此同时也要保证家人不被饿死。~~所以游戏有设定给送货员贿赂以让他直接把资源送给家人。~~
- AI：
    - 野外有狼这一种生物可以狩猎，狼用了一个简单的状态机来控制其行为。

游戏的系统基本就是这些了，现在想想还是觉得自己能在4个月完成这么多内容还是挺不容易。~~貌似当时即是我作为独立开发者的巅峰。~~

由于时间太久远，项目本身也比较庞大，这里就不分享源码了。不过感兴趣的朋友可以交流交流。~~主要当时代码写的稀烂，很多还是用C的面向过程的编程思路。。~~

这里是演示视频和设计文档。

## Youtube：
<iframe width="560" height="315" src="https://www.youtube.com/embed/yj5oR5m4sEQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 中文设计文档
我找不到了:(

## 英文设计文档
[点我](https://github.com/moecia/moecia.github.io/blob/master/pdf/DustDirtDocs_EN.pdf)