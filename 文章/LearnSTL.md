# C++ STL 完全学习指南：从入门到进阶


## 前言

STL（Standard Template Library，标准模板库）是 C++ 标准库的核心组成部分，是一个具有工业强度的、高效的 C++ 程序库。它包含了诸多在计算机科学领域常用的基本数据结构和基本算法，为 C++ 程序员提供了一个可扩展的应用框架，高度体现了软件的可复用性。

STL 基于泛型编程思想设计，以类型参数化的方式实现，基于模板，使得 STL 能够适用于各种不同的数据类型。


## 第一部分：入门篇

### 一、STL 的六大组件

STL 由六大组件构成：

| 组件 | 说明 |
|------|------|
| **容器（Containers）** | 封装各种数据结构，如数组、链表、集合、映射等 |
| **算法（Algorithms）** | 提供排序、查找、拷贝、合并等通用算法 |
| **迭代器（Iterators）** | 连接容器与算法的桥梁，用于遍历容器 |
| **函数对象（Function Objects / Functors）** | 行为像函数的对象，用于定制算法行为 |
| **适配器（Adapters）** | 对已有组件进行封装，改变其接口 |
| **分配器（Allocators）** | 负责内存分配与管理 |

STL 的核心思想是**算法与容器独立**，通过迭代器连接，配合函数对象实现灵活扩展。


### 二、容器（Containers）

容器是 STL 中用于存储数据的数据结构。容器大致分为四类：**序列容器、有序关联容器、无序关联容器、容器适配器**。

#### 2.1 序列容器（Sequence Containers）

序列容器中的元素按线性顺序存储，保持元素的插入顺序。

| 容器 | 说明 | 头文件 |
|------|------|--------|
| `vector` | 动态数组，支持随机访问，尾部插入删除高效 | `<vector>` |
| `deque` | 双端队列，头尾插入删除都高效 | `<deque>` |
| `list` | 双向链表，任意位置插入删除高效，不支持随机访问 | `<list>` |
| `forward_list` | C++11 引入，单向链表 | `<forward_list>` |
| `array` | C++11 引入，定长数组，C 风格数组的简单包装 | `<array>` |

**vector 基本用法示例：**
```cpp
#include <vector>
#include <iostream>

int main() {
    std::vector<int> vec;          // 创建空 vector
    vec.push_back(10);             // 尾部插入
    vec.push_back(20);
    vec.push_back(30);
    
    std::cout << vec[0] << std::endl;    // 随机访问：10
    std::cout << vec.size() << std::endl; // 大小：3
    
    for (int x : vec) {            // 范围 for 遍历
        std::cout << x << " ";
    }
    return 0;
}
```

**list 基本用法示例：**
```cpp
#include <list>
#include <iostream>

int main() {
    std::list<int> lst = {1, 2, 3, 4, 5};
    
    lst.push_front(0);             // 头部插入
    lst.push_back(6);              // 尾部插入
    
    auto it = lst.begin();
    std::advance(it, 3);
    lst.insert(it, 100);           // 在任意位置插入
    
    for (int x : lst) {
        std::cout << x << " ";     // 0 1 2 100 3 4 5 6
    }
    return 0;
}
```

#### 2.2 关联容器（Associative Containers）

关联容器通过键值对来存储和访问元素，能提供快速的查找功能，通常基于红黑树实现，元素自动按键排序。

| 容器 | 说明 | 头文件 |
|------|------|--------|
| `set` | 键的集合，不允许重复，自动排序 | `<set>` |
| `multiset` | 允许重复键的集合 | `<set>` |
| `map` | 键值对，键唯一 | `<map>` |
| `multimap` | 键值对，键可重复 | `<map>` |

**set 基本用法示例：**
```cpp
#include <set>
#include <iostream>

int main() {
    std::set<int> s;
    s.insert(5);
    s.insert(1);
    s.insert(3);
    s.insert(3);  // 重复插入，被忽略
    
    for (int x : s) {
        std::cout << x << " ";     // 1 3 5（自动排序）
    }
    
    if (s.find(3) != s.end()) {
        std::cout << "Found 3" << std::endl;
    }
    return 0;
}
```

