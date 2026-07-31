# week005: NumPy基础与应用（下）

![](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202509251336662.png)

> 在上周学习 NumPy 核心对象`ndarray`和`ufunc`基础操作的基础上，本周将深入探索 NumPy 的高级功能。包括数组的高级索引与切片、广播机制、形状操作、文件 IO 以及缺失值处理等关键技术，这些内容是实现高效数据处理和分析的核心，也是进一步学习 Pandas 等工具的重要基础。
>

## 学习目标

- 掌握特殊数组的创建方法（如全零、全一、单位阵等）
- <span style='color:blue;'>理解并熟练使用数组的形状操作（reshape、flatten、transpose等）</span>
- <span style='color:blue;'>熟练掌握数组的高级索引与切片技术</span>
- <span style='color:blue;'>理解NumPy的广播（Broadcasting）机制及其应用</span>
- <span style='color:blue;'>区分深拷贝与浅拷贝，避免数据共享带来的副作用</span>
- <span style='color:blue;'>学会组合多个数组（concatenate、stack等）</span>
- <span style='color:blue;'>掌握NumPy的文件输入输出（IO）操作</span>
- <span style='color:blue;'>能够处理数组中的 nan 和 inf 等特殊值</span>
- 熟悉其他常用函数（如where、clip、unique等）

## 知识点

> 本节讲义具体代码文件，请参考：`随书配套code\02numpy`文件夹
>
> ![image-20250928101628305](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202509281016640.png)

### 1. <font title='blue'>特殊数组创建</font>

NumPy 提供了多种便捷函数用于创建具有特定值的数组：

- `np.zeros(shape)`：创建全0数组
- `np.ones(shape)`：创建全1数组
- `np.full(shape, fill_value)`：创建指定值填充的数组
- `np.eye(N)` 或 `np.identity(N)`：创建N×N单位矩阵

示例：

```python
import numpy as np

a = np.zeros((3, 4))        # 3x4 零矩阵
b = np.ones((2, 2))         # 2x2 全1矩阵
c = np.eye(3)               # 3x3 单位阵
d = np.full((2, 3), 7)      # 2x3 数组，所有元素为7
```

### 2. <font title='red'>数组形状操作</font>

对数组进行维度变换是数据预处理中的常见操作：

- `reshape(shape)`：改变数组形状，使用-1可自动计算维度大小
- `flatten()`：展平为一维数组（返回副本），修改不影响原数组
- `ravel()`：展平为一维数组（返回视图，更节省内存），修改会影响原数组
- `transpose()` 或 `.T`：转置数组
- `squeeze()`：移除长度为1的维度，特别适用于处理神经网络中的批量数据

示例：

```python
import numpy as np

# 创建示例数组
arr = np.arange(12)  # [0, 1, 2, ..., 11]
print("原始数组:", arr)
print("原始形状:", arr.shape)
print()

# 1. reshape(shape) - 改变数组形状
reshaped = arr.reshape(3, 4)  # 改为3x4矩阵
print("reshape(3, 4):")
print(reshaped)
print("形状:", reshaped.shape)
print()

# 2. flatten() - 展平为一维数组（返回副本）
matrix = np.array([[1, 2, 3], [4, 5, 6]])
flattened = matrix.flatten()
print("原始矩阵:")
print(matrix)
print("flatten():", flattened)
print("修改副本不会影响原数组:")
flattened[0] = 99
print("修改后副本:", flattened)
print("原矩阵不变:\n", matrix)
print()

# 3. ravel() - 展平为一维数组（返回视图，更节省内存）
raveled = matrix.ravel()
print("ravel():", raveled)
print("修改视图会影响原数组:")
raveled[0] = 88
print("修改后视图:", raveled)
print("原矩阵也被修改:\n", matrix)
print()

# 4. transpose() 或 .T - 转置数组
matrix_2d = np.array([[1, 2, 3], [4, 5, 6]])
transposed = matrix_2d.transpose()  # 或 matrix_2d.T
print("原始矩阵:")
print(matrix_2d)
print("转置矩阵:")
print(transposed)
print("形状从", matrix_2d.shape, "变为", transposed.shape)
print()

# 5. squeeze() - 移除长度为1的维度
# 创建具有单一维度的数组
arr_3d = np.array([[[1, 2, 3]]])  # 形状为(1, 1, 3)
print("原始3D数组:")
print(arr_3d)
print("形状:", arr_3d.shape)
squeezed = arr_3d.squeeze()
print("squeeze()后:")
print(squeezed)
print("形状:", squeezed.shape)
print()

# 额外示例：多维数组的维度变换
arr_4d = np.random.rand(2, 1, 3, 1)  # 形状为(2, 1, 3, 1)
print("4D数组形状:", arr_4d.shape)
print("squeeze()后形状:", arr_4d.squeeze().shape)

# 复杂reshape示例
complex_arr = np.arange(24)
print("\n复杂reshape示例:")
print("原始数组形状:", complex_arr.shape)
print("reshape(2, 3, 4)后形状:", complex_arr.reshape(2, 3, 4).shape)
print("reshape(2, -1)自动计算维度:", complex_arr.reshape(2, -1).shape)  # -1表示自动计算
```

### 3. <font title='red'>数组索引与切片</font>

NumPy提供了强大的索引和切片功能，可以高效地访问和修改数组元素：

**<font title='blue'>基本索引与切片</font>**

