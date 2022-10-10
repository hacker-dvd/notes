### fcntl函数

作用：改变一个已经打开文件的访问控制属性

获取文件状态：`F_GETFL`

设置文件状态：`F_SETFL`



```c
#include <unistd.h>
#include <fcntl.h>
int fcntl(int fd, int cmd, ... /* arg */);
```

**设置过程：**

```c
int flag = fcntl(fd, F_GETFL);
flag |= O_NONBLOCK;
fcntl(fd, F_SETFL, flag);
```

