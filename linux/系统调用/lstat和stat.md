### stat函数：

函数原型：

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>

int stat(const char *pathname, struct stat *statbuf);
```

范例：查看文件大小

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <unistd.h>

int main(int argc, char *argv[]) {
	struct stat sbuf;
	int ret = stat(argv[1], &sbuf);
	if (ret == -1) {
		perror("stat error");
		exit(1);
	}
	printf("file size:%ld\n", sbuf.st_size);

	return 0;
}
```

stat 函数会穿透符号链接，假如为一个文件创建一个链接，访问这个链接返回的仍然是原文件的链接

若不想穿透符号链接，应该使用 lstat 函数(lstat 函数的形式)

