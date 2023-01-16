想要使用 PySpark 库完成数据处理，首先需要构建一个执行环境入口对象

PySpark 的执行环境入口对象是类 SparkContext 的类对象，通过 SparkContext 对象，完成数据输入，输入数据后得到 RDD(弹性分布式数据集)对象，对 RDD

对象进行迭代计算。最终通过 RDD 对象的成员方法，完成数据的输出工作

```python
from pyspark import SparkConf, SparkContext

# 创建 SparkConf 类对象
conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
# 基于 SparkConf 类对象创建 SparkContext 类对象
sc = SparkContext(conf=conf)
# 打印 PySpark 的运行版本
print(sc.version)
# 停止 SparkContext 对象的运行(停止 PySpark 程序)
sc.stop()
```

+ PySpark 支持通过 SparkContext 对象的 parallelize 成员方法，将 list / tuple / set / dict / str 转换为 PySpark 的 RDD 对象

    ```python
    from pyspark import SparkConf, SparkContext
    
    conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
    sc = SparkContext(conf=conf)
    
    rdd1 = sc.parallelize([1, 2, 3, 4, 5])
    rdd2 = sc.parallelize((1, 2, 3, 4, 5))
    rdd3 = sc.parallelize("abcdefg")
    rdd4 = sc.parallelize({1, 2, 3, 4, 5})
    rdd5 = sc.parallelize({"key1": "value1", "key2": "value2"})
    
    # 当想查看 RDD 里面有什么内容时，需要使用 collect 方法
    print(rdd1.collect())
    print(rdd2.collect())
    print(rdd3.collect())
    print(rdd4.collect())
    print(rdd5.collect())
    
    sc.stop()
    ```

    **注意：RDD 对象会将字符串的每个字符拆开保存，且对于 dict 只会保存 key 值**

+ PySpark 也支持通过 SparkContext 入口对象，来读取文件，来构建出 RDD 对象

    ```python
    from pyspark import SparkConf, SparkContext
    
    conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
    sc = SparkContext(conf=conf)
    
    rdd = sc.textFile("D:/BaiduNetdiskDownload/2011年1月销售数据.txt")
    print(rdd.collect())
    
    sc.stop()
    ```

+ map 算子

    map 算子是将 RDD 的数据一条条处理(处理的逻辑是基于 map 算子中接收的处理函数)，返回新的 RDD

    ```python
    # 语法
    rdd.map(func)
    # func: f:(T) -> U
    # f:表示这是一个函数(方式)
    # (T) -> U 表示的是方法的定义
    # ()表示传入参数,(T)表示传入一个参数，()表示没有传入参数
    # T，U都是泛型代称，表示任何类型
    # -> U 表示返回值
    # (A) -> (A) 表示这是接受一个参数传入的方法，传入参数类型不限，返回一个返回值，返回值和传入参数类型一致
    ```

    例子：

    ```python
    from pyspark import SparkConf, SparkContext
    import os
    # PySpark 无法自己找到 python 解释器的位置，需要自己手动指定
    os.environ['PYSPARK_PYTHON'] = "C:/Users/Thinkpad/AppData/Local/Programs/Python/Python310/python.exe"
    conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
    sc = SparkContext(conf=conf)
    
    rdd1 = sc.parallelize([1, 2, 3, 4, 5])
    # RDD 是支持链式调用的，可以在同一行连续进行修改
    rdd2 = rdd1.map(lambda x: x * 10).map(lambda x: x + 5)
    
    print(rdd2.collect())
    
    sc.stop()
    ```

+ flatMap 算子

    flatMap 算子用来解除嵌套

    ```python
    # 嵌套的 list
    lst = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
    # 如果解除了嵌套
    lst = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    ```

    示例：

    ```python
    from pyspark import SparkConf, SparkContext
    import os
    # PySpark 无法自己找到 python 解释器的位置，需要自己手动指定
    os.environ['PYSPARK_PYTHON'] = "C:/Users/Thinkpad/AppData/Local/Programs/Python/Python310/python.exe"
    conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
    sc = SparkContext(conf=conf)
    
    rdd = sc.parallelize(["a b c", "de f g", "h, lk"])
    print(rdd.flatMap(lambda x: x.split(" ")).collect())
    
    sc.stop()
    ```

+ reduceByKey 算子

    功能：针对 KV 型 RDD(即二元元组)，自动按照 key 分组，然后根据自身提供的聚合逻辑，完成组内数据的聚合操作

    语法：

    ```python
    rdd.reduceByKey(func)
    # func: (V, V) -> V
    # 接受 2 个传入参数(类型要一致)，返回一个返回值，类型和传入要求一致
    ```

    示例：

    ```python
    from pyspark import SparkConf, SparkContext
    import os
    # PySpark 无法自己找到 python 解释器的位置，需要自己手动指定
    os.environ['PYSPARK_PYTHON'] = "C:/Users/Thinkpad/AppData/Local/Programs/Python/Python310/python.exe"
    conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
    sc = SparkContext(conf=conf)
    
    rdd = sc.parallelize([('a', 1), ('a', 1), ('b', 1), ('b', 1), ('b', 1)])
    result = rdd.reduceByKey(lambda a, b: a + b)
    print(result.collect())  # 结果为[('b', 3), ('a', 2)]
    
    sc.stop()
    ```

