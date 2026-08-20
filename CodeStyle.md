# 五语言统一代码风格规范

> **原则**：统一优先，差异最小。仅保留语言生态强制差异。  
> **适用版本**：C++17 / Java 8 / Python 3.8 / Rust 1.60 / C11（及更高）。

---

## 一、全局统一规则（除 Java 外全部遵守）

| 规则 | 设定 | 示例 |
|------|------|------|
| 缩进 | 4 空格，禁用 Tab | — |
| 行宽 | ≤100 字符 | — |
| 类型（类/结构/枚举） | `PascalCase` | `SinglyList`, `Color` |
| 常量（含枚举常量） | `UPPER_SNAKE_CASE` | `MAX_SIZE`, `COLOR_RED` |
| 函数/方法 | `snake_case`（Java 例外） | `add_node_head` |
| 变量 | `snake_case`（Java 例外） | `current_node` |
| 大括号 | K&R（`{` 同行） | `if (ok) {` |
| 字符串 | 双引号 | `"hello"` |
| 导入顺序 | 标准库 → 第三方 → 本地 | — |
| 运算符 | 两侧各 1 空格 | `a + b` |
| 逗号 | 后 1 空格 | `a, b` |
| 空行 | 函数/方法间 1 空行 | — |
| 换行符 | LF（`\n`），文件末尾保留空行 | — |

> Python 无大括号（缩进定块）。

---

## 二、语言差异表

> 函数/方法、变量：Java 为 `camelCase`，其余 `snake_case`（见全局规则）。

| 项目 | C++ | Java | Python | Rust | C |
|------|-----|------|--------|------|-----|
| 私有成员 | 尾下划线 `data_` | 前缀 `m` → `mData` | 前导下划线 `_data` | 无（`pub` 控制） | 无（无封装） |
| 枚举常量 | `UPPER_SNAKE` | `UPPER_SNAKE` | — | 枚举变体 `PascalCase` | `UPPER_SNAKE` |
| 命名空间/包/模块 | 命名空间 `snake_case` | 包名全小写 | 包目录 `snake_case` | 模块 `snake_case` | 无（头文件划分） |
| 注释风格 | `//` + Doxygen | `//` + Javadoc | `#` + docstring | `//` + `///` | `//` 或 `/* */` |
| 扩展名 | `.hpp` / `.cpp` | `.java` | `.py` | `.rs` | `.h` / `.c` |

---

## 三、各语言补充规则

### C++

- **宏**：`SCREAMING_SNAKE`（如 `MY_VERSION`）。
- **模板参数**：`PascalCase`（简短单字母可例外，如 `T`）。
- **指针/引用**：`Type* p`, `Type& ref`（靠类型）。
- **访问修饰符**：`public:` / `private:` 不缩进，成员缩进 4 空格。
- **头文件**：`#ifndef`。

---

### Java

- **测试方法**：允许下划线（如 `testAddNode_head`），但团队内应统一。
- **类名与文件名**：必须一致，一个文件一个公开类。
- **导入**：字母序，无通配符（`*`）。
- **泛型/数组**：`List<Node>`，`int[] arr`。

---

### Python

- **类型注解**：必须，如 `def add(data: int) -> None:`。
- **入口**：`if __name__ == "__main__":`。
- **文档字符串**：Google 风格，`Args:` / `Returns:` 分节。

---

### Rust

- **注释**：文档注释 `///`（公开 API 必写），内部注释 `//!`。
- **借用**：`&T`, `&mut T`。

---

### C

- **结构体**：`PascalCase` + `typedef`，如 `typedef struct { ... } List;`。
- **宏**：`SCREAMING_SNAKE`。

---

## 四、文件命名规则

| 语言 | 类型 | 规则 | 示例 |
|------|------|------|------|
| C++ | 头文件 | `snake_case.hpp` | `singly_link_list.hpp` |
| C++ | 源文件 | `snake_case.cpp` | `main.cpp` |
| C++ | 测试 | `*_test.cpp` | `list_test.cpp` |
| C | 头文件 | `snake_case.h` | `list.h` |
| C | 源文件 | `snake_case.c` | `main.c` |
| C | 测试 | `*_test.c` | `list_test.c` |
| Java | 源文件 | 公开类名 + `.java` | `SinglyList.java` |
| Java | 测试 | `*Test.java` | `SinglyListTest.java` |
| Python | 模块 | `snake_case.py` | `linked_list.py` |
| Python | 包目录 | `snake_case/__init__.py` | `data_structures/` |
| Python | 测试 | `test_*.py` | `test_linked_list.py` |
| Rust | 模块 | `snake_case.rs` | `linked_list.rs` |
| Rust | 集成测试 | `tests/*_test.rs` | `tests/list_test.rs` |

> Java 文件名与公开类名一致为语法强制；其余语言无此要求。  
> Rust 单元测试写在模块内（`#[cfg(test)]`），集成测试放 `tests/`。

---

## 五、项目目录结构

### C++ / C（CMake）

```
project/
├── CMakeLists.txt
├── include/          # .hpp / .h
├── src/              # .cpp / .c
├── tests/            # *_test.cpp / *_test.c
├── .gitignore        # 忽略 build/
└── README.md
```

### Java（Gradle 标准）

```
project/
├── settings.gradle
├── build.gradle
├── .gitignore        # 忽略 build/
└── src/
    ├── main/java/com/example/list/SinglyList.java
    └── test/java/com/example/list/SinglyListTest.java
```

### Java（简单课程布局）

```
project/
├── Main.java
├── src/SinglyList.java
├── .gitignore        # 忽略 *.class, out/
└── README.md
```

### Python

```
project/
├── pyproject.toml    # 或 requirements.txt
├── .gitignore        # 忽略 venv/, __pycache__/
├── main.py
├── src/__init__.py
├── tests/test_*.py
└── README.md
```

### Rust（Cargo）

```
project/
├── Cargo.toml
├── .gitignore        # 忽略 target/
├── src/
│   ├── main.rs
│   ├── lib.rs
│   └── list.rs
├── tests/list_test.rs
└── README.md
```

---

## 六、格式化工具推荐

| 语言 | 工具 | 配置说明 |
|------|------|----------|
| C++ / C | `clang-format` | 基于本规范生成 `.clang-format` |
| Java | `google-java-format` | 直接使用，无需额外配置 |
| Python | `black` + `isort` | `black` 行宽设为 100，`isort` 配合 `black` |
| Rust | `rustfmt` | 默认配置即符合本规范 |

> 建议在 CI 中检查格式，或使用 pre-commit hook 自动格式化。
