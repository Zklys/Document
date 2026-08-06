# 五语言统一代码风格规范

> 统一优先，差异最小；只保留语言自身无法避免的差异。
> 覆盖：**C++ · Java · Python · Rust · C**

---

## 一、统一规则

| 规则 | 统一设定 | 示例 |
|------|---------|------|
| 缩进 | 4 空格，禁用 Tab | — |
| 行宽 | 最多 100 字符 | — |
| 类型名 | `PascalCase` | `SinglyList` |
| 常量 | `UPPER_SNAKE_CASE` | `MAX_SIZE` |
| 函数 / 方法 | `snake_case`（Java 例外） | `add_node_head` |
| 变量 | `snake_case`（Java 例外） | `current_node` |
| 大括号 | K&R（`{` 同行） | `if (ok) {` |
| 字符串 | 双引号 | `"hello"` |
| 导入 | 标准库 → 第三方 → 本地 | — |
| 运算符 | 两侧各 1 空格 | `a + b` |
| 逗号 | 后 1 空格 | `a, b` |
| 空行 | 函数 / 方法间 1 空行 | — |

> 例外：Python 无大括号（缩进定块）；Java 函数 / 变量为 `camelCase`（官方规范）。

## 二、语言差异表

| 项目 | C++ | Java | Python | Rust | C |
|------|-----|------|--------|------|-----|
| 函数 / 方法 | `snake_case` | `camelCase` | `snake_case` | `snake_case` | `snake_case` |
| 变量 | `snake_case` | `camelCase` | `snake_case` | `snake_case` | `snake_case` |
| 私有成员 | 尾下划线 `data_` | `m` 前缀 `mData` | 前导下划线 `_data` | 无（`pub` 控制） | 无（无封装） |
| 命名空间 / 包 / 模块 | 命名空间 `snake_case` | 包名全小写 | 包目录 `snake_case` | 模块 `snake_case` | 无（头文件划分） |
| 注释 | `//` + Doxygen | `//` + Javadoc | `#` + docstring | `//` + `///` | `//` / `/* */` |
| 扩展名 | `.hpp` / `.cpp` | `.java` | `.py` | `.rs` | `.h` / `.c` |

## 三、各语言补充规则

### C++

**命名**
- 私有成员：尾下划线，如 `head_`、`size_`
- 宏：`SCREAMING_SNAKE`，如 `MY_PROJECT_VERSION`
- 模板参数：`PascalCase`，如 `template <typename ValueType>`
- 命名空间：`snake_case`，如 `namespace linked_list`

**格式**
- 指针 / 引用：`Type* p`、`Type& ref`（靠类型）
- 访问修饰符 `public:` / `private:` 不缩进，成员缩进 4 空格
- 头文件用 `#pragma once`

**注释**
- 普通注释用 `//`；头文件公开接口用 Doxygen

### Java

**命名**
- 方法：`camelCase`，如 `addNodeHead`
- 私有成员：`m` 前缀，如 `mHead`、`mSize`
- 包名：全小写，如 `com.example.list`
- 测试方法可含下划线：`testAddNode_head`（可选）

**格式**
- 类名与文件名一致，一个文件一个公开类
- `package` 在最前，`import` 字母序、无通配符
- 泛型 `List<Node>`；数组 `int[] arr`

**注释**
- 公开类 / 方法用 Javadoc，含 `@param`、`@return`

### Python

**命名**
- 私有成员：前导下划线，如 `_head`、`_size`
- 模块名：`snake_case`，如 `linked_list.py`

**格式**
- 缩进 4 空格（语法强制）
- 类型注解：`def add_node(data: int) -> None:`
- 入口惯用 `if __name__ == "__main__":`

**注释**
- docstring（`"""`）Google 风格，`Args:` / `Returns:` 分节

### Rust

**命名**
- 类型 / trait / 枚举变体：`PascalCase`
- 私有成员：无标记（`pub` 控制）
- 模块 / crate：`snake_case`
- 常量：`UPPER_SNAKE_CASE`

