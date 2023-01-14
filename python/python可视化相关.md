+ json 相关

    json 的格式要么是一个单独的字典，要么是一个内部元素都为字典的列表

    ```python
    import json
    data = [{"name": "lc", "age": 16}, {"name": "llc", "age": 20}]
    
    data = json.dumps(data)
    # 如果 data 中含有中文时，需要写为 data = json.dumps(data, ensure_ascii = False)，表示不使用 ascii 码进行转换
    # 通过 json.dumps()方法将 python 数据转换为 json 数据
    data = json.loads(data)
    # 通过 json.loads()方法将 json 数据转换为 python 数据
    ```

    **注意：json字符串只能使用外边单引号内部双引号的形式，否则会出现错误**

#### pyecharts 相关

```python
from pyecharts.charts import Line
from pyecharts.options import TitleOpts, LegendOpts, ToolboxOpts,VisualMapOpts
# 标题控制选项，图例控制选项，工具箱控制选项，视觉映射选项

line = Line()  # 创建一个折线图对象
line.add_xaxis(["中国", "美国", "英国"])  # 添加 x 轴数据
line.add_yaxis("GDP", [20, 30, 10])  # 添加 y 轴数据

line.set_global_opts(
    title_opts=TitleOpts(title="GDP展示", pos_left="center", pos_bottom="1%"),  # 设置折线图的标题
    legend_opts=LegendOpts(is_show=True),  # 设置图例
    toolbox_opts=ToolboxOpts(is_show=True),  # 设置工具箱
    visualmap_opts=VisualMapOpts(is_show=True)
)

line.render()  # 通过 render 方法，将代码生成图像
```

折线图统计新冠感染人数：

```python
import json
from pyecharts.charts import Line
from pyecharts.options import  TitleOpts, LegendOpts, ToolboxOpts, VisualMapOpts, LabelOpts
f_us = open("D:\BaiduNetdiskDownload\美国.txt", "r", encoding="UTF-8")
us_data = f_us.read()
f_jp = open("D:\BaiduNetdiskDownload\日本.txt", "r", encoding="UTF-8")
jp_data = f_jp.read()
f_in = open("D:\BaiduNetdiskDownload\印度.txt", "r", encoding="UTF-8")
in_data = f_in.read()

us_data = us_data.replace("jsonp_1629344292311_69436(", "")
jp_data = jp_data.replace("jsonp_1629350871167_29498(", "")
in_data = in_data.replace("jsonp_1629350745930_63180(", "")
us_data = us_data[:-2]
jp_data = jp_data[:-2]
in_data = in_data[:-2]
us_dict = json.loads(us_data)
jp_dict = json.loads(jp_data)
in_dict = json.loads(in_data)
# 数据规范化
us_trend_data = us_dict['data'][0]['trend']
jp_trend_data = jp_dict['data'][0]['trend']
in_trend_data = in_dict['data'][0]['trend']
us_x_data = us_trend_data['updateDate'][:314]
jp_x_data = jp_trend_data['updateDate'][:314]
in_x_data = in_trend_data['updateDate'][:314]
# 将 2020 年的日期作为 x 轴
us_y_data = us_trend_data["list"][0]["data"][:314]
jp_y_data = jp_trend_data["list"][0]["data"][:314]
in_y_data = in_trend_data["list"][0]["data"][:314]
# 将 确诊人数作为 y 轴

line = Line()
line.add_xaxis(us_x_data)  # x 轴只需要一个国家的数据即可
line.add_yaxis("美国确诊人数", us_y_data, label_opts=LabelOpts(is_show=False))
line.add_yaxis("日本确诊人数", jp_y_data, label_opts=LabelOpts(is_show=False))
line.add_yaxis("印度确诊人数", in_y_data, label_opts=LabelOpts(is_show=False))
# 不显示每个点上的数据
line.set_global_opts(
    title_opts=TitleOpts(title="2020年美，日，印确诊人数", pos_left="center", pos_bottom="1%"),
    legend_opts=LegendOpts(is_show=True),
    toolbox_opts=ToolboxOpts(is_show=True),
    visualmap_opts=VisualMapOpts(is_show=True)
)

line.render()

f_us.close()
f_jp.close()
f_in.close()
```

地图统计相关：

