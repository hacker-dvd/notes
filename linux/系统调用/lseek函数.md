### lseek函数

**函数原型：**

```c
off_t lseek(int fd, off_t offset, int whence);
参数：
fd：文件描述符
offset：偏移量
whence：起始偏移位置：SEEK_SET/SEEK_CUR/SEEK_END
返回值：
成功：较起始位置偏移量
失败：-1，并设置errno
```

+ 文件的读或写使用同一偏移位置，因此在操作文件之前，最好加上一句：`lseek(fd, 0, SEEK_SET);`

通过 lseek 函数获取文件大小并对文件扩容

1. 获取文件大小：

    ```c
    int len = lseek(fd, 0, SEEK_END);
    printf("file size: %d\n", len);
    ```

2. 拓展文件大小：

    要想使文件大小真正拓展，必须引起 I/O 操作

    ```c
    // 将文件大小拓展100字节
    lseek(fd, 99, SEEK_END);
    write(fd, "\0", 1);  // 写入一个文件空洞
    ```

**使用 truncate 拓展文件大小**

函数原型：

```c
#include <unistd.h>
#include <sys/types.h>
int truncate(const char *path, off_t length);   // 注意，使用truncate改变文件大小必须保证文件存在
// int ftruncate(int fd, off_t length);
返回值：
成功返回0，失败返回-1，并设置errno
```