**格式**
- rustfmt 默认 4 空格 / 100 行宽 / K&R，天然一致
- 泛型 `Vec<T>`；借用 `&T`、`&mut T`

**注释**
- `//`；文档注释 `///`、`//!`；公开 API 必写文档注释

### C

**命名**
- 结构体：`PascalCase` + `typedef`
- 成员：`snake_case`
- 宏：`SCREAMING_SNAKE`
- 无命名空间 / 封装，头文件划分模块

**格式**
- `.h` / `.c`；`#pragma once`
- `//`（C99+），兼容 C89 用 `/* */`

**注释**
- 公开接口用 `//` 或 `/* */`；可用 Doxygen（可选）

## 四、文件命名规则

| 语言 | 文件类型 | 规则 | 示例 |
|------|---------|------|------|
| C++ | 头文件 | `snake_case` + `.hpp` | `singly_link_list.hpp` |
| C++ | 源文件 | `snake_case` + `.cpp` | `main.cpp` |
| C++ | 测试文件 | `*_test.cpp` | `list_test.cpp` |
| C | 头文件 | `snake_case` + `.h` | `singly_link_list.h` |
| C | 源文件 | `snake_case` + `.c` | `main.c` |
| C | 测试文件 | `*_test.c` | `list_test.c` |
| Java | 源文件 | 必须与公开类名一致 | `SinglyList.java` |
| Java | 测试文件 | `*Test.java` | `SinglyListTest.java` |
| Python | 模块文件 | `snake_case` + `.py` | `linked_list.py` |
| Python | 包目录 | `snake_case` + `__init__.py` | `data_structures/` |
| Python | 测试文件 | `test_*.py` | `test_linked_list.py` |
| Rust | 模块文件 | `snake_case` + `.rs` | `linked_list.rs` |
| Rust | 集成测试 | `tests/` 内 `*_test.rs` | `tests/list_test.rs` |

**要点**
- 除 Java 外，文件名不必与类名一致
- Java 文件名与公开类名一致是语法强制
- Java 一文件一公开类；Python 一模块可多类
- Rust 单测在模块内（`#[cfg(test)]`），集成测试在 `tests/`

## 五、项目文件结构

### C++ / C（CMake 布局）

```
project/
├── CMakeLists.txt        # 构建配置
├── include/              # 头文件（.hpp / .h）
├── src/                  # 源文件（.cpp / .c）
├── tests/                # 单元测试（*_test.cpp / *_test.c）
├── .gitignore            # 忽略 build/
├── build/                # 构建产物（不提交）
└── README.md
```

### Java — 标准 Gradle 布局

```
project/
├── settings.gradle       # Gradle 设置
├── build.gradle          # 构建与依赖配置
├── .gitignore            # 忽略 build/
└── src/
    ├── main/
    │   ├── java/com/example/list/SinglyList.java
    │   └── resources/              # 资源（可选）
    └── test/java/com/example/list/SinglyListTest.java
```

### Java — 简单课程布局

```
project/
├── Main.java             # 程序入口
├── src/SinglyList.java   # 其余源文件（可按包建子目录）
├── .gitignore            # 忽略 *.class、out/
└── README.md
```

### Python

```
project/
├── pyproject.toml        # 项目配置（或用 requirements.txt）
├── .gitignore            # 忽略 venv/、__pycache__/
├── main.py               # 程序入口
├── src/__init__.py       # 业务代码包
├── tests/                # 测试（test_*.py）
└── README.md
```

### Rust（Cargo 布局）

```
project/
├── Cargo.toml            # 项目配置与依赖
├── .gitignore            # 忽略 target/
├── src/
│   ├── main.rs           # 程序入口
│   ├── lib.rs            # 库入口（可选）
│   └── list.rs           # 模块
├── tests/list_test.rs    # 集成测试
└── README.md
```
