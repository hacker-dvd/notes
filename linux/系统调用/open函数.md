#### open函数

**函数原型：**

1. `int open (const char *pathname, int flags);`
2. `int open (const char *pathname, int flags, mode_t mode);`

返回值为一个文件描述符

flag 常用参数：

1. `O_RDONLY`：只读
2. `O_WRONLY`：只写
3. `O_RDWR`：读写
4. `O_APPEND`：追加
5. `O_CREAT`：创建对应文件
6. `O_EXCL`：如果要创建的文件已存在，则返回 -1，并且修改 errno 的值
7. `O_TRUNC`：打开文件的时候会将文件原本的内容全部丢弃，文件大小变为 0
8. `O_NONBLOCK`：如果路径名指向 FIFO/块文件/字符文件，则把文件的打开和后继 I/O设置为非阻塞模式（nonblocking mode）

创建文件时，权限会受到 umask 的影响，文件权限的实际计算方法为：

`文件权限 = mode & ~umask`

相关代码：

```c
// 头两个文件也可以写为
// #include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdio.h>
#include <errno.h>
#include <string.h>

int main() {
	/*
	int fd1;
	fd1 = open("tmp.txt", O_RDONLY);
	printf("fd1: %d\n", fd1);
	close(fd1);
	*/

	int fd2;
	fd2 = open("tmp.txt", O_RDONLY | O_CREAT, 0644); // rw-r--r--
	// fd2 = open("tmp.txt", O_RDONLY | O_CREAT | O_TRUNC, 0644);
	// 如果tmp.txt存在，则用只读方式将其打开并将该文件清空
	// 如果文件不存在，则创建该文件并将文件权限设置为644
	printf("fd2: %d\n", fd2);
    if (fd2 = -1) {
     	printf("errno: %d\n", errno);
        // 输出errno所代表的错误信息
        printf("%s\n", strerror(errno));
	}
	close(fd2);

	return 0;
}
```

**文件描述符**

形式：一个 int 型整数

`STDIN_FILENO`：对应的数为0

`STDOUT_FILENO`：对应的数为1

`STDERR_FILENO`:对应的数为2

文件描述符的最大值为1023，换言之，一个进程所能打开的文件数的最大值为1024

每次打开文件，对应的文件描述符一定是所有可用的文件描述符的最小值
