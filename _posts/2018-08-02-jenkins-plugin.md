---
layout: post
title: jenkins插件开发
date: 2018-08-02 10:53:00 +0800
---

#官方说明

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

#从扩展点出发

开发的起始切入点是寻找需要对jenkins进行扩展的阶段，这个是一切开始的基础。比如想要对构建后的某个操作做扩展，那么就
继承`Notifier`类，重载`perform`方法，这个方法是在构建过程中会执行的，主要的执行逻辑都在这个方法中。

对于需要与前端交互的属性，为其添加`get`和`set`方法，并且给`set`方法添加注解`DataBoundSetter`，也可给构造函数添加
注解`DataBoundConstructor`，这样以来在前端jelly中对变量的修改都会反应在对象中。

对于需要对前端表格做验证的，需要创建一个继承`BuildStepDescriptor<?>`的类，添加一些`doCheckXXX`的方法，前端输入时就
会在失焦时执行该方法。在该类上需要添加注解`Extension`表示此处作为扩展，再添加一个`Symbol`注解使其可以在pipeline中使用。

如果插件需要对从节点的文件做处理时，需要借助`FilePath`类，它表示了文件以及文件所在的节点。使用时需要先定义一个继承`FileCallable<?>`的类，
并且保证这个类时可以序列化的，重载其中的`invoke`方法，这个方法是关键，将需要在从节点执行的代码放在这里，当插件运行于从节点时，
就会执行该方法。这个类调用的话，需要通过`FilePath`对象调用`filePath.act(new FileCallableImpl())`，这样就会执行`invoke`，
但是，在从节点执行代码时无法加断电调试的，也无法打印输出，不过可以通过构造函数将`TaskListener`对象传进去，用来打印日志。

例子代码：
```java
public class MCDP extends Notifier implements SimpleBuildStep {

    private final String name;

    @DataBoundConstructor
    public MCDP(String name) {
        this.name = name;
    }

    @DataBoundSetter
    public String setName(String name) {
        this.name = name;
    }


    @Override
    public void perform(Run<?, ?> run, FilePath workspace, Launcher launcher, TaskListener listener) throws InterruptedException, IOException {
        workspace.act(new FilePath.FileCallable<Void>() {
            @Override
            public Void invoke(File f, VirtualChannel channel) throws IOException, InterruptedException {
                  // do something.
                  return null;
            }
        });
    }


    @Symbol("mcdp")
    @Extension
    public static final class DescriptorImpl extends BuildStepDescriptor<Publisher> {
        public FormValidation doCheckName(@QueryParameter String value)
                throws IOException, ServletException {
            if (value.length() == 0)
                return FormValidation.error("too short");
            return FormValidation.ok();
        }
    }
}
```