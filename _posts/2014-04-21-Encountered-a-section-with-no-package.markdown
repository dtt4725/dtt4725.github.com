---
layout: post
title:  Encountered a section with no Package
categories: tec
---

解决E: Encountered a section with no Package: header错误



> ubuntu机器上出现下面这个错误：

   Reading package lists... Error!
　　E: Encountered a section with no Package: header
　　E: Problem with MergeList /var/lib/apt/lists/ftp.sjtu.edu.cn_ubuntu_dists_precise-security_restricted_binary-i386_Packages
　　E: The package lists or status file could not be parsed or opened.

　　

> 可以按如下方法解决：

 `sudo rm /var/lib/apt/lists/* -vf`
 `sudo apt-get update`　   

    