# Python 编码规范

> 适用于 Python 3.8 及以上。**原则**：统一优先，差异最小。

## 1. 通用规则

- **缩进**：4 空格，禁用 Tab。
- **行宽**：≤ 100 字符。
- **类型**（类 / 结构 / 枚举）：`PascalCase`，如 `SinglyList`、`Color`。
- **常量**：`UPPER_SNAKE_CASE`，如 `MAX_SIZE`、`COLOR_RED`。
- **函数 / 方法**：`snake_case`，如 `add_node_head`。
- **变量**：`snake_case`，如 `current_node`。
- **代码块**：无大括号，由缩进确定层级。
- **字符串**：双引号，如 `"hello"`。
- **导入顺序**：标准库 → 第三方 → 本地。
- **运算符**：两侧各 1 空格，如 `a + b`。
- **逗号**：后 1 空格，如 `a, b`。
- **空行**：函数之间 1 空行。
- **换行符**：LF，文件末尾保留空行。

## 2. Python 特有规则

- **私有成员**：前导下划线，如 `_data`。
- **包目录**：`snake_case`，如 `data_structures/`。
- **注释**：`#`；对外接口用 docstring，如 `"""添加节点。"""`。
- **源文件扩展名**：`.py`。
- **类型注解**：必须标注，如 `def add(data: int) -> None:`。
- **程序入口**：`if __name__ == "__main__":`。
- **文档字符串**：Google 风格，`Args:` / `Returns:` 分节。

## 3. 文件命名

- **模块**：`snake_case.py`，如 `linked_list.py`。
- **包目录**：`snake_case/__init__.py`，如 `data_structures/`。
- **测试**：`test_*.py`，如 `test_linked_list.py`。

## 4. 项目目录结构

```text
project/
├── pyproject.toml    # 或 requirements.txt
├── .gitignore        # 忽略 venv/, __pycache__/
├── main.py
├── src/__init__.py
├── tests/test_*.py
└── README.md
```

## 5. 格式化工具

- `black` + `isort`：`black` 行宽设为 100，`isort` 配合 `black`。

建议在 CI 中检查格式，或使用 pre-commit hook 自动格式化。