```python
from pyecharts.charts import Map
from pyecharts.options import VisualMapOpts
map = Map()  # 构建地图对象
data = [
    ("北京市", 99),
    ("上海市", 199),
    ("湖南省", 299),
    ("广东省", 399),
    ("山西省", 499)
]
# 注意：每条数据后面必须添加省或者市才能显示数据
map.add("测试地图", data, "china");
map.set_global_opts(
    visualmap_opts=VisualMapOpts(
        is_show=True,
        is_piecewise=True,  # 开启手动校准颜色
        pieces=[
            {"min": 1, "max": 100, "label": "1 - 100", "color": "#CCFFFF"},
            {"min": 101, "max": 300, "label": "101 - 300", "color": "#FF6666"},
            {"min": 301, "max":500, "label": "301 - 500", "color": "#990033"}
        ]
        # 划定每个范围的颜色
    )
)
map.render()
```

全国疫情地图：

```python
import json
from pyecharts.charts import Map
from pyecharts.options import *
f = open("D:\BaiduNetdiskDownload\疫情.txt", "r", encoding="UTF-8")
data = f.read()
f.close()

data_dict = json.loads(data)
data_list = []  # 要构建地图的列表数据
province_data_list = data_dict["areaTree"][0]["children"]
for province_data in province_data_list:
    province_name = province_data["name"]
    province_confirm = province_data["total"]["confirm"]
    data_list.append((province_name, province_confirm))

map = Map()
map.add("各省份确诊人数", data_list, "china")
map.set_global_opts(
    title_opts=TitleOpts(title="全国疫情地图"),
    visualmap_opts=VisualMapOpts(
        is_show=True,
        is_piecewise=True,
        pieces=[
            {"min": 1, "max": 99, "label": "1 - 99人", "color": "#124356"},
            {"min": 100, "max": 999, "label": "100 - 999人", "color": "#4e5612"},
            {"min": 1000, "max": 4999, "label": "1000 - 4999人", "color": "#564b12"},
            {"min": 5000, "max": 9999, "label": "5000 - 9999人", "color": "##563a12"},
            {"min": 10000, "max": 99999, "label": "10000 - 99999人", "color": "#562612"},
            {"min": 100000, "label": "100000人以上", "color": "#561218"},
        ]
    )
)
map.render("全国疫情地图.html")
```

基础柱状图创建：

```python
from pyecharts.charts import Bar
from pyecharts.options import LabelOpts
bar = Bar()
bar.add_xaxis(["中国", "美国", "英国"])
bar.add_yaxis("GDP", [20, 30, 10], label_opts=LabelOpts(position="right"))
# 将数字标签放在右侧
bar.reversal_axis()
# 反转 x 轴和 y轴
bar.render()
```

柱状图颜色：

<img src="https://raw.githubusercontent.com/hacker-dvd/notes/master/img/202301131612546.png" style="zoom: 80%;" />

动态柱状图绘制：

```python
from pyecharts.charts import Bar, Timeline
from pyecharts.options import LabelOpts
from pyecharts.globals import ThemeType
bar1 = Bar()
bar1.add_xaxis(["中国", "美国", "英国"])
bar1.add_yaxis("GDP", [20, 30, 10], label_opts=LabelOpts(position="right"))
bar1.reversal_axis()

bar2 = Bar()
bar2.add_xaxis(["中国", "美国", "英国"])
bar2.add_yaxis("GDP", [30, 50, 20], label_opts=LabelOpts(position="right"))
bar2.reversal_axis()

bar3 = Bar()
bar3.add_xaxis(["中国", "美国", "英国"])
bar3.add_yaxis("GDP", [40, 60, 30], label_opts=LabelOpts(position="right"))
bar3.reversal_axis()

timeline = Timeline({"theme": ThemeType.LIGHT})
# 设置主题颜色
timeline.add(bar1, "点1")
timeline.add(bar2, "点2")
timeline.add(bar3, "点3")

timeline.add_schema(
    play_interval=1000,     # 自动播放的时间间隔，单位为毫秒
    is_timeline_show=True,  # 是否在自动播放的时候，选择时间线
    is_auto_play=True,      # 是否自动播放
    is_loop_play=True       # 是否循环播放
)
timeline.render()
```

GDP 动态柱状图绘制：

