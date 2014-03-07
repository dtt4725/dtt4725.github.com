---
layout: post
title:  "UnicodeDecodeError: 'ascii' codec can't decode byte 0xe5 "
categories: tec
---


#####two ways to resolve this problem:
> 1 specific the way you encoding :

{% highlight python %}
#! /usr/bin/env python 
# -*- coding: utf-8 -*- 

s = '中文' 
s.decode('utf-8').encode('gb18030') 
{% endhighlight %}

> 2 change sys.defaultencoding as your file's encoding :

{% highlight python %}
import sys 
reload(sys) 
sys.setdefaultencoding('utf-8') 

str = '中文' 
str.encode('gb18030')
{% endhighlight %}