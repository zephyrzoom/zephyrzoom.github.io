---
layout: post
title: Django预加载初始化禁用自动重载
date: 2018-08-01 19:00:00 +0800
---

Django启动app时，`ready()`方法会自动加载多次，可在启动命令禁用`python manage.py runserver --noreload`