**lua 学习网站：[lua](https://luatos.com/)**

+ lua 声明变量和python相似，均为`a = 1`这种直接声明即可，但所有声明的变量默认都是全局变量，想要声明局部变量需要使用`local a = 1`

    给多个变量同时赋值：`a, b = 1, 2`

+ lua 所有未被声明的变量的值均为`nil`

+ lua 支持16进制表示法，定义`a = 0x11`，打印a的值的结果为17，也支持科学计数法：`a = 1e10`

+ lua 支持乘幂运算：`print(a ^ 3)`(输出a的立方)，也支持左移，右移运算

+ lua 的字符串用双引号和单引号都可以，当想要输出多行文本时，可以采用两个中括号，里面的内容会保持原样输出

    ```lua
    a = [[
    1234567
    abcde
    hello
    ]]
    print(a)
    ```

    lua 两个字符串的连接不是靠 + ，而是靠 .. 进行连接

    ```lua
    a, b = "hello", "world"
    c = a..b
    print(c)
    ```

+ lua将数字转换为字符串：`a = tostring(10)`，这样就声明了一个为 10 的字符串

    将字符串转为数字：`a = tonumber("10")`，这样 a 即为一个值为10的数字，若转换失败，a 的值为 nil

+ lua 获取字符串的长度：在字符串前面添加 #

    ```lua
    a = "hello"
    print(#a)
    ```

+ lua 注释

    ```lua
    单行注释：
    --这是一个单行注释
    
    多行注释：
    --[[
    123
    456
    这里面的内容都会被注释
    ]]
    ```

+ lua 函数写法：

    ```lua
    --或者写为add = function(a, b)
    function add(a, b)
        print(a + b)
    end
    
    a, b = 10, 20
    add(a, b)
    ```

    lua 函数的返回值如果不指定的化默认返回为 nil，也可以显式的用 return 来指定返回值

    lua 函数的返回值可以为多个：

    ```lua
    function f(a, b, c)
        return a, b
    end
    
    local a, b = f(1, 2)
    print(a, b)
    ```

+ lua 的 table (类似于数组) 的形式和 python 类似，其中的元素可以是任意类型，用逗号进行分隔，**但下标从 1 开始**：

    ```lua
    a = {1, "ab", 2, {1, 2, 4}, function() end}
    print(a[1], a[4][2])
    ```

    如果对应的下标没有进行初始化的话，那么它的值就为 nil，我们可以直接通过下标对其进行赋值：`a[10] = 10`就可以直接对位置为 10 的地方赋值

    需要注意中间没有赋值的元素的值仍为 nil

+ 获取数组的长度(数组的元素个数)：`print(#a)`

+ table 插入元素，删除元素的方法

    ```lua
    a = {1, "ab", 2, {1, 2, 4}, function() end}
    a[10] = 11
    --在 table 的第一个未赋值的位置插入元素：
    table.insert(a, "123")
    print(a[6], a[11])
    --这里a[6]的值为刚赋值的123，而a[11]的值未被赋值
    
    --在指定位置插入元素，之后的元素会顺位后移
    --在第二个位置插入元素
    table.insert(a, 2, "hello")
    
    --在指定位置删除元素的方法，之后的元素会顺位前移
    --在删除元素之后，还会将删除的元素返回
    print(table.remove(a, 2))
    ```

+ table 以字符串作为下标

    ```lua
    a = {
        t1 = 1,
        t2 = "hello",
        3,
        ["my"] = "lc"
        t3 = 114514
    }
    --要注意输出时用引号括起来
    print(a["t1"], a[1])
    print(a["my"])
    --也可以用 . 运算符进行输出
    print(a.t3)
    ```

+ 全局表 _G

    在 lua 中所有的全局变量都会存在全局表 _G中，它是一个 table，例如，定义`a = 1`，那么`print(_G["a"])`输出的值就为 1

+ lua 中也存在 bool 类型 true 和 false，也存在`print(1 > 2)`这种表达式

+ **lua 中不等于的表示方式为`~=`**

+ lua 中与，或，非的表示形式为and，or，not

    lua 中只有 false 和 nil 代表假，其他值都代表真(0也代表真)

+ lua 判断语句：

    ```lua
    if condition1 then
        block1
    elseif condition2 then
        block2
    else
        block3
    end
    ```

+ lua 循环语句：

    ```lua
    --for循环
    for i = 1, 10, 2 do  --1, 10, 2分别代表开始值，结束值，步长
        print(i)
    --在循环期间可以使用break进行跳出
    --注意，在for循环期间更改循环的值时无效的，它会默认新创建一个 local 变量 i
    end
    ```

    ```lua
    --while循环
    local n = 10
    while n > 1 do
        print(i)
        n = n - 1  --注意 lua 不存在 ++ 和 -- 操作
    end
    ```

+ lua 直接将 ASCII 转换为字符

    ```lua
    s = string.char(0x30, 0x31, 0x32, 0x33)
    print(s)
    print(string.byte(s, 2))  --直接取出字符串的某一位
    ```

     

