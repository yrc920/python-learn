# week008: Pandas基础与应用

![image-20251009151811726](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202510091518097.png)

> 本周课程正式进入数据分析阶段的核心工具——**Pandas**。<span style='color:blue;'>Pandas 是基于 NumPy 的高性能数据分析与处理库</span>，提供了丰富的数据结构与强大的数据操作接口。无论是读取外部文件、数据清洗，还是计算统计特征，Pandas 都能显著提升分析效率，是数据科学领域最重要的基础工具之一。

## 学习目标

- 理解并掌握 Pandas 的两种核心数据结构：<span style='color:blue;'>Series 与 DataFrame</span>
- 熟悉 Pandas 的数据读取与保存方法（<span style='color:blue;'>Excel、CSV 等格式</span>）
- 掌握常见的数据清洗方法，包括<span style='color:blue;'>重命名、删除、缺失值处理与数据替换等</span>span>
- 学会使用 Pandas 的<span style='color:blue;'>描述性统计与聚合函数进行数据分析</span>span>
- 能够基于 Pandas 进行初步的数据探索与可视化准备

## 知识点

> 本节讲义具体代码文件，请参考：`随书配套code\01_numpy_pandas_matplotlib/code/03_Python_Pandas.ipynb`文件
>
> ![image-20251009152215918](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202510091522247.png)
>
> 

------

### <font title='blue'>1. 数据结构</font>

Pandas 的核心数据结构包括：

- <span style='color:blue;'>**Series**：带标签的一维数据序列，可视为带索引的数组。</span>
- <span style='color:blue;'>**DataFrame**：带行索引和列标签的二维表格，是 Pandas 的主要数据容器。</span>

#### （1）Series 的创建与索引

```python
from pandas import Series

# 通过列表创建 Series
x1 = Series([1, 2, 3, 4])

# 指定索引创建 Series
x2 = Series(data=[1, 2, 3, 4], index=['a', 'b', 'c', 'd'])

# 通过字典创建 Series
d = {'a': 1, 'b': 2, 'c': 3, 'd': 4}
x3 = Series(d)
```

<span style='color:blue;'>Series 拥有 `index` 和 `values` 两个基本属性，可通过索引名或位置访问元素。</span>

------

#### （2）DataFrame 的创建与基本操作

```python
from pandas import DataFrame

data = {
    'Chinese': [66, 95, 93, 90, 80],
    'Math': [30, 98, 96, 77, 90],
    'English': [65, 85, 92, 88, 90]
}

df1 = DataFrame(data)
df2 = DataFrame(data,
                index=['ZhangFei', 'GuanYu', 'LiuBei', 'DianWei', 'XuChu'],
                columns=['Chinese', 'Math', 'English'])
```

常用方法：

```python
df2.shape        # 查看维度
df2.columns      # 查看列名
df2.index        # 查看索引
df2.head(3)      # 查看前3行
df2.describe()   # 查看描述性统计信息
```

------

### <font title='blue'>2. 数据导入和导出</font>

Pandas 能与多种格式的数据文件进行交互，如 Excel、CSV、SQL 等。

```python
import pandas as pd
from pandas import DataFrame

# 从 Excel 文件读取数据
score = pd.read_excel('../data/heros.xlsx')

# 保存为 Excel 文件
score.to_excel('../data/result.xlsx')

# 从 CSV 文件读取数据
score = pd.read_csv('../data/heros.csv')

# 保存为 CSV 文件
score.to_csv('../data/result1.csv')
```

常用参数说明：

- `index_col`：指定哪一列作为行索引
- `usecols`：选择需要的列
- `encoding`：设置文件编码防止中文乱码

------

### <font title='blue'>3. 数据清洗</font>

<span style='color:blue;'>数据清洗是数据分析的核心环节，目标是去除错误、重复和缺失的信息，使数据结构整洁、值合法。</span>Pandas 提供了丰富的函数用于数据修改、删除、替换与缺失值处理。

