+ 模块的导入方式：`[from 模块名] import [模块 | 类 | 变量 | 函数 | *] [as 别名]`

+ 在执行当前 python 文件时，当前文件的 `__name__`会被转换为`__main__`

    因此，可以在每一个文件中写入：

    ```python
    if __name__ == '__main__':
        # 执行对应的操作
        # 这些操作只有当当前文件被执行时才会运行
    ```

+ 如果一个模块文件中有`__all__`变量，当使用`from 包名 import *`导入时，只能导入这个列表中的元素

    例如，在 test.py 文件中有：

    ```python
    __all__ = ['F1']
    def F1(): print("F1")
    def F2(): print("F2")
    ```

    当其他文件使用`from test.py import *`时，只能使用 F1 函数

    注意，`__all__一般写在__init__.py文件中`

+ python 包是一个文件夹，在该文件夹中包含了一个`__init__.py`文件和若干个模块文件

+ 安装包：

    1. `pip install numpy`

    2. `pip install -i 网址名 包名`

        例如，使用清华大学镜像站安装 numpy：

        `pip install -i https://pypi.tuna.tsinghua.edu.cn/simple numpy`

