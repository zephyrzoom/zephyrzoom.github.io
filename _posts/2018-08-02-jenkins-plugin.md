---
layout: post
title: jenkins插件开发
date: 2018-08-02 10:53:00 +0800
---

实现`org.kohsuke.stapler.StaplerProxy`的`getTarget`方法可以控制权限。

Jenkins大部分结构化配置都使用XStream存储，还有很少一部分使用文本和java property文件存储。
用`transient`关键字设置不需要持久化的属性。

□ Java Platform
 ↖
  □ “application classpath” (servlet container): java -jar jenkins.war
   ↖
    □ Jenkins core: jenkins.war!/WEB-INF/lib/*.jar
     ↖
      □ plugin A: $JENKINS_HOME/plugins/a.jpi!/WEB-INF/lib/*.jar
       ↖                                                             ↖
        □ plugin C: $JENKINS_HOME/plugins/c.jpi!/WEB-INF/lib/*.jar  ← □ UberClassLoader
     ↖ ↙                                                             ↙
      □ plugin B: $JENKINS_HOME/plugins/b.jpi!/WEB-INF/lib/*.jar
      ⋮
加载顺序：
1. 从启动Jenkins的war包
2. 加载核心模块
3. 加载插件
4. 插件C依赖插件A和B，`c.jpi!/META-INF/MANIFEST.MF`中就会有`Plugin-Dependencies: a:1.13,b:1.6`这样的代码
5. UberClassLoader是所有可用插件的代理