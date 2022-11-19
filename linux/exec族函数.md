### exec 族函数

在一个进程在运行过程中碰到 exec 函数之后，该进程之后的代码不会执行，改为执行 exec 对应的参数

返回值：

只有当 exec 执行失败之后才会返回-1，并设置 errno 值，否则不会返回值

**execlp 函数**

函数原型：

```c
#include <unistd.h>
int execlp(const char *file, const char *arg, .../* (char  *) NULL */);
```

**注意：**arg 参数的计算是从 argv[0] 开始计算，在所有参数输入完成之后，应该以 NULL 结尾

例：通过 execlp 函数实现 ls 命令

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]) {
	pid_t pid = fork();
	if (pid == 0) {  // 子进程
		execlp("ls", "ls", "-l", "-h", NULL);
		perror("exec error\n");  // exec执行成功的话不会执行
		exit(1);
	} else if (pid > 0) {
		sleep(1);
		printf("parent: %d\n", getpid());
	}

	return 0;
}
```

execlp 实现的是执行环境变可以查到的命令，当想要执行指定的一个文件时，可以使用 execl 函数

**execl 函数：**

函数原型：

```c
int execl(const char *pathname, const char *arg, .../* (char  *) NULL */);
```

注：省略号跟的是调用 pathname 文件所需要的参数，如果没有则不需要写

例：执行当前目录下的 a.out 文件

```c
execl("a.out", "a.out",/*中间提供对应的a.out需要的参数*/ NULL);
```

