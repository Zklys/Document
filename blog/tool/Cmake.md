# CMake 系统性指南：从入门到精通


## 第一部分：核心概念与认知

### 1.1 什么是 CMake？

CMake 是一个跨平台的自动化构建系统生成器。它**本身不是编译器**，而是一个**构建文件生成工具**（Build System Generator）。它的核心作用是读取名为 `CMakeLists.txt` 的配置文件，根据当前的操作系统和编译器环境，生成标准的构建文件：

| 平台 | 生成的构建文件 |
|------|---------------|
| Linux/Unix | Makefile |
| Windows | Visual Studio Solution (.sln) / MinGW Makefile |
| macOS | Xcode Project |

调用对应的构建工具（如 `make`、`MSBuild`、`xcodebuild`）完成实际的编译和链接工作。

### 1.2 为什么需要 CMake？

在没有 CMake 的时代，C/C++ 工程构建面临两大核心痛点：

- **跨平台适配成本极高**：不同平台有完全不同的构建系统，每新增一个平台就要手写一套适配配置
- **手写构建配置门槛高**：Makefile 语法晦涩复杂，中大型项目的依赖管理、链接规则维护难度极大

CMake 的核心价值可以用一句话概括：**一次编写规则，多处生成构建系统**。你只需维护一份 `CMakeLists.txt`，在不同平台上用不同生成器即可构建。

### 1.3 形象化理解

如果把开发软件比作盖一栋房子：
- **CMake（设计院）** ：读取你的设计需求（`CMakeLists.txt`），绘制出详细的施工图纸
- **Makefile/.sln（施工图纸）** ：详细规定材料处理顺序（编译、链接）
- **Make/Ninja/MSBuild（工头）** ：读懂图纸，指挥工人干活
- **Compiler [GCC/Clang/MSVC]（工人）** ：真正把源码翻译成机器码

### 1.4 核心术语

- **Target（目标）** ：CMake 构建出的最终产物，通常是可执行文件（Executable）或库文件（Library）
- **Command（指令）** ：`CMakeLists.txt` 中的函数调用，如 `project()`、`add_executable()`
- **Variable（变量）** ：用于存储信息的容器，如 `${CMAKE_SOURCE_DIR}`
- **源目录（Source Directory）** ：项目源代码所在目录，`CMakeLists.txt` 所在的位置
- **二进制目录（Build Directory）** ：CMake 将生成的 object 文件、库和可执行文件放在这里

### 1.5 安装 CMake

```bash
# Debian / Ubuntu
sudo apt-get install cmake

# CentOS / RHEL
sudo yum install cmake

# 验证安装
cmake --version
```

推荐使用 CMake 3.10+，更建议 3.16+ 或 3.20+。如需更快的构建，可安装 Ninja：

```bash
sudo apt-get install ninja-build
```


## 第二部分：快速入门

### 2.1 最简项目结构

```
Project/
├── CMakeLists.txt
└── main.cpp
```

### 2.2 代码示例

**main.cpp**:
```cpp
#include <iostream>
int main() {
    std::cout << "Hello, CMake!" << std::endl;
    return 0;
}
```

**CMakeLists.txt**:
```cmake
# 1. 指定 CMake 最低版本要求（必选）
cmake_minimum_required(VERSION 3.10)

# 2. 定义项目名称和支持语言
project(HelloWorld LANGUAGES CXX)

# 3. 指定 C++ 标准（推荐）
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 4. 添加可执行文件目标
add_executable(MyApp main.cpp)
```

### 2.3 标准构建流程

CMake 强烈推荐使用**外部构建**（Out-of-source Build），即源码与编译产物分离：

```bash
# 步骤1：创建并进入构建目录
mkdir build && cd build

# 步骤2：执行 cmake 配置，生成构建文件
cmake ..

# 步骤3：执行构建
make

# 步骤4：运行程序
./MyApp
```

