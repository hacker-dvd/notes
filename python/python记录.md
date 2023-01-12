+ 设置输出结束符：

    ```python
    a = 10
    while a < 10:
        print(a, end = ',')
        a = a + 1

+ range 和 len 结合，可以按索引迭代序列：

    ```python
    a = ['Mary', 'had', 'a', 'little', 'lamb']
    for i in range(len(a)):
        print(i, a[i])
    ```

+ 字符串格式化：

    1. 使用占位符进行格式化：

        ```python
        a, b = 1, "abc"
        print("%d, %s" % (a, b))
        # 注意，多个变量需要用括号将它们括起来
        
        # 利用占位符实现字符串拼接
        s1 = "abc"
        s2 = "def%s" % s1
        ```

        **常用的占位符：%s, %d, %f，其中数字可以直接转化为字符串，即填 %d 的地方也可以填 %s**

        对浮点数进行精度控制：

        **%m.nf** 代表浮点数的宽度为 m，小数点后的位数为 n，当数的宽度不足 m 时，前面会填充空格，当数的宽度大于 m 时，m 不起作用

        注意，这种精度控制方法会对小数进行四舍五入

    2. 快速对字符串进行格式化：

        ```python
        a, b = 10, "ABC"
        # 这里的 f 代表 format
        print(f"数字为{a},字符串为{b}")
        ```
    
    3. 表达式格式化：
    
        ```python
        print("1 * 1的结果是%d" % (1 * 1));
        print(f"1 * 2的结果是{1 * 2}")
        print("字符串在 python 中的类型名是 %s" % type("字符串"))
        ```
    
+ python 数据输入

    **input 默认以回车作为分隔符，换行不作为分隔符，且输入的所有内容默认当作字符串 str 看待**

    input 内可以输入提示内容，例：

    ```python
    a = input("输入一个数字：\n")
    print(a)
    ```

    input依次输入多个值：

    ```python
    a, b = input().split()
    print(a, b)
    
    k = int(input())  # 单个元素输入时进行强制类型转化
    print(k)
    
    # 在输入的同时完成强制类型转换：
    c, d = map(int, input().split())
    print(c, d)
    ```

+ map 函数的用法：

    **map()** 会根据提供的函数对指定序列做映射。

    `map(function, iterator, ...)`

    对 iterator 中每一个元素执行 function 函数，返回值是一个列表的迭代器

    ```python
    def square(x):
        return x ** 2
    
    a = [1, 2, 3, 4, 5]
    print(list(map(square, a)))
    # 注意 map 函数这里不是对 a 本体作用，而是生成一个新的列表，在 map 函数作用完之后 a 的值仍为[1, 2, 3, 4, 5]
    ```

+ python 随机数：

    ```python
    import random
    
    # 生成一个 0 到 1 范围的浮点数
    a = random.random()
    # random.randint(l, r) 生成一个[l, r]范围内的整数
    b = random.randint(0, 10)
    # random.uniform(l, r) 生成一个[l, r]范围内的浮点数
    c = random.uniform(1.5, 2.5);
    # 设(l, r, step) 为[l, r)范围内，公差为 step 的等差数列列表
    # random.uniform(l, r, step) 返回这个列表中随机一个数据
    d = random.randrange(10, 100, 2)  # 这个语句返回[10, 100)范围内任何一个偶数
    lst = ["a", "bc", "de"]
    # random.choice() 随机返回序列中的一个数据
    e = random.choice(lst)
    # random.shuffle() 随机打乱列表的顺序
    random.shuffle(lst)
    print(a, b, c, d, e, lst)
    ```

+ 函数相关

    1. 当函数内部试图修改全局区声明的变量时，不会发生效果

        ```python
        a = 10
        def f():
            a = 100
        
        f()
        print(a)
        # 调用结束之后，a 输出的结果仍为 10，这是因为在函数内部的 a 相当于重新定义一个变量 a，与全局变量 a 没有关系
        # 要想在函数内部修改全局变量，只需将上述函数修改为：
        def f():
            global a  # 声明 a 为全局变量
            a = 100
        ```

+ 序列的切片操作：`序列[起始下标 : 结束下标 : 步长]`

    **注意，这里结束下标的值不会被取到**

    将 a 字符串倒序：`b = a[::-1]`
    
+ 如果函数需要多个返回值，只需要按照返回值的顺序，写对应舒徐的多个变量接收即可，变量之间需要用逗号隔开

    ```python
    def test():
        return 1, 2
    a, b = test()
    print(a, b)
    ```

+ 函数参数：

    1. 位置参数：调用函数时根据函数定义的参数位置来传递参数

    2. 关键字参数：函数调用时通过"键 = 值"的形式传递参数

        ```python
        def user(name, age, gender):
            print(f"名字为{name}, 年龄为{age}, 性别为{gender}")
        
        user(age = 20, name = "lc", gender = "男")
        # 可以和位置参数混用，位置参数必须在前，且匹配参数顺序
        user("llc", gender = "男", age = 21)
        ```

    3. 缺省参数(默认参数)，所有位置参数必须出现在默认参数前，包括函数的定义和调用

    4. 不定长参数(分为位置传递和关键字传递)

         ```python
         def user(*args):
             print(args)
         
         user("lc")
         user("lc", 20);
         ```

        传进的所有参数都会被 args 变量手机，它会根据传进参数的位置合并为一个元组，args 是元组类型，这就是位置传递

        ```python
        def user(**kwargs):
            print(kwargs);
        user(name = "lc", age = 20)
        # 关键字传递中，参数是"键 = 值"的形式，根据这些键值对组成字典
        ```

+ lambda 匿名函数

    语法：`lambda 传入参数 : 函数体`

    **注意：函数体只能由一行代码**













