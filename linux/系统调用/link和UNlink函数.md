### link函数

作用：为文件创建一个链接

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

