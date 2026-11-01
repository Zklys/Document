# 一个普通的文档项目

个人技术笔记与资料整理。按主题分目录，一篇文档只讲一个主题。

## 目录

### 编程语言 · `blog/language/`

- [Java](blog/language/Java.md) — 18 部分：基础语法、数组与方法、OOP、核心 API、集合、异常、I/O、多线程、泛型、Lambda、Stream、JUC、反射、注解、NIO、JVM、设计模式、版本新特性
- [Python](blog/language/Python.md) — 8 部分：基础语法、OOP、进阶特性、并发与异步、测试与调试、性能优化、生态、最佳实践
- [C++ STL](blog/language/LearnSTL.md) — 11 章：六大组件、容器、迭代器、算法、函数对象、分配器、适配器、进阶用法与 C++17/20 新特性

### 编码规范

三份规范共用同一套命名与缩进约定，同一项目内可混用而风格一致。

- [C/C++ 编码规范](blog/language/语言规范C_C++.md) — 通用规则、C++ 特有规则、文件命名、CMake 目录结构
- [Java 编码规范](blog/language/语言规范Java.md) — 通用规则、Java 特有规则、文件命名、目录结构、格式化工具
- [Python 编码规范](blog/language/语言规范Python.md) — 通用规则、Python 特有规则、文件命名、目录结构、格式化工具

### 工具链 · `blog/tool/`

- [CMake](blog/tool/CMake.md) — 12 部分：核心概念、基础语法、现代 CMake、函数与宏、依赖管理、CTest、安装导出、高级特性、最佳实践、排错
- [CMake 实践](blog/tool/CMake实践.md) — 3 章：简介、基础项目、跟做官方教程
- [Gradle](blog/tool/Gradle.md) — 12 章：简介、核心概念、构建脚本、依赖管理、任务、插件、多项目、性能与排错

## 仓库约定

- **编码**：UTF-8，无 BOM。
- **换行**：仓库内以 LF 存储，Windows 检出为 CRLF，规则见 [.gitattributes](.gitattributes)。
- **命名**：目录用小写英文，文档名用主题名（如 `CMake.md`、`语言规范Java.md`）。
- **结构**：一个目录对应一个主题；新增文档放进对应主题目录，并在上面的目录中登记。
- **代码块**：围栏后标注语言（`cmake`、`cpp`、`python`、`kotlin`、`properties` 等，纯文本与目录树用 `text`），便于高亮与校对。