**更现代的方式**（CMake 3.13+）：
```bash
# 一步创建构建目录并配置
cmake -B build -S .

# 构建
cmake --build build

# 运行
./build/MyApp
```

`-B` 指定构建目录，`-S` 指定源码目录。


## 第三部分：CMake 基础语法

### 3.1 语法核心规则

- **大小写不敏感**：`cmake_minimum_required` 和 `CMAKE_MINIMUM_REQUIRED` 等效，**推荐小写**
- **指令格式**：`指令(参数1 参数2 ...)`，参数之间用空格或分号分隔
- **注释**：单行注释用 `#` 开头
- **变量引用**：使用 `${变量名}`

### 3.2 核心命令详解

#### cmake_minimum_required

指定运行此 `CMakeLists.txt` 所需的最低 CMake 版本：
```cmake
cmake_minimum_required(VERSION 3.10)
cmake_minimum_required(VERSION 3.10...3.20)  # 支持 3.10 到 3.20
```

调用时，它确保 CMake 将采用所列版本的行为。

#### project

定义项目名称，CMake 会自动生成相关变量：
```cmake
project(MyProject)                           # 最简形式
project(MyProject VERSION 1.0.0)             # 指定版本
project(MyProject LANGUAGES CXX CUDA)        # 指定语言（支持 C++ 和 CUDA）
```

`project()` 会设置以下变量：
- `PROJECT_NAME`：项目名称
- `PROJECT_VERSION`：项目版本
- `PROJECT_SOURCE_DIR`：项目源码目录
- `PROJECT_BINARY_DIR`：项目构建目录

#### add_executable

添加可执行文件目标：
```cmake
# 单源文件
add_executable(myapp main.cpp)

# 多源文件
add_executable(myapp main.cpp utils.cpp helper.cpp)

# 使用变量
set(SOURCES main.cpp utils.cpp helper.cpp)
add_executable(myapp ${SOURCES})
```

#### add_library

添加库目标：
```cmake
# 静态库（libmylib.a 或 mylib.lib）
add_library(mylib STATIC lib.cpp)

# 动态库（libmylib.so 或 mylib.dll）
add_library(mylib SHARED lib.cpp)

# 默认（通常为静态库）
add_library(mylib lib.cpp)
```

### 3.3 变量

#### 定义和使用

```cmake
# 定义变量
set(MY_VAR "Hello")
set(SOURCES main.cpp utils.cpp)

# 使用变量
message(STATUS "MY_VAR = ${MY_VAR}")
add_executable(myapp ${SOURCES})

# 追加变量
set(SOURCES ${SOURCES} helper.cpp)
list(APPEND SOURCES extra.cpp)   # 推荐方式
```

#### 常用内置变量

| 变量 | 说明 |
|------|------|
| `CMAKE_SOURCE_DIR` | 源代码树的根目录 |
| `CMAKE_BINARY_DIR` | 构建树的根目录 |
| `CMAKE_CURRENT_SOURCE_DIR` | 当前处理的源目录 |
| `CMAKE_CURRENT_BINARY_DIR` | 当前处理的构建目录 |
| `CMAKE_CXX_STANDARD` | C++ 标准版本 |
| `CMAKE_BUILD_TYPE` | 构建类型（Debug/Release） |

### 3.4 控制流

CMake 支持与其他脚本语言类似的控制流结构。

#### if 条件判断

```cmake
if(VAR)
    # VAR 为 ON/YES/TRUE/Y 或非零数字时为真
endif()

if(VAR STREQUAL "value")
    # 字符串比较
elseif(VAR MATCHES "regex")
    # 正则匹配
else()
    # 其他情况
endif()

# 常用条件
if(DEFINED VAR)           # 变量是否已定义
if(EXISTS file.txt)       # 文件是否存在
if(COMMAND command_name)  # 命令是否存在
```

#### foreach 循环

