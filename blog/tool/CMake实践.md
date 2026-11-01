# CMake 实践

- 另一篇：[CMake 系统性指南](CMake.md)。

---

## 1. CMake 简介

`cmake` 是跨平台的构建系统生成工具，读取 `CMakeLists.txt` 生成 `Makefile` 等构建文件，无需手写 `makefile`。

## 2. 基础项目

CMake 的配置写在 `CMakeLists.txt`（下文简称 CML）中，生成结果一般放在 `build` 目录。

```cmake
cmake_minimum_required(VERSION 4.3.0)
project(Hello)
add_executable(hello hello.cpp)
```

`cmake_minimum_required()` 设置 CMake 的最低版本。本机版本低于该值时，配置阶段报错。把参数改为 4.4 后：

```text
CMake Error at CMakeLists.txt:1 (cmake_minimum_required):
  CMake 4.4 or higher is required.  You are running version 4.3.2


-- Configuring incomplete, errors occurred!
```

第一行 `CMake Error at CMakeLists.txt:1 (cmake_minimum_required):` 指出错误位置：`CMakeLists.txt` 第 1 行的 `cmake_minimum_required`。第二行给出原因：需要 4.4，当前为 4.3.2。

### 2.1 运行 CMake

在项目目录下创建 `build`，进入其中执行 `cmake ..`。CML 位于上一级目录，因此用 `..` 指代。

- CMake 会自动查找 `CMakeLists.txt`。

执行后，当前目录（即 `build`）中生成缓存和 CMake 运行所需的全部文件，其中最重要的是 `Makefile`。随后用 `make` 或 `cmake --build .` 生成目标文件。

不切换目录时，可用以下命令：

- `cmake -B build`：指定生成目录。CML 的搜索目录不指定时默认为当前目录。
- `cmake --build ./build`：生成目标文件，目录指定为当前目录下的 `build`。

### 2.2 没有生成 Makefile

没有生成 `Makefile`，通常是默认生成器不是 Makefile。执行 `cmake --help`，`The following generators are available on this platform (* marks default)` 一行及下方会列出支持的生成器与默认项（带 `*`）。用 `-G` 手动指定：

`cmake -G "" ..`

双引号内填入系统支持且希望使用的生成器。

## 3. CMake 官方教程

以下按官方教程 Step 1 的流程整理，官方版本为 4.3.3。

### 3.1 可执行目标（TODO1–TODO4）

文件结构：

```text
.
│  CMakeLists.txt
│
├─MathFunctions
│      CMakeLists.txt
│      MathFunctions.cxx
│      MathFunctions.h
│
└─Tutorial
        CMakeLists.txt
        Tutorial.cxx
```

最外层的 `CMakeLists.txt`：

```cmake
# TODO1: Set the minimum required version of CMake to be 3.23

# TODO2: Create a project named Tutorial

# TODO3: Add an executable target called Tutorial to the project

# TODO4: Add the Tutorial/Tutorial.cxx source file to the Tutorial target

# TODO7: Add the MathFunctions library as a linked dependency
#        to the Tutorial target

# TODO11: Add the Tutorial subdirectory to the project

# TODO5: Add a library target called MathFunctions to the project

# TODO6: Add the source and header file located in Step1/MathFunctions to the
#        MathFunctions target, note that the intended way to include the
#        MathFunctions header is:
#          #include <MathFunctions.h>

# TODO13: Add the MathFunctions subdirectory to the project

```

TODO1：用 `cmake_minimum_required(VERSION 3.23)` 把最低版本设为 3.23。

> `cmake_minimum_required()` 通常是 CML 的第一条命令。

TODO2：用 `project(Tutorial)` 设置项目名。

> `project()` 会设置许多 CMake 使用的变量，详见官方文档。

TODO3：用 `add_executable(Tutorial)` 添加名为 `Tutorial` 的可执行目标。

> 源文件不在这一步关联，改用 TODO4 的方式。

TODO4：用 `target_sources(Tutorial PRIVATE Tutorial/Tutorial.cxx)` 关联 `Tutorial/Tutorial.cxx`。

