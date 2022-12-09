+ 单行注释：-- 注释内容或 # 注释内容

    多行注释：/* 注释的内容 */
    
+ `SHOW TABLES`：展示目前数据库下有多少个表

+ 创建数据表时判断要创建的数据库是否存在：`CREATE DATABASE IF NOT EXISTS test`

+ 创建数据库：`CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则]`

    例：`CREATE DATABASE test DEFAULT CHARSET UTF8MB4;`

    注：此处使用的 UTF8 默认的字长为 3 个字节，因此使用 MB4 将其扩展为 4 个字节

+ 查询当前位于哪个数据库：`SELECT DATABASE();`

+ 删除表：

    `DROP TABLE [IF EXISTS] 表名;`

    `TRUNCATE TABLE 表名;`  **删除指定表，并重新创建该表**

+ `SHOW CREATE TABLE 表名;`展示创建表的详细过程

+ 精度：数的整数部分和小数部分的位数和

    标度：数的小数部分位数

#### 表项的相关操作：

+ 添加字段：`ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];`

+ 修改数据类型：`ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);`

+ 修改字段名和字段类型：`ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];`

+ 删除字段：`ALTER TABLE 表名 DROP 字段名;`

+ 修改表名：`ALTER TABLE 表名 RENAME TO 新表名;`

+ 批量添加数据：

    `INSERT INTO 表名(字段名1, 字段名2, ...) VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);`

    `INSERT INTO 表名 VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);`

+ 修改数据：`UPDATE 表名 SET 字段名1 = 值1, 字段名2 = 值2, ...[WHERE 条件];`

+ 删除数据：`DELETE FROM 表名 [WHERE 条件];`

#### DQL语句：

+ 基础语法：

    SELECT 字段列表                                 4

    FROM 表名列表                                    1

    WHERE 条件列表                                 2

    GROUP BY 分组字段列表                    3

    HAVING 分组后的过滤条件列表           5

    ORDER BY 排序字段列表                    6

    LIMIT 分页参数                                     7

    **注意：此处数字为执行顺序**

+ 设置别名：`SELECT 字段1 [AS 别名1], 字段2[AS 别名2] ... FROM 表名;`，注意，这里 AS 关键字可以省略

+ 取出重复记录：`SELECT DISTINCT 字段列表 FROM 表名;`

+ WHERE 可以跟的条件列表

    1. 大于小于等比较符号，需要注意相等使用的是 =

    2. BETWEEN ... AND ... 在某个范围之内(含最大值和最小值)

    3. IN(...) 在 in 之后的列表中的值中多选一

        `SELECT * FROM student WHERE age in(18, 19, 20)`

    4. LIKE 进行模糊匹配，_ 代表单个字符，% 代表任意时间

    5. IS NULL / IS NOT NULL 判断是否为空

    6. AND 或 && 代表并且，OR 或 || 代表或者，NOT 或 ！代表非

+ 聚合函数：将一列数据看成一个整体，进行纵向计算

    语法：`SELECT 聚合函数(字段列表) FROM 表名;`

    常见聚合函数：count：统计数量，max，min：最大值最小值，avg：平均值，sum：求和

    **注意：所有的 NULL 值不参与函数运算**

+ **WHERE 和 HAVING 区别：**

    1. 执行时机不同：WHERE 是分组之前进行过滤，不满足 WHERE 条件，不参与分组，而 HAVING 是分组之后对结果进行过滤
    2. 判断条件不同：WHERE 不能对聚合函数进行判断，而 HAVING 可以


+ **执行顺序：where > 聚合函数 > having**

    **分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无意义**

+ 排序时，asc 代表升序排序，desc 代表降序排序

+ 分页查询：`select 字段列表 from 表名 limit 起始索引,查询记录数;`

    1. 起始索引从0开始，起始索引 = (查询页码 - 1) * 每一页显示记录数
    2. 分页查询时数据库的方言，不同的数据库有着不同的实现，mysql 中使用的是 limit
    3. 如果查询的是第一页数据，起始索引可以省略，只需要写查询记录数即可

#### 查询样例：

+ 根据性别分组，统计男性员工和女性员工的数量：`select gender, count(*) from emp group by gender;`

+ 根据性别分组，统计男性员工和女性员工的平均年龄：`select gender, avg(age) from emp group by gender;`

+ 查询年龄小于45岁的员工，并根据工作地址分组，获取员工数量大于等于3的工作地址：

    `select workAddress, count(*) from emp where age < 45 group by workAddress having count(*) >= 3;`
    
+ 查询年龄为20，21，22，23岁的女性员工信息：

    `select * from emp where gender='女' and age in(20, 21, 22, 23);`

+ 查询性别为男，并且年龄在20-40岁(含)以内的姓名为三个字的员工：

    `select * from emp where gender='男' and age (between 20 and 40) and name like '___';`

+ 统计员工表中，年龄小于60岁的，男性员工和女性员工的人数：

    `select gender, count(*) from emp where age < 60 group by gender;`

+ 查询所有年龄小于等于35岁员工的姓名和年龄，并对查询结果按年龄升序排序，如果年龄相同按照入职时间降序排序：

    `select name, age from emp where age <= 35 order by age, entrydate desc;`

+ 查询性别为男，且年龄在20-40岁(含)以内的前5个员工信息，对查询结果按年龄升序排序，年龄相同的按入职时间升序排序：

    `select * from emp where gender='男' and age (between 20 and 40) order by age, entrydate limit 5;`

#### DCL 语句(Data Control Language，数据控制语言)

+ 创建用户：`create user '用户名'@'主机名' identified by '密码';`

+ 修改用户密码：`alter user '用户名'@'主机名' identified with mysql_native_password by '新密码';`

+ 删除用户：`drop user '用户名'@'主机名';`

+ 权限管理：

    <img src="https://raw.githubusercontent.com/hacker-dvd/notes/master/img/image-20221209194618310.png" style="zoom: 67%;" />

    1. 查询权限：`show grants for '用户名'@'主机名';`
    2. 授予权限：`grants 权限列表 on 数据库.表名 to '用户名'@'主机名';`
    3. 撤销权限：`revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';`

DCL 示例：	

+ 创建用户 lc，只能在当前主机 localhost 上访问，密码为 12345：

    `create user 'lc'@'localhost' identified by '12345';`

+ 创建用户 lc，可以在任何主机上访问该数据库，密码为 12345：

    `create user 'lc'@'%' identified by '12345'`









