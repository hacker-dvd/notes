+ 闭包

    定义：在函数嵌套的前提下，内部函数使用了外部函数的变量，并且外部函数返回了内部函数，我们把这个外部函数变量称为闭包

    简单闭包示例：

    ```python
    def outer(logo):
        def inner(msg):
            print(f"<{logo}>{msg}<{logo}>")
        return inner
    
    fn = outer("lc")  # 相当于获得一个 logo 固定的标签
    fn("good")
    fn("better")
    ```

    当想要在内部函数中修改外部函数的变量时，需要使用 nonlocal 关键字修饰外部函数的变量：

    ```python
    def outer(num1):
        def inner(num2):
            nonlocal num1
            num1 += num2
            print(num1)
        return inner
    
    fn = outer(10)
    fn(10)
    ```

+ 装饰器

    装饰器其实也是一种闭包，其功能就是在不破坏目标函数原有的代码和功能的前提下，为目标函数增加新功能

    ```python
    def outer(func):
        def inner():
            print("添加功能1")
            func()
            print("添加功能2")
        return inner
    def just_for_fun():
        print("hh")
    fn = outer(just_for_fun)
    fn()
    
    # 写法 2(推荐写法)
    def outer(func):
        def inner():
            print("添加功能1")
            func()
            print("添加功能2")
        return inner
    
    @outer
    def just_for_fun():
        print("hh")
    
    just_for_fun()
    ```

+ 单例模式

    定义：保证一个类中只能有一个实例，并提供一个访问它的全局访问点

    使用场景：当一个类只能有一个实例，而客户可以从一个众所周知的访问点访问它时

    实现：

    ```python
    # 在一个文件 test 定义如下示例代码：
    class strTools:
        pass
    str_tool = strTools()
    
    # 在另一个文件中导入对象
    from test import str_tool
    s1, s2 = str_tool, str_tool
    # 此时 s1, s2 实际上是一个对象，用的同一块内存空间
    ```

+ 工厂模式：

    当需要大量创建一个类的实例的时候，可以使用工厂模式

    ```python
    class Person:
        pass
    class Worker(Person):
        pass
    class Student(Person):
        pass
    class Teacher(Person):
        pass
    class Factory:
        def get_person(self, p_type):
            if p_type == 'w':
                return Worker()
            elif p_type == 's':
                return Student()
            else:
                return Teacher()
    
    factory = Factory()
    worker = factory().get_person('w')
    student = factory().get_person('s')
    teacher = factory().get_person('t')
    ```

+ 多线程编程

    语法：

    ```python
    import threading
    thread_obj = threading.Thread([group [, target [, name [, args [kwargs]]]]])
    # group:暂时无用，未来功能预留参数
    # target:执行的目标任务名
    # args:以元组的方式给执行任务传参
    # kwargs:以字典的形式给字典传参
    # name:线程名，一般不用设置
    thread_obj.start()  # 启动线程，让线程开始工作
    ```

    示例：

    ```python
    import time
    import threading
    
    def sing(msg):
        while True:
            print(msg)
            time.sleep(1)
    def dance(msg):
        while True:
            print(msg)
            time.sleep(1)
    
    sing_thread = threading.Thread(target=sing, args=("sing",))
    dance_thread = threading.Thread(target=dance, kwargs={"msg": "dancing"})
    sing_thread.start()
    dance_thread.start()
    ```

    





