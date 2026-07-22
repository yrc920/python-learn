# week006: Matplotlib与Seaborn数据可视化（上）

![image-20251005213045933](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202510052130354.png)

> 在数据分析与科学计算中，数据可视化是理解数据结构与展示分析结果的关键环节。<span style='color:blue;'>Matplotlib</span> 是 Python 中最基础、功能最强的绘图库，能够实现精细化的图形控制； <span style='color:blue;'>Seaborn </span>则基于 Matplotlib 封装，提供了更简洁的 API 和美观的默认样式。  本周将重点学习如何使用 Matplotlib 绘制各种二维图形，并初步了解 Seaborn 的可视化特性。

## 学习目标

- <span style='color:blue;'>理解 Matplotlib 的整体架构与核心对象（Figure、Axes、Axis）</span>
- <span style='color:blue;'>掌握基本绘图命令：折线图、散点图、柱状图、直方图</span>
- <span style='color:blue;'>学会设置标题、坐标轴标签、图例、颜色与样式</span>
- 理解子图布局（subplot、subplots）与图形尺寸控制
- 了解中文显示与字体配置问题及解决方案
- <span style='color:blue;'>初步掌握 Seaborn 的基础绘图接口（如散点图、分类图）</span>
- <span style='color:blue;'>能够基于数据特征选择合适的可视化类型</span>

## 知识点

> 本节讲义具体代码文件，请参考：`随书配套code\01_numpy_pandas_matplotlib\code\04_Python_Matplotlib_seaborn.ipynb`
>
> ![image-20251005213427223](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202510052134547.png)

### <font title='blue'>1. 散点图 (Scatter Plot)</font>

*   **用途**: 用于展示两个变量之间的关系，是探索数据相关性最常用的图表。
*   **核心方法**: `matplotlib.pyplot.scatter()`
*   **关键参数**:
    *   `x, y`: 数据点的横纵坐标。
    *   `c`: 点的颜色，可以是单一颜色或一个数组（用于映射到颜色）。
    *   `s`: 点的大小。
    *   `alpha`: 透明度。
*   **Seaborn 优势**: `seaborn.scatterplot()` 可以轻松地通过 `hue` 参数根据第三个分类变量对点进行着色，实现多维数据可视化。

### <font title='blue'>2. 折线图 (Line Plot)</font>

*   **用途**: 用于显示数据随时间或有序类别变化的趋势。
*   **核心方法**: `matplotlib.pyplot.plot()`
*   **关键参数**:
    *   `x, y`: 数据点的横纵坐标。
    *   `color`, `linestyle`, `linewidth`: 控制线条的外观。
    *   `marker`: 在数据点上添加标记。
*   **应用场景**: 绘制时间序列数据、函数曲线等。

### <font title='blue'>3. 条形图 (Bar Plot)</font>

*   **用途**: 用于比较不同类别之间的数值大小。
*   **核心方法**: `matplotlib.pyplot.bar()` (垂直条形图), `matplotlib.pyplot.barh()` (水平条形图)。
*   **关键参数**:
    *   `x, height`: 条形的类别标签和高度。
    *   `color`: 条形的颜色。
    *   `width`: 条形的宽度。
*   **Seaborn 优势**: `seaborn.barplot()` 默认会计算并显示置信区间，更适合统计分析。

### 4. 直方图 (Histogram)

*   **用途**: 用于展示连续型数据的分布情况，即数据在不同区间（bins）内的频数或频率。
*   **核心方法**: `matplotlib.pyplot.hist()`
*   **关键参数**:
    *   `x`: 要绘制直方图的数据。
    *   `bins`: 分组的个数或具体的分组边界。
    *   `density`: 是否将频数归一化为密度（面积和为1）。
    *   `alpha`: 透明度，常用于叠加多个直方图。
*   **Seaborn 优势**: `seaborn.histplot()` 功能更强大，可以方便地叠加核密度估计(KDE)曲线。

### <font title='red'>5. 箱线图 (Box Plot)</font>

*   **用途**: 用于展示数据的五数概括（最小值、第一四分位数、中位数、第三四分位数、最大值）和异常值，非常适合比较不同组数据的分布和离散程度。
*   **核心方法**: `matplotlib.pyplot.boxplot()`
*   **关键参数**:
    *   `x`: 数据序列。
    *   `labels`: 每个箱线图的标签。
    *   `showfliers`: 是否显示异常值。
