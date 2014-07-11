---
layout: post
title:  Sikuli+Linux+Ruby+实例
categories: tec
---


在linux上安装sikuli对我来说还挺麻烦，先记载下来一个使用实例吧。

我的环境是Ubuntu 12.04, ruby开发环境

恶心之处是要装三个支持的东东openCV tesseract 和 LinuxVisionProxy，而这些在sikuli的官方文档上写的不太明白。

官方文档：
[http://www.sikulix.com/download.html](http://www.sikulix.com/download.html)


- 1.  安装jruby 

-

    rvm insall jruby
    rvm list
    rvm use "your jruby"

make sure your current ruby is jruby 




- 2.  安装OpenCV

不要在ubuntu的包管理工具里安装，因为貌似有问题

    sudo apt-get install libopencv-*
    sudo apt-get isntall python-opencv
    sudo apt-get install python-numpy

- 3.  安装tesseract

-

    sudo apt-get install tesseract-ocr-dev
    sudo apt-get install libtesseract-dev

- 4.  安装LinuxVisionProxy， 并且手动编译

-
下载 LinuxVisionProxy.zip：
[https://launchpad.net/sikuli/+download](https://launchpad.net/sikuli/+download)

    wget 某一个最新的 LinuxVisionProxy.zip 地址
    unzip Sikuli-xxx-Supplemental-LinuxVisionProxy.zip 
    ./makeVisionProxy

- 4.  安装sikuli jar包

下载 sikuli-setup.jar (md5)：
[https://launchpad.net/sikuli/+download](https://launchpad.net/sikuli/+download)

    安装: java -jar sikuli-setup.jar 
    运行: ./runIDE


可以看看安装完了之后的log文件，里面记载了报错信息，没有报错的话才行， 他的错有时候不抛到命令行


- 4.  安装sikuli/rukuli (rukuli是sikuli的ruby版本)

-

    gem install rukuli

- 5.  把sikuli/rukuli 添加到项目的Gemfile里面

编辑Gemfile

    source 'https://rubygems.org'
    gem 'rukuli'

- 6.  添加环境变量SIKULIX_HOME

-
    
    export SIKULIX_HOME=/path_of_sikuli-java.jar

- 6.  测试代码

-

      require 'java'
      require 'rukuli'
      Rukuli::Config.run do |config|
        config.image_path = "#{Dir.pwd}/images/"
        config.logging = false
      end

      screen = Rukuli::Screen.new
      screen.click(10, 10) # should open your apple menu
    
      或者
      @re = Rukuli::Region.new(10, 10, 1500, 1500)
      @re.click "#{@fileset}/post.jpg"