- **整数索引**：通过下标访问单个元素
- **切片操作**：使用`start:stop:step`语法获取子数组
- **多维索引**：使用逗号分隔的索引访问多维数组元素

**<font title='red'>高级索引技术</font>**

- **布尔索引**：使用布尔数组作为掩码筛选元素
- **花式索引**：使用整数数组索引获取特定位置的元素
- **多维索引**：在多维数组中使用组合索引

示例：

```python
import numpy as np

print("=== 基本索引与切片 ===")

# 创建示例数组
arr_1d = np.arange(10)  # [0, 1, 2, ..., 9]
arr_2d = np.arange(20).reshape(4, 5)  # 4x5矩阵

print("一维数组:", arr_1d)
print("二维数组:")
print(arr_2d)
print()

# 1. 基本索引
print("一维数组索引:")
print("arr_1d[3]:", arr_1d[3])        # 第4个元素
print("arr_1d[-1]:", arr_1d[-1])      # 最后一个元素
print()

print("二维数组索引:")
print("arr_2d[2, 3]:", arr_2d[2, 3])  # 第3行第4列元素
print("arr_2d[1]:", arr_2d[1])        # 第2行所有元素
print("arr_2d[:, 2]:", arr_2d[:, 2])  # 第3列所有元素
print()

# 2. 切片操作
print("一维数组切片:")
print("arr_1d[2:7]:", arr_1d[2:7])        # 索引2到6（不包括7）
print("arr_1d[::2]:", arr_1d[::2])        # 每隔一个元素
print("arr_1d[::-1]:", arr_1d[::-1])      # 反转数组
print()

print("二维数组切片:")
print("arr_2d[1:3, 2:4]:")                # 第2-3行，第3-4列
print(arr_2d[1:3, 2:4])
print("arr_2d[::2, ::2]:")                # 每隔一行一列
print(arr_2d[::2, ::2])
print()

print("=== 高级索引技术 ===")

# 3. 布尔索引（条件索引）
arr = np.array([3, 1, 4, 1, 5, 9, 2, 6])
print("原始数组:", arr)

# 创建布尔掩码
mask = arr > 3
print("布尔掩码(arr > 3):", mask)
print("满足条件的元素:", arr[mask])
print()

# 直接在索引中使用条件
print("大于3的元素:", arr[arr > 3])
print("偶数元素:", arr[arr % 2 == 0])
print()

# 二维数组布尔索引
arr_2d_bool = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print("二维数组:")
print(arr_2d_bool)
print("大于5的元素:", arr_2d_bool[arr_2d_bool > 5])
print()

# 4. 花式索引（Fancy Indexing）
arr_fancy = np.array([10, 20, 30, 40, 50, 60])
print("原始数组:", arr_fancy)

# 使用整数列表索引
indices = [1, 3, 5]
print("索引[1, 3, 5]处的元素:", arr_fancy[indices])
print()

# 二维数组花式索引
arr_2d_fancy = np.arange(12).reshape(3, 4)
print("二维数组:")
print(arr_2d_fancy)

# 选择特定行
print("选择第0行和第2行:")
print(arr_2d_fancy[[0, 2]])
print("选择第1行第2列和第2行第3列:")
print(arr_2d_fancy[[1, 2], [2, 3]])
print()

# 5. 组合索引
print("=== 组合索引示例 ===")

# 创建复杂数组
complex_arr = np.arange(24).reshape(2, 3, 4)
print("三维数组形状:", complex_arr.shape)
print("数组内容:")
print(complex_arr)
print()

# 组合使用各种索引
print("第一个二维数组的第2行:")
print(complex_arr[0, 1])                    # 整数索引
print("所有二维数组的第1列:")
print(complex_arr[:, :, 1])                 # 切片索引
print("第二个二维数组中大于10的元素:")
print(complex_arr[1][complex_arr[1] > 10])  # 布尔索引
print()

# 6. 索引修改值
print("=== 通过索引修改数组 ===")

modify_arr = np.array([1, 2, 3, 4, 5])
print("修改前:", modify_arr)

# 通过索引修改单个元素
modify_arr[2] = 99
print("修改索引2后:", modify_arr)

# 通过切片修改多个元素
modify_arr[1:4] = [88, 77, 66]
print("修改切片后:", modify_arr)

# 通过布尔索引修改满足条件的元素
modify_arr[modify_arr > 50] = 0
print("修改大于50的元素后:", modify_arr)
print()

# 7. 索引技巧
print("=== 实用索引技巧 ===")

# 使用where函数进行条件索引
data = np.array([1, 2, 0, 4, 0, 6])
indices = np.where(data != 0)  # 找到非零元素的索引
print("数组:", data)
print("非零元素索引:", indices)
print("非零元素:", data[indices])
print()

# 使用nonzero函数
nonzero_indices = np.nonzero(data)
print("nonzero()找到的索引:", nonzero_indices)
print("对应的非零值:", data[nonzero_indices])
```

**关键点总结：**

- 基本切片返回**视图**，修改会影响原数组
- 高级索引（布尔索引、花式索引）返回**副本**，修改不影响原数组
- 布尔索引特别适合基于条件筛选数据
- 花式索引可以灵活选择任意位置的元素组合
- 可以组合使用多种索引技术实现复杂的数据访问模式

### 4. <font title='red'>数组广播机制</font>

NumPy的广播（Broadcasting）机制是处理不同形状数组间算术运算的强大工具，它允许NumPy在执行元素级运算时<span style='color:blue;'>自动扩展较小数组的形状以匹配较大数组的形状，而无需显式复制数</span>据。

