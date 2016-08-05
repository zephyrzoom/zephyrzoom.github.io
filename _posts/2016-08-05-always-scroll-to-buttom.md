---
layout: post
title: 如何使滚动条始终处于底部
date: 2016-08-05 13:34:00 +0800
---

针对页面的某个元素`element`，有以下方法使滚动条始终处于底部。
 > 有时有的可能无效，还没彻底搞清楚。

1. `element.scrollTop = element.scrollHeight`

2. `element.scrollIntoView(false)`

3. `window.scrollTo(0,element.scrollHeight)`
