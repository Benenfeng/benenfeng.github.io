---
title: Wireshark 常用数据结构和方法分析
date: 2019-08-29 22:28:26
tags: [wireshark, tools]
---


### Wireshark 常用数据结构和方法分析


#### header_field_info 

```c
struct header_field_info {
    const char      *name;
    const char      *abbrev;
    enum ftenum     type;
    int             display;
    const void      *strings;
    guint64         bitmask;
    const char      *blurb;
    .....
};
```
下面依次介绍各个属性的含义：

* ***name**: 表示字段名称的字符串。这就是名字将出现在图形协议树中。它必须是非空的字符串。
* ***abbrev**: 带有字段缩写的字符串。缩写应该开始使用父协议的缩写，后跟一个点作为分隔器。例如，IP数据包中的“src”字段将具有“ip.src”作为缩写。如果，有多个级别的句点是可以接受的，例如，您的协议中有字段，然后再细分为子域。例如，TRMAC有多个错误字段，因此缩写遵循这种模式：“trmac.errors.iso”，“trmac.errors.noniso”等。缩写也是过滤器的标识符，同样为非空字符串。
* **type**: 此字段是用来标记该域的值得类型，常用的类型有：
  * FT_BOOLEN: bool 型， 0 表示 false, 1 表示 true
  * FT_CHAR: 一个8bit 的 char 字符
  * FT_UINT8: 8bit 无符号整型
  * FT_INT8: 8bit 有符号整型
  * ...
* **display**: 显示字段主要指明数据域展示的编码方式ASCLL、10进制、二进制还是16进制
* ***strings**: 用于某些枚举数据类型
* **bitmask**: 如果该字段是位域，那么位掩码就是掩码当与一个值进行AND运算时，只留下制作该字段所需的位
* ***blurb**: 用于对数据域的描述，可以为null表示，不能用“” 表示。

未完。。。

若有错误或者不当之处，还请大家批评指正！

