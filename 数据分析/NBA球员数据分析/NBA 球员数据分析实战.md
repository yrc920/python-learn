

## 实验一： NBA 球员数据分析实战

### 一、实验目的

1. 掌握使用 **Python**（主要是 Pandas、Matplotlib、Seaborn 等库）进行数据读取、清洗与可视化分析的基本流程。
2. 理解 NBA 球员常见统计指标（如两分命中率、助攻数、真实命中率、正负值、PACE 等）的含义与计算方法。
3. 学习从数据角度分析球员表现，探索不同特征与得分能力、效率指标之间的关系。
4. 通过可视化手段（箱线图、散点图、热力图等），掌握如何直观展示数据规律与特征分布。
5. 提升数据分析与体育数据建模的综合实践能力。

------

### 二、实验内容

1. **数据加载与预处理**
   - 导入并查看 NBA 球员数据集（CSV/Excel）。
   - 检查缺失值、异常值，并进行填充或删除操作。
   - 将字段名称与实际含义进行对应（如 `2P%` → 两分球命中率，`AST` → 助攻，`RPM` → 正负值等）。
2. **特征理解与统计分析**
   - 计算主要统计指标的均值、方差、最大值与最小值。
   - 分析进攻指标（如 2P%、3P%、FT%、Points）与防守指标（如 STL、BLK、DRB）之间的关系。
   - 对球员的正负值（RPM、ORPM、DRPM）进行排序与对比，探究对胜率的影响。
3. **可视化分析**
   - 绘制球员得分、助攻、篮板的分布图。
   - 使用箱线图观察不同指标的离群值。
   - 使用热力图展示各特征间的相关性。
   - 探索“PACE（比赛节奏）”与球队场均得分的相关趋势。
4. **综合探索与结果展示**
   - 建立简单的特征对比分析（例如：助攻与得分的关系）。
   - 根据进攻正负值（ORPM）与防守正负值（DRPM）绘制二维散点图，分析不同类型球员（进攻型/防守型）的分布特征。
   - 输出综合评价报告（如依据 `Wins_RPM`、`Points`、`AST` 综合得分的球员排名）。

------

### 三、实验环境

- **编程语言**：Python 3.9+
- **开发工具**：Jupyter Notebook / VS Code / PyCharm
- **依赖库**：
  - pandas
  - numpy
  - matplotlib
  - seaborn
  - openpyxl

------

### 四、实验步骤

1. **数据加载**

   ```python
   import pandas as pd
   df = pd.read_excel('nba_players.xlsx')
   df.head()
   ```

2. **字段解释与初步观察**

   - 根据 `nba字段.jpg` 对照表，添加字段中文注释。
   - 使用 `df.info()`、`df.describe()` 了解数据结构与分布。

3. **异常值与缺失值处理**

   - 统计空值：`df.isnull().sum()`

   - 对缺失数据进行均值填充或删除：

     ```python
     df.fillna(df.mean(), inplace=True)
     ```

4. **特征分析与可视化**

   - 绘制球员得分分布：

     ```python
     import matplotlib.pyplot as plt
     plt.hist(df['Points'], bins=20)
     plt.title('球员得分分布')
     plt.show()
     ```

   - 绘制助攻与得分的关系：

     ```python
     plt.scatter(df['AST'], df['Points'])
     plt.xlabel('助攻')
     plt.ylabel('得分')
     plt.title('助攻与得分关系散点图')
     plt.show()
     ```

   - 绘制各特征相关性的热力图：

     ```python
     import seaborn as sns
     corr = df.corr()
     sns.heatmap(corr, cmap='Blues', annot=False)
     plt.title('NBA 球员属性相关性热力图')
     plt.show()
     ```

5. **箱线图与异常值观察**

   - 使用箱线图展示各项指标分布情况，识别表现突出的球员：

     ```python
     sns.boxplot(data=df[['Points', 'AST', 'TRB', 'STL']])
     plt.title('主要统计指标箱线图')
     plt.show()
     ```

6. **正负值与胜率分析**

   - 探索 `Wins_RPM` 与球员综合表现的关系：

     ```python
     sns.scatterplot(x='RPM', y='Wins_RPM', data=df)
     plt.title('正负值与胜率正值关系图')
     plt.show()
     ```

7. **结果汇总与保存**

   - 输出前 10 名球员综合表现：

     ```python
     result = df.sort_values(by=['Wins_RPM'], ascending=False).head(10)
     result.to_excel('nba_top10.xlsx', index=False)
     ```

------

### 五、实验思考

1. 哪些指标最能反映球员的综合实力？是否存在“高得分但低效率”的球员？
2. 进攻正负值（ORPM）与防守正负值（DRPM）能否共同预测球队的胜率？
3. 当“PACE（节奏）”较快时，球员数据统计是否普遍上升？
4. 如果要构建一个“球员综合评分模型”，应如何加权不同指标（得分、助攻、篮板、正负值等）？

------

