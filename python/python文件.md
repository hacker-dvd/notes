+ open()打开函数

    ```python
    open(name, mode, encoding = "UTF-8")
    '''
    name: 打开的目标文件字符串(可以包含文件所在的具体路径)
    mode:设置打开文件的模式(访问模式)：只读，写入，追加
    encoding:编码格式，一般使用 UTF-8
    '''
    ```

    <img src="https://raw.githubusercontent.com/hacker-dvd/notes/master//img/202301121633388.png" style="zoom:67%;" />
    
+ read()方法：`文件对象.read(num)`

    num 表示要从文件中读取的数据的长度(单位为字节)，如果没有传入 num，那么就表示读取文件中所有的数据

+ readlines()方法：按照行的方式把整个文件中的内容进行一次性读取，并且返回的是一个列表，其中每一行的数据为一个元素

+ readline()方法：一次读取一行内容

+ for 循环读取文件行：

    ```python
    for line in open("python.txt", "r"):
        print(line)
    ```

+ 文件的关闭：close()

+ with open 用法

    ```python
    with open ("python.txt", "r") as f:
        f.readlines()
    # 通过 with open 对文件进行操作，可以在操作完成之后自动关闭 close 文件
    ```

+ f.write()方法：对文件 f 进行写操作

+ 内容刷新：f.flush()，当调用 flush 时，内容会真正写入文件，在用 close()关闭文件时，默认也会执行 flush()方法