```cmake
# 遍历列表
foreach(item ${MY_LIST})
    message(STATUS "Item: ${item}")
endforeach()

# 范围循环
foreach(i RANGE 10)
    message(STATUS "i = ${i}")
endforeach()

foreach(i RANGE 1 10 2)   # 从1到10，步长2
    # ...
endforeach()
```

#### while 循环

```cmake
while(condition)
    # ...
endwhile()
```


## 第四部分：现代 CMake（Modern CMake）

### 4.1 目标（Target）的概念

现代 CMake 的核心是以 **目标（Target）** 为中心进行构建配置。目标可以是可执行文件、库或自定义目标。每个目标都有其自己的属性，包括编译选项、链接选项、包含路径等。

### 4.2 PUBLIC / PRIVATE / INTERFACE

这三个关键字是理解现代 CMake 依赖传递机制的关键：

| 关键字 | 含义 | 使用方（消费者）是否继承 |
|--------|------|------------------------|
| `PRIVATE` | 仅当前目标需要 | 不继承 |
| `PUBLIC` | 当前目标和消费者都需要 | 继承 |
| `INTERFACE` | 仅消费者需要（当前目标不需要） | 继承 |

```cmake
# 示例：libA 依赖 libB
target_include_directories(libA PRIVATE include/internal)   # 仅 libA 内部使用
target_include_directories(libA PUBLIC include/public)       # libA 和使用者都需要
target_include_directories(libA INTERFACE include/interface) # 仅使用者需要

# 当 myapp 链接 libA 时
target_link_libraries(myapp libA)
# myapp 会自动继承 PUBLIC 和 INTERFACE 的包含路径
```

### 4.3 目标属性命令

```cmake
# 设置包含目录
target_include_directories(target
    PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src
    PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# 设置编译定义
target_compile_definitions(target
    PRIVATE DEBUG_MODE
    PUBLIC VERSION=1
)

# 设置编译选项
target_compile_options(target
    PRIVATE -Wall -Wextra
)

# 设置链接库
target_link_libraries(target
    PRIVATE my_internal_lib
    PUBLIC my_public_lib
)

# 设置链接选项
target_link_options(target
    PRIVATE -pthread
)
```

### 4.4 目标间依赖

```cmake
# 创建库
add_library(math STATIC math.cpp)
add_library(utils STATIC utils.cpp)

# 创建可执行文件并链接库
add_executable(myapp main.cpp)
target_link_libraries(myapp PRIVATE math utils)

# 库之间也可以相互依赖
target_link_libraries(utils PUBLIC math)  # 链接 math 到 utils
```

### 4.5 生成器表达式（Generator Expressions）

生成器表达式在构建系统生成期间求值，用于产生特定于每个构建配置的信息。语法形式为 `$<...>`。

```cmake
# 条件编译选项（Debug 和 Release 不同）
target_compile_definitions(myapp PRIVATE
    $<$<CONFIG:Debug>:DEBUG_MODE>
    $<$<CONFIG:Release>:NDEBUG>
)

# 条件链接
target_link_libraries(myapp PRIVATE
    $<$<PLATFORM_ID:Windows>:ws2_32>
    $<$<PLATFORM_ID:Linux>:pthread>
)

# 布尔表达式
$<0:...>   # 永远为假
$<1:...>   # 永远为真
```

生成器表达式支持空格、换行符等，传递给命令时整个表达式应用引号括起来。


## 第五部分：文件与目录管理

### 5.1 添加子目录

```cmake
# 添加子目录（该目录下必须有 CMakeLists.txt）
add_subdirectory(src)
add_subdirectory(lib)
```

### 5.2 文件收集

```cmake
# 手动列出源文件（推荐，明确可控）
set(SOURCES
    src/main.cpp
    src/helper.cpp
    src/parser.cpp
)

# 使用 file(GLOB) 自动收集（不推荐用于源文件，因为新增文件不会自动触发重新配置）
file(GLOB SOURCES "src/*.cpp")
file(GLOB_RECURSE SOURCES "src/**/*.cpp")  # 递归收集

# 更好的方式：显式列出 + 使用 aux_source_directory
aux_source_directory(src SOURCES)
```

