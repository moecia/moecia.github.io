---
layout:     post   				    # 使用的布局（不需要改）
title:      Unity项目文件结构		   	# 标题 
subtitle:             # 副标题
date:       2021-05-02 				# 时间
author:     Nathan 				# 作者
header-img: img/post-bg-unity-dev.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Unity开发
---

# Unity项目文件结构

## 引言

之前接手过一个Unity项目，这个项目文件结构极其糟糕（它甚至把代码放到了/Resources文件夹里)。于是在此期间读了一些Unity项目结构的相关文章，也参考了业界的朋友的建议，对项目结构重新组织了一下。这篇文章简单谈一下我对Unity项目文件结构的想法。

*TL;DL 为了让文件好找。*

当我还是学生的时候，项目通常都是一个人完成的，所以经常懒得整理就把文件都放在/Assets文件夹下。或做一些简单的文件分类再把文件按类别放置。由于开发者也就我一人，所以短期内的项目开发还是能找到各个文件的放置位置。这样的弊端显而易见，也就是当项目开发/维护的人数大于一人或项目长时间运作后，文件会变得难以查找。所以follow一个标准的文件结构来进行开发，会大大减少找文件的时间。

下面是我在之前的Unity项目用的文件结构，项目规模较小，所以这里的文件结构也仅做小项目参考。仅抛砖引玉，如果您有更好的思路，请评论赐教。


## 范例
```
Asset
├── 3rd:          Asset store素材
├── GameAssets:   团队美术素材
    ├── Animation
    └── Animator
    └── Audio
    └── FBXs
    └── Materials
    └── Pariticle Effects
    └── Skybox
    └── Textures
    └── UIs
    └── Video
    └── ...
├── Plugins
├── Prefabs
├── Resources
├── Scenes
├── Scripts:      我的代码框架用的MVC，之后的文章会详细讲一下这个框架
    ├── BLL:      业务逻辑相关代码，代码不继承于Monobehaviour class
    └── DAL:      数据交互相关代码
    └── View：    GameLoop controllers或UI Controllers，继承 Monobehaviour class
├── StreamingAssets
```
## 总结

总体而言没有绝对正确的项目结构，感兴趣的朋友可以阅读一下我的参考材料，每个项目都会有命名或者结构上的细微差别，但整体而言的方向不会差太多。(大型项目除外)

## 参考材料：
- https://zhuanlan.zhihu.com/p/64856900
- https://chowdera.com/2020/12/202012042254227956.html
- https://blog.csdn.net/lizhenxiqnmlgb/article/details/81450401