**广播规则**

NumPy遵循严格的广播规则：

- **规则1**：<font title='blue'>如果两个数组的维度数不同，会在维度较少的数组形状前面补1</font>

- **规则2**：<font title='blue'>如果两个数组在某个维度上的大小相同，或者其中一个数组在该维度大小为1，则认为这两个数组在该维度上兼容</font>

- **规则3**：<font title='blue'>数组可以沿大小为1的维度进行扩展，以匹配另一个数组的对应维度大小</font>

**广播应用场景**

- <span style='color:blue;'>数组与标量的运算</span>
- <span style='color:blue;'>不同形状但兼容的数组间的运算</span>
- <span style='color:blue;'>矩阵与向量的运算</span>span>

示例：

```python
import numpy as np

print("=== 广播机制基础 ===")

# 1. 标量与数组的广播（最简单的广播）
arr = np.array([[1, 2, 3], [4, 5, 6]])
scalar = 10

print("原始数组:")
print(arr)
print("标量:", scalar)
print("数组 + 标量:")
print(arr + scalar)  # 标量被广播到与数组相同形状
print("数组 * 标量:")
print(arr * scalar)
print()

# 2. 一维数组与二维数组的广播
vector = np.array([10, 20, 30])  # 形状: (3,)
matrix = np.array([[1, 2, 3], [4, 5, 6]])  # 形状: (2, 3)

print("一维数组:", vector)
print("二维数组:")
print(matrix)
print("二维数组 + 一维数组:")
print(matrix + vector)  # vector被广播为 [[10,20,30], [10,20,30]]
print()

# 3. 不同形状数组的广播
arr1 = np.array([[0], [1], [2]])  # 形状: (3, 1)
arr2 = np.array([10, 20, 30])     # 形状: (3,)

print("数组1 (形状 {}):".format(arr1.shape))
print(arr1)
print("数组2 (形状 {}):".format(arr2.shape))
print(arr2)
print("数组1 + 数组2:")
print(arr1 + arr2)  # arr1扩展为 (3,3), arr2扩展为 (3,3)
print()

print("=== 广播规则详解 ===")

# 规则1示例：维度补1
a = np.array([1, 2, 3])        # 形状: (3,)
b = np.array([[1], [2], [3]])  # 形状: (3, 1)

print("规则1 - 维度补1:")
print("a的形状:", a.shape, "-> 广播时视为:", (1, 3))
print("b的形状:", b.shape)
print("a + b:")
print(a + b)
print()

# 规则2和3示例：兼容性检查与扩展
arr_3x1 = np.array([[1], [2], [3]])  # 形状: (3, 1)
arr_1x3 = np.array([[10, 20, 30]])   # 形状: (1, 3)

print("规则2和3 - 兼容性与扩展:")
print("3x1数组:")
print(arr_3x1)
print("1x3数组:")
print(arr_3x1)
print("广播结果 (3x3):")
print(arr_3x1 + arr_1x3)
print()

print("=== 实际应用案例 ===")

# 案例1：数据标准化（减去均值，除以标准差）
data = np.random.rand(5, 3) * 10  # 5个样本，3个特征
print("原始数据 (5x3):")
print(data)

# 计算每个特征的均值和标准差
means = np.mean(data, axis=0)     # 形状: (3,)
stds = np.std(data, axis=0)       # 形状: (3,)

print("每个特征的均值:", means)
print("每个特征的标准差:", stds)

# 使用广播进行标准化
normalized_data = (data - means) / stds
print("标准化后的数据:")
print(normalized_data)
print("验证: 标准化后每个特征的均值应为0")
print("新均值:", np.mean(normalized_data, axis=0))
print()

# 案例2：图像处理中的颜色调整
# 模拟RGB图像 (高度, 宽度, 通道)
image = np.random.randint(0, 256, (4, 4, 3), dtype=np.uint8)
brightness_adjust = np.array([10, 20, 30])  # 每个通道的亮度调整值

print("模拟RGB图像 (4x4x3):")
print(image)
print("亮度调整值 (BGR):", brightness_adjust)

# 使用广播机制调整图像亮度
brightened_image = np.clip(                    # 限制像素值在有效范围内
    image.astype(np.int16) + brightness_adjust, # 关键步骤：广播加法
    0, 255                                     # 限制范围：最小值0，最大值255
).astype(np.uint8)                             # 转换回8位无符号整数格式
print()

# 案例3：矩阵与向量的点积模拟
matrix = np.array([[1, 2, 3], [4, 5, 6]])  # 2x3
weights = np.array([0.1, 0.2, 0.3])        # 3个权重

print("矩阵 (2x3):")
print(matrix)
print("权重向量:", weights)

# 模拟加权求和（类似神经网络中的全连接层）
weighted_sum = np.sum(matrix * weights, axis=1)  # 广播：weights扩展到2x3,axis=1表示沿水平方向求和（保留行，合并列）
print("加权求和结果:", weighted_sum)
print()

print("=== 广播失败案例 ===")

# 不兼容的形状会导致广播错误
try:
    arr_a = np.array([[1, 2, 3]])        # 形状: (1, 3)
    arr_b = np.array([[1, 2], [3, 4]])   # 形状: (2, 2)
    result = arr_a + arr_b
except ValueError as e:
    print("广播错误:", e)
    print("原因: 形状(1,3)和(2,2)在第二个维度上不兼容 (3 ≠ 2)")
print()

# 使用reshape使数组兼容
print("=== 通过reshape实现广播兼容 ===")

arr_1d = np.array([1, 2, 3])  # 形状: (3,)
arr_2d = np.array([[1, 2], [3, 4]])  # 形状: (2, 2)

print("无法直接广播的数组:")
print("一维数组:", arr_1d)
print("二维数组:")
print(arr_2d)

# 通过reshape使数组兼容
arr_1d_reshaped = arr_1d.reshape(3, 1)  # 形状变为 (3, 1)
arr_2d_reshaped = arr_2d.T.reshape(2, 2, 1)  # 形状变为 (2, 2, 1)

print("reshape后:")
print("arr_1d形状:", arr_1d_reshaped.shape)
print("arr_2d形状:", arr_2d_reshaped.shape)
print()

print("=== 高级广播技巧 ===")

# 使用np.newaxis添加新维度
arr = np.array([1, 2, 3])
print("原始数组:", arr, "形状:", arr.shape)

# 添加新维度
arr_col = arr[:, np.newaxis]  # 形状: (3, 1)
arr_row = arr[np.newaxis, :]  # 形状: (1, 3)

print("列向量形式:")
print(arr_col, "形状:", arr_col.shape)
print("行向量形式:")
print(arr_row, "形状:", arr_row.shape)
print()

# 使用广播进行外积计算
vector1 = np.array([1, 2, 3])
vector2 = np.array([4, 5, 6])

# 方法1: 使用newaxis
outer1 = vector1[:, np.newaxis] * vector2  # (3,1) * (3,) → (3,3)
# 方法2: 使用reshape
outer2 = vector1.reshape(3, 1) * vector2.reshape(1, 3)

print("向量外积计算:")
print("vector1:", vector1)
print("vector2:", vector2)
print("外积结果:")
print(outer1)
print("两种方法结果相同:", np.array_equal(outer1, outer2))
print()

print("=== 性能考虑 ===")

# 广播vs显式复制
large_matrix = np.random.rand(1000, 1000)
vector = np.random.rand(1000)

print("大型矩阵运算性能比较:")
import time

# 方法1: 使用广播（内存高效）
start = time.time()
result_broadcast = large_matrix + vector
time_broadcast = time.time() - start

# 方法2: 显式复制向量（内存消耗大）
start = time.time()
vector_tiled = np.tile(vector, (1000, 1))  # 显式复制为1000x1000
result_explicit = large_matrix + vector_tiled
time_explicit = time.time() - start

print("广播方法时间: {:.4f}秒".format(time_broadcast))
print("显式复制时间: {:.4f}秒".format(time_explicit))
print("结果相等:", np.array_equal(result_broadcast, result_explicit))
print("广播更高效:", time_broadcast < time_explicit)
```