**map 基本用法示例：**
```cpp
#include <map>
#include <iostream>
#include <string>

int main() {
    std::map<std::string, int> scores;
    scores["Alice"] = 95;
    scores["Bob"] = 87;
    scores["Charlie"] = 92;
    
    for (const auto& pair : scores) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    // 按 key 自动排序：Alice: 95, Bob: 87, Charlie: 92
    
    std::cout << "Bob's score: " << scores["Bob"] << std::endl;
    return 0;
}
```

#### 2.3 无序容器（Unordered Containers）

C++11 引入，基于哈希表实现，元素无序存储，只关心"元素是否存在"或"键与值的对应关系"。

| 容器 | 说明 | 头文件 |
|------|------|--------|
| `unordered_set` | 无序集合，不允许重复 | `<unordered_set>` |
| `unordered_multiset` | 无序集合，允许重复 | `<unordered_set>` |
| `unordered_map` | 无序映射，键唯一 | `<unordered_map>` |
| `unordered_multimap` | 无序映射，键可重复 | `<unordered_map>` |

**unordered_map 基本用法示例：**
```cpp
#include <unordered_map>
#include <iostream>
#include <string>

int main() {
    std::unordered_map<std::string, int> scores;
    scores["Alice"] = 95;
    scores["Bob"] = 87;
    scores["Charlie"] = 92;
    
    // 元素无序输出
    for (const auto& pair : scores) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    return 0;
}
```

#### 2.4 容器适配器（Container Adapters）

容器适配器并不是真正的容器，它们对容器进行包装，使其表现出另一种行为。

| 适配器 | 说明 | 默认底层容器 | 头文件 |
|--------|------|-------------|--------|
| `stack` | 后进先出（LIFO） | `deque` | `<stack>` |
| `queue` | 先进先出（FIFO） | `deque` | `<queue>` |
| `priority_queue` | 优先级队列 | `vector` | `<queue>` |

**stack 基本用法示例：**
```cpp
#include <stack>
#include <iostream>

int main() {
    std::stack<int> st;
    st.push(10);
    st.push(20);
    st.push(30);
    
    while (!st.empty()) {
        std::cout << st.top() << " ";  // 30 20 10
        st.pop();
    }
    return 0;
}
```

#### 2.5 容器的共有函数

所有 STL 容器都提供以下共有函数：

| 函数 | 说明 |
|------|------|
| `begin()` / `cbegin()` | 返回指向开头元素的迭代器 |
| `end()` / `cend()` | 返回指向末尾下一个元素的迭代器 |
| `size()` | 返回容器内元素个数 |
| `empty()` | 返回容器是否为空 |
| `clear()` | 清空容器 |
| `swap()` | 交换两个容器 |


### 三、迭代器（Iterators）

迭代器是 STL 中最重要的概念之一，它提供了一种访问容器内元素的统一方式。可以把迭代器理解为指向容器元素的指针，通过迭代器可以遍历容器中的元素，而无需关心容器的内部实现细节。

#### 3.1 迭代器的分类

迭代器分为五种类别，形成层次结构：

| 类别 | 说明 | 支持的操作 | 典型容器 |
|------|------|-----------|---------|
| **输入迭代器** | 只读，单向，只能遍历一次 | `++`, `*`（读） | `istream_iterator` |
| **输出迭代器** | 只写，单向，只能遍历一次 | `++`, `*`（写） | `ostream_iterator` |
| **正向迭代器** | 可读写，单向，可多次遍历 | `++`, `*`（读写） | `forward_list` |
| **双向迭代器** | 可读写，双向移动 | `++`, `--`, `*` | `list`, `set`, `map` |
| **随机访问迭代器** | 最强大，支持随机访问 | `++`, `--`, `+`, `-`, `[]`, `<`, `>` | `vector`, `array`, `deque` |

迭代器类别之间是**层次包含**关系：随机访问迭代器 > 双向迭代器 > 正向迭代器 > 输入/输出迭代器。

#### 3.2 迭代器的基本操作

```cpp
#include <vector>
#include <iostream>

int main() {
    std::vector<int> vec = {10, 20, 30, 40, 50};
    
    // 正向遍历
    for (std::vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) {
        std::cout << *it << " ";    // 10 20 30 40 50
    }
    
    // 逆向遍历
    for (std::vector<int>::reverse_iterator rit = vec.rbegin(); rit != vec.rend(); ++rit) {
        std::cout << *rit << " ";    // 50 40 30 20 10
    }
    
    // 随机访问
    auto it = vec.begin();
    std::advance(it, 3);             // 向前移动 3 步
    std::cout << *it;                // 40
    return 0;
}
```

