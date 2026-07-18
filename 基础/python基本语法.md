# week003-1: Python基础语法

![image-20250918093500325](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202509180935488.png)

> 本小节系统讲解Python基础语法，涵盖输入输出、条件判断和循环语句。掌握这些核心概念，为后续编程实践奠定坚实基础。

[TOC]

## 学习目标

- 掌握Python基本输入输出方法
- <span style='color:blue;'>理解并熟练使用条件判断语句</span>
- <span style='color:blue;'>熟练运用for循环和while循环</span>
- <span style='color:blue;'>熟悉列表、元组、字典、集合等常用数据结构</span>
- 掌握Python注释规范
- 了解模块/包的引用方式
- 掌握函数的定义与使用

## 知识点

### 1. 输入与输出

```python
# 输入与输出
name = input("What's your name?")
sum_01 = 100+100
print("name is {},sum is {}".format(name,sum_01))
```

> Python中使用`input()`函数获取用户输入，使用`print()`函数进行输出。输出格式化有多种方式，如`print('name is %s' % name)`和`print("name is {},sum is {}".format(name,sum_01))`。
>

### <font title='blue'>2. 判断语句</font>

```python
# 条件判断
score = 60
if score >= 90:
    print('Excellent')
else:
    if score < 60:
        print('Fail')
    else:
        print('Good Job')
```

> Python的条件判断语句使用`if`、`elif`、`else`，注意：
> - 条件表达式后必须加冒号（:）
> - 代码块通过缩进（空格或tab）表示，相同层次的代码缩进必须一致
> - Python没有大括号{}来分隔代码块，而是使用缩进
>

### <font title='blue'>3. 循环语句：for ... in</font>

```python
# for循环
sum001 = 0
for number in range(11):
    sum001 = sum001 + number  # 0+1+2+...+10
    print("number is {},sum001 is {}".format(number,sum001))
    
print(sum001)
```

> `for`循环用于固定次数的循环，常用`range()`函数指定循环范围：
>
> - `range(11)`表示0到10（不包括11）
> - `range(1,11)`表示1到10
> - `range(1,11,2)`表示1,3,5,7,9
>

### 4. 循环语句：while

```python
# while循环
sum = 0
number = 1
while number < 11:
    sum = sum + number
    number = number + 1
print(sum)
```

> `while`循环用于条件循环，适合循环次数不确定的场景。循环条件满足时执行循环体，条件不满足时退出循环。

### <font title='blue'>5. 数据类型</font>

#### <font title='red'>5.1 列表：[ ]</font>

```python
# 列表
lists= ['zhangfei','guanyu','liubei']
# 列表中添加元素
lists.append('dianwei')
print(lists)
print(len(lists))
# 在指定位置添加元素
lists.insert(1,'diaochan')
print(lists)
# 删除末尾元素
lists.pop()
print(lists)
lists.pop()
print(lists)
```

> 列表是可变序列，支持增删改查：
> - `append()`：在尾部添加元素
> - `insert(index, element)`：在指定位置插入元素
> - `pop()`：删除尾部元素
> - `len()`：获取列表长度
>

#### 5.2 元组：( )

```python
# 元组的使用
tuples = ('zhangfei','65')
print(tuples[0])

# 列表转化为元组
temp_list = [123, 'zhangfei', 'guanyu', 'liubei']
temp_tuple = tuple(temp_list)
print(temp_tuple)
```

> 元组是不可变序列，一旦创建就不能修改：
> - 不能使用`append()`、`insert()`等方法
> - 可以像列表一样通过索引访问元素
>

#### <font title='red'>5.3 字典：{ }</font>

```python
# 字典的使用
#定义一个dictionary
score = {'guanyu':96,'zhangfei':95}
print(score)

#添加一个元素
score['guanyu'] = 95
print(score)

# #删除一个元素
# score.pop('zhangfei')
# print(score)

# #查看key是否存在
# print('zhangfei'  in score)
# print(score)

# #查看一个key对应的值
# print(score.get('zhaoyun'))
# print(score.get('jike'))
# score['dianwei'] = 100
# print(score)
```

> 字典是键值对集合，支持增删改查：
> - 通过`key`获取对应的`value`
> - `score['key'] = value`添加/更新元素
> - `score.pop('key')`删除元素
> - `score.get('key', default)`获取值，不存在时返回默认值
>

#### 5.4 集合：set

```python
# 集合的使用
s = set(['zhangfei', 'guanyu', 'liubei'])
print(s)
s.add('dianwei')
print(s)
s.remove('zhangfei')
print(s)
print('jike' not in s)
```

> 集合是无序不重复元素集：
> - `add()`：添加元素
> - `remove()`：删除元素
> - `in`：检查元素是否在集合中
>

### 6. 注释：#

```python
# -*- coding: utf-8 -*-
'''
这是多行注释，用三个单引号
这是多行注释，用三个单引号 
这是多行注释，用三个单引号
'''
"""
123
456
789
"""
```

> Python中使用`#`表示单行注释，使用三个单引号或三个双引号表示多行注释。如果代码中包含中文，建议在文件开头添加`# -*- coding: utf-8 -*`。
>

### 7. 引用模块/包：import

```python
# 导入一个模块
import model_name

# 导入多个模块
import module_name1, module_name2

# 导入包中指定模块
from package_name import moudule_name

# 导入包中所有模块
from package_name import *
```

> Python使用`import`语句引入模块/包，常用方式有：
> - `import module`：导入整个模块
> - `from module import function`：导入模块中的特定函数
> - `from module import *`：导入模块中的所有内容
>

### <font title='blue'>8. 函数：def</font>

```python
def addone(score):
    return score + 1
print(addone(99)) # class用法
```

> 函数定义以`def`开头，后接函数名和参数列表，使用`return`返回结果：
> - `def function_name(parameters):`
> - `return value`
>

## 小结

- <span style='color:blue;'>Python基础语法包括输入输出、条件判断、循环语句等核心概念</span>
- <span style='color:blue;'>列表、元组、字典、集合是Python中常用的数据结构，各有特点</span>
- <span style='color:blue;'>代码缩进是Python语法的重要特征，必须统一</span>
- 注释规范对代码可读性至关重要
- 模块导入是Python实现代码复用的基本方式
- 函数是组织代码、实现功能复用的重要手段，掌握这些基础语法是进行更复杂编程任务的前提