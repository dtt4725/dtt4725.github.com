---
layout: post
title:  Jenkins plugin introduce
categories: tec
---

在jenkins里面，有很多配置，很多地方需要注意，坑多

####Extended Choice parameter
在jenkins里面，你可以用默认的choice设定简单的checkbox, 如果需要复杂一点的话， 可以安装插件extended Choice parameter。
以下的例子会展示如何使用该插件实现拥有默认值的checkbox
1. 准备property file(在master server上准备，不需要在jenkins里面准备)
> vi test.property
> a=m,n
> b=x,y,z
> c=${a},${b}

1. 安装插件
2. 在job的参数化构建过程里面，添加extended Choice parameter
3. 输入名字， 比如是para1
4. 选择basic parameter types - > checkbox, 选择',' 为分隔符（空格无法成为分隔符）， 如果选择"quote value"的话，表示值会被双引号包起来， 类似 a="m,n"
5. 在choose source for value里面选择 property file
> 路径输入在master上面的全路径 xxx/test.property
> property key 输入c
6. 在choose source for Default value里面选择 property file
> 路径输入在master上面的全路径 xxx/test.property
> property key 输入a

经过检验，现在para1显示 m,n,x,y,z， 其中m,n为默认选项， 也就是para1的值默认是m,n  para1=m,n

####Trigger/call builds on other projects
在构建中，使用这个插件，可以勾选“Block until the triggered projects finish their builds”
这样，在以后的构建中，可以使用内定的参数
> LAST_TRIGGERED_JOB_NAME="Last project started"
TRIGGERED_JOB_NAMES="Comma separated list of all triggered projects"
TRIGGERED_BUILD_NUMBER_<project name>="Last build number triggered"
TRIGGERED_BUILD_NUMBERS_<project name>="Comma separated list of build numbers triggered"
TRIGGERED_BUILD_RESULT_<project name>="Last triggered build result of project"
TRIGGERED_BUILD_RESULT_<project name>_RUN_<build number>= "Result of triggered build for build number"
TRIGGERED_BUILD_RUN_COUNT_<project name>= "Number of builds triggered for the project"

比如如果我的job名字是abc
我可以使用
TRIGGERED_BUILD_NUMBER_abc 来得到build number， 不错哦~

