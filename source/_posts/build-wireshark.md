---
title: Ubuntu 16.04 下源码编译运行wireshark
date: 2019-08-10 22:30:36
tags: [wireshark, tools] 
---

## Ubuntu 16.04 下源码编译运行wireshark

##### 安装依赖文件

* apt update && apt build-dep -y wireshark && apt install -y git
* apt install -y python3-ply quilt libsysemd-dev

##### 下载源码
* git clone --depth=1 https://code.wireshark.org/review/wireshark

##### 编译运行

1. mkdir wireshark-ninja 
2. cd wireshark-ninjia
3. cmake -G Ninja ../wireshark 
4. ninja 
5. sudo ./run/wireshark

wireshark需要管理员权限，否则找不到网络设备接口。