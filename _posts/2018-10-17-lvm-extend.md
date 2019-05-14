---
layout: post
title: lvm虚拟磁盘扩容
date: 2018-10-17 16:41:00 +0800
---

1. `lsblk`查看分配的新磁盘，例如`/dev/sdb`
2. `pvcreate /dev/sdb`创建物理卷
3. `vgscan`扫描已经创建的卷组，例如查到有`vg_newlab`组
4. `vgextend vg_newlab /dev/sdb`用新建的物理卷扩展卷组
5. `lvextend -l +100%FREE /dev/mapper/lv_root`扩展逻辑卷`lv_root`
6. `resize2fs /dev/mapper/lv_root`重新计算逻辑卷大小