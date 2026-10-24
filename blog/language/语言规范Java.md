# Java 编码规范
## 1. 通用规则

| 规则 | 设定 |
|------|------|
| 缩进 | 4 空格，禁用 Tab |
| 行宽 | ≤100 字符 |
| 类型（类/结构/枚举） | `PascalCase`，如 `SinglyList`, `Color` |
| 常量（含枚举常量） | `UPPER_SNAKE_CASE`，如 `MAX_SIZE`, `COLOR_RED` |
| 函数/方法 | `camelCase`（Java 例外） |
| 变量 | `camelCase`（Java 例外） |
| 大括号 | K&R，`{` 同行，如 `if (ok) {` |
| 字符串 | 双引号，如 `"hello"` |
| 导入顺序 | 标准库 → 第三方 → 本地；字母序，无通配符 `*` |
| 运算符 | 两侧各 1 空格，如 `a + b` |
| 逗号 | 后 1 空格，如 `a, b` |
| 空行 | 函数/方法间 1 空行 |
| 换行符 | LF（`\n`），文件末尾保留空行 |

## 2. Java 特有规则

- **私有成员**：前缀 `m`，如 `mData`。
- **枚举常量**：`UPPER_SNAKE`。
- **包名**：全小写。
- **注释风格**：`//` + Javadoc。
- **扩展名**：`.java`。
- **测试方法**：允许下划线，如 `testAddNode_head`，但团队内应统一。
- **类名与文件名**：必须一致，一个文件一个公开类。
- **泛型/数组**：`List<Node>`，`int[] arr`。

## 3. 文件命名

| 类型 | 规则 | 示例 |
|------|------|------|
| 源文件 | 公开类名 + `.java` | `SinglyList.java` |
| 测试 | `*Test.java` | `SinglyListTest.java` |

> Java 文件名与公开类名一致为语法强制。

## 4. 项目目录结构

### Gradle 标准

```
project/
├── settings.gradle
├── build.gradle
├── .gitignore        # 忽略 build/
└── src/
    ├── main/java/com/example/list/SinglyList.java
    └── test/java/com/example/list/SinglyListTest.java
```

### 简单课程布局

```
project/
├── Main.java
├── src/SinglyList.java
├── .gitignore        # 忽略 *.class, out/
└── README.md
```

## 5. 格式化工具

| 工具 | 配置说明 |
|------|----------|
| `google-java-format` | 直接使用，无需额外配置 |

建议在 CI 中检查格式，或使用 pre-commit hook 自动格式化。

---
