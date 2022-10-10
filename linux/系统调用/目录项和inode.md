### inode:

本质为结构体，存储文件的属性信息，如权限，大小，类型，时间，用户，盘块位置，也叫做文件属性管理结构，大多 inode

存储在磁盘上，少量 inode 存储在内存上。

一个文件由两部分组成：

1. dentry(directory entry，目录项)，有两个主要成员：文件名，inode 编号
2. inode 本体

所以硬链接创建的只是不同的 dentry 而已，所用的 inode 是相同的

![](https://github.com/hacker-dvd/notes/raw/master/img/image-20221010101338001.png)