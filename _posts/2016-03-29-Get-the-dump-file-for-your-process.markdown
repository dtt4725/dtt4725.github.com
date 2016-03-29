---
layout: post
title:  Get the dump file for your process
categories: tec
---


#### Prepare tools
jmap : http://docs.oracle.com/javase/7/docs/technotes/tools/share/jmap.html
mat : download from http://www.eclipse.org/mat/downloads.php

#### Get the dump file
1. get the pid of your process
> ps -ef | grep .... (In Windows, you may need to install procexp64 to get the pid )
2. generate the dump file
> jmap -dump:format=b,file=test.bin <pid> 


#### Analyze the file
1. open file from mat(type:suspects)
2. click OQL
3. input search command:
> select class
> select * from com.compositesw.cdms.ds.teradata.TeradataConnection
> select string
> SELECT s FROM java.lang.String s WHERE s.toString().equals("test1234567890") 

4. search carefully from the result