#### 3.3 迭代器的有效性

使用迭代器时需要注意：
- 迭代器必须合法有效
- 不能解引用尾后迭代器（`end()`）
- 区间必须合法：指向同一容器，从第一个迭代器出发能够到达第二个迭代器


### 四、算法（Algorithms）

STL 算法是用于操作容器中数据的函数模板，通过迭代器来访问容器中的元素，实现算法与容器的解耦。

STL 提供了约 60 多种通用算法，主要分布在 `<algorithm>` 和 `<numeric>` 头文件中。

#### 4.1 常用算法分类

**非变序算法**（不修改容器内容）：
- `find` / `find_if`：查找元素
- `count` / `count_if`：计数
- `for_each`：遍历
- `adjacent_find`：查找相邻重复元素

**变序算法**（可能修改容器）：
- `copy` / `copy_if`：拷贝
- `replace` / `replace_if`：替换
- `remove` / `remove_if`：移除
- `transform`：转换
- `reverse`：反转

**排序和比较算法**：
- `sort`：排序，时间复杂度 O(n log n)
- `stable_sort`：稳定排序
- `partial_sort`：部分排序
- `nth_element`：使第 n 个元素位于其最终有序位置上
- `merge`：合并
- `max` / `min`：最大/最小值

**数值算法**（`<numeric>`）：
- `accumulate`：累加
- `inner_product`：内积
- `partial_sum`：部分和
- `adjacent_difference`：相邻差

#### 4.2 算法使用示例

```cpp
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> vec = {5, 2, 8, 1, 9, 3};
    
    // 排序
    std::sort(vec.begin(), vec.end());  // 1 2 3 5 8 9
    
    // 查找
    auto it = std::find(vec.begin(), vec.end(), 5);
    if (it != vec.end()) {
        std::cout << "Found: " << *it << std::endl;
    }
    
    // 遍历
    std::for_each(vec.begin(), vec.end(), [](int x) {
        std::cout << x << " ";
    });
    
    // 计数
    int count = std::count_if(vec.begin(), vec.end(), [](int x) {
        return x > 4;
    });
    std::cout << "Count > 4: " << count << std::endl;  // 3
    
    return 0;
}
```

#### 4.3 C++98 以来的算法演进

C++98 标准批准了约 70 个算法，C++11 新增了约 20 个算法。

**C++11 新增算法包括**：`all_of`、`any_of`、`none_of`、`copy_if`、`copy_n`、`find_if_not`、`iota`、`is_sorted`、`is_sorted_until`、`shuffle`、`move`、`move_backward` 等。


### 五、函数对象（Function Objects / Functors）

函数对象是行为类似函数的对象，从实现角度来看，是重载了 `operator()` 的类或类模板。

#### 5.1 基本用法

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

class Adder {
public:
    int operator()(int a, int b) const {
        return a + b;
    }
};

int main() {
    Adder add;
    std::cout << add(3, 5) << std::endl;  // 8
    
    // 在算法中使用函数对象
    std::vector<int> vec = {1, 2, 3, 4, 5};
    std::transform(vec.begin(), vec.end(), vec.begin(), [](int x) {
        return x * 2;
    });
    // vec: 2 4 6 8 10
    return 0;
}
```

#### 5.2 STL 内置函数对象

STL 在 `<functional>` 中提供了多种内置函数对象：

| 函数对象 | 说明 |
|----------|------|
| `std::plus<T>` | 加法 |
| `std::minus<T>` | 减法 |
| `std::multiplies<T>` | 乘法 |
| `std::divides<T>` | 除法 |
| `std::modulus<T>` | 取模 |
| `std::negate<T>` | 取负 |
| `std::equal_to<T>` | 等于 |
| `std::not_equal_to<T>` | 不等于 |
| `std::greater<T>` | 大于 |
| `std::less<T>` | 小于 |
| `std::logical_and<T>` | 逻辑与 |
| `std::logical_or<T>` | 逻辑或 |
| `std::logical_not<T>` | 逻辑非 |

```cpp
#include <functional>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> vec = {5, 2, 8, 1, 9, 3};
    
    // 使用 greater 进行降序排序
    std::sort(vec.begin(), vec.end(), std::greater<int>());
    // vec: 9 8 5 3 2 1
    
    return 0;
}
```


## 第二部分：进阶篇

### 六、分配器（Allocators）

分配器负责空间的配置与管理。STL 容器通过分配器来管理内存分配，默认使用 `std::allocator`。

#### 6.1 自定义分配器

```cpp
#include <memory>
#include <vector>
#include <iostream>

