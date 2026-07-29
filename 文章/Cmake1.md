# Cmake的使用方法
- 另一篇[CMake](Cmake.md)。
---
- ## cmake的简单介绍

`cmake`是一个辅助项目程序构建系统，能够跨平台构建程序。
能够生成`makefile`文件，能够让你省去编写复杂`makefile`文件的功夫`（大概也许会重新花在编写cmake的文件上？）`

## 一个基础的cmake项目

- 一般来说cmake的内容都写在一个叫做`CMakeLists.txt`的文件里。
- 而文件生成则是在`build`文件夹中。

示例：如下

```javascript
cmake_minimum_required(VERSION 4.3.0)
project(Hello)
add_executable(hello hello.cpp)
```

`cmake_minimum_required()`是用来设置cmake的最低版本的，意思是你的cmake版本最低不能低于这个版本。
如果我们把参数改为4.4的话，cmake会报出这样的错误：如下

```javascript
CMake Error at CMakeLists.txt:1 (cmake_minimum_required):
  CMake 4.4 or higher is required.  You are running version 4.3.2


-- Configuring incomplete, errors occurred!
```

我们查看报错的信息的话，能够看出`CMake Error at CMakeLists.txt:1 (cmake_minimum_required):`这一行cmake报出了错误出现在了`CMakeLists.txt`文件的第一行的`cmake_minimum_required`中。
第二行cmake报出了我们需要4.4的cmake而我使用的cmake是4.3.2版本所以报出了错误。

### cmake的运行

我们可以在项目文件夹中创建一个叫做`build`然后在此文件夹中运行`cmake ..`
因为你的`CMakeLists.txt`文件大概在上一级目录，所以我们使用`..`来代替上级目录。

- cmake会自动识别`CMakeList.txt`文件。

然后cmake会在你当前所在的文件夹，也就是`build`文件夹中生成cmake的缓存和所有cmake运行所需要的文件以及最重要的`Makefile`文件。
随后使用`make`命令或`cmake --build .`即可生成目标文件。
如果你不想移动你的目录位置的话，你可以使用以下的命令来生成。

- `cmake -B build`这条命令指定了cmake的生成目录。（你可以指定`CMakeList.txt`的搜索目录，不指定就为当前目录。）
- `cmake --build ./build`这条命令是生成目标文件，但指定了目录为当前目录中的build目录。

#### 可能出现的问题

如果你并没有生成`makefile`文件，那么极有可能是你的cmake默认并没有采用makefile的生成策略。
你可以使用`cmake --help`命令在`The following generators are available on this platform (* marks default)`行和此行下面看到你的cmake支持的生成策略和默认使用的策略（带*号的）
解决方法就是在使用中可以使用参数手动指定。
`cmake -G "" ..`双引号中填入你希望使用并且系统所支持的生成策略。

## cmake官方的文档教程

接下来就会根据cmake的官方教程流程来书写。

- 官方的版本为4.3.3

### step1

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

在最外层的`CMakeLists.txt`文件中：

```javascript
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

在todo1中我们需要使用`cmake_minimum_required(VERSION 3.23)`来将cmake的最低版本设置为3.23。

> - 一般来说`cmake_minimum_requide()`都会是CML(CMakeLists.txt的缩写)的第一条命令。

在tudo2中我们需要使用`project(Tutorial)`来设置项目名称为Tutorial。

> - `project()`在实际运行过程中会设置许多cmake需要使用的变量，如果你希望了解或者手动设置可以查看官方文档。

在todo3中我们要使用`add_executable(Tutorial)`来为项目添加一个名为`Tutorial`的可执行目标。

> - 与之前不同的是，我们将使用另一种方式将源文件和可执行目标关联，在第四步中。

在tudo4中我们要使用`target_sources(Tutorial PRIVATE Tutorial/Tutorial.cxx)`来将可执行目标和当前目录下，Tutorial目录内的Tutorial.cxx文件关联。

> - 在这里对于可执行文件Tutorial没有任何其他目标会链接它，所以它的源文件、库等等都需要设置为`PRIVATE`
> - 即使你不设置也不会报错，但是为了防止人类的各种迷惑操作，请使用`PRIVATE`将其限定。

ok！暂时性的，我们编写了一份可用的CML，现在你可以执行它了。

```javascript
# TODO1: Set the minimum required version of CMake to be 3.23
cmake_minimum_required(VERSION 3.23)
# TODO2: Create a project named Tutorial
project(Tutorial)
# TODO3: Add an executable target called Tutorial to the project
add_executable(Tutorial)
# TODO4: Add the Tutorial/Tutorial.cxx source file to the Tutorial target
target_sources(Tutorial PRIVATE Tutorial/Tutorial.cxx)
```

在cmake官方给的Tutorial.cxx的作用是给一个数开平方。

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

---
在todo5中我们要使用`add_library(MathFunctions)`它的作用是构建一个叫做MathFunctions的库。

> - `add_library()`的作用和`add_executable()`很相似，但它是针对库的。

在todo6中我们要将`MathFunctions.cxx`所需要的头文件添加进去。
在这个文件中加入`#include "MathFunctions.h"`
同时我们也要把`MathFunctions`所需要的源文件和头文件与它关联起来。
我们使用如下的方式：

