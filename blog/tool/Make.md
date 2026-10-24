# Make
## make是什么？
- make是一个命令工具，是一个解释makefile中指令的命令工具，一般来说，大多数的IDE都有这个命令，比如：Delphi 的 make，Visual C++的nmake，Linux下GNU的make。
- make在逐渐成为编译的一种选择。
- 其余可以去往百度百科查看。
## 使用make之前请检查是否安装make
### 可使用`make --version` 检验
```sh
GNU Make 4.4.1
Built for x86_64-w64-mingw32
Copyright (C) 1988-2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```
- GNU的4.4.1版本的make的输出信息。

## 一个简单的 Makefile 文件示例:
```sh
main: main.c
	gcc main.c -o main
```
- 对应的C语言文件,一个简单的helloworld示例。
```c
#include <stdio.h>

int main(){
	printf("hello world");
	return 0;
}
```
注：
- 禁止在命令前使用空格。
- 在 makefile 严格区分制表符和空格。
- make不止可以运行编译C语言，如：java、Python、C++之类的同样可以使用make构建。
------
#### 在命令行输入make之后
![make](https://i-blog.csdnimg.cn/direct/ead6445fe9d04b3abb03615e14f230e2.png#pic_center)
  
make将makefile中的命令执行。  
此处命令为gcc的命令，如果不清楚可以参考我的[关于GCC的一些东西](GCC.md)学习一下。
- 但是在日常使用过程中我们的编译和链接的过程更推荐分开写，如下：
```sh
main: main.o
	gcc main.o -o main
main.o:main.c
	gcc -c main.c
```
#### 在命令行输入make之后会依次运行：`gcc -c main.c`  `gcc main.o -o main` 这两条命令。
如图：
![make](https://i-blog.csdnimg.cn/direct/d0973b62dbf1486faab916d9fd1e0706.png#pic_center)
运行程序main.exe不出意外会出现以下情况：  
```sh
hello world
```
- 如果是一个黑窗口一闪而过可以将C语言代码改为：
```c
#include <stdio.h>

int main(){
	printf("hello world");
	getchar();
	return 0;
}
```
- 对C语言感到疑惑的可以看[C语言](C语言.md)。
# Makefile文件语法
## 注释
- `#`后的全部为注释
```sh
# 这是注释`在这里插入代码片`
```
## 伪目标
不生成实际文件的目标，常用于执行特定操作：

```sh
.PHONY: clean
clean:
	rm -f *.o
```
- 在命令行输入`make clean`后就会执行`rm -f *.o`。
- 在windows命令行中可以使用`del`命令替代`rm`命令。

## 自定义变量
```sh
CC = gcc
CFLAGS = -Wall -O2
SOURCES = main.c utils.c
OBJECTS = $(SOURCES:.c=.o)

program: $(OBJECTS)
	$(CC) $(CFLAGS) -o program $(OBJECTS)
```
实际运行替换为：
```sh
program: main.c utils.c
	gcc -Wall -O2 -o program main.o utils.o
```
- `OBJECTS = $(SOURCES:.c=.o)`  

创建变量 `OBJECTS`,  
使用模式替换语法：`$(变量:旧后缀=新后缀)`
将 `SOURCES` 中所有 `.c` 文件替换为 `.o` 文件  
结果：`OBJECTS = main.o utils.o`  
## 模式规则
```shell
# 通用规则：将所有的 .c 文件编译为 .o 文件
%.o: %.c
	gcc -c $< -o $@
```
- `$<` 与 `$@`在接下来的自动变量中会说明。
## 自动变量
| 变量  |      含义       |         示例         |
| :-: | :-----------: | :----------------: |
| $@  |  当前规则的目标文件名   |       main.o       |
| $<  |   第一个依赖文件名    |       main.c       |
| $^  |   所有依赖文件列表    |   main.c utils.c   |
| $?  |  比目标新的依赖文件列表  | utils.c (如果只有它被修改) |
| $*  | 不包含扩展名的目标文件名  |  main (对于 main.o)  |
| $%  | 归档成员名 (用于库文件) |  lib.a(member.o)   |
- 详细说明
### 1. $@  目标文件
```shell
main.o: main.c
	gcc -c $< -o $@
# 等价于: gcc -c main.c -o main.o
```
### 2. $<  第一个依赖
```shell
%.o: %.c
	gcc -c $< -o $@
# 对于 main.o: main.c → gcc -c main.c -o main.o
```
### 3. $^  所有依赖
```shell
program: main.o utils.o
	gcc $^ -o $@
# 等价于: gcc main.o utils.o -o program
```
### 4. $?  更新的依赖
```shell
main: util1.c util2.c
	gcc -c $? -o $@
# 在任意.c文件更新时重新编译对应文件
```
### 5. $*  无扩展名目标
```sh
%.o: %.c
	gcc -c $< -o $@
	echo "Built object: $*"
# 输出: Built object: main
```
### 6. $% 这个符号在通常使用中相对较少，在本文不做相关介绍。
## 函数
- MakeFile中提供的默认函数和用法较多，单开了一篇。
- [链接](Makefile提供的内置函数.md)
## 包含其他文件
```sh
include config.mk
-include optional.mk  # 表示文件不存在时不报错
```
- 字面意思
## 完整示例
```sh
# 变量定义
CC = gcc
CFLAGS = -Wall
TARGET = myapp
SOURCES = $(wildcard *.c)
OBJECTS = $(SOURCES:.c=.o)

# 默认目标
all: $(TARGET)

# 链接目标文件
$(TARGET): $(OBJECTS)
	$(CC) $(CFLAGS) -o $@ $^

# 编译源文件
%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

# 清理
.PHONY: clean
clean:
	rm -f $(OBJECTS) $(TARGET)

# 安装
install: $(TARGET)
	cp $(TARGET) /usr/local/bin/

# 依赖关系
main.o: main.h utils.h
utils.o: utils.h
```
## 高级特性
这一块建议动手实操试试
#### 多目标规则
```sh
output1 output2: source.txt
	generate $< $@
```
#### 静态模式规则
```sh
OBJECTS = main.o utils.o

$(OBJECTS): %.o: %.c
	gcc -c $< -o $@
```
#### 命令前缀
```sh
target:
	-rm file.txt    # 忽略错误
	@echo "Done"    # @ 不显示命令本身
```