**关键点总结：**
- 广播机制让不同形状的数组可以进行元素级运算，无需显式复制数据
- <span style='color:blue;'>遵循严格的维度兼容性规则：维度对齐，大小为1的维度可以扩展</span>
- 广播是<span style='color:blue;'>内存高效</span>的，避免了不必要的数据复制
- 常用技巧：`np.newaxis`添加维度、`reshape`调整形状实现广播兼容
- 在数据标准化、图像处理、机器学习等领域有广泛应用
- <span style='color:blue;'>广播失败时通常是因为数组形状在某个维度上不兼容且大小都不为</span>1

### 5. <font title='red'>深拷贝与浅拷贝</font>

（deep copy）和浅拷贝（shallow copy）的区别至关重要，这关系到数据的安全性和内存效率。错误的使用可能导致意外的数据修改。

**拷贝类型概述**

- **浅拷贝（视图，view）**：<font title='red'>共享原始数组的数据，修改浅拷贝会影响原数组</font>
- **深拷贝（副本，copy）**：<font title='red'>创建完全独立的数据副本，修改深拷贝不会影响原数组</font>
- **无拷贝**：简单的赋值操作，创建对同一数组对象的引用

**关键操作对比**

| 操作类型         | 拷贝方式 | 内存共享 | 修改影响原数组 |
| ---------------- | -------- | -------- | -------------- |
| 直接赋值         | 无拷贝   | 是       | 是             |
| 视图操作`view()` | 浅拷贝   | 是       | 是             |
| `copy()`方法     | 深拷贝   | 否       | 否             |

示例：

