## 函数
Makefile 提供的内置函数：
### 1. `$(subst from,to,text)`  字符串替换
```sh
STR = hello world
NEW_STR = $(subst hello,goodbye,$(STR))
# NEW_STR = goodbye world
```
- 将`STR`中的`hello`替换为`goodbye`并存入`NEW_STR`中。
### 2. `$(patsubst pattern,replacement,text)` 模式替换
```sh
FILES = foo.c bar.c baz.s
OBJS = $(patsubst %.c,%.o,$(FILES))
# OBJS = foo.o bar.o baz.s
```
- 将`FILES`文件中的`.c`文件替换为了`.o`文件并存入`OBJS`中。
### 3. $(strip string)  去除前后空格
```sh
STR =   hello   world   
CLEAN = $(strip $(STR))
# CLEAN = hello world
```
- 这没啥说的
### 4. $(findstring find,in)  查找子串
```sh
STR = hello world
FOUND = $(findstring hello,$(STR))
# FOUND = hello
```
- 这也没啥可说的
### 5. $(filter pattern...,text)  过滤匹配模式
```sh
FILES = foo.c bar.h baz.o qux.c
C_FILES = $(filter %.c,$(FILES))
# C_FILES = foo.c qux.c
```
- 去除了所有非`.c`结尾的文件。
 ### 6. $(filter-out pattern...,text)  过滤不匹配模式
```sh
FILES = foo.c bar.h baz.o qux.c
NON_C_FILES = $(filter-out %.c,$(FILES))
# NON_C_FILES = bar.h baz.o
```
- 去除了所有`.c`结尾的文件。
 ### 7. $(sort list)  排序并去重
```sh
WORDS = foo bar baz foo
SORTED = $(sort $(WORDS))
# SORTED = bar baz foo
```
- 按26个字母的顺序排序并去重。
 ### 8. $(word n,text)  获取第n个单词
```sh
WORDS = apple banana cherry
SECOND = $(word 2,$(WORDS))
# SECOND = banana
```
- 字面意思
### 9. $(wordlist s,e,text)  获取单词子列表
```sh
WORDS = one two three four five
SUBLIST = $(wordlist 2,4,$(WORDS))
# SUBLIST = two three four
```
- 第二个至第四个单词(包括2、4)的所有单词。
### 10.  $(words text)  统计单词数
```sh
WORDS = apple banana cherry
COUNT = $(words $(WORDS))
# COUNT = 3
```
- 字面意思x2
### 11. $(firstword text)  获取第一个单词
```sh
WORDS = apple banana cherry
FIRST = $(firstword $(WORDS))
# FIRST = apple
```
- 字面意思x3
### 12. $(lastword text)  获取最后一个单词
```sh
WORDS = apple banana cherry
LAST = $(lastword $(WORDS))
# LAST = cherry
```
- 字面意思x4
### 13. $(wildcard pattern)  通配符扩展
```sh
SOURCES = $(wildcard *.c)
HEADERS = $(wildcard include/*.h)
```
- 字面意思x5
### 15. $(dir names...)  提取目录部分
```sh
DIRS = $(dir src/main.c inc/header.h)
# DIRS = src/ inc/
```
- 字面意思x6
### 16. $(notdir names...)  提取文件名部分
```sh
FILES = $(notdir src/main.c inc/header.h)
# FILES = main.c header.h
```
- 字面意思x7
### 17. $(suffix names...)  提取扩展名
```sh
EXTS = $(suffix main.c header.h utils.o)
# EXTS = .c .h .o
```
- 字面意思x8
### 18. $(basename names...)  去掉扩展名
```sh
BASES = $(basename src/main.c header.h)
# BASES = src/main header
```
- 字面意思x9
### 19. $(addsuffix suffix,names...)  添加后缀
```sh
OBJECTS = $(addsuffix .o,main utils)
# OBJECTS = main.o utils.o
```
- 字面意思x10
### 20. $(addprefix prefix,names...)  添加前缀
```sh
SOURCES = $(addprefix src/,main.c utils.c)
# SOURCES = src/main.c src/utils.c
```
- 字面意思x11
### 21. $(join list1,list2)  连接两个列表
```sh
DIRS = src/ inc/
FILES = main.c header.h
JOINED = $(join $(DIRS),$(FILES))
# JOINED = src/main.c inc/header.h
```
- 字面意思x12
### 22. $(realpath names...)  获取绝对路径
```sh
ABS_PATH = $(realpath somefile.c)
```
- 字面意思x13
### 23. $(abspath names...)  获取绝对路径
```sh
ABS_PATH = $(abspath somefile.c)
```
- 字面意思x14
### 24. $(if condition,then-part[,else-part])  条件判断
```sh
DEBUG = 1
CFLAGS = $(if $(DEBUG),-g,-O2)
# CFLAGS = -g
```
- 判断`DEBUG`的bool值，来确定`CFLAGS`的选择值。
### 25. 如下
```sh
$(or condition1,condition2,...)  逻辑或
# 如果 VAR1 或 VAR2 非空，则条件成立
$(and condition1,condition2,...)  逻辑与
# 所有条件都非空时返回最后一个非空值
```
- 这两个 字面意思x15
### 26. $(foreach var,list,text)  循环处理
```sh
DIRS = src lib test
CLEAN_DIRS = $(foreach dir,$(DIRS),$(dir)/*.o)
# CLEAN_DIRS = src/*.o lib/*.o test/*.o
```
- 字面意思x16
### 27. $(call variable,param,...)  调用用户定义函数
```sh
reverse = $(2) $(1)
FOO = $(call reverse,hello,world)
# FOO = world hello
```
- 建议和第28一起阅读。
### 28. 用户自定义函数
```sh
# 定义函数
define compile_template
$(1): $(2)
	$$(CC) $$(CFLAGS) -c $$< -o $$@
endef

# 使用函数
$(eval $(call compile_template,main.o,main.c))
$(eval $(call compile_template,utils.o,utils.c))
```
- `$(1)`  第一个参数（目标文件）
- `$(2)`  第二个参数（源文件）
- `$$` 用于转义，防止立即展开
- `call` 调用函数并传递参数
- `eval` 将生成的文本作为 Makefile 代码执行

### 29. $(value variable)  获取未展开的变量值

```sh
VAR = foo
VBC = $(VAR)
VALUE = $(value $(VBC))
# VALUE = $(VBC) (不展开)
```
- 若不添加`value`则`VALUE = foo`
### 30. $(eval text)  动态生成 Makefile 代码
```sh
define TEMPLATE
$(1): $(2)
	$$(CC) $$^ -o $$@
endef

$(eval $(call TEMPLATE,program,main.o utils.o))
```
- `$(call TEMPLATE,program,main.o utils.o)` 展开为：

```sh
program: main.o utils.o
	$(CC) $^ -o $@
```
- `$(eval ...)` 将这段文本作为 Makefile 代码解析
### 31.  $(origin variable)  获取变量来源

```sh
# 返回值：undefined, default, environment, file, command line, override, automatic
```
### 32. $(flavor variable)  获取变量类型
```sh
# 返回值：undefined, recursive, simple
```
### 33. $(shell command)  执行 shell 命令

```sh
CURRENT_DATE = $(shell date)
FILES_COUNT = $(shell ls *.c | wc -l)
```
- 习惯shell的应该一眼就看出来了吧。
- 不习惯的应该也用不上，仅做展示了