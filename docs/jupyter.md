# 通过Jupyter链接Excel与python

使用AI交互生成python代码，仅仅需要微小的改动，即可成功运行

我们可以在Jupyter中用python代码分析表格数据，制作图表...

本篇请结合视频和代码食用，[代码下载](https://ifwz.lanzouw.com/i73ge16wg2wf)，使用jupyter打开即可

## Excel数据传给Jupyter

视频演示

<iframe frameborder="0" src="https://api.paugram.com/bili?bv=BV1mP411k7Ms&style=gray" style="height: 160px; width: 100%"></iframe>


```python
from xlwings import load
load()  #从表格读取数据
```

获取单元格内容

```python
import os
import xlwings as xw
# xw.Book()  # 新建一个文档
# xw.Book('test.xlsx')  # 打开一个已有的文档
sh1=xw.Book('test.xlsx').sheets[0] #sheet1

print(sh1.range('A3').value) # 读取指定单元格的数据，这里读的是A3
```

改变单元格内容

```python
sh1.range('B2').value = 6 # 给指定单元格赋值，这里赋值的是B2
```

使用xlwings绘制图表（非图片）

```python
chart1 =sh1.charts.add(100,100) #添加表格
chart1.chart_type = 'xy_scatter_lines_no_markers' #设置图标类型是xy散点连线图
chart1.set_source_data(ws1.range("A2:A2396,B2:B2396")) #A列与B列
```

xlwings绘制图表支持类型 （具体见xlwings的[官方文档](https://docs.xlwings.org/zh_CN/latest/quickstart.html)）

```
3d_area, 3d_area_stacked, 3d_area_stacked_100, 3d_bar_clustered, 3d_bar_stacked, 3d_bar_stacked_100, 3d_column, 3d_column_clustered, 3d_column_stacked, 3d_column_stacked_100, 3d_line, 3d_pie, 3d_pie_exploded, area, area_stacked, area_stacked_100, bar_clustered, bar_of_pie, bar_stacked, bar_stacked_100, bubble, bubble_3d_effect, column_clustered, column_stacked, column_stacked_100, combination, cone_bar_clustered, cone_bar_stacked, cone_bar_stacked_100, cone_col, cone_col_clustered, cone_col_stacked, cone_col_stacked_100, cylinder_bar_clustered, cylinder_bar_stacked, cylinder_bar_stacked_100, cylinder_col, cylinder_col_clustered, cylinder_col_stacked, cylinder_col_stacked_100, doughnut, doughnut_exploded, line, line_markers, line_markers_stacked, line_markers_stacked_100, line_stacked, line_stacked_100, pie, pie_exploded, pie_of_pie, pyramid_bar_clustered, pyramid_bar_stacked, pyramid_bar_stacked_100, pyramid_col, pyramid_col_clustered, pyramid_col_stacked, pyramid_col_stacked_100, radar, radar_filled, radar_markers, stock_hlc, stock_ohlc, stock_vhlc, stock_vohlc, surface, surface_top_view, surface_top_view_wireframe, surface_wireframe, xy_scatter, xy_scatter_lines, xy_scatter_lines_no_markers, xy_scatter_smooth, xy_scatter_smooth_no_markers
```

## 将数据传回表格

视频演示

<iframe frameborder="0" src="https://api.paugram.com/bili?bv=BV1iW4y1Z7hT&style=gray" style="height: 160px; width: 100%"></iframe>

```python
import pandas as pd
from xlwings import view

df=pd.DataFrame(data={'one':[0,1,2,3],'two':[5,6,7,8]})
df
```

将数据传回表格，仅用这一个函数即可

```
view(df)  #传回表格，在表格中显示
```

## 使用几种python绘图库绘制图表

### 使用matplotlib绘制图表添加到表格

视频演示

<iframe frameborder="0" src="https://api.paugram.com/bili?bv=BV1yX4y1Y78y&style=gray" style="height: 160px; width: 100%"></iframe>

```python
# 绘制图表
import matplotlib.pyplot as plt
import xlwings as xw
fig = plt.figure()
plt.plot([1, 2, 3])

sheet = xw.Book('test.xlsx').sheets[0]
sheet.pictures.add(fig, name='MyPlot', update=True)
```

### 使用其他绘图库例子

视频演示

<iframe frameborder="0" src="https://api.paugram.com/bili?bv=BV1wV4y187Hn&style=gray" style="height: 160px; width: 100%"></iframe>

**bokeh**

```python
from bokeh.plotting import figure, output_file, show
# 准备数据
x = [1, 2, 3, 4, 5]
y = [6, 7, 2, 4, 5]
#在notbook中展示
#output_notebook()
# 创建一个带有标题和轴标签的图表
p = figure(title="simple line example", x_axis_label='x', y_axis_label='y')
# 添加一个带有图例和线条粗细的线条渲染器
p.line(x, y, line_width=2)
# 展示结果
show(p)
```

**seaborn**

```python
import seaborn as sns
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt

# 定义一个绘图函数
def sinplot(flip=1):
    x = np.linspace(0, 14, 100)
    for i in range(1, 7):
        plt.plot(x, np.sin(x + i*.5)*(7-i)*flip)
sns.set()  # 使用seaborn的默认设置
sinplot()
plt.show()
```

**plotly**

```python
import plotly.graph_objects as go # 导入plotly.graph_objects
import numpy as np

# 生成数据
t = np.linspace(0, 10, 100)  # 生成0到10之间的100个数字
y = np.sin(t)                # 求正弦值

# 绘图
data=go.Scatter(x=t,y=y,mode='markers')# 调用Scatter函数，并设置模式为散点图
fig = go.Figure(data)      # 将散点图放在图层上
fig.show()   # 显示绘图
```
### 生成词云


视频演示

<iframe frameborder="0" src="https://api.paugram.com/bili?bv=BV1pV4y1t75L&style=gray" style="height: 160px; width: 100%"></iframe>

更多用法，请自行学习python探索