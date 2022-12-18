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

    2. 找到链接器目录，在输入 -> 附加依赖项中插入`libmysql.lib`

    将`C:\Program Files\MySQL\MySQL Server 8.0\lib`中的`libmysql.dll`复制到`C:\Windows\System32`下即可

### mysql 函数：

1. 常用的字符串函数：

    <img src="https://raw.githubusercontent.com/hacker-dvd/notes/master/img/image-20221209201426787.png" style="zoom: 67%;" />

    想要查询一个函数的作用时，可以采用 select 关键字，例如，`select concat('hello ', 'world');`

    **注意，substring 函数中字符串的索引是从1开始的**

2. 常见的数值函数：

    <img src="https://raw.githubusercontent.com/hacker-dvd/notes/master/img/image-20221209203036431.png" style="zoom:67%;" />

    例：生成一个6位的随机数：`select lpad(round(rand() * 1000000, 0), 6, 0);`

3. 常见的日期函数：

    <img src="https://raw.githubusercontent.com/hacker-dvd/notes/master/img/image-20221209204003951.png" style="zoom:67%;" />

    可以将 now 函数和 year，month，day 函数一起搭配使用：`select year(now());`

    查看当前时间向后推进70天的时间：`select date_add(now(), interval 70 day);`

    **向前推进的话只需要在数字前面加一个负号即可**

4. 常见的流程控制函数：

    <img src="https://raw.githubusercontent.com/hacker-dvd/notes/master/img/image-20221209205352299.png" style="zoom:67%;" />

    注意，ifnull 函数在判断 value1 是否为空时，仅当 value1 为 null 时才执行 value2，其余的情况都执行 value1

    例：查询 emp 表的员工姓名和工作地址，当工作地址为北京或上海时，为一线城市，其他情况为二线城市：

    ```sql
    select name, (case workAddress when '北京' then '一线城市' when '上海' then '一线城市' else '二线城市' end) as '工作地址' from emp;
    ```


#### 约束

+ 常见的约束分类：

    <img src="https://raw.githubusercontent.com/hacker-dvd/notes/master/img/image-20221218162115746.png" alt="image-20221218162115746" style="zoom:67%;" />

















