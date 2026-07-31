# week007: Matplotlib基础（下）

![image-20251006180323759](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202510061803843.png)

> 在上一节中，我们学习了Matplotlib的基础绘图功能，如绘制简单的折线图、散点图等。本节将继续深入Matplotlib的更多高级功能，帮助我们创建更加美观、信息丰富的图表。<span style='color:blue;'>我们将学习如何自定义图表的样式、添加文本和注释、管理图形窗口以及创建包含多个子图的复杂布局</span>。掌握这些技能将使我们能够更有效地通过数据可视化来传达信息。

## 学习目标

- <span style='color:blue;'>掌握如何设置和自定义线条的样式</span>
- <span style='color:blue;'>学会为图表添加标题、轴标签，并自定义坐标轴</span>
- <span style='color:blue;'>能够在图表中添加文本注释和标记点</span>
- <span style='color:blue;'>理解如何创建和管理图形窗口</span>
- <span style='color:blue;'>熟练使用`plt.subplot`和`plt.subplots`来创建和管理多个子图</span>
- 了解Matplotlib的一些进阶绘图技巧

## 知识点

> 本节讲义具体代码文件，请参考：`随书配套code\01_numpy_pandas_matplotlib\04matplotlib`文件夹
>
> ![image-20251006175449731](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202510061754818.png)

### <font title='blue'>1. 设置线条样式</font>

线条样式是图表视觉效果的基础，可以通过`plt.plot()`函数的参数或对线条对象的属性进行设置。

- 通过**`plt.plot`**设置：在绘制线条时直接指定样式
- 通过**`set_xxx`**单独设置：先获取线条对象，再分别设置其属性

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

# 方法1: 在plot中直接设置
plt.plot(x, y1, color='red', linestyle='--', marker='o', markersize=5, label='sin(x)')

# 方法2: 通过set_xxx方法设置
line, = plt.plot(x, y2, label='cos(x)')
line.set_color('blue')
line.set_linestyle('-')
line.set_marker('^')
line.set_markersize(8)

plt.show()
```

### <font title='blue'>2. 设置轴和标题</font>

使用`plt.xlabel()`, `plt.ylabel()`和`plt.title()`来设置轴标签和标题。对于中文标题，需要指定字体。

```python
import matplotlib.pyplot as plt
import matplotlib.font_manager as font_manager
import numpy as np

y = [np.random.randint(10) for x in range(10)]
# 加载中文字体
font = font_manager.FontProperties(fname=r"C:\Windows\Fonts\msyh.ttc", size=20)

plt.plot(y)
plt.xlabel('X轴')
plt.ylabel('Y轴')
plt.title("折线图", fontproperties=font)  # 使用中文字体
plt.show()
```

### 3. 设置文本注释和marker

使用`plt.text()`添加文本，使用`plt.annotate()`添加带箭头的注释。

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)
# 在指定位置添加文本
plt.text(5, 0, 'sin(x)', fontsize=12, color='blue')
# 添加带箭头的注释
plt.annotate('最大值', xy=(np.pi/2, 1), xytext=(2, 1.5),
             arrowprops=dict(facecolor='black', shrink=0.05))
plt.show()
```

### <font title='blue'>4. 设置图形窗口</font>

`plt.figure()`用于创建新的图形窗口，并可以控制其大小和分辨率。

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y = np.sin(x)

# 创建一个大小为10x6英寸的图形窗口
plt.figure(figsize=(10, 6))
plt.plot(x, y)
plt.title('正弦波')
plt.show()
```

### <font title='red'>5. 绘制多个子图</font>

创建多个子图是进行数据对比分析的常用方法。

- **`plt.subplot`**: 用于逐个创建子图，适合简单的布局。
- **`plt.subplots`**: 一次性创建所有子图，返回`Figure`和`Axes`对象，更灵活且推荐使用。

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)

# 使用plt.subplots创建2x2的子图布局
fig, axes = plt.subplots(2, 2, figsize=(10, 8))

# 分别在四个子图中绘制不同的函数
axes[0, 0].plot(x, np.sin(x))
axes[0, 0].set_title('sin(x)')
axes[0, 1].plot(x, np.cos(x))
axes[0, 1].set_title('cos(x)')
axes[1, 0].plot(x, np.tan(x))
axes[1, 0].set_title('tan(x)')
axes[1, 1].plot(x, x**2)
axes[1, 1].set_title('x^2')

plt.tight_layout()  # 自动调整子图间距，防止重叠
plt.show()
```

### 6. 图形绘制进阶

使用`matplotlib`进行画图时，最常用的图形有：<span style='color:blue;'>条形图、直方图、散点图、饼图、箱线图、雷达图</span>；关于每种图形具体的绘制方法和常见属性，详情参考如下代码即可：

![image-20251006182833368](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202510061828462.png)

## 小结

1.  **<span style='color:blue;'>掌握线条样式自定义</span>**：学习了通过`plt.plot()`函数的参数（如`color`, `linestyle`, `marker`）和线条对象的`set_xxx`方法（如`set_color`, `set_linestyle`）来精细控制线条的外观，包括颜色、线型和数据点标记。
1. **<span style='color:blue;'>学会添加文本与标签</span>**：掌握了使用`plt.xlabel()`, `plt.ylabel()`, 和 `plt.title()`为图表添加坐标轴标签和标题的方法。特别地，通过`matplotlib.font_manager`加载中文字体，解决了中文显示的乱码问题。
1. **<span style='color:blue;'>掌握注释与标记技巧</span>**：学会了使用`plt.text()`在指定位置添加说明性文本，以及使用功能更强大的`plt.annotate()`创建带有箭头的注释，以突出显示图表中的关键数据点。
1. **<span style='color:blue;'>理解图形与子图管理</span>**：了解了`plt.figure()`用于创建和配置独立图形窗口（控制大小、分辨率）。重点学习了`plt.subplots()`，它能一次性创建包含多个子图的布局，并返回`Figure`和`Axes`对象，是创建复杂多图分析的标准方法，配合`plt.tight_layout()`可有效避免元素重叠。