```python
import numpy as np

print("=== 深拷贝与浅拷贝基础 ===")

# 创建原始数组
original = np.array([[1, 2, 3], [4, 5, 6]])
print("原始数组:")
print(original)
print("原始数组ID:", id(original))
print()

print("=== 1. 无拷贝：直接赋值 ===")
# 直接赋值 - 创建引用（无拷贝）
reference = original
print("直接赋值后:")
print("reference ID:", id(reference))
print("两个ID相同:", id(original) == id(reference))

# 修改引用会影响原数组
reference[0, 0] = 99
print("修改reference[0,0] = 99后:")
print("原数组:", original)
print("reference:", reference)
print("验证: 原数组也被修改")
print()

# 恢复原数组
original[0, 0] = 1
print()

print("=== 2. 浅拷贝：视图操作 ===")
# 浅拷贝示例 - 视图操作
view_copy = original.view()  # 显式创建视图
slice_view = original[:, 1:3]  # 切片操作返回视图
reshape_view = original.reshape(3, 2)  # reshape返回视图

print("浅拷贝方式:")
print("view()创建的视图ID:", id(view_copy))
print("切片操作的视图ID:", id(slice_view))
print("reshape的视图ID:", id(reshape_view))
print("与原数组ID不同:", id(original) != id(view_copy))

# 验证视图共享数据
print("\n修改视图会影响原数组:")
view_copy[0, 1] = 88
print("修改view_copy[0,1] = 88后:")
print("原数组:", original)
print("view_copy:", view_copy)
print("验证: 原数组也被修改")
print()

# 恢复原数组
original[0, 1] = 2
print()

print("=== 3. 深拷贝：独立副本 ===")
# 深拷贝示例
deep_copy = original.copy()  # 创建深拷贝
flatten_copy = original.flatten()  # flatten返回副本

print("深拷贝方式:")
print("copy()创建的副本ID:", id(deep_copy))
print("flatten()创建的副本ID:", id(flatten_copy))

# 验证深拷贝独立性
print("\n修改深拷贝不会影响原数组:")
deep_copy[1, 2] = 77
print("修改deep_copy[1,2] = 77后:")
print("原数组:", original)
print("deep_copy:", deep_copy)
print("验证: 原数组保持不变")
print()

print("=== 4. 常见操作的拷贝行为 ===")
# 测试各种操作的拷贝行为
test_arr = np.array([[10, 20, 30], [40, 50, 60]])

operations = {
    "直接赋值": lambda x: x,
    "切片 [:2, 1:]": lambda x: x[:2, 1:],
    "reshape": lambda x: x.reshape(3, 2),
    "ravel()": lambda x: x.ravel(),
    "flatten()": lambda x: x.flatten(),
    "copy()": lambda x: x.copy(),
    "T (转置)": lambda x: x.T
}

print("各种操作的拷贝行为测试:")
for name, op in operations.items():
    result = op(test_arr)
    # 检查是否共享数据
    shares_memory = np.shares_memory(test_arr, result)
    print(f"{name:15} -> 共享内存: {shares_memory}")
print()

print("=== 5. 判断拷贝类型的方法 ===")
arr = np.array([[1, 2], [3, 4]])
view = arr.view()
copy = arr.copy()

print("判断拷贝类型的方法:")
print("np.shares_memory(arr, view):", np.shares_memory(arr, view))
print("np.shares_memory(arr, copy):", np.shares_memory(arr, copy))
print("view.base is arr:", view.base is arr)  # 视图的base指向原数组
print("copy.base is arr:", copy.base is arr)  # 副本的base为None
print("copy.base is None:", copy.base is None)
print()

print("=== 6. 实际应用场景 ===")

print("场景1：数据预处理中的安全拷贝")
# 原始数据
raw_data = np.array([[1.0, 2.0, 3.0], [4.0, 5.0, 6.0]])
print("原始数据:", raw_data)

# 错误做法：使用视图进行标准化（会影响原数据）
normalized_wrong = raw_data / 10.0  # 这会创建新数组，但演示概念
print("标准化后的数据（新数组）:", normalized_wrong)

# 正确做法：如果需要保留原数据，先创建副本
data_copy = raw_data.copy()
processed_data = data_copy / 10.0
print("处理后数据:", processed_data)
print("原数据保持不变:", raw_data)
print()

print("场景2：大型数组的内存优化")
# 创建大型数组
large_array = np.random.rand(1000, 1000)
print("大型数组形状:", large_array.shape)

# 需要处理数组的一部分
# 方法1：使用切片（视图，内存高效）
subset_view = large_array[100:200, 100:200]  # 视图，不复制数据

# 方法2：使用copy（独立副本，内存消耗大）
subset_copy = large_array[100:200, 100:200].copy()

print("子集视图共享内存:", np.shares_memory(large_array, subset_view))
print("子集副本共享内存:", np.shares_memory(large_array, subset_copy))

# 根据需求选择：如果需要修改子集但不影响原数组，使用copy
# 如果只是读取或临时处理，使用视图节省内存
print()

print("场景3：函数参数传递")
def process_data(data, in_place=False):
    """处理数据函数示例"""
    if in_place:
        # 原地修改（危险，会影响原数据）
        data *= 2
        return data
    else:
        # 创建副本进行处理（安全）
        data_copy = data.copy()
        data_copy *= 2
        return data_copy

test_data = np.array([1, 2, 3, 4, 5])
print("原始测试数据:", test_data)

# 安全方式
result_safe = process_data(test_data, in_place=False)
print("安全处理结果:", result_safe)
print("原数据保持不变:", test_data)

# 危险方式（原地修改）
result_danger = process_data(test_data, in_place=True)
print("危险处理结果:", result_danger)
print("原数据被修改:", test_data)
print()

print("=== 7. 高级拷贝技巧 ===")

print("结构化数组的拷贝")
# 创建结构化数组
structured_arr = np.array([(1, 'a', 2.5), (2, 'b', 3.7)], 
                         dtype=[('id', 'i4'), ('code', 'U1'), ('value', 'f8')])
print("结构化数组:", structured_arr)

# 拷贝结构化数组
structured_copy = structured_arr.copy()
print("拷贝后修改不会影响原数组")
structured_copy['id'] = [10, 20]
print("原数组:", structured_arr['id'])
print("拷贝数组:", structured_copy['id'])
print()

print("带掩码数组的拷贝")
# 创建带掩码的数组
masked_arr = np.ma.array([1, 2, 3, 4, 5], mask=[0, 0, 1, 0, 0])
print("带掩码数组:", masked_arr)
print("掩码:", masked_arr.mask)

# 拷贝掩码数组
masked_copy = masked_arr.copy()
print("拷贝后修改掩码:")
masked_copy.mask = [1, 0, 0, 0, 1]
print("原数组掩码:", masked_arr.mask)
print("拷贝数组掩码:", masked_copy.mask)
print()

print("=== 8. 性能考虑 ===")
# 比较拷贝操作的性能
large_test = np.random.rand(1000, 1000)

import time

# 测试视图操作性能
start = time.time()
view_ops = large_test[100:900, 100:900]  # 视图，快速
view_time = time.time() - start

# 测试深拷贝性能
start = time.time()
deep_ops = large_test[100:900, 100:900].copy()  # 深拷贝，较慢
deep_time = time.time() - start

print("性能比较 (1000x1000数组的子集操作):")
print("视图操作时间: {:.6f}秒".format(view_time))
print("深拷贝操作时间: {:.6f}秒".format(deep_time))
print("视图比深拷贝快 {:.1f}倍".format(deep_time / view_time))
print("在内存充足且需要数据安全时使用深拷贝")
print("在需要高性能且可接受数据共享时使用视图")
```

