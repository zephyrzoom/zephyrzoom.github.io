---
layout: post
title: 游戏编程
date: 2017-09-09 14:54:00 +0800
---

游戏的架构分三层：游戏应用层、游戏逻辑层和游戏视图层
![game architecture](/image/game-arch.png)

游戏应用层负责如下图功能：
![application layer](/image/application-layer.png)

游戏逻辑层：
![game logic](/image/game-logic.png)

游戏视图层(人)：
![game view human](/image/game-view.png)

游戏视图层(AI)：
![game view AI](/image/ai-agent.png)

联机游戏网络架构：
![network architecture](/image/network-arch.png)

视频接口使用OpenGL，音频接口使用FMod

代码规范：每个大阔号单独占一行。

保持编码风格的一致:函数命名如驼峰式，函数名的定义如get find等，都有统一的含义无论用在哪里。

不要将重要的代码隐藏，比如在构造函数中进行远程连接。避免在构造函数和函数重载中进行重要的。操重要的操作应该显式调用。如果拷贝构造函数中有复制几十m的文件操作，可以设置为private，否则被人调用你的接口时会以为是很小的文件操作。

类的继承关系应该尽量平缓，深度控制在2到3层。

区分组成和继承，这只是展示给别人看的，编译器不关心。

虚函数重载层次太多会造成上层修改导致连锁反映。定义虚函数时应该使用模版设计模式将业务分开，比如update操作应该区分为aiupdate、moveupdate、collisionupdate，这样重载就分开了，他们之间的执行顺序都在上层定义。

使用纯虚函数做接口，使用接口有很多好处，可以屏蔽实现细节。

使用工厂模式，有序的构造复杂对象，还可以用buildxxx延迟构造时间。
