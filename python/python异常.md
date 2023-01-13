+ 捕获常规异常

    ```python
    try:
        可能发生错误的代码
    except:
        如果出现异常执行的代码
    ```

+ 捕获指定异常

    ```python
    try:
        可能发生错误的代码
    except 异常名 as 别名：
    	如果出现此异常执行的代码
    
    # 注意，别名捕获的是上面抛出的异常，可以直接用 print 将其输出
    ```

+ 捕获多个异常：

    ```python
    try:
        print(name)
        print(1 / 0)
    except (NameError, ZeroDivisionError) as e:
        print(e)
    # 注意，这里虽然出现了两个错误，但在执行到 print(name) 时就会转到 except 输出 name 'name' is not defined
    ```

+ 捕获所有异常

    ```python
    try:
        可能发生的错误代码
    except Exception as e:
        print(e)
    ```

+ 异常 else 表示如果没有异常要执行的代码，finally 表示无论是否有异常都需要执行的代码

    ```python
    try:
        可能发生错误的代码
    except Exception as e:
        print(e)
    else:
        没有异常要执行的代码
    finally:
        必定要执行的代码
    ```

+ **异常是具有传递性的**