**关键点总结：**
- **直接赋值**创建引用，修改会影响原数组
- **视图操作**（<span style='color:blue;'>切片、reshape等</span>）创建浅拷贝，共享数据内存
- **`copy()`方法**创建深拷贝，数据完全独立
- 使用`np.shares_memory()`判断数组是否共享内存
- 根据需求选择拷贝方式：数据安全用深拷贝，内存优化用浅拷贝
- 在函数参数传递时特别注意拷贝行为，避免意外修改原数据
- <span style='color:blue;'>视图操作性能远高于深拷贝，适合处理大型数据子集</span>

### 6. <font title='blue'>不同数组组合</font>

NumPy 提供了多种组合数组的函数，`concatenate`、`stack`、`dot`、`vstack` 和 `hstack` 是其中最核心的几个。

| 函数             | 主要目的                            | 维度变化                 | 关键要求                              |
| :--------------- | :---------------------------------- | :----------------------- | :------------------------------------ |
| `np.concatenate` | 沿**现有轴**拼接数组                | 维度不变，连接轴长度增加 | 非连接轴的维度必须相同                |
| `np.stack`       | 沿**新轴**堆叠数组                  | 维度增加1                | 所有输入数组形状必须完全相同          |
| `np.dot`         | 计算数组的**点积**（内积/矩阵乘法） | 结果维度由运算规则决定   | 维度必须满足点积/矩阵乘法的兼容性要求 |

#### **1. `np.concatenate` - 沿现有轴连接**

将数组沿**已有轴**拼接，维度不变，连接轴长度增加。

```python
import numpy as np

a = np.array([[1, 2], [3, 4]])  # (2, 2)
b = np.array([[5, 6], [7, 8]])  # (2, 2)

# 沿 axis=0 (行) 连接
print(np.concatenate((a, b), axis=0))
# [[1 2]
#  [3 4]
#  [5 6]
#  [7 8]]  -> 形状 (4, 2)

# 沿 axis=1 (列) 连接
print(np.concatenate((a, b), axis=1))
# [[1 2 5 6]
#  [3 4 7 8]]  -> 形状 (2, 4)
```

<font title='red'>**关键**：非连接轴的维度必须相同。</font>

---

#### **2. `np.vstack` 和 `np.hstack` - 垂直与水平堆叠**

`vstack` 和 `hstack` 是 `concatenate` 的便捷函数，用于常见场景。

```python
a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6], [7, 8]])

# np.vstack 等价于 np.concatenate(..., axis=0)
print(np.vstack((a, b)))
# 垂直堆叠（按行），结果同 concatenate(axis=0)

# np.hstack 等价于 np.concatenate(..., axis=1)
print(np.hstack((a, b)))
# 水平拼接（按列），结果同 concatenate(axis=1)
```

**关键**：`vstack` 常用于将多个样本（行）合并，`hstack` 常用于将多个特征（列）合并。

---

#### **3. `np.stack` - 沿新轴堆叠**

创建**新维度**，将数组堆叠起来，维度增加1。

```python
a = np.array([1, 2, 3])  # (3,)
b = np.array([4, 5, 6])  # (3,)

# 沿新轴堆叠
print(np.stack((a, b), axis=0))
# [[1 2 3]
#  [4 5 6]]  -> 形状 (2, 3)

print(np.stack((a, b), axis=1))
# [[1 4]
#  [2 5]
#  [3 6]]  -> 形状 (3, 2)
```

**关键**：所有输入数组必须形状完全相同。

---

#### **4. `np.dot` - 计算点积**

计算数组的点积，最常用于向量内积和矩阵乘法。

```python
# 1D数组：计算内积 (标量)
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])
print(np.dot(a, b))  # 1*4 + 2*5 + 3*6 = 4 + 10 + 18 = 32

# 2D数组：矩阵乘法
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
print(np.dot(A, B))
# [[19 22]
#  [43 50]]
```

**关键**：矩阵乘法要求维度兼容。可使用 `@` 运算符代替 `np.dot`。

### 7. <font title='blue'>文件操作（IO操作）</font>

NumPy 提供了便捷的函数用于数组的保存与加载，支持二进制格式（`.npy`, `.npz`）和文本格式（如 CSV），适用于不同场景下的数据持久化需求。