*   **Seaborn 优势**: `seaborn.boxplot()` 和 `seaborn.violinplot()` (小提琴图) 提供了更美观、信息更丰富的变体。

### <font title='blue'>6. 饼图 (Pie Chart)</font>

*   **用途**: 用于展示一个整体中各部分所占的比例。
*   **核心方法**: `matplotlib.pyplot.pie()`
*   **关键参数**:
    *   `x`: 每部分的数值。
    *   `labels`: 每部分的标签。
    *   `autopct`: 显示百分比。
    *   `startangle`: 起始角度。
*   **注意**: 当类别过多或比例接近时，饼图可能不易阅读，条形图有时是更好的选择。

### <font title='red'>7. 热力图 (Heatmap)</font>

*   **用途**: 用颜色的深浅来表示二维数据矩阵中每个单元格的数值大小，常用于展示相关性矩阵、混淆矩阵等。
*   **核心方法**: `matplotlib.pyplot.imshow()` (更底层), `seaborn.heatmap()` (更常用、更方便)。
*   **关键参数 (Seaborn)**:
    *   `data`: 二维数据矩阵（如DataFrame）。
    *   `annot`: 是否在格子中显示数值。
    *   `cmap`: 颜色映射方案。
    *   `fmt`: 数值的显示格式。

### 8. 蜘蛛图/雷达图

*   **用途**: 用于展示一个对象在多个维度上的表现，形成一个多边形，便于进行多维度的综合比较。
*   **实现方法**: Matplotlib 本身没有直接的 `spiderplot` 函数，需要通过极坐标 (`polar=True`) 和 `plot` 函数来手动绘制。
*   **步骤**:
    1.  将数据按维度排列。
    2.  计算每个维度对应的角度。
    3.  在极坐标系中绘制折线并填充。
*   **应用场景**: 评估球员的综合能力、产品的各项指标对比等。

### 9. <font title='red'>Figure、Axes、Axis三层结构</font>

| 概念       | 英文   | 中文        | 角色                         | 类比                                 |
| :--------- | :----- | :---------- | :--------------------------- | :----------------------------------- |
| **Figure** | Figure | 图形        | 顶级容器，所有元素的父对象   | **整张画布**或**相框**               |
| **Axes**   | Axes   | 坐标系/子图 | 数据的绘图区域，拥有绘图方法 | 画布上的**一个画框**                 |
| **Axis**   | Axis   | 坐标轴      | 控制坐标轴的尺度、刻度和标签 | 画框上的**带刻度的尺子**（x轴和y轴） |

![image-20251005220552039](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202510052205123.png)

### 10. <font title='blue'>Matplotlib 与 Seaborn 对比</font>

| 比较项       | Matplotlib             | Seaborn                      |
| ------------ | ---------------------- | ---------------------------- |
| **定位**     | 通用绘图库，灵活、底层 | 高层接口，强调统计与美观     |
| **数据接口** | 支持任意数组           | 与 Pandas DataFrame 无缝结合 |
| **默认样式** | 需手动设置             | 内置主题优雅美观             |
| **典型用途** | 自定义图形、科研级绘制 | 快速统计分析与数据探索       |
| **扩展能力** | 强，支持3D/动画等      | 中等，依赖 Matplotlib 实现   |

## 小结

1. <span style='color:blue;'>**Matplotlib** 是 Python 中最基础的可视化工具，提供精细化图形控制与多种绘图方式。</span>
2. <span style='color:blue;'>掌握 `Figure`、`Axes`、`Axis` 三层结构是理解绘图逻辑的关键</span>。
3. 学会使用不同图形类型（折线、散点、柱状、直方）来表达不同数据关系。
4. <span style='color:blue;'>理解子图布局、样式控制、中文配置等绘图技巧。</span>
5. <span style='color:blue;'>**Seaborn** 提供更高层次的接口与默认美学，适合快速探索性分析</span>。
6. Matplotlib 强调“底层精确控制”，Seaborn 强调“高层美观表达”，两者常配合使用。

------

