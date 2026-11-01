# C++ 编码规范

> 适用于 C/C++ 项目。**原则**：统一优先，差异最小。

## 1. 通用规则

- **缩进**：4 空格，禁用 Tab。
- **行宽**：≤ 100 字符。
- **类型**（类 / 结构 / 枚举）：`PascalCase`，如 `SinglyList`、`Color`。
- **常量**（含枚举常量）：`UPPER_SNAKE_CASE`，如 `MAX_SIZE`、`COLOR_RED`。
- **函数 / 方法**：`snake_case`，如 `add_node_head`。
- **变量**：`snake_case`，如 `current_node`。
- **大括号**：K&R 风格，`{` 与语句同行，如 `if (ok) {`。
- **字符串**：双引号，如 `"hello"`。
- **包含顺序**：标准库 → 第三方 → 本地。
- **运算符**：两侧各 1 空格，如 `a + b`。
- **逗号**：后 1 空格，如 `a, b`。
- **空行**：函数之间 1 空行。
- **换行符**：LF，文件末尾保留空行。

## 2. C++ 特有规则

- **私有成员**：尾下划线，如 `data_`。
- **命名空间**：`snake_case`，如 `my_project`。
- **注释**：`//`；对外接口用 Doxygen，如 `/// @brief 计算长度`。
- **源文件扩展名**：`.hpp` / `.cpp`。
- **宏**：`SCREAMING_SNAKE`，如 `MY_VERSION`。
- **模板参数**：`PascalCase`，单字母可例外，如 `T`。
- **指针 / 引用**：`*`、`&` 靠近标识符，如 `Type *p`、`Type &ref`。
- **访问修饰符**：`public:` / `private:` 不缩进，成员缩进 4 空格。
- **头文件保护**：`#ifndef`，如 `#ifndef LIST_HPP`。

## 3. 文件命名

- **头文件**：`snake_case.hpp`，如 `singly_link_list.hpp`。
- **源文件**：`snake_case.cpp`，如 `main.cpp`。
- **测试**：`*_test.cpp`，如 `list_test.cpp`。

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

## 5. 格式化工具

- `clang-format`：使用仓库内 `.clang-format`，行宽设为 100。

建议在 CI 中检查格式，或使用 pre-commit hook 自动格式化。
