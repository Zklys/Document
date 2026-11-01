# C++ STL

## 1. STL 六大组件

| 组件 | 说明 |
|------|------|
| **容器（Containers）** | 封装各种数据结构，如数组、链表、集合、映射等 |
| **算法（Algorithms）** | 提供排序、查找、拷贝、合并等通用算法 |
| **迭代器（Iterators）** | 连接容器与算法的桥梁，用于遍历容器 |
| **函数对象（Function Objects / Functors）** | 行为像函数的对象，用于定制算法行为 |
| **适配器（Adapters）** | 对已有组件进行封装，改变其接口 |
| **分配器（Allocators）** | 负责内存分配与管理 |

核心思想：算法与容器独立，通过迭代器连接，函数对象负责定制。

## 2. 容器

容器分为四类：序列容器、有序关联容器、无序关联容器、容器适配器。

### 2.1 序列容器

元素按线性顺序存储，保持插入顺序。

| 容器 | 说明 | 头文件 |
|------|------|--------|
| `vector` | 动态数组，支持随机访问，尾部插入删除高效 | `<vector>` |
| `deque` | 双端队列，头尾插入删除都高效 | `<deque>` |
| `list` | 双向链表，任意位置插入删除高效，不支持随机访问 | `<list>` |
| `forward_list` | C++11 引入，单向链表 | `<forward_list>` |
| `array` | C++11 引入，定长数组，C 风格数组的简单包装 | `<array>` |

**vector 示例**

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

**list 示例**

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

### 2.2 关联容器

按键组织元素，通常基于红黑树，自动按键排序，查找快。

| 容器 | 说明 | 头文件 |
|------|------|--------|
| `set` | 键的集合，不允许重复，自动排序 | `<set>` |
| `multiset` | 允许重复键的集合 | `<set>` |
| `map` | 键值对，键唯一 | `<map>` |
| `multimap` | 键值对，键可重复 | `<map>` |

**set 示例**

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

**map 示例**

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

### 2.3 无序容器

C++11 引入，基于哈希表，元素无序，只关注元素是否存在或键值对应。

| 容器 | 说明 | 头文件 |
|------|------|--------|
| `unordered_set` | 无序集合，不允许重复 | `<unordered_set>` |
| `unordered_multiset` | 无序集合，允许重复 | `<unordered_set>` |
| `unordered_map` | 无序映射，键唯一 | `<unordered_map>` |
| `unordered_multimap` | 无序映射，键可重复 | `<unordered_map>` |

**unordered_map 示例**

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

### 2.4 容器适配器

适配器不是容器，而是包装已有容器、改变其接口。

| 适配器 | 说明 | 默认底层容器 | 头文件 |
|--------|------|-------------|--------|
| `stack` | 后进先出（LIFO） | `deque` | `<stack>` |
| `queue` | 先进先出（FIFO） | `deque` | `<queue>` |
| `priority_queue` | 优先级队列 | `vector` | `<queue>` |

**stack 示例**

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

### 2.5 容器共有函数

所有容器都提供 `begin()` / `cbegin()`、`end()` / `cend()`、`size()`、`empty()`、`clear()`、`swap()`。

## 3. 迭代器

迭代器提供访问容器元素的统一方式，可视为指向元素的指针，遍历时不必关心容器内部实现。

### 3.1 迭代器分类

五类迭代器，层次包含：随机访问 > 双向 > 正向 > 输入/输出。

| 类别 | 说明 | 支持的操作 | 典型容器 |
|------|------|-----------|---------|
| **输入迭代器** | 只读，单向，只能遍历一次 | `++`, `*`（读） | `istream_iterator` |
| **输出迭代器** | 只写，单向，只能遍历一次 | `++`, `*`（写） | `ostream_iterator` |
| **正向迭代器** | 可读写，单向，可多次遍历 | `++`, `*`（读写） | `forward_list` |
| **双向迭代器** | 可读写，双向移动 | `++`, `--`, `*` | `list`, `set`, `map` |
| **随机访问迭代器** | 最强大，支持随机访问 | `++`, `--`, `+`, `-`, `[]`, `<`, `>` | `vector`, `array`, `deque` |

### 3.2 迭代器基本操作

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

### 3.3 迭代器有效性

- 迭代器必须合法有效。
- 不能解引用尾后迭代器（`end()`）。
- 区间必须合法：指向同一容器，从第一个迭代器出发能够到达第二个迭代器。

## 4. 算法

算法是操作容器元素的函数模板，通过迭代器访问元素，与容器解耦；主要分布在 `<algorithm>` 和 `<numeric>`。

### 4.1 常用算法分类

**非变序**（不修改元素）：`find` / `find_if`、`count` / `count_if`、`for_each`、`adjacent_find`（查找相邻重复元素）。

**变序**：`copy` / `copy_if`、`replace` / `replace_if`、`remove` / `remove_if`、`transform`、`reverse`。

**排序与比较**：`sort`（O(n log n)）、`stable_sort`、`partial_sort`、`nth_element`（第 n 个元素落到最终有序位置）、`merge`（合并）、`max` / `min`。

**数值**（`<numeric>`）：`accumulate`（累加）、`inner_product`（内积）、`partial_sum`（部分和）、`adjacent_difference`（相邻差）。

