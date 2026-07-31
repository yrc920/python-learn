# week004: NumPy基础与应用（上）

![image-20250919130839594](C:/Users/jikeyang/AppData/Roaming/Typora/typora-user-images/image-20250919130839594.png)

> NumPy（Numerical Python）是Python科学计算的基础库，提供了高效的多维数组对象和用于处理这些数组的函数。它是许多高级数据分析库（如Pandas、SciPy、Scikit-learn等）的底层依赖。本周我们重点学习NumPy的两个核心对象：`ndarray`（N维数组）和`ufunc`（通用函数），并掌握其基本操作和常用功能。
>

[TOC]

## 学习目标

- 理解NumPy的核心对象：`ndarray` 和 `ufunc`
- <span style='color:blue;'>掌握创建数组的多种方式</span>
- 学会使用结构化数组（Structured Array）
- <span style='color:blue;'>熟悉NumPy的算术运算、统计函数和排序操作</span>
- <span style='color:blue;'>能够使用NumPy进行基本的数据处理和分析</span>

## 知识点

### 1. ndarray对象

NumPy的核心是`ndarray`（N维数组），它是一个用于<span style='color:blue;'>存储同类型数据</span>的多维容器。每个数组都有以下属性：

- `shape`：数组的形状（维度）
- `dtype`：数组中元素的数据类型

示例：
```python
import numpy as np

a = np.array([1, 2, 3])          # 一维数组
b = np.array([[1, 2, 3], [4, 5, 6]])  # 二维数组
print(a.shape)  # (3,)
print(b.shape)  # (2, 3)
print(a.dtype)  # int64
```

### 2. 创建数组

除了使用`np.array()`，还可以使用以下方法创建数组：

- `np.arange(start, stop, step)`：<span style='color:blue;'>创建等差数组（左闭右开）</span>
- `np.linspace(start, stop, num)`：<span style='color:blue;'>创建指定数量的等间隔数组（闭区间）</span>

示例：
```python
x1 = np.arange(1, 11, 2)   # [1, 3, 5, 7, 9]
x2 = np.linspace(1, 9, 5)  # [1., 3., 5., 7., 9.]
```

### 3. 结构化数组

使用`dtype`定义结构类型，创建类似表格的结构化数组：

```python
persontype = np.dtype({
    'names': ['name', 'age', 'chinese', 'math', 'english'],
    'formats': ['S32', 'i', 'i', 'i', 'f']
})

peoples = np.array([
    ("ZhangFei", 32, 75, 100, 90),
    ("GuanYu", 24, 85, 96, 88.5)
], dtype=persontype)

print(peoples['age'])  # 提取年龄字段
```

### <font title='red'>4. ufunc运算</font>

`ufunc`（通用函数）是对数组进行逐元素操作的函数，底层由C实现，效率极高。

常用算术运算：
- `np.add()`, `np.subtract()`, `np.multiply()`, `np.divide()`
- `np.power()`, `np.remainder()`

示例：
```python
print(np.add(x1, x2))  # 加法
print(np.power(x1, x2))  # 幂运算
```

### <font title='red'>5. 统计函数</font>

NumPy提供了丰富的统计函数：

- `np.min()`, `np.max()`：最小/最大值（可沿轴计算）
- `np.percentile()`：百分位数
- `np.median()`, `np.mean()`：中位数、平均值
- `np.average()`：加权平均
- `np.std()`, `np.var()`：标准差、方差

示例：
```python
a = np.array([[1,2,3], [4,5,6]])
print(np.min(a, axis=0))  # 每列最小值
print(np.mean(a, axis=1)) # 每行平均值
```

### <font title='blue'>6. 排序</font>

使用`np.sort()`对数组进行排序：

```python
a = np.array([[4,3,2], [2,4,1]])
print(np.sort(a, axis=0))  # 按列排序
print(np.sort(a, axis=1))  # 按行排序
```

## 小结

- NumPy是Python科学计算的基础，核心是`ndarray`和`ufunc`
- <span style='color:blue;'>掌握数组的创建、索引、切片和属性访问</span>
- 熟悉结构化数组的定义和使用场景
- <span style='color:blue;'>学会使用NumPy进行算术运算、统计分析和排序</span>

