#### find命令

参数类型：

1. -type 按文件类型搜索
2. -name 按文件名进行搜索：文件名应该用引号' '括起来
3. -maxdepth 指定最大搜索深度，此参数只能作为第一位使用
4. -size 按文件的大小进行搜索 

注： find 命令通过文件类型搜索普通文件时，不能用`-`，而应该使用`f`来搜索普通文件

输出的规格大小说明: 

默认为b，为512 byte，是磁盘的最小存储大小(也就是说，即使一个文件只用了10 byte 大小，实际存储它的时候也会用512 byte)

其他常用单位: 

c: 字符     k: KB     M: M     G: GB

注：stat 显示的三个时间

1. 最近访问时间：atime
2. 最近更改时间 (指文件类型被更改)：mtime
3. 最近改动时间：ctime

**三种 time 后加的参数：**

1. -n 最后一次修改在最近n天之内
2. +n 最后一次修改在 n+1 天之前
3. n 最后一次修改在距今 n 天到 n + 1 天之前

例子：

`find ./ -name '*.txt'`：搜索当前目录下的名为 *.txt 的所有文件

`find ./ type l `：搜索当前目录下所有软链接文件

`find ./ -maxdepth 1 -name .py`：在搜索深度为1的情况下，输出所有的 py 文件

`find ./ -size +20M -size -50M`：搜索当前目录下大小大于20MB小于50MB的文件

**对find命令找出的目标执行操作：**

`find ./ -name '*.py' -exec ls -l {} \;`：将当前目录下的 python文件找出并打印其信息

`find ./ -type l -exec rm -r {} \;`：将当前目录下的所有软链接文件找出并删除

注：-exec 代表对 find 命令执行的结果进行操作， {} 代表 find 找出的命令的集合，; 代表语句的结束，\ 是对 ; 的转义

-exec 在执行 rm 操作时，不会进行询问操作，假设相对要操作的文件进行询问，应将-exec 换为-ok

注意：ps 命令可以对结果起作用，但 find 查询的结果不会对管道进行相应

若想对 find 命令使用管道，则应该使用 xargs 参数

xargs 和 exec 的主要区别：exec 会一次性处理 find 找出的所有文件，而 xargs 当结果集数量过大时，会进行分片处理

xargs 的效率比 exec 效率更高

`find ./ -name '*.py' | xargs ls -l`：将当前目录找到的所有 python 文件输出

注意：由于 xargs 的默认分隔符为空格，因此，当我查找文件的当前目录下有文件名带空格的文件时，执行例如：

`find ./ -type f | xargs ls -l`的命令会报错

因此，我们将分隔符由空格换成 0 ，即执行：

`find ./ -type f -print0 | xargs -0 ls -l`即可正常输出

因此，假如说我们操作的文件有空格，要么使用 -exec 参数，要么使用 -print0 参数