```python
import numpy as np

# 创建示例数组
data = np.array([[1, 2, 3], [4, 5, 6]])

# --- 1. 保存为二进制文件 (.npy) ---
# np.save() 将单个数组保存为 .npy 文件（高效、推荐用于 NumPy 内部使用）
np.save('array_data.npy', data)
print("数组已保存至 'array_data.npy'")

# --- 2. 从 .npy 文件加载 ---
# np.load() 读取 .npy 文件并恢复数组
loaded_data = np.load('array_data.npy')
print("从 .npy 加载的数据:\n", loaded_data)

# --- 3. 保存多个数组到一个压缩文件 (.npz) ---
arr1 = np.array([1, 2, 3])
arr2 = np.array([4, 5, 6, 7])
# np.savez() 可保存多个数组到一个 .npz 文件（未压缩）
np.savez('arrays_archive.npz', array1=arr1, array2=arr2)
print("多个数组已保存至 'arrays_archive.npz'")

# np.savez_compressed() 保存为压缩格式，节省磁盘空间
np.savez_compressed('arrays_compressed.npz', array1=arr1, array2=arr2)
print("多个数组已压缩保存至 'arrays_compressed.npz'")

# --- 4. 从 .npz 文件加载 ---
# 加载 .npz 文件返回一个类似字典的对象
archive = np.load('arrays_archive.npz')
print("从 .npz 加载的数组1:", archive['array1'])  # 通过键名访问
print("从 .npz 加载的数组2:", archive['array2'])
archive.close()  # 推荐显式关闭文件

# --- 5. 保存为文本文件 (CSV 格式) ---
# np.savetxt() 将数组保存为文本文件，适合与其他程序（如 Excel）交互
np.savetxt('data.csv', data, delimiter=',', header='col1,col2,col3', comments='',fmt="%d")
print("数组已保存为 CSV 文件 'data.csv'")

# --- 6. 从文本文件 (CSV) 加载 ---
# np.loadtxt() 读取文本格式数据
# 注意：要求每行元素数量一致
loaded_csv = np.loadtxt('data.csv', dtype=np.int64,delimiter=',', skiprows=1)  # skiprows=1 跳过表头
print("从 CSV 加载的数据:\n", loaded_csv)
```

#### ✅ 使用建议：
- **`.npy` / `.npz`**：用于 Python/NumPy 环境下的高效读写，支持任意维度数组。
- **CSV 文本格式**：适合跨平台共享、人类可读，但仅限二维数值数据，性能较低。
- 若数据包含缺失值（nan），保存为 CSV 时会以 `nan` 字符串形式写入。

以下是 `np.savetxt`/`np.loadtxt` 与 `np.save`/`np.load` 的对比表格:

| 特性                       | `np.savetxt` / `np.loadtxt`                               | `np.save` / `np.load`                                |
| :------------------------- | :-------------------------------------------------------- | :--------------------------------------------------- |
| **主要用途**               | 读写文本格式文件（如 CSV、TSV）                           | 读写二进制 `.npy` 文件                               |
| **典型文件扩展名**         | `.csv`, `.txt`                                            | `.npy`                                               |
| **是否支持设置 header**    | ✅ 支持（使用 `header=` 参数，`comments=''` 可避免注释符） | ❌ 不支持                                             |
| **是否支持多数组存储**     | ❌ 仅支持单个二维数组                                      | ✅ 支持单个任意维度数组                               |
| **是否支持 3D 及以上数组** | ❌ 仅支持 1D 或 2D 数组                                    | ✅ 完全支持（任意维度）                               |
| **数据可读性**             | ✅ 文本格式，人类可读，可用 Excel 打开                     | ❌ 二进制格式，不可直接阅读                           |
| **存储效率**               | ⚠️ 较低（文本存储占用空间大）                              | ✅ 高（二进制存储紧凑）                               |
| **读写速度**               | ⚠️ 较慢（需解析文本）                                      | ✅ 快（直接读写二进制）                               |
| **跨平台兼容性**           | ✅ 高（通用文本格式）                                      | ⚠️ 一般（主要在 Python/NumPy 环境使用）               |
| **推荐使用场景**           | 与外部系统交换数据、数据导出、需要查看原始数据            | NumPy 内部数据持久化、保存中间计算结果、高维数组存储 |

### 8.<font title='red'> nan和inf值处理</font>

在实际数据处理中，经常会遇到<span style='color:blue;'>缺失值（`nan`）和无穷大值（`inf`）</span>。NumPy 提供了专门的表示和处理方法，正确理解并处理这些特殊值对数据分析至关重要。

