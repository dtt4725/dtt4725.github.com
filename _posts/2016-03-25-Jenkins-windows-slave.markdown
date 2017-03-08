---
layout: post
title:  Jenkins windows slave
categories: tec
---
在jenkins里面，做一件事有很多种方法，本文类似最佳实践，会略过一些特殊情况下的特殊配置

#### Add windows server as slave
1. 在新建结点的时候选择“Launch slave agents via Java web start”
2. 登录到windows slave
3. 在slave中打开jenkins, 下载JNLP 文件
4. 如果没有java的话，需要在slave上装java, 如果有的话可以直接运行jnlp文件，完成安装
5. 返回jenkins, 会看到server已经连上在线

#### 关于cygwin
公司的windows机子上装了cygwin,很多shell命令都是在这里做的，所以在jenkins里面，我们也需要在cygwin里面运行命令
1. 安装插件[cygpath](https://wiki.jenkins-ci.org/display/JENKINS/Cygpath+Plugin)
2. 在windows slave job中，添加execute shell job
3. 明确windows server 中cygwin的path, 例如我的server里面path是C:\dev\tools\cygwin\bin\bash.exe
4. 在execute shell里面最开始写如下两行
> #!C:\dev\tools\cygwin\bin\bash.exe
> export PATH=/cygdrive/c/dev/tools/cygwin/bin:$PATH 

再继续写 ls -all， 可以看到已经可以执行shell命令了



