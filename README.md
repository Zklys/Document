# 一个普通的文档项目

个人技术笔记与资料整理。按主题分目录，一篇文档只讲一个主题。

## 目录

### 编程语言 · `blog/language/`

| 文档 | 内容 |
|------|------|
| [Java](blog/language/Java.md) | Java 入门（语法、OOP、集合、异常、I/O、多线程）与进阶（泛型、Lambda、Stream、JUC、反射、JVM）教学大纲 |
| [Python](blog/language/Python.md) | Python 基础语法、面向对象、并发与异步、测试调试、性能优化、常用生态 |
| [C++ STL](blog/language/LearnSTL.md) | STL 六大组件、容器、迭代器与常用算法，入门到进阶 |

### 编码规范

三份规范采用同一套命名与缩进约定，同一项目内可混用而风格一致。

| 文档 | 内容 |
|------|------|
| [C/C++ 编码规范](blog/language/语言规范C_C++.md) | 通用规则、C++ 特有规则、文件命名、CMake 目录结构 |
| [Java 编码规范](blog/language/语言规范Java.md) | 通用规则、Java 特有规则、文件命名、目录结构、格式化工具 |
| [Python 编码规范](blog/language/语言规范Python.md) | 通用规则、Python 特有规则、文件命名、目录结构、格式化工具 |

### 工具链 · `blog/tool/`

| 文档 | 内容 |
|------|------|
| [CMake](blog/tool/CMake.md) | CMake 系统性指南：核心概念、语法、现代 CMake、依赖管理、CTest、安装导出、最佳实践 |
| [CMake 实践](blog/tool/CMake实践.md) | 从零跑通一个 CMake 项目，并跟做官方教程 |
| [Gradle](blog/tool/Gradle.md) | Gradle 构建工具：核心概念、构建脚本、依赖管理、任务与插件、多项目构建 |

## 仓库约定

- **编码**：UTF-8，无 BOM。
- **换行**：仓库内以 LF 存储，Windows 检出为 CRLF，规则见 [.gitattributes](.gitattributes)。
- **命名**：目录用小写英文，文档名用主题名（如 `CMake.md`、`语言规范Java.md`）。
- **结构**：一个目录对应一个主题；新增文档放进对应主题目录，并在上面的目录表中登记。
- **代码块**：围栏后标注语言（`cmake`、`cpp`、`python`、`properties`，纯文本用 `text`），便于高亮与校对。
