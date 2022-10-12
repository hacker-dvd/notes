### link函数

作用：为文件创建一个硬链接

函数原型：

```c
#include <unistd.h>
int link(const char *oldpath, const char *newpath);
```

### unlink函数

作用：删除文件的目录项

特征：清除文件时，如果文件的硬链接数到0了，没有 dentry 相对应，但该文件仍不会马上释放，要等所有打开该文件的进程关闭该

文件，系统才会挑时间将该文件释放掉

函数原型：

```c
#include <unistd.h>
int unlink(const char *pathname);
```

`readlink xxx`读取软链接文件内容(链接目标)

**readlink 函数**

```c
#include <unistd.h>
ssize_t readlink(const char *pathname, char *buf, size_t bufsiz);  // 将链接文件的内容读到 buf 缓冲区中
```

**rename 函数**

作用：重命名一个文件

```c
#include <stdio.h>
int rename(const char *oldpath, const char *newpath);
```

