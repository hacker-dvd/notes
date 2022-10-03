#### makefile

**命名：**只能为`makefile`或者`Makefile`

执行 makefile 只需要键入`make`命令即可

**一个规则：**

```makefile
/*
目标:依赖条件
(此处必须有一个 tab 缩进)要执行的命令
*/
// 假设想用 test.c 来生成 test 可执行文件：
test:test.c
    gcc test.c -o test
```

**makefile 基本工作原理：**

基本原则：

1. 若想生成目标，检查规则中的依赖条件是否存在，若不存在，则寻找是否由规则用来生成依赖文件

    例如，上述 makefile 文件也可以改写为：

    ```makefile
    test:test.o
    	gcc test.o -o test
    test.o:test.c
    	gcc -c test.c -o test.o
    ```

2. 检查规则中的目标是否需要更新，必须检查它的所有依赖，依赖中由任意一个被更新，则目标必须被更新

    换句话说，目标的时间必须晚于依赖条件的时间，若不满足，则需要更新目录

假设 test.c 依赖于 add.c, sub.c, mul.c 进行编译，我们可以这样书写 makefile 程序：

```makefile
test:test.c add.c sub.c mul.c
	gcc test.c add.c sub.c mul.c -o test
```

但是，假如这样写的话，我更改 test.c, add.c, sub.c, mul.c 中的任意一个，都会导致4个文件重新编译链接，会造成一定的资源浪费，这

是不必要的，我们应该将编译链接分开进行：

```makefile
test:test.o add.o sub.o mul.o
	gcc test.o add.o sub.o mul.o -o test
	# 这一步是必不可少的，链接相对于编译耗费的资源很少，且可执行程序的执行需要这些二进制文件的共同链接
test.o:test.c
	gcc -c test.c -o test.o
# add 部分
add.o:add.c
	gcc -c add.c -o add.o
# sub 部分
sub.o:sub.c
	gcc -c sub.c -o sub.o
# mul 部分
mul.o:mul.c
	gcc -c mul.c -o mul.o
```

假如说 makefile 文件的形式是这样的话，如果我们更改 sub.c 文件，系统就会发现 sub.c 文件的 ctime 比 sub.o 的 ctime 晚，进而执行

sub 部分而更新 sub.o 的 ctime，从而导致 test 的依赖关系的 ctime 时间被更新，导致 test 部分被执行，其他部分由于没有检测到更新

时间的变化而没有被执行

需要注意的是，makefile 文件默认只将 第一部分的依赖关系作为 makefile 的最终目标进行执行，假如说我们最终想要生成的文件的依赖

关系不在第一部分的话，我们需要手动指定 makefile 文件的最终目标：

`ALL:test`

这样 makefile 就会认为最终的目标是 test 文件的生成了

**两个函数：**

1. `src=$(wildcard ./*.c)`：匹配当前目录下的所有`.c`文件，将文件名组成列表，赋值给`src`

    假如当前目录有`add.c, sub.c,mul.c`，执行完此命令之后`src`的值变为`src=add.c sub.c mul.c`

2. `obj=$(patsubst %.c, %.o, $(src))`：将参数3中包含参数1的部分全部转换为参数2的形式。执行完之后 obj 的值为：

    `obj=add.o sub.o mul.o`

使用上述两个函数原 makefile 文件可以改写为：

```makefile
src=$(wildcard ./*.c)
obj=$(patsubst %.c, %.o, $(src))
ALL:test
test:$(obj)
	gcc $(obj) -o test
	# 这一步是必不可少的，链接相对于编译耗费的资源很少，且可执行程序的执行需要这些二进制文件的共同链接
test.o:test.c
	gcc -c test.c -o test.o
# add 部分
add.o:add.c
	gcc -c add.c -o add.o
# sub 部分
sub.o:sub.c
	gcc -c sub.c -o sub.o
# mul 部分
mul.o:mul.c
	gcc -c mul.c -o mul.o
```

快速删除中间产生的非源码部分：`clean`函数

假如说想将`make`产生的`.o`文件全部删掉，我们可以在 makefile 文件中写入：

```makefile
clean:
	-rm -rf $(obj)
	# -rm的 - 作用为假设没有对应的文件，rm删除命令不会报错
```

想要清除对应的文件时，只需运行：`make clean`

注意，我们推荐先运行`make clean -n`来查看删除的文件内容，确认无误之后再执行`make clean`，这样可以防止误删文件

**三个自动变量：**

1. `$@`：表示规则中的目标

2. `$<`：表示规则中的第一个依赖条件

    如果将该变量应用于模式规则中，它可将条件列表中的依赖一次取出，套用模式规则

3. `$^`：表示规则中的所有依赖条件，组成一个列表，以空格隔开，如果这个列表中有重复的项则消除重复项

注：三个自动变量只能替换命令中的目标或条件

使用自动变量之后 makefile 文件可以改写为：

```makefile
src=$(wildcard ./*.c)
obj=$(patsubst %.c, %.o, $(src))
ALL:test
test:$(obj)
	gcc $^ -o $@
	# 这一步是必不可少的，链接相对于编译耗费的资源很少，且可执行程序的执行需要这些二进制文件的共同链接
test.o:test.c
	gcc -c $< -o $@
# add 部分
add.o:add.c
	gcc -c $< -o $@
# sub 部分
sub.o:sub.c
	gcc -c $< -o $@
# mul 部分
mul.o:mul.c
	gcc -c $< -o $@
```

模式规则：可以发现，add部分，sub部分，mul部分的内容类型几乎相同，我们可以将其更改为：

```makefile
%.o:%.c
	gcc -c $< -o $@
```

伪目标：假设 makefile 文件所在目录中含有名为 clean 或者 ALL 的文件，则会干扰 makefile 文件中 clean 命令和 all 命令的执行

此时，我们要生成对应的伪目标，使其正常运行：`.PHONY clean ALL`

**makefile 文件的最终形态：**

```makefile
src=$(wildcard ./*.c)
obj=$(patsubst %.c, %.o, $(src))
ALL:test
test:$(obj)
	gcc $^ -o $@
%.o:%.c
	gcc -c $< -o $@
clean:
	-rm -rf $(obj)
.PHONY: clean ALL
```

假如说 makefile 文件的名称不为 makefile 或者 Makefile(设为tmp)，那么只需执行`make -f tmp`



