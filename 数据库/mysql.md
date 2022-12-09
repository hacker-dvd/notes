+ mysql 服务的启动和终止

    1. win + r 召唤出命令窗口之后，输入 services.msc ，然后找到 mysql8.0，即可选择开启或者关闭

    2. terminal 中输出`net start mysql80`可启动，输入`net stop mysql80`可终止

+ mysql 客户端连接

    1. 在 windows 下，打开 MySQL 8.0 Command Line Client，输入密码即可

    2. 使用系统自带的命令行工具执行指令：`mysql [-h 127.0.0.1] [-P 3306] -u root -p`

        注：中括号里面的内容是默认项可以省略，其中 -h 指定 ip 地址，-P 指定端口号

        -u 指定的是用户，-p 指定的是密码

        如果想在任意目录下都使用此命令，我们应该进行环境变量的配置。

        一般情况下，安装的位置为`C:\Program Files\MySQL\MySQL Server 8.0\bin\`下，只需将此添加到系统环境变量的 path 中即可
    
+ vs 连接 mysql 数据库

    <img src="https://raw.githubusercontent.com/hacker-dvd/notes/master/img/image-20221205170330886.png" style="zoom: 67%;" />

    点击属性

    1. 找到**VC++目录**

        (1) 在包含目录中插入`C:\Program Files\MySQL\MySQL Server 8.0\include`

        (2) 在库目录中插入`C:\Program Files\MySQL\MySQL Server 8.0\lib`

    2. 找到链接器目录，在附加依赖项中插入`libmysql.lib`

    将`C:\Program Files\MySQL\MySQL Server 8.0\lib`中的`libmysql.dll`复制到`C:\Windows\System32`下即可