### 4.2 算法使用示例

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

### 4.3 C++98 以来的算法演进

C++98 批准约 70 个算法，C++11 新增约 20 个：`all_of`、`any_of`、`none_of`、`copy_if`、`copy_n`、`find_if_not`、`iota`、`is_sorted`、`is_sorted_until`、`shuffle`、`move`、`move_backward` 等。

## 5. 函数对象

函数对象行为像函数，实现上是重载了 `operator()` 的类或类模板。

### 5.1 基本用法

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

### 5.2 内置函数对象

`<functional>` 提供的函数对象均写作 `std::xxx<T>`：

**算术**：`plus`、`minus`、`multiplies`、`divides`、`modulus`、`negate`。

**比较**：`equal_to`、`not_equal_to`、`greater`、`less`。

**逻辑**：`logical_and`、`logical_or`、`logical_not`。

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

## 6. 分配器

分配器负责内存的配置与释放，容器默认使用 `std::allocator`。

### 6.1 自定义分配器

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

### 6.2 分配器使用场景

内存池、共享内存、内存跟踪与调试、特定硬件的内存分配。

## 7. 适配器

适配器转换容器、迭代器或函数对象的接口。容器适配器见 2.4，本节讲迭代器与函数对象适配器。

### 7.1 迭代器适配器

| 适配器 | 说明 | 头文件 |
|--------|------|--------|
| `reverse_iterator` | 逆向迭代器 | `<iterator>` |
| `back_insert_iterator` | 尾部插入迭代器 | `<iterator>` |
| `front_insert_iterator` | 头部插入迭代器 | `<iterator>` |
| `insert_iterator` | 任意位置插入迭代器 | `<iterator>` |
| `istream_iterator` | 输入流迭代器 | `<iterator>` |
| `ostream_iterator` | 输出流迭代器 | `<iterator>` |

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

### 7.2 函数对象适配器（C++11 前）

| 适配器 | 说明 |
|--------|------|
| `bind1st` | 绑定第一个参数 |
| `bind2nd` | 绑定第二个参数 |
| `not1` | 一元取反 |
| `not2` | 二元取反 |
| `mem_fun` | 成员函数适配器 |
| `mem_fun_ref` | 成员函数引用适配器 |

C++11 之后用 `std::bind` 和 Lambda 表达式替代这些适配器。

## 8. 深入理解迭代器

### 8.1 迭代器失效

不同操作的失效规则：

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

### 8.2 迭代器标签

迭代器标签区分迭代器类别，用于算法优化：

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

### 8.3 自定义迭代器

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

## 9. 算法进阶

### 9.1 算法复杂度

| 算法 | 时间复杂度 | 说明 |
|------|-----------|------|
| `sort` | O(n log n) | 内省排序 |
| `stable_sort` | O(n log n) | 稳定排序 |
| `partial_sort` | O(n log k) | 部分排序 |
| `nth_element` | O(n) | 第 n 个元素排序 |
| `find` | O(n) | 线性查找 |
| `binary_search` | O(log n) | 二分查找 |
| `lower_bound` / `upper_bound` | O(log n) | 边界查找 |

### 9.2 算法与 Lambda

Lambda 是配合算法的主要方式：

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

### 9.3 并行算法（C++17）

C++17 通过执行策略（Execution Policy）并行执行算法：

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

执行策略：`seq` 顺序执行、`par` 并行执行、`par_unseq` 并行且向量化执行，均位于 `std::execution`。

## 10. C++17 / C++20 新特性

### 10.1 范围库（C++20）

范围库扩展迭代器与泛型算法，使两者可以组合使用。

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

### 10.2 受约束算法（C++20）

`std::ranges` 提供大多数算法的受约束版本：

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

### 10.3 多态分配器（C++17）

C++17 引入 `std::pmr` 命名空间，提供多态分配器：

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

## 11. 性能优化与最佳实践

### 11.1 容器选择

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

### 11.2 性能优化技巧

**1. 预留空间**

```cpp
std::vector<int> vec;
vec.reserve(1000);  // 预分配空间，避免多次 reallocation
```

**2. 用 emplace 代替 push**

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

- 小数据量用线性查找，大数据量考虑二分查找。
- 已排序数据用 `lower_bound` / `upper_bound`。
- 只需第 n 个元素时用 `nth_element`，不必完整排序。

### 11.3 常见陷阱

1. 迭代器失效：修改容器后，原有迭代器可能失效。
2. 解引用尾后迭代器：`end()` 不指向有效元素。
3. 空容器操作：对空容器调用 `front()` / `back()` 是未定义行为。
4. 比较器不满足严格弱序：自定义比较器必须满足严格弱序。
5. 遍历中修改容器：可能导致迭代器失效或未定义行为。

## 附录：常用头文件速查

- **容器**：`<vector>`、`<deque>`、`<list>`、`<forward_list>`、`<array>`、`<set>`、`<map>`、`<unordered_set>`、`<unordered_map>`
- **容器适配器**：`<stack>`、`<queue>`
- **算法**：`<algorithm>`、`<numeric>`
- **迭代器与函数对象**：`<iterator>`、`<functional>`
- **内存**：`<memory>`
- **范围与执行**：`<ranges>`、`<execution>`