template <typename T>
class CustomAllocator {
public:
    using value_type = T;
    
    CustomAllocator() = default;
    
    template <typename U>
    CustomAllocator(const CustomAllocator<U>&) {}
    
    T* allocate(std::size_t n) {
        std::cout << "Allocating " << n << " elements" << std::endl;
        return static_cast<T*>(::operator new(n * sizeof(T)));
    }
    
    void deallocate(T* p, std::size_t n) {
        std::cout << "Deallocating " << n << " elements" << std::endl;
        ::operator delete(p);
    }
};

int main() {
    std::vector<int, CustomAllocator<int>> vec;
    vec.push_back(10);
    vec.push_back(20);
    return 0;
}
```

#### 6.2 分配器的使用场景

自定义分配器的常见应用场景包括：
- 内存池管理
- 共享内存分配
- 内存跟踪和调试
- 特定硬件的内存分配


### 七、适配器（Adapters）

适配器用于转换容器、迭代器或函数对象的接口，使原本不兼容的组件可以一起工作。

#### 7.1 容器适配器

已在 2.4 节介绍，包括 `stack`、`queue`、`priority_queue`。

#### 7.2 迭代器适配器

| 适配器 | 说明 | 头文件 |
|--------|------|--------|
| `reverse_iterator` | 逆向迭代器 | `<iterator>` |
| `back_insert_iterator` | 尾部插入迭代器 | `<iterator>` |
| `front_insert_iterator` | 头部插入迭代器 | `<iterator>` |
| `insert_iterator` | 任意位置插入迭代器 | `<iterator>` |
| `istream_iterator` | 输入流迭代器 | `<iterator>` |
| `ostream_iterator` | 输出流迭代器 | `<iterator>` |

**迭代器适配器示例：**
```cpp
#include <iterator>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> vec = {1, 2, 3};
    std::vector<int> dest;
    
    // 使用 back_inserter 在尾部插入
    std::copy(vec.begin(), vec.end(), std::back_inserter(dest));
    // dest: 1 2 3
    
    // 使用 ostream_iterator 输出到标准输出
    std::copy(dest.begin(), dest.end(), 
              std::ostream_iterator<int>(std::cout, " "));
    // 输出: 1 2 3
    return 0;
}
```

#### 7.3 函数对象适配器（C++11 之前）

C++11 之前，`<functional>` 提供了多种函数适配器：

| 适配器 | 说明 |
|--------|------|
| `bind1st` | 绑定第一个参数 |
| `bind2nd` | 绑定第二个参数 |
| `not1` | 一元取反 |
| `not2` | 二元取反 |
| `mem_fun` | 成员函数适配器 |
| `mem_fun_ref` | 成员函数引用适配器 |

**注意**：C++11 之后推荐使用 `std::bind` 和 Lambda 表达式替代这些适配器。


### 八、深入理解迭代器

#### 8.1 迭代器失效

迭代器失效是 STL 使用中最常见的问题之一。不同容器的操作会导致不同的迭代器失效情况。

| 容器 | 操作 | 失效情况 |
|------|------|---------|
| `vector` | `push_back` | 若 reallocation，所有迭代器失效 |
| `vector` | `insert` / `erase` | 插入/删除位置之后的迭代器失效 |
| `deque` | `push_front` / `push_back` | 所有迭代器可能失效 |
| `deque` | `insert` / `erase` | 中间插入/删除使所有迭代器失效 |
| `list` | `insert` / `erase` | 仅被删除元素的迭代器失效 |
| `set` / `map` | `insert` / `erase` | 仅被删除元素的迭代器失效 |
| `unordered_map` | `rehash` / `reserve` | 所有迭代器可能失效 |

```cpp
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};
    
    // 错误示例：迭代器失效
    for (auto it = vec.begin(); it != vec.end(); ++it) {
        if (*it == 3) {
            vec.erase(it);  // it 失效！
            // 正确的做法：it = vec.erase(it);
        }
    }
    
    // 正确示例
    for (auto it = vec.begin(); it != vec.end();) {
        if (*it == 3) {
            it = vec.erase(it);  // erase 返回下一个有效迭代器
        } else {
            ++it;
        }
    }
    return 0;
}
```

#### 8.2 迭代器标签（Iterator Tags）

STL 使用迭代器标签来区分迭代器类别，用于算法优化：

```cpp
#include <iterator>
#include <vector>
#include <list>

