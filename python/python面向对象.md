+ 类的成员方法的定义语法：

    ```python
    def 方法名(self, 形参1, ..., 形参n):
        方法体
    ```

    在方法内部，如果想要访问类的成员变量，必须使用 self，但在传入参数的时候，self 是透明的，可以不用管它

    ```python
    class Student:
        name = None
        age = None
        def print_message(self):
            print(f"姓名为{self.name}，年龄为{self.age}")
    ```

+ 构造方法

    python 类可以使用：`__init__()`方法，称之为构造方法，可以实现：

    1. 在创建类对象(构造类)的时候，会自动执行
    2. 在创建类对象(构造类)的时候，将传入参数自动传递给`__init__`方法使用

    ```python
    class Student:
        name = None
        age = None
        tel = None
        def __init__(self, name, age, tel):
            self.name = name
            self.age = age
            self.tel = tel
            print("构造方法的调用")
    
    stu = Student("lc", 20, "123456")
    ```

+ `__str__`字符串方法：控制类转换为字符串的行为

+ `__lt__`小于符号比较方法

    传入参数：other，另一个类对象

    返回值：True 或 False

+ `__le__`小于等于比较方法

    传入参数：other，另一个类对象

    返回值：True 或 False

+ `__eq__`等于比较方法

    传入参数：other，另一个类对象

    返回值：True 或 False

    不实现`__eq__`方法，对象之间可以比较，但是比较的是内存地址，因此不同对象比较的结果一定是 False

    实现了`__eq__`方法，就可以按照自定义规则判断两个对象是否相等了

```python
class Student:
    name = None
    age = None
    def __init__(self, name, age):
        self.name = name
        self.age = age
    def __str__(self):
        return f"Student类对象,name = {self.name}, age = {self.age}"
    def __lt__(self, other):
        return self.age < other.age
    def __le__(self, other):
        return self.age <= other.age
    def __eq__(self, other):
        return self.age == other.age

stu1 = Student("lc", 20)
print(stu1)
print(str(stu1))
# 若不写__str__方法，则两者输出的都是该对象的内存地址
# 若写了__str__方法，则两者输出的都是 Student类对象,name = lc,age = 20
stu2 = Student("llc", 21)
print(stu1 < stu2)   # 结果为 True
print(stu1 > stu2)   # 结果为 False
print(stu1 <= stu2)  # 结果为 True
print(stu1 >= stu2)  # 结果为 False
print(stu1 == stu2)  # 结果为 False
```

+ 封装

    私有的成员变量和成员方法的命名都以两个下划线开始

    ```python
    class Phone:
        __current_voltage = 1  # 当前手机电压
        def __keep_single_core(self):
            print("让 CPU 以单核模式运行")
        def call_by_5g(self):
            if self.__current_voltage >= 1:
                print("right")
            else:
                self.__keep_single_core()
                print("wrong")
    
    phone = Phone()
    phone.call_by_5g()
    ```

+ 继承

    ```python
    class 类名(父类1, 父类2, ..., 父类n):
        类内容体
    ```

    多个父类中，如果有同名的成员，那么默认以继承顺序(从左到右)为优先级，即先继承的保留，后继承的被覆盖

    子类继承父类的成员属性和成员变量时，如果对其不满意，那么可以对其进行复写(即在子类中重新定义同名的属性或方法即可)

    ```python
    class Phone:
        producer = None
        def info(self):
            pass
    
    class myPhone(Phone):
        producer = "apple"
        def info(self):
            print("I love HuaWei")
    
    tt = myPhone()
    print(tt.producer)
    tt.info()
    ```

    如果子类需要使用被复写的父类的成员，需要特殊的调用方式

    使用成员变量：`父类名.成员变量`或`super().成员变量`

    使用成员方法：`父类名.成员方法(self)`或`super().成员方法`

+ 类型注解

    类型：

    1. 变量的类型注解
    2. 函数(方法)形参列表和返回值的类型注解

    变量类型注解语法：

    1. `变量名: 类型名 = 值`(类型可以为容器或类)
    2. 通过注释进行类型注解：`# type: 类型名`

    函数类型注解语法：

    ```python
    def 函数方法名(形参名: 类型, 形参名: 类型, ...) -> 返回值类型:
        pass
    ```

    使用`Union[类型, ..., 类型]`可以定义联合类型注解：

    ```python
    from typing import Union
    my_list: list[Union[str, int]] = [1, 2, "a", "b"]
    my_dict: dict[str, Union[str, int]] = {"name": "lc", "age": 20}
    
    def func(data: Union[int, str]) -> Union[int, str]:
        pass
    ```

+ 多态：完成某个行为时，使用不同的对象会得到不同的状态

    常用于函数(方法)形参声明接受父类对象，实际传入父类的子类对象进行工作(即以父类做定义声明，以子类做实际工作，用以获得同一行为的不同状态)

    ```python
    class Animal:
        def speak(self):
            pass
    
    class Dog(Animal):
        def speak(self):
            print("汪汪汪")
    
    class Cat(Animal):
        def speak(self):
            print("喵喵喵")
    
    def make_noise(animal: Animal):
        animal.speak()
    
    dog = Dog()
    cat = Cat()
    
    make_noise(dog)
    make_noise(cat)
    ```

    这种设计的含义是父类用于确定有哪些方法，具体的方法实现由子类自行决定

    抽象类：含有抽象方法的类称为抽象类

    抽象方法：方法体是空实现的(pass)称为抽象方法



















