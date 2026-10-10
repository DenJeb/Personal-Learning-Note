# Python 数据分析

> Python 数据分析主要围绕三个库展开：**NumPy** 负责数值计算，**Pandas** 负责数据处理，**Matplotlib**（以及 seaborn）负责数据可视化。三者配合，构成完整的数据分析流程。

使用前先安装依赖库：

```PowerShell
pip install numpy pandas matplotlib seaborn
```

## 目录

- [1. NumPy](#numpy)
  - [1.1 ndarray 的性质](#1-1)
  - [1.2 ndarray 的创建](#1-2)
  - [1.3 ndarray 的索引与切片](#1-3)
  - [1.4 ndarray 的运算](#1-4)
  - [1.5 NumPy 常用函数](#1-5)
- [2. Pandas](#pandas)
  - [2.1 核心数据结构——Series](#2-1)
  - [2.2 核心数据结构——DataFrame](#2-2)
  - [2.3 数据分析常用操作](#2-3)
- [3. Matplotlib（数据可视化）](#matplotlib)
  - [3.1 可视化简介](#3-1)
  - [3.2 折线图（plot）](#3-2)
  - [3.3 条形图（bar）](#3-3)
  - [3.4 饼图（pie）](#3-4)
  - [3.5 散点图（scatter）](#3-5)
  - [3.6 箱线图（boxplot）](#3-6)
  - [3.7 seaborn](#3-7)
- [4. 总结](#summary)

---

## 1. NumPy <a id="numpy"></a>

NumPy（Numerical Python）是 Python 科学计算的基础库，其核心是 **ndarray**（N 维数组），可以理解为"高性能的列表"——计算速度比 Python 原生列表更快，功能也更丰富。

### 1.1 ndarray 的性质 <a id="1-1"></a>

- **多维性**：ndarray 可以是一维、二维甚至更高维度的数组。
- **同质性**：同一个 ndarray 中的所有元素必须是相同的数据类型。

### 1.2 ndarray 的创建 <a id="1-2"></a>

ndarray 的常用创建方式如下：

| 创建方式 | 语法 | 说明 |
|---------|------|------|
| 基础创建 | `arr = np.array([1, 2, 3])` | 从列表创建 |
| copy 创建 | `arr = np.copy(arr)` | 复制一份独立的数组 |
| 预定义形状 | `arr = np.zeros(shape)` | 全为 0 |
| | `arr = np.ones(shape)` | 全为 1 |
| | `arr = np.empty(shape)` | 未初始化（内存中的随机值） |
| 全填充指定值 | `arr = np.full(shape, fill_value)` | 所有元素填充为指定值 |
| 根据已有数组创建（形状已定） | `arr2 = np.zeros_like(arr1)` | 全为 0 |
| | `arr2 = np.ones_like(arr1)` | 全为 1 |
| | `arr2 = np.empty_like(arr1)` | 未初始化 |
| | `arr2 = np.full_like(arr1, fill_value)` | 填充指定值 |
| 等差数列 | `arr = np.arange(start, stop, step)` | 左闭右开 |
| 等间隔数列 | `arr = np.linspace(start, stop, num)` | 左闭右闭，num 为元素个数 |
| 等比数列 | `arr = np.logspace(start, stop, num, base)` | 左闭右闭，base 为底数 |
| 单位矩阵 | `arr = np.eye(行, 列)` | 主对角线为 1，其余为 0 |
| 对角矩阵 | `arr = np.diag(对角元素1, ...)` | 按给定元素构造对角矩阵 |
| 随机数组 | `arr = np.random.rand(行, 列)` | 0~1 之间的随机数 |
| 指定范围的随机浮点数组 | `arr = np.random.uniform(low, high, size)` | 均匀分布 |
| 指定范围的随机整数数组 | `arr = np.random.randint(low, high, size)` | 均匀分布的整数 |
| 正态分布数组 | `arr = np.random.randn(size)` | 大部分值在 0 附近 |
| 生成随机种子 | `np.random.seed(42)` | 之后生成的随机数固定不变，便于复现实验结果 |

> ❓ 为什么需要随机种子？不设置种子时，每次运行生成的随机数都不同；设置种子后，随机数序列可复现，方便调试和对比结果。

### 1.3 ndarray 的索引与切片 <a id="1-3"></a>

#### 1.3.1 一维数组

- 取所有元素：`arr[:]`
- 取 [m, n) 的元素：`arr[m:n]`
- 取 [m, n) 的元素，步长为 k：`arr[slice(m:n:k)]`（k = -1 表示从后往前取）
- bool 索引：`arr[(arr > m) & (arr < n)]`

#### 1.3.2 二维数组

- 取所有元素：`arr[:, :]`
- 取行 [m, n)、列 [p, q) 的元素：`arr[m:n, p:q]`
- 从后往前（变行不变列）：`arr[::-1]`
- bool 索引（输出一维数组）：`arr[(arr > m) & (arr < n)]`
- 第 m 行大于 n 的元素：`arr[m - 1][arr[m - 1] > n]`
- 第 m 列大于 n 的元素：`arr[:, m - 1][arr[:, m - 1] > n]`

> ❓ 为什么 bool 索引用 `&` 而不是 `and`？`and`、`or` 不能对数组进行逐元素的布尔运算，必须使用按位运算符 `&`、`|`。

### 1.4 ndarray 的运算 <a id="1-4"></a>

- 一维、二维数组：`arr1 + arr2` —— 对应位置的元素逐个相加
- 列表（实现相同的效果需要循环）：

```Python
for i in range(len(list1)):
    list1[i] += list2[i]
```

- 矩阵乘法：`arr1 @ arr2`

> ⚠️ 注意：
> - `arr1 * arr2` 是矩阵**相同位置的元素相乘**，不是矩阵乘法。
> - 列表的合并是 `list1 + list2`（拼接），而 ndarray 的 `+` 是逐元素相加——两者语义完全不同。

### 1.5 NumPy 常用函数 <a id="1-5"></a>

常用函数按功能分为 7 类：

| 分类 | 函数 | 说明 |
|------|------|------|
| 基本数学 | `np.sqrt(x)` | 平方根 |
| | `np.exp(x)` | e 的 x 次方 |
| | `np.log(x)` | 自然对数 |
| | `np.sin(x)` | 正弦 |
| | `np.abs(x)` | 绝对值 |
| | `np.power(a, b)` | a 的 b 次方 |
| | `np.round(x, n)` | 四舍五入，保留 n 位小数 |
| 统计 | `np.sum(x)` | 求和 |
| | `np.mean(x)` | 平均值 |
| | `np.median(x)` | 中位数 |
| | `np.std(x)` | 标准差 |
| | `np.var(x)` | 方差 |
| | `np.min(x) / np.max(x)` | 最小值 / 最大值 |
| | `np.percentile(x, q)` | 第 q 分位数 |
| 比较 | `np.greater(a, b)` | a > b |
| | `np.less(a, b)` | a < b |
| | `np.equal(a, b)` | a == b |
| | `np.where(条件, 满足返回, 不满足返回)` | 按条件返回对应值 |
| 排序 | `np.sort(x)` | 排序 |
| 去重 | `np.unique(x)` | 去重 |
| 分割 | `np.split(x, m)` | 分成 m 个 |
| 重整（改变形状） | `np.reshape(x, shape)` | 改变数组形状 |

---

## 2. Pandas <a id="pandas"></a>

Pandas 建立在 NumPy 之上，是处理表格型数据的核心库，提供两种核心数据结构：

| 数据结构   | 维度 | 说明                     |
|-----------|------|--------------------------|
| Series    | 一维 | 带索引的数组             |
| DataFrame | 二维 | 表格，由行索引和列名组成 |

### 2.1 核心数据结构——Series <a id="2-1"></a>

Series 是带**标签（索引）**的一维数组，可以理解为"带索引的列表"。

#### 2.1.1 Series 的创建

```Python
import pandas as pd

# 初始创建
s1 = pd.Series([1, 2, 3])
print(s1)

# 自定义 index
s2 = pd.Series([1, 2, 3], index=['A', 'B', 'C'])
print(s2)

# 自定义 name
s3 = pd.Series([1, 2, 3], index=['A', 'B', 'C'], name='嘿嘿')
print(s3)

# 通过字典创建
s4 = pd.Series({"A": 1, "B": 2, "C": 3})
print(s4)

# 通过已有 Series 选取部分索引
s5 = pd.Series(s4, index=["A", "C"])
print(s5)
```

#### 2.1.2 Series 的属性

```Python
import pandas as pd

s = pd.Series([1, 2, 3], index=['A', 'B', 'C'], name="test")
print(s.index)    # 索引
print(s.values)   # 值
print(s.name)     # 名称
print(s.dtype)    # 数据类型
print(s.shape)    # 形状
print(s.ndim)     # 维度数
print(s.size)     # 元素个数
```

访问单个元素的四种方式对比：

| 方法 | 索引类型 | 是否支持切片 |
|------|---------|-------------|
| `loc` | 显式（标签） | 支持 |
| `iloc` | 隐式（位置） | 支持 |
| `at` | 显式（标签） | 不支持 |
| `iat` | 隐式（位置） | 不支持 |

#### 2.1.3 Series 访问数据

```Python
import pandas as pd

s = pd.Series([1, 2, 3, 4, 5, 6], index=['A', 'B', 'C', 'D', 'E', 'F'], name="test")
# bool 索引访问
print(s[s > 3])
# 访问前 5 个数据（默认）
print(s.head())
# 访问前 3 个数据
print(s.head(3))
# 访问后 5 个数据（默认）
print(s.tail())
# 访问后 3 个数据
print(s.tail(3))
```

#### 2.1.4 Series 的常用方法

Series 的常用方法见下图：

![image.png](<图片和附件/image 1.png>)

![image.png](<图片和附件/image 2.png>)

### 2.2 核心数据结构——DataFrame <a id="2-2"></a>

DataFrame 是带**行索引**和**列名**的二维表格，可以看作多个 Series 按列组合。

#### 2.2.1 DataFrame 的创建

```Python
import numpy as np
import pandas as pd

# 通过 Series 创建
s1 = pd.Series([1, 2, 3, 4, 5])
s2 = pd.Series([6, 7, 8, 9, 10])
df = pd.DataFrame({"Line1": s1, "Line2": s2})
type(df)
type(df["Line1"])

# 通过字典创建
df = pd.DataFrame(
    {
        "id": ['a', 'b', 'c', 'd', 'e'],
        "name": ["Jack", "Curry", "Jeb", "Pluto", "Den"],
        "age": [11, 36, 18, 16, 19],
    }, index=[1, 2, 3, 4, 5], columns=["id", "name", "age"]
)
df
```

#### 2.2.2 DataFrame 的属性

```Python
# df 沿用 2.2.1 创建的 DataFrame
print(df)
print(df.index)    # 行索引
print(df.columns)  # 列名
print(df.values)   # 数据值
print(df.shape)    # 形状（5 行 3 列）
print(df.ndim)     # 维度数
print(df.size)     # 元素个数
print(df.dtypes)   # 每列的数据类型
print(df.T)        # 转置
```

#### 2.2.3 DataFrame 访问数据

```Python
# df 沿用 2.2.1 创建的 DataFrame
print(df['name'])            # 访问单列
print(df.name)               # 通过属性名访问单列
print(df[['name', 'age']])   # 访问多列
print(df.tail(3))            # 访问后 3 行
print(df[df['age'] >= 18])   # bool 索引（筛选年龄 >= 18 的行）
print(df.sample)             # 随机抽样
```

#### 2.2.4 DataFrame 的常用方法

DataFrame 的常用方法见下图：

![image.png](<图片和附件/image 3.png>)

![image.png](<图片和附件/image 4.png>)

### 2.3 数据分析常用操作 <a id="2-3"></a>

以下小节的代码共用 `import pandas as pd`，不再重复写出。

#### 2.3.1 数据的导入与导出

```Python
df = pd.read_csv("文件位置")   # 导入（后缀决定函数：read_csv / read_excel 等）
df.to_csv("文件位置")          # 导出
```

#### 2.3.2 缺失值的处理

```Python
df.dropna()                     # 删除包含缺失值的行
df.fillna({"列名": 填充值})      # 用指定值填充缺失值
df.ffill()                      # 向前填充（用上一行的值）
df.bfill()                      # 向后填充（用下一行的值）
```

> ⚠️ `dropna()` 会删除包含缺失值的**整行**数据，可能造成数据量损失，使用前需权衡。

#### 2.3.3 重复值的处理

```Python
df.duplicated(['列名'])        # 检测重复值
df.drop_duplicates(['列名'])   # 删除重复值
```

#### 2.3.4 数据变形（重塑）

宽表和长表可以互相转换，也可以把一列拆成多列：

```Python
data = pd.DataFrame(
    {
        'name': ['Stephen Curry', 'Klay Thompson', 'Kevin Durant', 'Draymond Green', 'Andre Iguodala'],
        'math': [85, 92, 78, 88, 95],
        'english': [90, 88, 85, 92, 80],
        'chinese': [75, 80, 88, 85, 90],
    }, index=range(1, 6)
)
print(data)

# 宽表转长表
melt_data = pd.melt(data, id_vars=['name'], var_name='subject', value_name='score')
print(melt_data)

# 长表转宽表
pivot_data = pd.pivot(melt_data, index=['name'], columns='subject', values='score')
print(pivot_data)

# 分列：按空格把姓名拆成姓、名两列
data[['first_name', 'last_name']] = data['name'].str.split(' ', expand=True)
print(data)
```

#### 2.3.5 数据分箱

```Python
# data 沿用 2.3.4 中分列前的 DataFrame

# 分箱：按分数区间划分等级
chinese_grade = pd.cut(data['chinese'], bins=[0, 60, 80, 100]).value_counts()
math_grade = pd.cut(data['math'], bins=[0, 60, 80, 100]).value_counts()
english_grade = pd.cut(data['english'], bins=[0, 60, 80, 100], labels=['不及格', '及格', '优秀'])
print(chinese_grade)
print(math_grade)
print(english_grade)

# 修改列名
data.rename(columns={'chinese': '语文', 'math': '数学', 'english': '英语', 'name': '姓名'}, inplace=True)
print(data)
```

#### 2.3.6 时间数据处理

```Python
d = pd.Timestamp('2026-06-07 09:00:00')
print(type(d))
print("年：", d.year)
print("季度：", d.quarter)
print("月：", d.month)
print("日：", d.day)
print("时：", d.hour)
print("分：", d.minute)
print("秒：", d.second)
print("星期：", d.day_name())
print("转换为年：", d.to_period("Y"))
print("转换为季度：", d.to_period("Q"))
print("转换为月：", d.to_period("M"))
print("转换为日：", d.to_period("D"))

new_df = pd.DataFrame({
    'num': [100, 200, 300, 400, 500],
    'date': ['20260607', '20260608', '20260609', '20260610', '20260611']
})
new_df['datetime'] = pd.to_datetime(new_df['date'])
new_df['weekday'] = new_df['datetime'].dt.day_name()
print(new_df)
print(new_df.info())

# 设置索引
new_df.set_index('datetime', inplace=True)
new_df.drop(columns=['date'], inplace=True)
print(new_df)

# 时间间隔
d1 = pd.Timestamp('2025-06-05 09:00:00')
d2 = pd.Timestamp('2026-06-01 21:00:00')
print(d2 - d1)

# 生成时间序列
days = pd.date_range(start='2025-06-07', end='2026-06-01', freq='D')
print(days)
days = pd.date_range(start='2025-06-07', periods=10, freq='D')  # 生成 10 个日期
print(days)
```

#### 2.3.7 分组聚合

```Python
# 接 2.3.6 中的 new_df
# 分组：按列分组，查看每组的索引
print(new_df.groupby('weekday').groups)
# 聚合：对某列使用聚合函数
print(new_df.groupby('weekday')['num'].mean())
```

---

## 3. Matplotlib（数据可视化） <a id="matplotlib"></a>

Matplotlib 是 Python 最基础的可视化库，seaborn 和 Pandas plot 都建立在它之上。

### 3.1 可视化简介 <a id="3-1"></a>

| 工具 | 说明 | 优点 | 缺点 |
|------|------|------|------|
| matplotlib | Python 最基础的可视化库 | 灵活强大、定制性强 | 代码多、风格基础 |
| seaborn | 基于 matplotlib 的高级接口 | 风格美观、统计图方便 | 对简单图略繁琐 |
| Pandas plot | 快速图表，调用 `.plot()` | 快捷、适合 EDA | 图表样式较少 |

常见图表及其应用场景：

| 图表类型 | 应用场景 |
|---------|---------|
| 折线图（plot） | 趋势随时间变化 |
| 条形图（bar） | 类别之间对比 |
| 饼图（pie） | 整体组成比例 |
| 散点图（scatter） | 两变量相关性 |
| 箱线图（boxplot） | 数据分布、异常值 |

> ⚠️ matplotlib 默认字体不支持中文，绘图前需要先设置中文字体：`rcParams['font.sans-serif'] = ['SimHei']`；若负号显示异常，可再加一行 `rcParams['axes.unicode_minus'] = False`。

### 3.2 折线图（plot） <a id="3-2"></a>

```Python
# 折线图-plot
import matplotlib.pyplot as plt
from matplotlib import rcParams  # 导入 rcParams 模块，用于设置图表的字体
rcParams['font.sans-serif'] = ['SimHei']

# 创建图表，指定图表的大小
plt.figure(figsize=(10, 5))

# 数据准备
month = ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12']
sales = [1080, 1330, 1210, 1190, 1230, 1250, 1320, 1340, 1360, 1280, 1300, 1250]

# 绘制折线图
plt.plot(month, sales, label='开销记录')  # label 参数用于添加图例标签

# 添加标题
plt.title('2026年每月开销趋势', color='green', fontsize=20)

# 添加坐标轴标签
plt.xlabel('月份', fontsize=12)
plt.ylabel('开销（元）', fontsize=12)

# 添加图例
plt.legend(loc='upper left', fontsize=8)

# 添加网格线
plt.grid(True, alpha=0.4, linestyle='--')  # alpha 设置网格线透明度，linestyle 设置网格线样式为虚线

# 设置坐标轴间隔
plt.yticks(range(1000, 1400, 20))

# 打印每点的坐标，并在图上标注数值
for x, y in zip(month, sales):
    print(x, y)
    plt.text(x, y + 1, str(y), ha='center', va='bottom', fontsize=10)  # ha 设置水平对齐方式，va 设置垂直对齐方式

# 显示图表
plt.show()
```

### 3.3 条形图（bar） <a id="3-3"></a>

```Python
# 条形图-bar
# 公共设置（导入、字体、figsize）与结尾的 plt.show() 同 3.2，此处省略

# 数据准备
AI_tool = ['ChatGPT', 'Claude', 'Gemini', 'DeepSeek', 'GLM', 'Qwen', 'Minimax']
score = [99, 98, 96, 95, 90, 93, 85]

# 绘制条形图
plt.bar(AI_tool, score, label='评分', width=0.5, color='skyblue')

# 添加标题
plt.title('2026热门AI工具测评', color='blue', fontsize=20)

# 添加坐标轴标签
plt.xlabel('AI工具', fontsize=12)
plt.ylabel('评分', fontsize=12)

# 添加网格线（参数含义见 3.2）
plt.grid(axis='y', alpha=0.3, linestyle='--')

# 添加图例
plt.legend(loc='upper right', fontsize=10)

# 设置坐标轴间隔
plt.yticks(range(0, 110, 10))
```

### 3.4 饼图（pie） <a id="3-4"></a>

```Python
# 饼图-pie
# 公共设置（导入、字体、figsize）与结尾的 plt.show() 同 3.2，此处省略

# 数据准备
areas = ['中国', '美国', '俄罗斯', '欧洲', '日本', '其他']
gdp = [19.6, 30.5, 2.7, 19.4, 4.3, 33.9]
colors = ['cornflowerblue', 'palegreen', 'navajowhite', 'lightcoral', 'hotpink', 'skyblue']
explode = [0.1, 0.0, 0.0, 0.0, 0.0, 0.0]  # 爆炸式饼图设计（数值表示突出块的位置），可用可不用

# 绘制饼图
plt.pie(gdp,
        labels=areas,
        autopct='%.2f%%',
        startangle=90,
        colors=colors,
        explode=explode,
        shadow=True)  # autopct 表示占比数字格式，startangle 表示旋转角度，shadow 表示阴影

# 添加标题
plt.title('2025年中、美、俄、欧、日、其他国家GDP对比', fontsize=20)

# 添加图例
plt.legend(loc='lower left', fontsize=7)
```

### 3.5 散点图（scatter） <a id="3-5"></a>

```Python
# 散点图-scatter
# 公共设置（导入、字体、figsize）与结尾的 plt.show() 同 3.2，此处省略
import random  # 散点图需要生成随机数据

# 数据准备
x = []
y = []
for i in range(1000):
    tmp_x = random.uniform(1, 10)
    x.append(tmp_x)
    tmp_y = 2 * tmp_x + random.gauss(0, 2)
    y.append(tmp_y)

# 绘制散点图
plt.scatter(x, y,
            color='blue',
            alpha=0.5,
            s=10)  # alpha 设置透明度，s 设置点的大小

# 添加标题
plt.title('随机数散点图', fontsize=20)

# 添加坐标轴标签
plt.xlabel('自变量', fontsize=12)
plt.ylabel('因变量', fontsize=12)

# 设置 x、y 轴范围
plt.xlim(0, 10)
plt.ylim(0, 30)

# 设置 x、y 轴刻度
plt.xticks(range(0, 11, 1), fontsize=10)
plt.yticks(range(0, 31, 1), fontsize=10)

# 画回归线
plt.plot([0, 10], [0, 20], color='red', linewidth=2, linestyle='-')  # linestyle='-' 表示实线

# 打印数据（前 50 个）
for i in range(50):
    print(f"({x[i]}, {y[i]})")
```

### 3.6 箱线图（boxplot） <a id="3-6"></a>

![image.png](<图片和附件/image 5.png>)

```Python
# 箱线图-boxplot
# 公共设置（导入、字体）与结尾的 plt.show() 同 3.2，此处省略

# 数据准备
data = {
    '语文': [80, 90, 89, 93, 85, 86, 81, 72, 77],
    '数学': [90, 82, 98, 88, 92, 67, 79, 63, 71],
    '英语': [85, 92, 78, 97, 95, 74, 78, 69, 79],
    '物理': [68, 74, 72, 85, 63, 97, 79, 61, 90],
    '化学': [71, 99, 66, 83, 95, 78, 88, 72, 93],
    '生物': [84, 67, 96, 75, 89, 62, 91, 77, 100],
}

plt.figure(figsize=(10, 7.5))  # 图表尺寸与 3.2 不同，单独设置
plt.boxplot(data.values(), tick_labels=data.keys())  # tick_labels 表示 x 轴的标签

# 添加标题
plt.title("各科成绩分布", fontsize=20)
plt.ylabel("分数", fontsize=12)

# 添加坐标轴刻度
plt.yticks(range(60, 101, 1), fontsize=8)
plt.xticks(fontsize=12)

# 添加网格线
plt.grid(True, axis='y', linestyle='--', alpha=0.5)
```

### 3.7 seaborn <a id="3-7"></a>

matplotlib 能绘制的图，seaborn 也能绘制，且更强大。seaborn 的 `pairplot()` 函数能一次性绘制多种图表。下面用 seaborn 重绘 3.3 的条形图：

```Python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# 中文字体设置同 3.2；数据沿用 3.3 的 AI_tool 和 score
data = pd.DataFrame({'AI': AI_tool, '评分': score})

# 绘制柱状图
plt.figure(figsize=(10, 5))
sns.barplot(data=data, x='AI', y='评分')

# 添加网格线
plt.grid(axis='y', alpha=0.5, linestyle='--')

# 显示图表
plt.show()
```

---

## 4. 总结 <a id="summary"></a>

数据分析的完整流程：

```Plain Text
┌─────────────────────────────────────────────┐
│ ① 数据导入                                  │
│                                             │
│    df = pd.read_csv("文件位置")             │
└──────────────────┬──────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────┐
│ ② 数据清洗                                  │
│                                             │
│    dropna / fillna / 去重 / 分箱            │
└──────────────────┬──────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────┐
│ ③ 数据分析                                  │
│                                             │
│    groupby / melt / 时间序列                │
└──────────────────┬──────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────┐
│ ④ 数据可视化                                │
│                                             │
│    plt.plot / bar / pie / scatter ...       │
└─────────────────────────────────────────────┘
```

先用 Pandas 导入并清洗数据，再用 Pandas 分析，最后用 Matplotlib 展示结果。