template <typename Iterator>
void advanced_algorithm(Iterator first, Iterator last) {
    using Category = typename std::iterator_traits<Iterator>::iterator_category;
    
    if constexpr (std::is_same_v<Category, std::random_access_iterator_tag>) {
        // 随机访问迭代器优化路径
    } else {
        // 通用路径
    }
}
```

#### 8.3 自定义迭代器

```cpp
#include <iterator>

template <typename T>
class MyContainer {
public:
    class Iterator : public std::iterator<std::forward_iterator_tag, T> {
    public:
        Iterator(T* ptr) : m_ptr(ptr) {}
        
        T& operator*() { return *m_ptr; }
        Iterator& operator++() { ++m_ptr; return *this; }
        bool operator==(const Iterator& other) const { return m_ptr == other.m_ptr; }
        bool operator!=(const Iterator& other) const { return !(*this == other); }
        
    private:
        T* m_ptr;
    };
    
    Iterator begin() { return Iterator(m_data); }
    Iterator end() { return Iterator(m_data + m_size); }
    
private:
    T m_data[100];
    std::size_t m_size = 0;
};
```


### 九、算法进阶

#### 9.1 算法复杂度

理解算法的复杂度对于选择合适的算法至关重要：

| 算法 | 时间复杂度 | 说明 |
|------|-----------|------|
| `sort` | O(n log n) | 内省排序 |
| `stable_sort` | O(n log n) | 稳定排序 |
| `partial_sort` | O(n log k) | 部分排序 |
| `nth_element` | O(n) | 第 n 个元素排序 |
| `find` | O(n) | 线性查找 |
| `binary_search` | O(log n) | 二分查找 |
| `lower_bound` / `upper_bound` | O(log n) | 边界查找 |

#### 9.2 算法与 Lambda 表达式

现代 C++ 中，Lambda 表达式是配合 STL 算法的最佳方式：

```cpp
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // 使用 Lambda 筛选偶数
    auto even_count = std::count_if(vec.begin(), vec.end(), 
        [](int x) { return x % 2 == 0; });
    
    // 使用 Lambda 自定义排序
    std::sort(vec.begin(), vec.end(), 
        [](int a, int b) { return a > b; });
    
    // 使用 Lambda 进行转换
    std::transform(vec.begin(), vec.end(), vec.begin(),
        [](int x) { return x * x; });
    
    return 0;
}
```

#### 9.3 并行算法（C++17）

C++17 引入了并行算法，可以通过执行策略（Execution Policy）来并行执行算法：

```cpp
#include <algorithm>
#include <execution>
#include <vector>

int main() {
    std::vector<int> vec(1000000);
    // 填充数据...
    
    // 并行排序
    std::sort(std::execution::par, vec.begin(), vec.end());
    
    // 并行查找
    auto result = std::find(std::execution::par, vec.begin(), vec.end(), 42);
    
    return 0;
}
```

执行策略类型：
- `std::execution::seq`：顺序执行
- `std::execution::par`：并行执行
- `std::execution::par_unseq`：并行且向量化执行


### 十、C++17 / C++20 新特性

#### 10.1 范围库（Ranges，C++20）

C++20 引入了范围库，是对迭代器和泛型算法库的扩展，使得迭代器和算法可以组合使用，并减少错误。

```cpp
#include <ranges>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // 使用 ranges 进行链式操作
    auto result = vec 
        | std::views::filter([](int x) { return x % 2 == 0; })
        | std::views::transform([](int x) { return x * x; })
        | std::views::take(3);
    
    for (int x : result) {
        std::cout << x << " ";  // 4 16 36
    }
    return 0;
}
```

#### 10.2 受约束算法（C++20）

C++20 在 `std::ranges` 命名空间中提供了大多数算法的受约束版本：

```cpp
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> vec = {5, 2, 8, 1, 9, 3};
    
    // C++17 方式
    std::sort(vec.begin(), vec.end());
    
    // C++20 ranges 方式
    std::ranges::sort(vec);
    
    auto it = std::ranges::find(vec, 5);
    
    return 0;
}
```

#### 10.3 C++17 新增容器

C++17 引入了 `std::pmr` 命名空间，提供多态分配器支持：

```cpp
#include <vector>
#include <memory_resource>

