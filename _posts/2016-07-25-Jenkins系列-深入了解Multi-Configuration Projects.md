---
layout: post
title:  Jenkins系列 - 深入了解Multi-Configuration Projects
categories: tec
---

###什么情况下需要使用Multi-Configuration Projects
当你需要使用一个"Matrix"来组织所需的参数

当你的多个job有类似的step，而你又不想为此创建多个job


####我们的应用场景
简单介绍一下我们的情况：

1. 我们的test case使用test suite来组织， 每个suite包含了大量的test case。 
2. 我们需要持续跑的test suite， 跑每个suite的步骤相同，但不同的suite所需的可能略有不同（有些suite只能跑在特定的机子上，有些suite需要单独的处理步骤。
3. 每个suite跑的时候需要独占某台机子，多个suite不能同时跑在一台机子上。
4. 入口需要配置可以单独跑某个suite, 也可以同时跑多个suite，同时跑的话可能需要机子排队。

####我们的策略
根据以上的场景，有几种可行方案：

1. 使用Multi-Configuration Projects（目前的方案）
    优点. 所有的配置都在一个job里，方便更改，减少需要维护的job数量
    缺点1. 如果不同参数所需差异太大，请忽略这种方案。
    缺点2. Jenkins默认提供的各种view对这种job支持的都不好
    缺点3. 某些插件对该job支持的不佳，比如set job name插件

    
2. 使用job模板来为每个suite建立单独的job（如果模板支持的好，会考虑转入）
    优点. 多个job,可以充分使用Jenkins提供的filter组织view
    缺点1. 现有的Jenkins模板插件，对于需求2支持的不佳（这是让我摒弃该方案的根本原因）
    缺点2. 维护多个job的成本（即使有模板也需要偶尔维护）

3. 规避模板，使用manage script来统一管理job的核心部分
    优点1. 多个job,可以充分使用Jenkins提供的filter组织view
	优点2. 对于不同suite的不同需求，可以得到很好的实现
	缺点. 维护工作量太大，虽然核心部分统一管理，但其余部分如果需要改动，工作量仍是巨大
4. 其余的可能性，我认为总会存在更好的解决方案，可能是个简单的插件，也可能是聪明的配置，以后有时间再去研究了
	
###如何使用Multi-Configuration Projects

- 1.新建job选择"构建一个多配置项目"

- 2.如果需要为子job建单独的目录，可以在"Advanced Project Options"选"Use custom child workspace"
- 3.如果你需要把job传进来的参数转化为变量，强烈建议装插件[dynamic-axis](https://wiki.jenkins-ci.org/display/JENKINS/DynamicAxis+Plugin)，用法见下图：
![screenshot](/assets/images/articles/2016/07/dynamic-axis.png )
- 4.对于某些suite需要跑在特定机子，首先需要给你的slave node建label, 建议把所有的slave建一个统一的label， 例如"regression", 然后针对特定的slave, 加上特定的label:
![screenshot](/assets/images/articles/2016/07/multi-slave.png )
在"Combination Filter" 加上server的limit:
<pre class="brush: shell; gutter: true;">
((label=="linux_cdata").implies(SUITE=="cdata"))&&((label=="linux_kong").implies(SUITE=="kong"))&&((label=="linux_bd").implies(SUITE=="BD-server" || SUITE=="BD-i18n"))&&((label=="regression").implies(SUITE!="BD-server" && SUITE!="BD-i18n" && SUITE!="cdata"&& SUITE!="kong"))
</pre>
- 5.构建中的设置完全随意，如果后面需要发可配的邮件，请记得会多这样的一个选项"Trigger for matrix projects	" 。

###运行结果
是不是炫酷呢
![screenshot](/assets/images/articles/2016/07/multi-result.png )

    