> **注意**：`file(GLOB)` 收集源文件存在隐患——新增文件不会自动触发 CMake 重新配置，可能导致构建遗漏。推荐显式列出源文件，或配合 `CONFIGURE_DEPENDS` 使用。

### 5.3 包含其他 CMake 文件

```cmake
# 包含自定义模块
include(cmake/MyModule.cmake)

# 设置模块搜索路径
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_SOURCE_DIR}/cmake)
```


## 第六部分：函数与宏

### 6.1 函数（Function）

函数有自己的**局部作用域**——函数内定义的变量不会传播回调用者：

```cmake
function(my_function arg1 arg2)
    set(LOCAL_VAR "inside function")
    message(STATUS "arg1 = ${arg1}, arg2 = ${arg2}")
    # ${ARGN} 包含所有额外参数
    message(STATUS "extra args: ${ARGN}")
endfunction()

# 调用
my_function("hello" "world" "extra1" "extra2")
```

### 6.2 宏（Macro）

宏进行**文本替换**，没有独立的作用域——宏内定义的变量会影响调用者的作用域：

```cmake
macro(my_macro arg1 arg2)
    set(MACRO_VAR "defined in macro")  # 会影响调用者！
    message(STATUS "arg1 = ${arg1}, arg2 = ${arg2}")
endmacro()

my_macro("hello" "world")
message(STATUS "MACRO_VAR = ${MACRO_VAR}")  # 可以访问到
```

### 6.3 函数 vs 宏

| 特性 | 函数（Function） | 宏（Macro） |
|------|-----------------|-------------|
| 作用域 | 局部作用域 | 全局作用域（文本替换） |
| 变量传递 | 参数作为变量 | 参数进行字符串替换 |
| 返回值 | 通过 `PARENT_SCOPE` 返回 | 直接修改调用者变量 |

函数推荐用于大多数场景，因为其作用域更可控。如需从函数返回变量给调用者：

```cmake
function(get_version)
    set(VERSION "1.0.0" PARENT_SCOPE)
endfunction()

get_version()
message(STATUS "Version: ${VERSION}")
```


## 第七部分：依赖管理

### 7.1 find_package

`find_package` 用于查找系统中已安装的依赖包。它有两种模式：

- **Module 模式**：搜索 `Find<Package>.cmake` 文件
- **Config 模式**：搜索 `<Package>Config.cmake` 文件（通常由包本身提供）

```cmake
# 基本用法
find_package(Boost REQUIRED)

# 指定组件
find_package(Boost REQUIRED COMPONENTS filesystem system)

# 使用查找结果
if(Boost_FOUND)
    target_link_libraries(myapp PRIVATE Boost::filesystem Boost::system)
endif()

# 查找 OpenCV
find_package(OpenCV REQUIRED)
target_link_libraries(myapp PRIVATE ${OpenCV_LIBS})
target_include_directories(myapp PRIVATE ${OpenCV_INCLUDE_DIRS})
```

### 7.2 find 系列命令

```cmake
# 查找程序
find_program(DOXYGEN_EXECUTABLE doxygen)
if(DOXYGEN_EXECUTABLE)
    message(STATUS "Found Doxygen: ${DOXYGEN_EXECUTABLE}")
endif()

# 查找库
find_library(MATH_LIBRARY m)
target_link_libraries(myapp PRIVATE ${MATH_LIBRARY})

# 查找路径
find_path(OPENSSL_INCLUDE_DIR openssl/ssl.h)
find_file(CONFIG_FILE config.json)
```

### 7.3 自定义 Find 模块

当第三方库不提供 Config 文件时，可以自己编写 Find 模块：

