# C++ STL

## 目录

- [1. 容器Container：存数据](#container)
  - [1.1 vector（动态数组）](#1-1)
  - [1.2 list（双向链表）](#1-2)
  - [1.3 stack（栈）](#1-3)
  - [1.4 queue（队列）](#1-4)
  - [1.5 deque（双向队列）](#1-5)
  - [1.6 set（集合）](#1-6)
  - [1.7 map（映射）](#1-7)
  - [1.8 unordered_set / unordered_map（哈希容器）](#1-8)
  - [1.9 priority_queue（优先队列）](#1-9)
  - [1.10 pair](#1-10)
- [2. Iterator：访问数据（类似指针）](#iterator)
  - [2.1 特性](#2-1)
  - [2.2 函数](#2-2)
  - [2.3 遍历方式](#2-3)
  - [2.4 迭代器分类](#2-4)
  - [2.5 迭代器失效](#2-5)
- [3. algorithm：对数据操作（sort/find...）](#algorithm)
- [4. 仿函数与 lambda](#functor)
  - [4.1 仿函数（函数对象）](#4-1)
  - [4.2 lambda 表达式（C++11）](#4-2)

---

## 1. 容器Container：存数据 <a id="container"></a>

### 1.1 vector（动态数组） <a id="1-1"></a>

#### 1.1.1 特性

- 连续内存（类似数组）
- 尾部插入快（摊还 O(1)）
- 支持随机访问 O(1)

#### 1.1.2 常用函数：

| 函数 | 作用 |
|---|---|
| `push_back(x)` | 尾部插入元素 x |
| `pop_back()` | 删除尾部元素 |
| `size()` | 返回元素个数 |
| `empty()` | 判断是否为空 |
| `clear()` | 清空所有元素 |
| `at(i)` | 访问下标 i 的元素（越界报错） |
| `front()` | 返回第一个元素 |
| `back()` | 返回最后一个元素 |
| `begin(), end()` | 返回首元素迭代器和尾后迭代器 |
| `insert(pos, x)` | 在 pos 位置插入元素 x |
| `erase(pos)` | 删除 pos 位置的元素 |

### 1.2 list（双向链表） <a id="1-2"></a>

#### 1.2.1 特性

- 非连续内存
- 插入删除 O(1)
- 不支持随机访问

#### 1.2.2 常用函数

| 函数 | 作用 |
|---|---|
| `push_back(x)` | 尾部插入元素 x |
| `push_front(x)` | 头部插入元素 x |
| `insert(pos, x)` | 在 pos 位置插入元素 x |
| `erase(pos)` | 删除 pos 位置的元素 |
| `remove(x)` | 删除所有值为 x 的元素 |

### 1.3 stack（栈） <a id="1-3"></a>

#### 1.3.1 特性

- 后进先出（LIFO）

#### 1.3.2 常用函数

| 函数 | 作用 |
|---|---|
| `push(x)` | 入栈（压入栈顶） |
| `pop()` | 出栈（删除栈顶元素） |
| `top()` | 返回栈顶元素 |
| `empty()` | 判断栈是否为空 |

### 1.4 queue（队列） <a id="1-4"></a>

#### 1.4.1 特性

- 先进先出（FIFO）

#### 1.4.2 常用函数

| 函数 | 作用 |
|---|---|
| `push(x)` | 入队（尾部插入） |
| `pop()` | 出队（删除队首元素） |
| `front()` | 返回队首元素 |
| `back()` | 返回队尾元素 |

### 1.5 deque（双向队列） <a id="1-5"></a>

#### 1.5.1 特性

- 两端插入/删除都快 O(1)
- 支持随机访问
- 内存不完全连续

#### 1.5.2 常用函数

| 函数 | 作用 |
|---|---|
| `push_back(x)` | 尾部插入元素 x |
| `push_front(x)` | 头部插入元素 x |
| `pop_back()` | 删除尾部元素 |
| `pop_front()` | 删除头部元素 |
| `front()` | 返回头部元素 |
| `back()` | 返回尾部元素 |

### 1.6 set（集合） <a id="1-6"></a>

#### 1.6.1 特性

- 自动排序
- 元素去重唯一
- 底层：红黑树 O(log n)

#### 1.6.2 常用函数

| 函数 | 作用 |
|---|---|
| `insert(x)` | 插入元素 x |
| `erase(x)` | 删除元素 x |
| `count(x)` | 元素 x 是否存在（0/1） |
| `find(x)` | 查找 x，返回迭代器 |
| `lower_bound(x)` | 返回第一个 ≥ x 的迭代器 |
| `upper_bound(x)` | 返回第一个 > x 的迭代器 |

### 1.7 map（映射） <a id="1-7"></a>

#### 1.7.1 特性

- key 自动排序
- key 唯一

#### 1.7.2 常用函数

| 函数 | 作用 |
|---|---|
| `mp[key] = val` | 插入或修改键值对 |
| `insert({k, v})` | 插入键值对 {k, v} |
| `erase(k)` | 删除键 k 的键值对 |
| `find(k)` | 查找键 k，返回迭代器 |
| `count(k)` | 键 k 是否存在（0/1） |

### 1.8 unordered_set / unordered_map（哈希容器） <a id="1-8"></a>

#### 1.8.1 特性

- 底层：哈希表，增删查平均 O(1)
- 无序（元素不自动排序）
- key 唯一

#### 1.8.2 常用函数（与 set/map 相同）

| 函数 | 作用 |
|---|---|
| `insert(x)` | 插入元素 x |
| `erase(x)` | 删除元素 x |
| `count(x)` | x 是否存在（0/1） |
| `find(x)` | 查找 x，返回迭代器 |

#### 1.8.3 与 set/map 的区别

- set/map：红黑树，自动排序，查询 O(log n)
- unordered_set/unordered_map：哈希表，无序，查询平均 O(1)
- 需要排序用 set/map；只查重、查映射用 unordered

### 1.9 priority_queue（优先队列） <a id="1-9"></a>

#### 1.9.1 特性

- 底层：堆
- 默认大顶堆：top() 永远是最大元素
- 常用于求前 K 大、Dijkstra 最短路

#### 1.9.2 常用函数

| 函数 | 作用 |
|---|---|
| `push(x)` | 入队 |
| `pop()` | 出队（删除队首/最大元素） |
| `top()` | 返回队首（最大）元素 |
| `empty()` | 判断是否为空 |
| `size()` | 返回元素个数 |

#### 1.9.3 小顶堆（队首最小）

```C++
priority_queue<int, vector<int>, greater<int>> pq;
```

### 1.10 pair <a id="1-10"></a>

#### 1.10.1 特性

- 将两个值组合成一个整体（map 里存的就是 pair）
- 不是容器，但常与容器配合使用

#### 1.10.2 常用用法

| 用法 | 作用 |
|---|---|
| `make_pair(a, b)` | 创建 pair |
| `{a, b}` | 初始化 pair（C++11） |
| `p.first` | 第一个值 |
| `p.second` | 第二个值 |

---

## 2. Iterator：访问数据（类似指针） <a id="iterator"></a>

### 2.1 特性 <a id="2-1"></a>

- “广义指针”，用来遍历容器元素，连接容器和算法
- C++11可用auto代替iterator

### 2.2 函数 <a id="2-2"></a>

| 函数 | 作用 |
|---|---|
| `begin(), end()` | 返回指向首元素/尾后位置的迭代器 |
| `rbegin(), rend()` | 返回反向迭代器（从尾到头遍历） |
| `cbegin(), cend()` | 返回 const 迭代器（这里的 c 是 const） |

### 2.3 遍历方式 <a id="2-3"></a>

- 传统迭代器遍历：`for(auto it = v.begin(); it != v.end(); it++)`，用 `*it` 访问元素
- 范围 for（C++11）：`for(auto x : v)`，直接得到元素
- 要修改元素时用引用：`for(auto &x : v)`

### 2.4 迭代器分类 <a id="2-4"></a>

- 随机访问迭代器：vector、deque，支持 `it + n`、`it[n]`、`it1 - it2`
- 双向迭代器：list、set、map，支持 `++`、`--`
- 前向迭代器：unordered_set、unordered_map，只支持 `++`

### 2.5 迭代器失效 <a id="2-5"></a>

- vector 插入/删除元素后，原迭代器可能失效（可能重新分配内存）
- list、set、map 删除元素后，只有指向被删元素的迭代器失效
- 解决办法：用 erase 返回的新迭代器

```C++
it = v.erase(it); // erase 返回下一个元素的迭代器
```

---

## 3. algorithm：对数据操作（sort/find...） <a id="algorithm"></a>

| 功能 | 用法 |
|---|---|
| 默认升序 | `sort(v.begin(), v.end());` |
| 降序 | `sort(v.begin(), v.end(), greater<int>());` |
| 自定义排序 | `bool cmp(int a, int b){ return a > b; }`<br>`sort(v.begin(), v.end(), cmp);` |
| 自定义排序（lambda） | `sort(v.begin(), v.end(), [](int a, int b){ return a > b; });` |
| 反转 | `reverse(v.begin(), v.end());` |
| 去重 | `sort(v.begin(), v.end());`<br>`v.erase(unique(v.begin(), v.end()), v.end());` |
| 二分查找 | `binary_search(v.begin(), v.end(), x)` |
| 二分查找（下界/上界） | `lower_bound(v.begin(), v.end(), x); // 第一个 ≥ x 的迭代器`<br>`upper_bound(v.begin(), v.end(), x); // 第一个 > x 的迭代器` |
| 极值 | `max_element(v.begin(), v.end());`<br>`min_element(v.begin(), v.end());` |
| 查找 | `find(v.begin(), v.end(), x)` |
| 计数 | `count(v.begin(), v.end(), x)` |
| 填充 | `fill(v.begin(), v.end(), 0); // 全部填充为 0` |
| 交换 | `swap(v1, v2);` |
| 全排列 | `next_permutation(v.begin(), v.end()); // 下一个排列，配合 do-while 枚举` |
| 求和 | `accumulate(v.begin(), v.end(), 0); // 初始值 0（<numeric> 头文件）` |
| 合并 | `merge(a.begin(), a.end(), b.begin(), b.end(), c.begin());` |

---

## 4. 仿函数与 lambda <a id="functor"></a>

### 4.1 仿函数（函数对象） <a id="4-1"></a>

- 重载了 `operator()` 的类的对象，可以像函数一样调用
- STL 算法里的 `greater<int>` 就是一个仿函数，用来比较两个数

```C++
class Greater{
public:
    bool operator()(int a, int b){
        return a > b;
    }
};

Greater g;
g(3, 5); // 像函数一样调用
```

### 4.2 lambda 表达式（C++11） <a id="4-2"></a>

- 匿名函数，不必单独定义函数
- 格式：`[捕获列表](参数列表){ 函数体 }`
- 常用于 sort 自定义排序

```C++
sort(v.begin(), v.end(), [](int a, int b){
    return a > b;
}); // 不用写 cmp 函数，直接传匿名函数
```