------

#### （1）修改列名与索引

```python
# 修改列名
df.rename(columns={'Chinese': '语文', 'Math': '数学', 'English': '英语'}, inplace=True)

# 修改行索引
df.rename(index={'ZhangFei': '张飞', 'GuanYu': '关羽'}, inplace=True)
```

<span style='color:blue;'>`inplace=True` 表示直接在原 DataFrame 上修改。</span>

------

#### （2）删除行或列

```python
# 删除列
df = df.drop(columns=['语文'])

# 删除行
df = df.drop(index=['关羽'])
```

参数控制方向：`columns` 为行，`index` 为列。

------

#### （3）缺失值检测与处理

```python
# 检查是否有缺失值
df.isnull()

# 删除含有缺失值的行
df = df.dropna()

# 用常数填充缺失值
df = df.fillna(0)

# 用列均值填充
df['数学'].fillna(df['数学'].mean(), inplace=True)
```

<span style='color:blue;'>缺失值（NaN）在统计分析中常导致错误，**应在分析前进行清洗**。</span>

------

#### （4）数据去重与替换

```python
# 删除重复行
df = df.drop_duplicates()

# 替换特定值
df = df.replace('无', 0)
```

这些操作常用于清理手工录入数据或合并后的重复记录。

------

#### （5）数据类型转换

```python
# 将字符串列转为数值类型
df['数学'] = df['数学'].astype(int)

# 将数值列转为字符串
df['姓名'] = df['姓名'].astype(str)
```

数据类型统一有助于后续计算与统计分析。

------

#### （6）字符串与列操作

```python
# 去除字符串首尾空格
df['姓名'] = df['姓名'].str.strip()

# 修改列顺序
df = df[['姓名', '语文', '数学', '英语']]
```

------

### <font title='red'>4. 常见统计函数</font>

Pandas 提供丰富的统计分析与数据汇总功能，帮助快速了解数据特征。

------

#### （1）描述性统计

```python
df.describe()
```

`describe()` 会返回每列的计数、均值、标准差、最小值、四分位数与最大值。

------

#### （2）聚合函数

```python
df.sum()       # 每列求和
df.mean()      # 每列平均值
df.median()    # 中位数
df.max()       # 最大值
df.min()       # 最小值
df.std()       # 标准差
df.var()       # 方差
df.count()     # 非空值数量
```

这些函数支持 `axis` 参数，用于控制按行或按列统计。

------

#### （3）条件筛选与统计

```python
# 筛选满足条件的数据
df[df['语文'] > 90]

# 条件求均值
df[df['数学'] > 80]['英语'].mean()
```

条件表达式可与逻辑运算符 `&`、`|` 组合实现复杂筛选。

------

#### （4）排序与排名

```python
# 按列排序
df.sort_values(by='数学', ascending=False)

# 按索引排序
df.sort_index()

# 添加成绩排名列
df['数学排名'] = df['数学'].rank(ascending=False)
```

------

#### （5）分组统计（初步）

```python
# 按类别分组并计算平均值
df.groupby('班级')['语文'].mean()

# 多列分组统计
df.groupby(['班级', '性别'])['数学'].mean()
```

分组（GroupBy）是 Pandas 的核心特性之一，在下节课中将进一步展开。

------

## 小结

1. <span style='color:blue;'>Pandas 提供 **Series** 与 **DataFrame** 两种核心数据结构，支持灵活的标签化操作</span>
2. 可通过 `read_excel()`、`read_csv()` 等函数高效导入外部数据
3. <span style='color:blue;'>数据清洗包括重命名、删除、缺失值处理、去重、替换等，是数据分析的关键步骤</span>
4. Pandas 内置丰富的统计函数与聚合工具，能快速完成数据汇总与特征提取
5. <span style='color:blue;'>熟练掌握 Pandas 的数据操作与统计分析，是后续数据建模与机器学习的基础</span>

------