```cmake
# 创建 cmake/FindMyLib.cmake
find_path(MYLIB_INCLUDE_DIR mylib.h)
find_library(MYLIB_LIBRARY mylib)

include(FindPackageHandleStandardArgs)
find_package_handle_standard_args(MyLib
    REQUIRED_VARS MYLIB_INCLUDE_DIR MYLIB_LIBRARY
)

if(MYLIB_FOUND AND NOT TARGET MyLib::MyLib)
    add_library(MyLib::MyLib UNKNOWN IMPORTED)
    set_target_properties(MyLib::MyLib PROPERTIES
        INTERFACE_INCLUDE_DIRECTORIES ${MYLIB_INCLUDE_DIR}
        IMPORTED_LOCATION ${MYLIB_LIBRARY}
    )
endif()
```

然后在 `CMakeLists.txt` 中：

```cmake
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_SOURCE_DIR}/cmake)
find_package(MyLib REQUIRED)
target_link_libraries(myapp PRIVATE MyLib::MyLib)
```


## 第八部分：测试（CTest）

CMake 通过 `enable_testing()` 和 `add_test()` 命令与 CTest 直接集成。

### 8.1 启用测试

```cmake
# 在顶层 CMakeLists.txt 中
enable_testing()
```

### 8.2 添加测试

```cmake
# 简单测试：运行可执行文件
add_test(NAME test1 COMMAND myapp --test)

# 带参数的测试
add_test(NAME test_math COMMAND myapp --math-test)

# 设置测试属性
set_tests_properties(test1 PROPERTIES
    TIMEOUT 10           # 超时 10 秒
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)

# 使用生成器表达式
add_test(NAME config_test COMMAND myapp --config $<CONFIG>)
```

### 8.3 运行测试

```bash
# 在构建目录中
ctest

# 详细输出
ctest --output-on-failure

# 运行特定测试
ctest -R test1

# 并行运行
ctest -j 4
```


## 第九部分：安装与导出

### 9.1 安装目标

```cmake
# 安装可执行文件
install(TARGETS myapp
    RUNTIME DESTINATION bin
)

# 安装库
install(TARGETS mylib
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
)

# 安装头文件
install(FILES mylib.h DESTINATION include)
install(DIRECTORY include/ DESTINATION include
    FILES_MATCHING PATTERN "*.h"
)
```

### 9.2 安装目录

```cmake
# 安装整个目录
install(DIRECTORY resources/
    DESTINATION share/myapp/resources
)

# 安装配置文件
install(FILES config.json
    DESTINATION etc/myapp
)
```

### 9.3 导出目标

导出目标使得其他项目可以通过 `find_package` 找到并使用你的库：

```cmake
# 安装时导出目标
install(TARGETS mylib
    EXPORT MyLibTargets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    INCLUDES DESTINATION include
)

# 安装导出文件
install(EXPORT MyLibTargets
    FILE MyLibTargets.cmake
    NAMESPACE MyLib::
    DESTINATION lib/cmake/MyLib
)

# 生成 Config 文件
include(CMakePackageConfigHelpers)
configure_package_config_file(
    ${CMAKE_SOURCE_DIR}/cmake/Config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/MyLibConfig.cmake
    INSTALL_DESTINATION lib/cmake/MyLib
)
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/MyLibConfig.cmake
    DESTINATION lib/cmake/MyLib
)
```

### 9.4 导出构建树（不安装即可使用）

```cmake
# 导出到构建目录
export(TARGETS mylib
    FILE ${CMAKE_BINARY_DIR}/MyLibTargets.cmake
    NAMESPACE MyLib::
)
```

其他项目使用：
```cmake
# 直接使用构建目录中的导出文件
list(APPEND CMAKE_PREFIX_PATH /path/to/MyLib/build)
find_package(MyLib REQUIRED)
```


## 第十部分：高级特性

### 10.1 自定义命令（add_custom_command）

