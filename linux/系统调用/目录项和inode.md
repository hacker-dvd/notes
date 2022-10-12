### inode:

本质为结构体，存储文件的属性信息，如权限，大小，类型，时间，用户，盘块位置，也叫做文件属性管理结构，大多 inode

存储在磁盘上，少量 inode 存储在内存上。

一个文件由两部分组成：

1. dentry(directory entry，目录项)，有两个主要成员：文件名，inode 编号
2. inode 本体

所以硬链接创建的只是不同的 dentry 而已，所用的 inode 是相同的

![](https://github.com/hacker-dvd/notes/raw/master/img/image-20221010101338001.png)

### 目录权限：

**r 权限**：目录可以被浏览

**w 权限**：创建，修改，删除文件

**x 权限**：目录可以被打开，进入

### 目录操作函数：

```c
// opendir 打开目录
#include <sys/types.h>
#include <dirent.h>
DIR *opendir(const char *name);

//readdir 读取目录，一次只能读一个目录项
#include <dirent.h>
struct dirent *readdir(DIR *dirp);

// 关闭目录
#include <sys/types.h>
#include <dirent.h>
int closedir(DIR *dirp);
```



用目录操作函数实现`ls -a`命令

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <unistd.h>
#include <dirent.h>
#include <string.h>

int main(int argc, char *argv[]) {
	DIR *dp = opendir(argv[1]);   // 获取打开的目录指针
	if (dp == NULL) {
		perror("opendir error");
		exit(1);
	}
	struct dirent *sdp;
	while ((sdp = readdir(dp)) != NULL) {  // 一项一项的读取目录项
		printf("%s ",sdp->d_name);
	}
	printf("\n");
	closedir(dp);
    
	return 0;
}
```

