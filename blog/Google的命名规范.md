Google 的 C++ 和 Java 命名规范差异很大。核心区别在于：**C++ 使用 `snake_case` 和 `k` 前缀，而 Java 使用 `camelCase` 并禁止使用特殊前缀。**

下面是这两者具体规则的对比：

| 标识符类型         | C++ 命名规范                                        | Java 命名规范                                      |
| :------------ | :---------------------------------------------- | :--------------------------------------------- |
| **文件/包**      | 全小写，单词间用下划线 `_` 或连字符 `-` 分隔，如 `my_class.cc`     | 包名全小写，如 `com.example.myproject`                |
| **类/类型**      | **大驼峰 (PascalCase)**，如 `MyExcitingClass`        | **大驼峰 (UpperCamelCase)**，如 `MyClass`           |
| **函数/方法**     | **大驼峰 (PascalCase)**，如 `MyExcitingFunction()`   | **小驼峰 (lowerCamelCase)**，如 `sendMessage()`     |
| **变量(局部/全局)** | 全小写，单词间用下划线分隔 (snake_case)，如 `table_name`       | **小驼峰 (lowerCamelCase)**，如 `myVariable`        |
| **类成员变量**     | 全小写，snake_case，并以**下划线 `_` 结尾**，如 `table_name_` | **小驼峰 (lowerCamelCase)**，无特殊后缀                 |
| **常量**        | 以 `k` 开头，后接大驼峰，如 `kDaysInAWeek`                 | 全大写，单词间用下划线分隔 (UPPER_SNAKE_CASE)，如 `MAX_VALUE` |
| **类型参数**      | 如果含义清晰，用单个 `T`；如果需要描述，就用大驼峰                     | 强制**使用单个大写字母**。不允许使用多单词的大驼峰名称。\|               |

### ⚙️ 核心理念与通用原则

无论在哪种语言中，Google 风格指南都遵循一些共同的理念：

*   **描述性与可读性**：命名必须清晰、有描述性，避免使用含糊或只有项目内部才懂的缩写。
*   **一致性**：在项目中保持命名风格的一致性是最高原则。
*   **避免特殊前缀/后缀**：Google 风格**不鼓励**使用如 `mName`、`s_name`、`name_` 等特殊前缀或后缀来标识变量作用域或类型。C++ 中成员变量末尾的 `_` 是主要的例外。

### 💎 总结

| 语言 | 命名风格 | 核心特点 |
| :--- | :--- | :--- |
| **C++** | `snake_case` + `k` 前缀 | 风格混合，通过下划线和 `k` 前缀区分不同类型标识符 |
| **Java** | `camelCase` | 风格统一，主要依靠大小写来区分（类名为大写开头，其余为小写开头） |

如果想深入了解，可以查阅官方的完整指南：
*   **C++**：[Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)
*   **Java**：[Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)