```javascript
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

前两个参数分别是名称和依赖文件，和之前类似就不再详细解释了。
注意到我们这里使用了新的声明参数`PUBLIC`表示共有，表示其他东西也可能依赖这些东西。

> - `FILE_SET`是cmake-3.23+之后新加入的功能，目的是让依赖文件更好管理，它的作用是创建一个文件集合，由你自己起名，我这边叫做：`math_lib`它不允许大写字母和数字开头。
> - `TYPE HEADERS`表明这个文件集合管理的是头文件（其他参数之后再进行讨论）
> cmake是允许你的文件集合的名字和类型相同的，所以你可以省略掉`TYPE HEADERS`直接写`FILE_SET HEADERS`
> 这在小型项目或者维护难度不高的情况下是完全没问题的，但更推荐你书写完整格式。
> - `BASE_DIRS`表示你依赖文件的存储位置。
> - `FILES`表示你具体有哪些文件。

MathFunctions.h中的内容。

```cpp
#pragma once

namespace mathfunctions {
double sqrt(double x);
}

```

MathFunctions.cxx中的内容。

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

此时再重新构建cmake和我们的目标文件你会看到库文件也一并生成了。

---
在todo7中我们需要将库链接到我们的可执行文件中去。
`target_link_libraries(Tutorial PRIVATE MathFunctions)`

> - `PRIVATE INTERFACE PUBLIC`这是cmake中的范围关键词。
> - `PRIVATE`只属于当前目标，在被链接时不传递。
> - `INTERFACE`不属于当前目标，在被链接时传递。
> - `PUBLIC`属于当前目标，在被链接时传递。
> 更为详细的介绍请参考官方介绍。

在todo8中我们要把`MathFunctions`的头文件加入`Tutorial.cxx`文件中。
在todo9中我们要调用`MathFunctions.h`中的函数，代替原有的std中的函数。
在todo10中我们要把原本的CML中的关于`Tutorial`的操作移动到`Tutorial`目录下的CML中，注意路径变化。
在todo11中我们需要将子目录中的`CMakeLists.txt`纳入管理或者调用（是的CML还可以使用其他目录中的CML）
我们使用`add_subdirectory(Tutorial)`来将`Tutorial`纳入管理。
在todo12、todo13中我们使用同样的方法将原本CML中关于`MathFunctions`的操作转移，并将其纳入管理。

- 为了防止构建时与原本的构建出现冲突，我们可以使用`--clean-first`参数来清理。

使用`cmake --build build --clean-first`来生成。
以下时所有文件的内容。

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

```javascript
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

```javascript
# TODO10: Move all the Tutorial target commands to this CMakeLists.txt. Ensure
#          that all paths are updated to be relative to this new location.
add_executable(Tutorial)

target_sources(Tutorial PRIVATE Tutorial.cxx)

target_link_libraries(Tutorial PRIVATE MathFunctions)

```

`./MathFunctions`

```javascript
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

- 我将所有已经不需要的操作注释了，如果不喜欢可以自行删除。
