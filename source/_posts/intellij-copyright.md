---
title: Intellij 自动生成Copyright 和 LF CRLF的默认选择
date: 2019-09-12 11:56:17
tags: tools
---


### Intellij 自动生成Copyright 和 LF CRLF的默认选择

#### Intellij 自动生成Copyright

1.  File->Settings… 
2.  Editor -> File and Code Templates, 添加你的Copyright 
    ![intellij-copyright1.png](/images/intellij-copyright1.png)
    

3.  File and Code Templates -> Files -> Class,  加入 `#parse(your copyright.java)` 
   ![intellij-copyright2.png](/images/intellij-copyright2.png)


#### LR CRLF 选择
   
   1.  File->Settings… 
   2.   Editor -> Code Style -> Line separator: 选择 Unix and OS X
       ![image.png](/images/intellij-lr.png)