`add_custom_command` 用于定义生成特定文件的自定义构建步骤。

**第一种签名：生成文件**
```cmake
add_custom_command(
    OUTPUT generated.cpp
    COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_SOURCE_DIR}/template.cpp generated.cpp
    DEPENDS ${CMAKE_SOURCE_DIR}/template.cpp
    COMMENT "Generating source file..."
)
```

**第二种签名：为目标添加构建事件**
```cmake
add_custom_command(
    TARGET myapp
    POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        ${CMAKE_SOURCE_DIR}/config.json
        $<TARGET_FILE_DIR:myapp>/config.json
    COMMENT "Copying config file..."
)
```

### 10.2 自定义目标（add_custom_target）

`add_custom_target` 创建一个没有输出的目标，每次构建都会执行：

```cmake
# 创建文档生成目标
add_custom_target(docs
    COMMAND ${DOXYGEN_EXECUTABLE} Doxyfile
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    COMMENT "Generating documentation..."
)

# 添加依赖：构建 myapp 时自动生成文档？
# 不直接添加依赖，而是让用户可以手动构建
# 或使用 ALL 关键字让每次构建都执行
add_custom_target(run_tests ALL
    COMMAND ${CMAKE_CTEST_COMMAND}
    COMMENT "Running tests..."
)
```

### 10.3 跨平台可移植命令

CMake 提供了 `-E` 选项作为跨平台实用命令：

```cmake
# 跨平台复制
${CMAKE_COMMAND} -E copy source.txt dest.txt

# 跨平台删除
${CMAKE_COMMAND} -E remove -f file.txt

# 跨平台创建目录
${CMAKE_COMMAND} -E make_directory dir

# 条件复制（仅当源文件更新）
${CMAKE_COMMAND} -E copy_if_different source.txt dest.txt

# 创建符号链接
${CMAKE_COMMAND} -E create_symlink target link
```

### 10.4 CMake 预设（Presets）

CMake 3.19+ 支持预设（Presets），可以在 `CMakePresets.json` 中定义常用的配置组合：

```json
{
    "version": 3,
    "configurePresets": [
        {
            "name": "default",
            "generator": "Ninja",
            "binaryDir": "${sourceDir}/build",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Release",
                "CMAKE_CXX_STANDARD": "17"
            }
        },
        {
            "name": "debug",
            "inherits": "default",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Debug"
            }
        }
    ]
}
```

使用预设：
```bash
cmake --preset default
cmake --build build
```

### 10.5 工具链文件（Toolchain File）

用于交叉编译或指定特定的编译器：

```cmake
# toolchain.cmake
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)

set(CMAKE_C_COMPILER arm-linux-gnueabihf-gcc)
set(CMAKE_CXX_COMPILER arm-linux-gnueabihf-g++)

set(CMAKE_FIND_ROOT_PATH /usr/arm-linux-gnueabihf)
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
```

使用：
```bash
cmake -DCMAKE_TOOLCHAIN_FILE=toolchain.cmake ..
```


## 第十一部分：最佳实践

### 11.1 项目结构规范

```
Project/
├── CMakeLists.txt              # 顶层配置
├── cmake/                      # 自定义 CMake 模块
│   ├── FindMyLib.cmake
│   └── MyModule.cmake
├── include/                    # 公共头文件
│   └── project/
│       └── header.h
├── src/                        # 源文件
│   ├── CMakeLists.txt
│   └── main.cpp
├── tests/                      # 测试
│   ├── CMakeLists.txt
│   └── test_main.cpp
└── examples/                   # 示例
    ├── CMakeLists.txt
    └── example.cpp
```

### 11.2 推荐的顶层 CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.16)
project(MyProject VERSION 1.0.0 LANGUAGES CXX)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 设置构建类型默认值
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Build type" FORCE)
endif()

# 添加模块路径
list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)

# 包含子目录
add_subdirectory(src)
if(BUILD_TESTING)
    enable_testing()
    add_subdirectory(tests)
