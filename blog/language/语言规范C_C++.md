# C++ 编码规范
## 1. 通用规则

| 规则 | 设定 |
|:------:|:------:|
| 缩进 | 4 空格，禁用 Tab |
| 行宽 | ≤100 字符 |
| 类型（类/结构/枚举） | `PascalCase`，如 `SinglyList`, `Color` |
| 常量（含枚举常量） | `UPPER_SNAKE_CASE`，如 `MAX_SIZE`, `COLOR_RED` |
| 函数/方法 | `snake_case`，如 `add_node_head` |
| 变量 | `snake_case`，如 `current_node` |
| 大括号 | K&R，`{` 同行，如 `if (ok) {` |
| 字符串 | 双引号，如 `"hello"` |
| 导入/包含顺序 | 标准库 → 第三方 → 本地 |
| 运算符 | 两侧各 1 空格，如 `a + b` |
| 逗号 | 后 1 空格，如 `a, b` |
| 空行 | 函数/方法间 1 空行 |
| 换行符 | LF（`\n`），文件末尾保留空行 |

## 2. C++ 特有规则

- **私有成员**：尾下划线，如 `data_`。
- **枚举常量**：`UPPER_SNAKE`。
- **命名空间**：`snake_case`。
- **注释风格**：`//` + Doxygen。
- **扩展名**：`.hpp` / `.cpp`。
- **宏**：`SCREAMING_SNAKE`，如 `MY_VERSION`。
- **模板参数**：`PascalCase`；简短单字母可例外，如 `T`。
- **指针/引用**：更靠近标识符，如 `Type *p`, `Type &ref`。
- **访问修饰符**：`public:` / `private:` 不缩进，成员缩进 4 空格。
- **头文件**：使用 `#ifndef`。

## 3. 文件命名

| 类型 | 规则 | 示例 |
|------|------|------|
| 头文件 | `snake_case.hpp` | `singly_link_list.hpp` |
| 源文件 | `snake_case.cpp` | `main.cpp` |
| 测试 | `*_test.cpp` | `list_test.cpp` |

## 4. 项目目录结构（CMake）

```text
project/
├── CMakeLists.txt
├── include/          # .hpp / .h
├── src/              # .cpp / .c
├── tests/            # *_test.cpp / *_test.c
├── .gitignore        # 忽略 build/
└── README.md
```

---