> 没有其他目标链接 `Tutorial`，所以它的源文件、库等都设为 `PRIVATE`。不设置也不会报错，但 `PRIVATE` 可以避免误用。

至此 CML 可用：

```cmake
# TODO1: Set the minimum required version of CMake to be 3.23
cmake_minimum_required(VERSION 3.23)
# TODO2: Create a project named Tutorial
project(Tutorial)
# TODO3: Add an executable target called Tutorial to the project
add_executable(Tutorial)
# TODO4: Add the Tutorial/Tutorial.cxx source file to the Tutorial target
target_sources(Tutorial PRIVATE Tutorial/Tutorial.cxx)
```

官方提供的 `Tutorial.cxx` 用于对一个数开平方。

```cpp
// A simple program that computes the square root of a number
#include <cmath>
#include <iostream>
#include <string>

// TODO8: Include the MathFunctions header

int main(int argc, char* argv[])
{
  if (argc < 2) {
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
  }

  // convert input to double
  double const inputValue = std::stod(argv[1]);

  // TODO9: Use the mathfunctions::sqrt function
  // calculate square root
  double const outputValue = std::sqrt(inputValue);
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
}

```

### 3.2 库目标（TODO5–TODO6）

TODO5：用 `add_library(MathFunctions)` 构建名为 `MathFunctions` 的库。

> `add_library()` 与 `add_executable()` 用法相似，针对的是库。

TODO6：在 `MathFunctions.cxx` 中加入 `#include "MathFunctions.h"`，并把源文件和头文件关联到 `MathFunctions`：

```cmake
target_sources(
  MathFunctions 
PRIVATE 
  MathFunctions/MathFunctions.cxx 
PUBLIC 
  FILE_SET math_lib
  TYPE HEADERS
  BASE_DIRS Mathfunctions
  FILES Mathfunctions/Mathfunctions.h
)
```

前两个参数是名称和依赖文件。`PUBLIC` 表示其他目标也可能依赖这些内容。

> `FILE_SET` 是 CMake 3.23+ 的功能，用于管理依赖文件。它创建一个文件集合，名字由使用者指定，本例为 `math_lib`；名字不允许以大写字母或数字开头。
> `TYPE HEADERS` 表示该集合管理头文件。集合名与类型相同时可省略，直接写 `FILE_SET HEADERS`；小型项目或维护量低时可以这样写，但推荐完整格式。
> `BASE_DIRS` 是依赖文件的存放位置，`FILES` 是具体文件。

`MathFunctions.h`：

```cpp
#pragma once

namespace mathfunctions {
double sqrt(double x);
}

```

`MathFunctions.cxx`：

```cpp
#include <iostream>
#include "MathFunctions.h"//在todo6中手动新增的

namespace {
// a hack square root calculation using simple operations
double mysqrt(double x)
{
  if (x <= 0) {
    return 0;
  }

  double result = x;

  // do ten iterations
  for (int i = 0; i < 10; ++i) {
    if (result <= 0) {
      result = 0.1;
    }
    double delta = x - (result * result);
    result = result + 0.5 * delta / result;
    std::cout << "Computing sqrt of " << x << " to be " << result << std::endl;
  }
  return result;
}
}

namespace mathfunctions {
double sqrt(double x)
{
  return mysqrt(x);
}
}

```

重新构建后，库文件一并生成。

### 3.3 链接与子目录（TODO7–TODO13）

---

TODO7：把库链接到可执行文件。

`target_link_libraries(Tutorial PRIVATE MathFunctions)`

> `PRIVATE`、`INTERFACE`、`PUBLIC` 是范围关键词：`PRIVATE` 属于当前目标，被链接时不传递；`INTERFACE` 不属于当前目标，被链接时传递；`PUBLIC` 属于当前目标，被链接时传递。详见官方文档。

TODO8：在 `Tutorial.cxx` 中加入 `MathFunctions` 的头文件。

TODO9：调用 `MathFunctions.h` 中的函数，替换 `std` 中的实现。

