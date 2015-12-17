---
layout: post
title:  Set up Jenkins and start your work
categories: tec
---

都拿linux 说事吧，越来越受不了windows了

#### Install jenkins
从官网<http://jenkins-ci.org/>下war包，然后
nohup java -jar jenkins.war > nohub.log 2>&1 &
打开localhost:8080 即可访问

#### Plugin
其实jenkins也没什么好说的，基本拼凑拼凑plugin自己写点脚本就能用了, 每个plugin都有自己的一套用法，我也不是全部很熟悉，这个只能随用随查了。
列一下常用的plugin吧
* build-name-setter 可以改build name
* copy artifact plugin 运行一个job, 从另一个job的archive artifact里面考东西，注意jenkins 只支持master 和slave 之间的任意目录拷文件， 对于slave之间，只能从当前job访问其他job特定的archive目录， 不能从当前job拷文件到其他job
* copy to slave plugin 支持从master考文件到slave, 以及从slave拷文件到master
* email extension plugin 可以发出漂酿的邮件
* Git plugin
* Git changelog
* Job Configuration History plugin 不用怕丢失曾经的配置修改信息了
* SSH slaves plugin 必备的，如果需要slave的话
* Workspace cleanup plugin 可以帮着删你的workspace，好使
* performance 配置可以比较jmeter等report

#### Suggest
按照浩浩的说法，尽量都在master ->系统设置里面配置
* 所有的slave， 尽量用同一种配置，如果有linux和windows， 起码保证linux的都一种配置，windows的都一种配置。
* 全局属性, 可以配置git_repo 等其他全局的属性
* Git、Jre，Ant, Maven 按照linux/windows slave 来划分，写好名字和path(就不建议自己装了。。。毕竟工程里都是用的自带的)
* 配置默认邮件设置和邮件服务器

#### Slave安装
* Linux
1. 到系统管理->管理节点里，新建一个dumb Slave
2. 启动方法选择via SSH, 填写ip和登陆用户名密码
3. 然后会让你点个按钮装东东，装上即可，没错的话很快就能连上slave


#### Someting spacial
* 使用Trigger parameterized build on other project的时候，如果是从jobA 调用jobB, 传的参数jobB没有定义也可以，直接可以使用
* 因为slave中间文件交流很困难，可以把需要的东东写入文件放到master， 然后slave从master读取文件找需要的东东
* 如果你想在一个job里的shell里面定义变量到其他地方用，那么只能写文件了。。。export啥都不管用！
* 如果一个slave上面可以跑n个job, 但是slave设置了单线程，那么一起start这些job,也是会一个一个执行的
* 删除archive的方法，要不是设置自动删build,要不是自己手动删build,这样里面的archive 才会删掉

