---
layout:     post   				    # 使用的布局（不需要改）
title:      使用Firebase的消息推送		   	# 标题 
subtitle:             # 副标题
date:       2021-05-08 				# 时间
author:     Nathan 				# 作者
header-img: img/post-bg-unity-dev.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Unity开发
---

# 使用Firebase做消息推送

## 引言

之前用做消息推送走了不少弯路，主要Firebase自己的文档写的不太不清楚。这里记录一下我配置**安卓**的推送。

## 准备

1. Google账号
2. 梯子（如在墙内）
3. Unity
4. 可供测试的手机
5. Postman

## 步骤

前两部请参考Firebase的文档和配置视频，这里没什么坑，基本一遍能配置完成。

1. 参考下面的视频配置项目。
<iframe width="560" height="315" src="https://www.youtube.com/embed/A6du3DUTIPI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

2. 参考[该文档]("https://firebase.google.com/docs/cloud-messaging/unity/client")配置Unity环境。

3. build app到手机上。运行app获取Device Token(registration_ids)。这里可以通过Debug模式把Device Token print到unity console里。**注意只有手机端可以生成Device Token，PC上是无法生成的。**

4. 得到Device Token后，在Firebase控制台-Project Setting-Cloud Messaging下找到Server Key和SenderID。这些都将会用于设备注册和消息推送。

5. 打开Postman，call api注册设备。
Header参考下图：
- ![Hearder](/img/firebase-header.jpg)
这里把Authrozation那里把key=后面换成你的server key, project_id的值换成你的sender id

Body: 
- ![Body](/img/firebase-header.jpg)
这里的notification_key_name可以随意设置，通常这里可以设置成游戏中的用户名。 registration_ids数组里加入你需要注册的设备的Device token。

成功注册后，Response会返回一个token，这个token将会用于该设备的消息推送。

此外firebase api还支持remove操作，即为把注册的设备从推送列表中移除。把operation里的内容换成remove即可。

6. 推送消息
推送消息的url为
Header参考下图：
- ![Hearder](/img/send-notification-header.jpg)

Body: 
- ![Body](/img/send-notification-body.jpg)
这里to的替换成上面create api得到的token

如果成功，你的手机将会收到推送消息。