TODO10：把 `Tutorial` 相关的命令移到 `Tutorial` 目录下的 CML，并更新路径。

TODO11：用 `add_subdirectory(Tutorial)` 把该子目录纳入构建。

TODO12、TODO13：用同样的方法迁移 `MathFunctions` 相关的命令，并纳入构建。

为避免与原有构建冲突，可用 `--clean-first` 清理：

`cmake --build build --clean-first`

以下是全部文件内容。

`Tutorial.cxx`

```cpp
// A simple program that computes the square root of a number
#include <iostream>
#include <string>
#include "MathFunctions.h"
// TODO8: Include the MathFunctions header

int main(int argc, char* argv[])
{
  if (argc < 2) {
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
  }

  // convert input to double
  double const inputValue = std::stod(argv[1]);

  // TODO9: Use the mathfunctions::sqrt function
  // calculate square root
  double const outputValue = mathfunctions::sqrt(inputValue);
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
}

```

`MathFunctions.cxx`

```cpp
#include <iostream>
#include "MathFunctions.h"

namespace {
// a hack square root calculation using simple operations
double mysqrt(double x)
{
  if (x <= 0) {
    return 0;
  }

  double result = x;

  // do ten iterations
  for (int i = 0; i < 10; ++i) {
    if (result <= 0) {
      result = 0.1;
    }
    double delta = x - (result * result);
    result = result + 0.5 * delta / result;
    std::cout << "Computing sqrt of " << x << " to be " << result << std::endl;
  }
  return result;
}
}

namespace mathfunctions {
double sqrt(double x)
{
  return mysqrt(x);
}
}

```

`MathFunctions.h`

```cpp
#pragma once

namespace mathfunctions {
double sqrt(double x);
}

```

`./CML`

```cmake
# TODO1: Set the minimum required version of CMake to be 3.23
cmake_minimum_required(VERSION 3.23)
# TODO2: Create a project named Tutorial
project(Tutorial)
# TODO3: Add an executable target called Tutorial to the project
#add_executable(Tutorial)
# TODO4: Add the Tutorial/Tutorial.cxx source file to the Tutorial target
#target_sources(Tutorial PRIVATE Tutorial/Tutorial.cxx)
# TODO7: Add the MathFunctions library as a linked dependency
#        to the Tutorial target
#target_link_libraries(Tutorial PRIVATE MathFunctions)
# TODO11: Add the Tutorial subdirectory to the project
add_subdirectory(Tutorial)
# TODO5: Add a library target called MathFunctions to the project
#add_library(MathFunctions)
# TODO6: Add the source and header file located in Step1/MathFunctions to the
#        MathFunctions target, note that the intended way to include the
#        MathFunctions header is:
#          #include <MathFunctions.h>
# target_sources(
#  	MathFunctions 
# PRIVATE 
# 	MathFunctions/MathFunctions.cxx 
# PUBLIC 
# 	FILE_SET math_lib
# 	TYPE HEADERS
# 	BASE_DIRS Mathfunctions
# 	FILES Mathfunctions/Mathfunctions.h
# )
# TODO13: Add the MathFunctions subdirectory to the project
add_subdirectory(MathFunctions)
```

`./Tutorial`

```cmake
# TODO10: Move all the Tutorial target commands to this CMakeLists.txt. Ensure
#          that all paths are updated to be relative to this new location.
add_executable(Tutorial)

target_sources(Tutorial PRIVATE Tutorial.cxx)

target_link_libraries(Tutorial PRIVATE MathFunctions)

```

`./MathFunctions`

```cmake
# TODO12: Move all the MathFunctions target commands to this CMakeLists.txt.
#         Ensure that all paths are updated to be relative to this new location.
add_library(MathFunctions)

target_sources(
	MathFunctions 
PRIVATE 
	MathFunctions.cxx 
PUBLIC 
	FILE_SET math_lib
	TYPE HEADERS
	FILES Mathfunctions.h
)
```

- 已不需要的操作均已注释，可以直接删除。
