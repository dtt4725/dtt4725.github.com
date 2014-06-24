---
layout: post
title:  Capybara can't find element
categories: automation
---


使用浏览器xpath，css工具能够找到元素，但是在capybara里面打死找不到

可能是元素的css属性有问题，比如opacity(不透明度)设置成0， 或者其他样式设置问题。 因为对css不太熟悉，这个需要到时候慢慢排查。 解决思路是找到影响的css后，使用js或者jquery实时改变样式，再对元素做操作。以下用的是jquery，相关知识去w3c查。

{% highlight ruby %}

    script = "$('#css_element_id').css({opacity: 100});"
    page.driver.browser.execute_script script 
{% endhignlight %}