```python
import numpy as np

# --- 1. 创建包含 nan 和 inf 的数组 ---
data = np.random.randint(0, 10, size=(3, 5)).astype(np.float64)  # 生成整数并转为浮点型
print("原始数据:\n", data)

# 设置缺失值：将某个元素设为 np.nan (Not a Number)
data[0, 1] = np.nan
print("\n设置 nan 后的数据:\n", data)

# 设置 inf：除以 0 会产生 inf 或 -inf
with np.errstate(divide='ignore', invalid='ignore'):  # 忽略警告
    result = data / 0
print("\ndata / 0 的结果:\n", result)
# 注意：0/0 -> nan, 正数/0 -> inf, 负数/0 -> -inf

# --- 2. 关于 nan 的重要特性 ---
# 特性1: nan 不等于任何值，包括它自己
print("\nnan == nan ?", np.nan == np.nan)  # 输出: False

# 特性2: 任何包含 nan 的计算结果通常也是 nan
print("nan * 2 =", data[0, 1] * 2)  # 输出: nan

# --- 3. 判断 nan 值 ---
# 使用 np.isnan() 判断哪些位置是 nan
nan_mask = np.isnan(data)
print("\nnan 位置的布尔掩码:\n", nan_mask)
print("nan 出现在哪些位置:", np.where(nan_mask))  # 返回 nan 的索引

# --- 4. 处理 nan 值的常用方法 ---

# 方法一：过滤掉 nan（返回不含 nan 的一维数组）
clean_data = data[~np.isnan(data)]  # ~ 表示取反，np.isnan(data) 为 True 的位置被排除
print("\n过滤掉 nan 后的数据:", clean_data)

# 方法二：删除包含 nan 的行
rows_with_nan = np.where(np.isnan(data))[0]  # 获取含有 nan 的行索引
data_no_nan_rows = np.delete(data, rows_with_nan, axis=0)  # axis=0 表示按行删除
print("\n删除含 nan 的行后:\n", data_no_nan_rows)

# 方法三：用特定值填充 nan（如 0）
data_filled_0 = data.copy()
data_filled_0[np.isnan(data_filled_0)] = 0
print("\n用 0 填充 nan 后:\n", data_filled_0)

# 方法四：用每列的均值填充 nan（更合理的填充策略）
data_filled_mean = data.copy()
for col in range(data_filled_mean.shape[1]):  # 遍历每一列
    column = data_filled_mean[:, col]
    # 提取该列非 nan 的值
    non_nan_values = column[~np.isnan(column)]
    # 计算均值
    col_mean = non_nan_values.mean()
    # 将该列的 nan 替换为均值
    column[np.isnan(column)] = col_mean
print("\n用各列均值填充 nan 后:\n", data_filled_mean)
```

#### ✅ 总结：

1. **`nan`**：表示“不是一个数字”（Not A Number），属于<span style='color:blue;'>浮点类型</span>。用于标记缺失或无效数据。
2. **`inf`**：表示“无穷大”（Infinity），在除以零时出现（正数/0 → `inf`，负数/0 → `-inf`）。
3. **`nan` 的运算特性**：`nan` 参与任何计算的结果通常都是 `nan`。
4. **`nan` 的比较特性**：`nan != nan` 恒成立，因此不能用 `==` 判断，必须使用 `np.isnan()`。
5. 处理策略：
   - **删除**：使用 `np.delete()` 删除含 `nan` 的行或列。
   - **替换**：用常数（如 0）、均值、中位数等填充 `nan`。
6. **注意**：`np.delete()` 中 `axis=0` 表示操作行，这与许多其他函数（如 `sum(axis=1)` 表示按行求和）的习惯相反，需特别留意。

### <font title='yellow'>9. 其他常见函数</font>

NumPy 提供了丰富的数学与随机函数，适用于数据处理和科学计算。

```python
import numpy as np

# --- 随机数生成 ---
# np.random.uniform(low, high, size): 生成 [low, high) 区间内的均匀分布随机数
rand_uniform = np.random.uniform(0, 10, size=5)
print("均匀分布随机数:", rand_uniform)

# --- 基本数学函数 ---
arr = np.array([-4.3, -2.7, -1, 0, 1.2, 4.8])

print("原始数组:", arr)
print("绝对值 np.abs():", np.abs(arr))        # [4.3 2.7 1.  0.  1.2 4.8]
print("平方根 np.sqrt():", np.sqrt(np.abs(arr)))  # 对绝对值开方
print("平方 np.square():", np.square(arr))    # [18.49 7.29 1.   0.   1.44 23.04]
print("指数 np.exp():", np.exp(arr))          # e^x
print("自然对数 np.log():", np.log(np.abs(arr[1:])))  # ln(x)，需避免0和负数
print("符号 np.sign():", np.sign(arr))        # [-1. -1. -1.  0.  1.  1.]
print("向上取整 np.ceil():", np.ceil(arr))    # [-4. -2. -1.  0.  2.  5.]
print("四舍五入 np.round():", np.round(arr))  # [-4. -3. -1.  0.  1.  5.]
print("向下取整 np.floor():", np.floor(arr))  # [-5. -3. -1.  0. -1.  4.]

# --- 条件与去重 ---
data = np.array([1, 2, 2, 3, 3, 3, -1, 0, 1])

# np.where(条件, x, y): 满足条件取x，否则取y
result = np.where(data > 0, data, 0)
print("正数保留，其余变0:", result)

# np.unique(arr): 返回唯一值（去重并排序）
unique_vals = np.unique(data)
print("唯一值:", unique_vals)  # [-1  0  1  2  3]
```

------

## 小结

1. **广播机制**：NumPy 能自动对不同形状的数组进行兼容性扩展，实现高效向量化运算。
2. **数组拷贝**：明确区分视图（共享内存）与深拷贝（独立内存），避免意外修改原数据。
3. **文件操作**：`npy/npz` 适合高效存储多维数组，`savetxt/loadtxt` 适用于二维 CSV 数据交互。
4. **特殊值处理**：`nan` 和 `inf` 需用 `np.isnan()` 等函数识别，合理填充或删除以保证计算正确性。
5. **常用函数**：`where`、`clip`、`unique` 及数学函数（如 `exp`、`log`、`round`）极大提升数据处理效率。