给定一个文本，统计每个单词出现的次数：

```python
from pyspark import SparkConf, SparkContext
import os
# PySpark 无法自己找到 python 解释器的位置，需要自己手动指定
os.environ['PYSPARK_PYTHON'] = "C:/Users/Thinkpad/AppData/Local/Programs/Python/Python310/python.exe"
conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
sc = SparkContext(conf=conf)

rdd = sc.textFile("D:/BaiduNetdiskDownload/tt.txt")
# 注意此处需要使用 flatMap，因为文本文件会根据换行将文本划分为一个个列表，因此需要解嵌套
word_rdd = rdd.flatMap(lambda x: x.split(" "))  # 取出所有的单词
# 将所有的单词转换为二元元组，单词为 key，value 设为 1
rdd1 = word_rdd.map(lambda word: (word, 1))
rdd2 = rdd1.reduceByKey(lambda a, b: a + b)
rdd3 = rdd2.sortBy(lambda x: x[1], ascending=False, numPartitions=1)
print(rdd3.collect())
sc.stop()
```

+ filter 算子

    ```python
    # 功能：过滤掉你不想要的数据
    rdd.filter(func)
    # func: (T) -> bool
    # 返回是 True 的数据被保留，False 的数据被丢弃
    ```

    示例：

    ```python
    from pyspark import SparkConf, SparkContext
    import os
    # PySpark 无法自己找到 python 解释器的位置，需要自己手动指定
    os.environ['PYSPARK_PYTHON'] = "C:/Users/Thinkpad/AppData/Local/Programs/Python/Python310/python.exe"
    conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
    sc = SparkContext(conf=conf)
    
    rdd1 = sc.parallelize([1, 2, 3, 4, 5])
    # 只对偶数进行保留
    rdd2 = rdd1.filter(lambda num: num % 2 == 0)
    print(rdd2.collect())
    sc.stop()
    ```

+ distinct 算子

    功能：对 RDD 数据进行去重，返回新的 RDD

    ```python
    from pyspark import SparkConf, SparkContext
    import os
    # PySpark 无法自己找到 python 解释器的位置，需要自己手动指定
    os.environ['PYSPARK_PYTHON'] = "C:/Users/Thinkpad/AppData/Local/Programs/Python/Python310/python.exe"
    conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
    sc = SparkContext(conf=conf)
    
    rdd = sc.parallelize([1, 1, 2, 3, 4, 4, 4, 5, 6])
    print(rdd.distinct().collect())
    sc.stop()
    ```

+ sortBy 算子

    功能：对 RDD 数据进行排序，基于你指定的排序依据

    语法

    ```python
    rdd.sortBy(func, ascending=False, numPartition=1)
    # func: (T) -> U:告知按照 rdd 中的哪个数据进行排序，比如 lambda x: x[1]表示按照 rdd 中的第二列元素进行排序
    # ascending True 升序 False 降序
    # numPartition:用多少个分区进行排序
    ```

统计商品相关信息：

```python
from pyspark import SparkConf, SparkContext
import os
import json
# PySpark 无法自己找到 python 解释器的位置，需要自己手动指定
os.environ['PYSPARK_PYTHON'] = "C:/Users/Thinkpad/AppData/Local/Programs/Python/Python310/python.exe"
conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
sc = SparkContext(conf=conf)

file_rdd = sc.textFile("D:/BaiduNetdiskDownload/orders.txt")
json_str_rdd = file_rdd.flatMap(lambda x: x.split("|"))
dict_rdd = json_str_rdd.map(lambda x: json.loads(x))  # 将 json 字符串转换为字典
# 取出城市和销售额数据
city_with_money_rdd = dict_rdd.map(lambda x: (x['areaName'], int(x['money'])))
# 按城市分组按销售额聚合
city_result_rdd = city_with_money_rdd.reduceByKey(lambda a, b: a + b)
# 按销售额降序排序
result_rdd1 = city_result_rdd.sortBy(lambda x: x[1], ascending=False, numPartitions=1)
# print(result_rdd1.collect())
# 取出全部商品类别
category_rdd = dict_rdd.map(lambda x: x['category']).distinct()
# print(category_rdd.collect())
# 过滤出北京市的数据
Beijing_data_rdd = dict_rdd.filter(lambda x: x['areaName'] == '北京')
# 取出全部商品类别
result_rdd2 = Beijing_data_rdd.map(lambda x: x['category']).distinct()
print(result_rdd2.collect())

sc.stop()
```