endif()

# 安装
include(CMakePackageConfigHelpers)
# ... 安装配置 ...
```

### 11.3 关键原则

1. **始终使用外部构建**：源码与构建产物分离
2. **使用 target_* 命令而非 set 命令**：现代 CMake 推荐基于目标的配置
3. **明确 PUBLIC/PRIVATE/INTERFACE 作用域**：正确管理依赖传递
4. **避免使用 file(GLOB) 收集源文件**：显式列出源文件更可靠
5. **CMake 代码应像生产代码一样对待**：保持可维护、优雅和简洁
6. **指定 CMake 最低版本**：使用 `cmake_minimum_required` 保证兼容性
7. **使用 CMake 预设**：标准化团队的构建配置


## 第十二部分：常见问题与排查

### 12.1 常见错误

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| `CMake Error: Could not find CMAKE_ROOT` | CMake 安装不完整 | 重新安装 CMake |
| `Target "xxx" links to itself` | 循环依赖 | 检查 `target_link_libraries` 依赖关系 |
| `Cannot find source file` | 源文件路径错误 | 检查路径是否正确，使用绝对路径或相对于 `CMAKE_CURRENT_SOURCE_DIR` |
| `CMake Error: The source directory does not appear to contain CMakeLists.txt` | 在错误的目录执行 `cmake` | 确保在构建目录执行 `cmake <源码路径>` |

### 12.2 调试技巧

```cmake
# 打印变量值
message(STATUS "MY_VAR = ${MY_VAR}")

# 打印带颜色的消息
message("-- Configuring done")

# 显示所有变量（调试用）
get_cmake_property(_variableNames VARIABLES)
foreach(_varName ${_variableNames})
    message(STATUS "${_varName}=${${_varName}}")
endforeach()

# 显示目标属性
get_target_property(SOURCES myapp SOURCES)
message(STATUS "myapp SOURCES: ${SOURCES}")
```

### 12.3 清理构建

```bash
# 删除构建目录（最彻底）
rm -rf build/

# 或使用 CMake 清理
cmake --build build --target clean
```

### 12.4 常用命令速查

| 命令 | 用途 |
|------|------|
| `cmake_minimum_required` | 指定最低 CMake 版本 |
| `project` | 定义项目 |
| `add_executable` | 添加可执行文件目标 |
| `add_library` | 添加库目标 |
| `target_link_libraries` | 链接库 |
| `target_include_directories` | 设置包含目录 |
| `target_compile_definitions` | 设置编译宏 |
| `target_compile_options` | 设置编译选项 |
| `find_package` | 查找依赖包 |
| `add_subdirectory` | 添加子目录 |
| `add_test` | 添加测试 |
| `install` | 安装文件/目标 |
| `set` | 设置变量 |
| `list` | 操作列表 |
| `if`/`elseif`/`else`/`endif` | 条件判断 |
| `foreach`/`endforeach` | 循环 |
| `function`/`endfunction` | 定义函数 |
| `macro`/`endmacro` | 定义宏 |
| `add_custom_command` | 自定义构建命令 |
| `add_custom_target` | 自定义构建目标 |
| `message` | 输出信息 |


## 结语

CMake 作为 C/C++ 领域事实上的工程构建标准，其核心价值在于 **"一处配置，到处构建"** 。从最基础的单文件项目到复杂的大型工程，CMake 都能提供可靠、可维护的构建方案。

学习 CMake 的最佳路径是**循序渐进**：先掌握核心命令（`cmake_minimum_required`、`project`、`add_executable`、`add_library`、`target_link_libraries`），然后逐步学习现代 CMake 的目标属性管理和依赖传递机制，最后掌握测试、安装、导出等工程化能力。

记住：**把 CMake 代码当作生产代码一样对待**——保持整洁、可维护、有注释，你的团队和未来的自己都会感谢你。