int main() {
    // 使用多态分配器
    std::pmr::vector<int> vec;  // 使用默认的多态分配器
    vec.push_back(10);
    return 0;
}
```


### 十一、性能优化与最佳实践

#### 11.1 容器选择指南

| 场景 | 推荐容器 | 原因 |
|------|---------|------|
| 需要频繁随机访问 | `vector` / `array` | O(1) 随机访问 |
| 频繁在尾部插入/删除 | `vector` | 尾部操作高效 |
| 频繁在任意位置插入/删除 | `list` | O(1) 插入删除 |
| 频繁在头尾插入/删除 | `deque` | 头尾都高效 |
| 需要快速查找（有序） | `set` / `map` | O(log n) 查找 |
| 需要快速查找（无序） | `unordered_set` / `unordered_map` | O(1) 平均查找 |
| LIFO 操作 | `stack` | 专用接口 |
| FIFO 操作 | `queue` | 专用接口 |
| 需要优先级排序 | `priority_queue` | 自动维护堆 |

#### 11.2 性能优化技巧

**1. 预留空间**
```cpp
std::vector<int> vec;
vec.reserve(1000);  // 预分配空间，避免多次 reallocation
```

**2. 使用 emplace 代替 push**
```cpp
std::vector<std::pair<int, std::string>> vec;
vec.emplace_back(1, "hello");  // 直接在容器中构造
vec.push_back({1, "hello"});   // 先构造再移动
```

**3. 避免不必要的拷贝**
```cpp
// 不好的做法
for (auto x : vec) { ... }  // 拷贝每个元素

// 好的做法
for (const auto& x : vec) { ... }  // 引用
for (auto&& x : vec) { ... }       // 完美转发
```

**4. 选择合适的算法**
- 小数据量使用线性查找，大数据量考虑二分查找
- 对已排序数据使用 `lower_bound` / `upper_bound`
- 使用 `nth_element` 而非完整排序（仅需第 n 个元素时）

#### 11.3 常见陷阱

1. **迭代器失效**：修改容器后，原有迭代器可能失效
2. **尾后迭代器解引用**：`end()` 不指向有效元素
3. **空容器操作**：对空容器调用 `front()` / `back()` 是未定义行为
4. **比较器不严格弱序**：自定义比较器必须满足严格弱序要求
5. **在遍历中修改容器**：可能导致迭代器失效或未定义行为


### 十二、总结

STL 是 C++ 编程不可或缺的工具库，掌握 STL 是成为优秀 C++ 开发者的必经之路。

**学习路径建议**：

1. **入门阶段**：掌握常用容器（`vector`、`list`、`map`、`set`）的基本操作，熟悉迭代器的使用
2. **进阶阶段**：深入理解各种容器的内部实现和性能特点，掌握 STL 算法的使用
3. **高级阶段**：理解分配器、适配器的工作原理，能够自定义迭代器和分配器，熟悉 C++17/20 的新特性

STL 就像一个强大的工具箱，里面装满了各种各样实用的工具。熟练使用这些工具，能够大大提高开发效率和代码质量。


### 附录：常用头文件速查

| 头文件 | 内容 |
|--------|------|
| `<vector>` | `vector` 容器 |
| `<deque>` | `deque` 容器 |
| `<list>` | `list` 容器 |
| `<forward_list>` | `forward_list` 容器 |
| `<array>` | `array` 容器 |
| `<set>` | `set`、`multiset` 容器 |
| `<map>` | `map`、`multimap` 容器 |
| `<unordered_set>` | `unordered_set`、`unordered_multiset` 容器 |
| `<unordered_map>` | `unordered_map`、`unordered_multimap` 容器 |
| `<stack>` | `stack` 容器适配器 |
| `<queue>` | `queue`、`priority_queue` 容器适配器 |
| `<algorithm>` | 通用算法 |
| `<numeric>` | 数值算法 |
| `<iterator>` | 迭代器和迭代器适配器 |
| `<functional>` | 函数对象和函数适配器 |
| `<memory>` | 分配器 |
| `<ranges>` | 范围库（C++20） |
| `<execution>` | 并行执行策略（C++17） |