```python
from pyecharts.charts import Bar, Timeline
from pyecharts.options import *
from pyecharts.globals import ThemeType

f = open("D:/BaiduNetdiskDownload/1960-2019全球GDP数据.csv", "r", encoding="GB2312")
# ANSI 型的文件的文件解码格式为 GB2312
data_lines = f.readlines()
f.close()
data_lines.pop(0)

data_dict = {}
for line in data_lines:
    year = int(line.split(",")[0])
    country = line.split(",")[1]
    gdp = float(line.split(",")[2])  # 将科学计数法转换为浮点数
    try:
        data_dict[year].append([country, gdp])
    except KeyError:
        data_dict[year] = []
        data_dict[year].append([country, gdp])

timeline = Timeline({"theme": ThemeType.LIGHT})
sorted_year_list = sorted(data_dict.keys())  # 对年份进行排序

for year in sorted_year_list:
    data_dict[year].sort(key=lambda element: element[1], reverse=True)
    year_data = data_dict[year][0:8]  # 每一年 gdp 前 8 的国家和相应的 gdp
    x_data, y_data = [], []
    for tt in year_data:
        x_data.append(tt[0])
        y_data.append(tt[1] / 100000000)

    x_data.reverse()
    y_data.reverse()
    # 将 x 轴的数据和 y 轴的数据反转，使得 GDP 高的数据位于图表上方
    bar = Bar()
    bar.add_xaxis(x_data)
    bar.add_yaxis("GDP(亿)", y_data, label_opts=LabelOpts(position="right"))
    bar.reversal_axis()
    timeline.add(bar, str(year))
    bar.set_global_opts(
        title_opts=TitleOpts(title=f"{year}全球前8GDP")
        # 设置每一年图标的标题
    )

timeline.add_schema(
    play_interval=1000,     # 自动播放的时间间隔，单位为毫秒
    is_timeline_show=True,  # 是否在自动播放的时候，选择时间线
    is_auto_play=True,      # 是否自动播放
    is_loop_play=True       # 是否循环播放
)
timeline.render()
```

数据销售额统计：

```python
import json
from pyecharts.charts import Bar
from pyecharts.options import LabelOpts, TitleOpts, InitOpts
from pyecharts.globals import ThemeType

class Record:
    def __init__(self, date, order_id, money, province):
        self.date = date
        self.order_id = order_id
        self.money = money
        self.province = province
    def __str__(self):
        return f"{self.date}, {self.order_id}, {self.money}, {self.province}"

class FileReader:
    def read_data(self) -> list[Record]:
        pass

class TextFileReader(FileReader):
    def __init__(self, path):
        self.path = path
    def read_data(self) -> list[Record]:
        f = open(self.path, "r", encoding="UTF-8")
        record_list: list[Record] = []
        for line in f.readlines():
            line = line.strip()
            data_list = line.split(",")
            record = Record(data_list[0], data_list[1], int(data_list[2]), data_list[3])
            record_list.append(record)

        f.close()
        return record_list

class JsonFileReader(FileReader):
    def __init__(self, path):
        self.path = path
    def read_data(self) -> list[Record]:
        f = open(self.path, "r", encoding="UTF-8")
        record_list: list[Record] = []
        for line in f.readlines():
            data_dict = json.loads(line)
            record = Record(data_dict["date"], data_dict["order_id"], int(data_dict["money"]), data_dict["province"])
            record_list.append(record)

        f.close()
        return record_list

if __name__ == '__main__':
    text_file_reader = TextFileReader("D:/BaiduNetdiskDownload/2011年1月销售数据.txt")
    json_file_reader = JsonFileReader("D:/BaiduNetdiskDownload/2011年2月销售数据JSON.txt")
    jan_data: list[Record] = text_file_reader.read_data()  # 1 月份的数据
    feb_data: list[Record] = json_file_reader.read_data()  # 2 月份的数据
    all_data: list[Record] = jan_data + feb_data  # 将两个月数据合并

    data_dict = {}
    # 计算每个日期对应的销售额之和
    for record in all_data:
        if record.date in data_dict.keys():
            data_dict[record.date] += record.money
        else:
            data_dict[record.date] = record.money

    bar = Bar(init_opts=InitOpts(theme=ThemeType.LIGHT))
    bar.add_xaxis(list(data_dict.keys()))
    bar.add_yaxis("销售额", list(data_dict.values()), label_opts=LabelOpts(is_show=False))

    bar.set_global_opts(
        title_opts=TitleOpts(title="每日销售额", pos_left="center", pos_bottom="1%")
    )

    bar.render()
```



