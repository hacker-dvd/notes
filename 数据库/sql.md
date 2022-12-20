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

#### 多表关系

+ 一对多关系(一个员工只能在一个部门内，但一个部门可以有多个学生)

    **实现：在多的一方建立外键，指向一的一方的主键**

+ 多对多关系(一个学生可以选择多门课程，一门课程也可以被多个学生选择)

    **实现：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键**

+ 一对一关系(用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中)

    **实现：在任意一方加入主键，关联另一方的主键，并且设置外键为唯一的(unique)**

#### 多表查询

+ 多表查询分类

    1. 连接查询

        1.1. 内连接：相当于查询 A，B 交集部分数据

        1.2. 外连接：

        1.2.1. 左外连接：查询左表中的所有数据，以及两张表交集部分数据

        1.2.2. 右外连接：查询游标中的所有数据，以及两张表交集部分数据

        1.3. 自连接：当前表与自身的连接查询，自连接必须使用表别名

    2. 子查询

+ 内连接(查询 A，B 交集部分)

    隐式内连接：`select 字段列表 from 表1, 表2 where 条件`

    显式内连接：`select 字段列表 from 表1 [inner] join 表2 on 连接条件`

    例：查询每一个员工的姓名，及关联部门(dept)的名称

    隐式内连接实现：`select e.name, d.name from emp e, dept d where e.dept_id == d.id`

    显式内连接实现：`select e.name, d.name from emp e inner join dept d on e.dept_id == d.id`

+ 外连接

    1. 左外连接：`select 字段列表 from 表1 left [outer] join 表2 on 条件`

        例：查询 emp 表的所有信息和对应的部门信息：

        `select e.*, dept.name from emp e left outer join dept d on e.dept_id == d.id`

    2. 右外连接：`select 字段列表 from 表1 right [outer] join 表2 on 条件`

        例：查询 dept 表的所有信息和对应的员工信息：

        `select d.*, e.* from emp e right outer join dept d on e.dept_id == d.id`

    **注意：右外连接能实现的通过左外连接都能实现，只需要将左表和右表的顺序交换即可**

+ 自连接

    自连接查询，可以是内连接查询，也可以外连接查询

    语法：`select 字段列表  from 表A 别名A join 表A 别名B on 条件`

    例：

    1. 查询员工及其所属领导的名字：`select a.name, b.name from emp a, emp b where a.managerid = b.id;`

    2. 查询员工及其所属领导的名字，如果没有领导，也许要查询出来：

        `select a.name '员工', b.name '领导' from emp a left outer join emp b on a.managerid = b.id;`

+ 联合查询(union)

    对于联合查询，就是把多次查询的结果合并起来，形成一个新的结果集

    语法：`select 字段列表 from 表A ... union [all] select 字段列表 from 表B ...`

    **注意：假如加上 all 关键字，则会将查询的结果直接拼接起来，但假如去掉 all 关键字，则会对查询的结果进行去重处理**

    例：将薪资低于 5000 的员工，和年龄大于 50 岁的员工全部查询出来

    `select * from emp where salary < 5000 union select * from emp where age > 50;`

    **注意：对于联合查询来说，多张表的列数必须保持一致，字段类型也需要保持一致**

+ 子查询

    sql 语句中嵌套了 select 语句，称为嵌套查询，又称子查询

    `select * from t1 where column1 = (select column1 from t2);`

    子查询外部的语句可以是 insert / update / delete / select 中的任意一个

    根据子查询结果的不同，分为：

    1. 标量子查询(子查询的结果为单个值)
    2. 列子查询(子查询结果为一列)
    3. 行子查询(子查询结果为一行)
    4. 表子查询(子查询结果为多行多列)

    根据子查询的位置，分为：where 之后，from 之后，select 之后

+ 列子查询：

    <img src="https://raw.githubusercontent.com/hacker-dvd/notes/master/img/image-20221219104815339.png" alt="image-20221219104815339" style="zoom:67%;" />

    例：

    1. 查询销售部和市场部的所有员工信息：

        `select * from emp where dept_id in (select id from dept where name = '销售部' or name = '市场部');`

    2. 查询比财务部所有人工资都高的员工信息

        `select * from emp where salary > all(select salary from emp where dept_id = (select id from dept where name = '财务部'))`

+ 行子查询

    例：查询与张无忌的薪资及直属领导相同的员工信息

    `select * from emp where (salary, managerid) = (select salary, managerid from emp where name = '张无忌');`

+ 表子查询

    主要的操作符为 IN

    例：查询与A，B的职位和薪资相同的员工信息

    `select * from emp where (job, salary) in (select job, salary from emp where name = 'A' or name = 'B');`

+ 查询案例：

    1. 查询年龄小于 30  岁的员工的姓名，年龄，职位，部门信息(显式内连接实现)

        `select e.name, e.age, e.job, d.name from emp e inner join dept d on e.dept_id = d.id where e.age < 30;`

    2. 查询拥有员工的部门 id，部门名称

        `select distinct  d.id, d.name from emp e, dept d where e.dept_id = d.id;`

    3. 查询所有年龄大于 40 岁的员工，及其归属的部门名称，如果员工没有分配部门，也需要展示出来

        `select e.*, d.name from emp e left outer join dept d on e.dept_id = d.id where e.age > 40;`

    4. 查询所有员工的工资等级

        `select e.*, s.grade from emp e, salgrade s where e.salary >= s.losal and e.salary <= s.hisal;`

    5. 查询研发部所有员工信息及工资等级

        `select e.*, s.grade from emp e, dept d, salgrade s where e.dept_id = d.id and (e.salary between s.losal and s.hisal) and d.name = '研发部';`

    6. 查询低于本部门平均工资的员工信息

        `select * from emp e2 where e2.salary < (select avg(e1.salary) from emp e1 where e1.dept_id = e2.dept_id);`





