+ collect 算子

    功能：将 RDD 各个分区内的数据，同一收集到 driver 中，形成一个 list 对象

+ reduce 算子

    ```python
    # 语法：
    rdd.reduce(func)
    # func: (T, T) -> T
    # 返回值和参数要求类型一致
    
    # 示例：
    rdd = sc.parallelize(range(1, 10))
    # 将 rdd 的数据进行累加求和
    print(rdd.reduce(lambda a, b: a + b))
    ```

+ take 算子

    功能：取出 RDD 的前 N 个元素，组合成 list 并返回

    ```python
    print(sc.parallelize([3, 2, 1, 4, 5, 6]).take(5))
    # 结果为[3, 2, 1, 4, 5]
    ```

+ count 算子

    功能：计算 RDD 中有多少条数据，返回值是一个数字

    ```python
    print(sc.parallelize([3, 2, 1, 4, 5, 6])).count()
    # 结果为 6
    ```

+ saveAsTextFile 算子

    功能：将 RDD 的数据写入到文本文件中

    修改 rdd 分区为 1 个(将所有内容输入到一个文件中)：

    1. 将 SparkConf 对象设置属性全局并行度为 1

        ```python
        conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
        conf.set("spark.default.parallelism", "1")
        sc = SparkContext(conf=conf)
        ```

    2. 创建 RDD 的时候设置(parallelize 方法传入 numSlices 参数为 1)

        ```python
        rdd1 = sc.parallelize([1, 2, 3, 4, 5], 1)
        或
        rdd1 = sc.parallelize([1, 2, 3, 4, 5], numSlices=1)
        ```

    示例代码：
    ```python
    from pyspark import SparkConf, SparkContext
    import os
    import json
    # PySpark 无法自己找到 python 解释器的位置，需要自己手动指定
    os.environ['PYSPARK_PYTHON'] = "C:/Users/Thinkpad/AppData/Local/Programs/Python/Python310/python.exe"
    os.environ['HADOOP_HOME'] = "D:/hadoop/hadoop-3.0.0"
    conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
    # 将 SparkConf 对象设置属性全局并行度为 1
    conf.set("spark.default.parallelism", "1")
    sc = SparkContext(conf=conf)
    
    rdd1 = sc.parallelize([1, 2, 3, 4, 5])
    rdd2 = sc.parallelize([("hello", 3), ("spark", 5), ("hi", 10)])
    rdd3 = sc.parallelize([[1, 3, 5], [6, 7, 9], [11, 13, 15]])
    
    rdd1.saveAsTextFile("D:/output1")
    rdd2.saveAsTextFile("D:/output2")
    rdd3.saveAsTextFile("D:/output3")
    
    sc.stop()
    ```

搜索热度统计案例：

```python
from pyspark import SparkConf, SparkContext
import os
import json
# PySpark 无法自己找到 python 解释器的位置，需要自己手动指定
os.environ['PYSPARK_PYTHON'] = "C:/Users/Thinkpad/AppData/Local/Programs/Python/Python310/python.exe"
os.environ['HADOOP_HOME'] = "D:/hadoop/hadoop-3.0.0"
conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
# 将 SparkConf 对象设置属性全局并行度为 1
conf.set("spark.default.parallelism", "1")
sc = SparkContext(conf=conf)

file_rdd = sc.textFile("D:/BaiduNetdiskDownload/search_log.txt")
# 求以小时为精度的热门搜索时间 TOP3
# 1.取出全部的时间并转换为小时
# 2.转换为小时的二元元组
# 3.用 key 分组聚合 value
# 4.降序排序
# 5.取前三
result1 = file_rdd.map(lambda x: (x.split("\t")[0][:2], 1)).reduceByKey(lambda a, b: a + b).\
    sortBy(lambda x: x[1], ascending=False, numPartitions=1).take(3)
print(result1)
# 求热门搜索词 TOP3
result2 = file_rdd.map(lambda x: (x.split("\t")[2], 1)).reduceByKey(lambda a, b: a + b).\
    sortBy(lambda x: x[1], ascending=False, numPartitions=1).take(3)
print(result2)
# 统计黑马程序员关键词在什么时候搜索最多
result3 = file_rdd.map(lambda x: x.split("\t")).filter(lambda x: x[2] == '黑马程序员').map(lambda x: (x[0][:2], 1)).\
    reduceByKey(lambda a, b: a + b).sortBy(lambda x: x[1], ascending=False, numPartitions=1).take(1)
print(result3)

# 将 RDD 转换为 json 格式并以文件的形式输出，需要先将其转化为一个字典
file_rdd.map(lambda x: x.split("\t")).\
    map(lambda x: {"time": x[0], "user_id": x[1], "key_word": x[2], "rank1": x[3], "rank2": x[4], "URL": x[5]}).\
    saveAsTextFile("D:/output_json")

sc.stop()
```











