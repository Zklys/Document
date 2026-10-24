# Python 完全学习指南：从入门到进阶


## 前言

Python 是一门广泛使用的高级编程语言，因其简洁、易读以及强大的库支持而备受欢迎。它语法简洁如“伪代码”，生态完善能覆盖从后端到数据分析的全场景，就业面广（Web 开发、自动化、AI 应用等都能做）。本文将从零开始，系统性地带你掌握 Python 的全部核心知识。


## 第一部分：Python 基础

### 1.1 环境搭建

学习 Python 的第一步是搭建开发环境：

- **安装 Python**：从 Python 官网下载并安装适合你操作系统的 Python 版本（建议 Python 3.8 或更新版本）
- **选择 IDE 或文本编辑器**：推荐 PyCharm、VSCode，或轻量级的 Sublime Text
- **包管理工具**：熟悉 pip（Python 包管理器）的使用
- **虚拟环境**：学会为不同项目创建独立的虚拟环境（venv），避免依赖冲突

### 1.2 基本语法

#### 变量与数据类型

Python 中的变量不需要声明类型，直接赋值即可。基本数据类型包括：

```python
# 整数
age = 25

# 浮点数
price = 99.99

# 字符串
name = "Python"

# 布尔值
is_valid = True

# 空值
nothing = None
```

#### 运算符

Python 支持丰富的运算符：

```python
# 算术运算符
print(10 + 3)   # 加法
print(10 - 3)   # 减法
print(10 * 3)   # 乘法
print(10 / 3)   # 除法（浮点）
print(10 // 3)  # 整除
print(10 % 3)   # 取余
print(10 ** 3)  # 幂运算

# 比较运算符
print(10 > 3)   # True
print(10 == 3)  # False

# 逻辑运算符
print(True and False)  # False
print(True or False)   # True
print(not True)        # False
```

#### 控制结构

**条件判断（if-elif-else）**：

```python
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "D"

print(f"成绩等级：{grade}")
```

**循环（for 和 while）**：

```python
# for 循环 - 遍历列表
fruits = ["苹果", "香蕉", "橙子"]
for fruit in fruits:
    print(fruit)

# for 循环 - 使用 range
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

# while 循环
count = 0
while count < 5:
    print(count)
    count += 1
```

### 1.3 函数

函数是组织代码的基本单元：

```python
# 定义函数
def greet(name, greeting="你好"):
    """打招呼函数"""
    return f"{greeting}，{name}！"

# 调用函数
print(greet("张三"))           # 你好，张三！
print(greet("李四", "Hello"))  # Hello，李四！

# 可变参数
def sum_all(*args):
    return sum(args)

print(sum_all(1, 2, 3, 4, 5))  # 15

# 关键字参数
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Python", version=3.12)
```

### 1.4 数据结构

Python 提供了丰富的数据结构：

#### 列表（List）

有序、可变的数据集合：

```python
# 创建列表
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]

# 访问元素
print(numbers[0])    # 1
print(numbers[-1])   # 5

# 切片
print(numbers[1:4])  # [2, 3, 4]

# 修改元素
numbers[0] = 10

# 添加元素
numbers.append(6)
numbers.insert(0, 0)

# 删除元素
numbers.remove(3)
popped = numbers.pop()

# 列表推导式
squares = [x**2 for x in range(10)]
```

#### 元组（Tuple）

有序、不可变的数据集合：

```python
# 创建元组
point = (10, 20)
single = (1,)  # 注意逗号

# 解包
x, y = point
print(x, y)  # 10 20

# 元组不可修改
# point[0] = 5  # 会报错
```

#### 字典（Dictionary）

键值对存储：

```python
# 创建字典
person = {
    "name": "张三",
    "age": 25,
    "city": "北京"
}

# 访问
print(person["name"])
print(person.get("email", "未提供"))

# 修改/添加
person["age"] = 26
person["email"] = "zhangsan@example.com"

# 遍历
for key, value in person.items():
    print(f"{key}: {value}")

# 字典推导式
squared = {x: x**2 for x in range(5)}
```

#### 集合（Set）

无序、不重复的元素集合：

```python
# 创建集合
colors = {"red", "green", "blue"}

# 添加元素
colors.add("yellow")

# 删除元素
colors.remove("green")

# 集合运算
set1 = {1, 2, 3}
set2 = {2, 3, 4}

print(set1 & set2)  # 交集 {2, 3}
print(set1 | set2)  # 并集 {1, 2, 3, 4}
print(set1 - set2)  # 差集 {1}
```

### 1.5 字符串处理

Python 的字符串操作非常强大：

```python
text = "  Hello, Python!  "

# 去除空白
print(text.strip())  # "Hello, Python!"

# 大小写转换
print(text.upper())  # "  HELLO, PYTHON!  "
print(text.lower())  # "  hello, python!  "

# 分割与连接
words = text.strip().split(", ")
print(words)  # ["Hello", "Python!"]
joined = "-".join(words)
print(joined)  # "Hello-Python!"

# 格式化
name = "World"
print(f"Hello, {name}!")
print("Hello, {}!".format(name))

# 字符串方法
print(text.startswith("  Hello"))  # True
print(text.endswith("!"))          # True
print(text.find("Python"))         # 9
print(text.replace("Python", "World"))
```

### 1.6 文件操作

文件读写是编程的基本技能：

```python
# 写入文件
with open("example.txt", "w", encoding="utf-8") as f:
    f.write("Hello, Python!\n")
    f.write("这是第二行")

# 读取文件
with open("example.txt", "r", encoding="utf-8") as f:
    content = f.read()
    print(content)

# 逐行读取
with open("example.txt", "r", encoding="utf-8") as f:
    for line in f:
        print(line.strip())

# 追加写入
with open("example.txt", "a", encoding="utf-8") as f:
    f.write("\n这是追加的内容")
```

### 1.7 异常处理

异常处理是程序健壮性的保障：

```python
# 基本异常处理
try:
    num = int(input("请输入一个数字："))
    result = 10 / num
    print(f"结果是：{result}")
except ValueError as e:
    print(f"输入错误：{e}")
except ZeroDivisionError as e:
    print(f"除数不能为零：{e}")
except Exception as e:
    print(f"未知错误：{e}")
else:
    print("没有发生异常")
finally:
    print("无论如何都会执行")

# 自定义异常
class ValidationError(Exception):
    pass

def validate_age(age):
    if age < 0:
        raise ValidationError("年龄不能为负数")
    if age > 150:
        raise ValidationError("年龄超出合理范围")
    return age
```

### 1.8 模块与包

模块和包是代码组织和复用的基础：

```python
# 导入模块
import math
import random
from datetime import datetime
from os import path as os_path

# 使用模块
print(math.sqrt(16))           # 4.0
print(random.randint(1, 100))  # 随机数
print(datetime.now())          # 当前时间

# 创建自己的模块（保存为 mymodule.py）
# def hello(): print("Hello!")
# 然后导入：import mymodule

# __name__ 的用法
if __name__ == "__main__":
    print("作为主程序运行")
```

### 1.9 编码规范

遵循 PEP 8 规范可以提高代码的可读性和可维护性：

- 使用 4 个空格缩进
- 每行不超过 79 个字符
- 函数之间空两行
- 使用有意义的变量名
- 添加适当的注释和文档字符串


## 第二部分：面向对象编程（OOP）

### 2.1 类与对象

类是对象的模板，对象是类的实例：

```python
class Person:
    """人员类"""
    
    # 类属性（所有实例共享）
    species = "智人"
    
    # 构造方法
    def __init__(self, name, age):
        self.name = name          # 实例属性
        self.age = age
        self._private = "私有"    # 约定私有
    
    # 实例方法
    def introduce(self):
        return f"我叫{self.name}，今年{self.age}岁"
    
    # 类方法
    @classmethod
    def from_birth_year(cls, name, birth_year):
        import datetime
        age = datetime.datetime.now().year - birth_year
        return cls(name, age)
    
    # 静态方法
    @staticmethod
    def is_adult(age):
        return age >= 18

# 创建实例
person1 = Person("张三", 25)
person2 = Person.from_birth_year("李四", 2000)

print(person1.introduce())
print(person2.introduce())
print(Person.is_adult(20))  # True
```

### 2.2 继承

继承允许子类复用父类的属性和方法：

```python
# 单继承
class Student(Person):
    def __init__(self, name, age, student_id):
        super().__init__(name, age)
        self.student_id = student_id
    
    def introduce(self):
        return f"{super().introduce()}，学号：{self.student_id}"

student = Student("王五", 20, "2024001")
print(student.introduce())

# 多继承
class Flyable:
    def fly(self):
        return "飞起来了"

class Bird(Flyable, Person):
    pass

bird = Bird("小鸟", 1)
print(bird.fly())
print(bird.introduce())
```

### 2.3 封装与属性

通过私有属性和 property 实现封装：

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.__balance = balance  # 私有属性（名称修饰）
    
    @property
    def balance(self):
        """通过属性访问余额"""
        return self.__balance
    
    @balance.setter
    def balance(self, value):
        if value < 0:
            raise ValueError("余额不能为负数")
        self.__balance = value
    
    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("存款金额必须为正")
        self.__balance += amount
        return self.__balance
    
    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("取款金额必须为正")
        if amount > self.__balance:
            raise ValueError("余额不足")
        self.__balance -= amount
        return self.__balance

account = BankAccount("张三", 1000)
print(account.balance)        # 1000
account.deposit(500)
print(account.balance)        # 1500
# account.__balance = 0       # 无法直接访问
```

### 2.4 特殊方法（魔术方法）

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Vector({self.x}, {self.y})"
    
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        return Vector(self.x - other.x, self.y - other.y)
    
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    def __len__(self):
        return int((self.x**2 + self.y**2) ** 0.5)

v1 = Vector(3, 4)
v2 = Vector(1, 2)
print(v1 + v2)      # Vector(4, 6)
print(v1 == v2)     # False
print(len(v1))      # 5
```


## 第三部分：进阶特性

### 3.1 迭代器与生成器

生成器可以按需生成值，而不是一次性生成整个序列：

```python
# 迭代器
class CountDown:
    def __init__(self, start):
        self.current = start
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current < 0:
            raise StopIteration
        value = self.current
        self.current -= 1
        return value

for num in CountDown(5):
    print(num)  # 5, 4, 3, 2, 1, 0

# 生成器函数（使用 yield）
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()
for _ in range(10):
    print(next(fib))  # 0, 1, 1, 2, 3, 5, 8, 13, 21, 34

# 生成器表达式
squares = (x**2 for x in range(10))
for square in squares:
    print(square)
```

### 3.2 装饰器

装饰器可以在不修改函数本身的情况下增强函数的功能：

```python
import time
from functools import wraps

# 基础装饰器
def timing_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} 执行时间: {end - start:.4f} 秒")
        return result
    return wrapper

@timing_decorator
def slow_function():
    time.sleep(1)
    return "完成"

print(slow_function())

# 带参数的装饰器
def retry(max_retries=3):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_retries):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    print(f"重试 {attempt + 1}/{max_retries}: {e}")
                    if attempt == max_retries - 1:
                        raise
        return wrapper
    return decorator

@retry(max_retries=5)
def unstable_function():
    import random
    if random.random() < 0.7:
        raise ValueError("随机失败")
    return "成功"
```

### 3.3 上下文管理器

上下文管理器用于管理资源（文件、数据库连接等）：

```python
# 使用 with 语句（内置上下文管理器）
with open("example.txt", "w") as f:
    f.write("自动管理资源")

# 自定义上下文管理器（类方式）
class DatabaseConnection:
    def __init__(self, db_name):
        self.db_name = db_name
    
    def __enter__(self):
        print(f"连接到数据库: {self.db_name}")
        self.connection = f"<连接: {self.db_name}>"
        return self.connection
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"关闭数据库连接: {self.db_name}")
        if exc_type:
            print(f"发生异常: {exc_val}")
        return False  # 不抑制异常

with DatabaseConnection("test.db") as conn:
    print(f"使用: {conn}")

# 自定义上下文管理器（contextlib 方式）
from contextlib import contextmanager

@contextmanager
def temp_directory(path):
    import os
    original_dir = os.getcwd()
    os.chdir(path)
    try:
        yield path
    finally:
        os.chdir(original_dir)

with temp_directory("/tmp"):
    print("在临时目录中工作")
```

### 3.4 闭包与高阶函数

```python
# 闭包
def make_multiplier(factor):
    def multiplier(x):
        return x * factor
    return multiplier

double = make_multiplier(2)
triple = make_multiplier(3)

print(double(10))  # 20
print(triple(10))  # 30

# 高阶函数
def apply_operation(func, values):
    return [func(x) for x in values]

numbers = [1, 2, 3, 4, 5]
print(apply_operation(lambda x: x**2, numbers))  # [1, 4, 9, 16, 25]

# 内置高阶函数
print(list(map(lambda x: x*2, numbers)))      # [2, 4, 6, 8, 10]
print(list(filter(lambda x: x > 2, numbers))) # [3, 4, 5]
from functools import reduce
print(reduce(lambda x, y: x * y, numbers))    # 120
```

### 3.5 描述符

描述符用于管理属性的访问行为：

```python
class PositiveNumber:
    def __set_name__(self, owner, name):
        self.name = name
    
    def __get__(self, instance, owner):
        if instance is None:
            return self
        return instance.__dict__.get(self.name, 0)
    
    def __set__(self, instance, value):
        if value < 0:
            raise ValueError(f"{self.name} 必须为正数")
        instance.__dict__[self.name] = value

class Product:
    price = PositiveNumber()
    stock = PositiveNumber()
    
    def __init__(self, price, stock):
        self.price = price
        self.stock = stock

p = Product(100, 50)
# p.price = -10  # 会抛出 ValueError
```

### 3.6 元类

元类是类的类，用于控制类的创建行为：

```python
# 元类的基本使用
class Meta(type):
    def __new__(cls, name, bases, dct):
        # 自动添加一个属性
        dct['created_at'] = '2026-01-01'
        return super().__new__(cls, name, bases, dct)

class MyClass(metaclass=Meta):
    pass

print(MyClass.created_at)  # 2026-01-01

# 元类实现单例模式
class SingletonMeta(type):
    _instances = {}
    
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Singleton(metaclass=SingletonMeta):
    def __init__(self, value):
        self.value = value

s1 = Singleton(1)
s2 = Singleton(2)
print(s1 is s2)     # True
print(s1.value)     # 1
```

### 3.7 函数式编程工具

```python
from functools import partial, lru_cache

# partial - 固定部分参数
def power(base, exponent):
    return base ** exponent

square = partial(power, exponent=2)
cube = partial(power, exponent=3)

print(square(5))  # 25
print(cube(5))    # 125

# lru_cache - 缓存函数结果
@lru_cache(maxsize=128)
def fibonacci_memo(n):
    if n < 2:
        return n
    return fibonacci_memo(n-1) + fibonacci_memo(n-2)

print(fibonacci_memo(40))  # 快速计算
```


## 第四部分：并发与异步编程

### 4.1 多线程（Threading）

多线程适用于 I/O 密集型任务：

```python
import threading
import time

def print_numbers(name, count):
    for i in range(count):
        print(f"{name}: {i}")
        time.sleep(0.1)

# 创建线程
threads = []
for i in range(3):
    t = threading.Thread(target=print_numbers, args=(f"线程{i}", 5))
    threads.append(t)
    t.start()

# 等待所有线程完成
for t in threads:
    t.join()

# 线程锁
lock = threading.Lock()
shared_counter = 0

def increment():
    global shared_counter
    for _ in range(100000):
        with lock:  # 使用锁保护共享资源
            shared_counter += 1

t1 = threading.Thread(target=increment)
t2 = threading.Thread(target=increment)
t1.start()
t2.start()
t1.join()
t2.join()
print(shared_counter)  # 200000
```

### 4.2 多进程（Multiprocessing）

多进程适用于 CPU 密集型任务，可以绕过 GIL：

```python
from multiprocessing import Process, Pool, cpu_count
import time

def cpu_intensive(n):
    """计算密集型任务"""
    result = 0
    for i in range(n):
        result += i ** 2
    return result

# 使用进程池
if __name__ == "__main__":
    numbers = [10**6] * 4
    
    # 单进程
    start = time.time()
    results = [cpu_intensive(n) for n in numbers]
    print(f"单进程: {time.time() - start:.2f}s")
    
    # 多进程（进程池）
    with Pool(processes=cpu_count()) as pool:
        start = time.time()
        results = pool.map(cpu_intensive, numbers)
        print(f"多进程: {time.time() - start:.2f}s")
```

### 4.3 异步编程（AsyncIO）

asyncio 适用于高并发的 I/O 操作：

```python
import asyncio
import aiohttp

# 基本协程
async def say_hello(name, delay):
    await asyncio.sleep(delay)
    print(f"Hello, {name}!")
    return f"Done: {name}"

# 运行单个协程
# asyncio.run(say_hello("World", 1))

# 并发运行多个协程
async def main():
    tasks = [
        say_hello("Alice", 2),
        say_hello("Bob", 1),
        say_hello("Charlie", 3),
    ]
    results = await asyncio.gather(*tasks)
    print(results)

# asyncio.run(main())

# 异步 HTTP 请求示例
async def fetch_url(session, url):
    async with session.get(url) as response:
        return await response.text()

async def fetch_multiple():
    async with aiohttp.ClientSession() as session:
        urls = [
            "https://httpbin.org/get",
            "https://httpbin.org/ip",
        ]
        tasks = [fetch_url(session, url) for url in urls]
        results = await asyncio.gather(*tasks)
        for result in results:
            print(result[:100])  # 只打印前100字符
```

### 4.4 并发工具（concurrent.futures）

提供高层次的线程池和进程池接口：

```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor

def task(n):
    return n ** 2

# 线程池
with ThreadPoolExecutor(max_workers=4) as executor:
    results = list(executor.map(task, range(10)))
    print(results)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# 进程池
with ProcessPoolExecutor(max_workers=4) as executor:
    results = list(executor.map(task, range(10)))
    print(results)
```

### 4.5 GIL 与并发选择

Python 的 GIL（全局解释器锁）限制了同一时间只能有一个线程执行 Python 字节码。选择并发模型的原则：

- **I/O 密集型任务**：多线程或异步 IO
- **CPU 密集型任务**：多进程
- **高并发网络应用**：异步 IO（asyncio）


## 第五部分：测试与调试

### 5.1 单元测试（unittest）

```python
import unittest

def add(a, b):
    return a + b

def divide(a, b):
    if b == 0:
        raise ValueError("除数不能为零")
    return a / b

class TestMathFunctions(unittest.TestCase):
    def test_add(self):
        self.assertEqual(add(2, 3), 5)
        self.assertEqual(add(-1, 1), 0)
        self.assertEqual(add(0, 0), 0)
    
    def test_divide(self):
        self.assertEqual(divide(10, 2), 5)
        self.assertEqual(divide(7, 2), 3.5)
    
    def test_divide_by_zero(self):
        with self.assertRaises(ValueError):
            divide(10, 0)

if __name__ == "__main__":
    unittest.main()
```

### 5.2 pytest

pytest 是更现代化的测试框架：

```python
# test_example.py
import pytest

def multiply(a, b):
    return a * b

def test_multiply():
    assert multiply(2, 3) == 6
    assert multiply(-1, 5) == -5
    assert multiply(0, 100) == 0

# 参数化测试
@pytest.mark.parametrize("a,b,expected", [
    (2, 3, 6),
    (-1, 5, -5),
    (0, 100, 0),
])
def test_multiply_parametrized(a, b, expected):
    assert multiply(a, b) == expected

# 运行: pytest test_example.py -v
```

### 5.3 调试技巧

```python
import pdb
import logging

# 使用 pdb 调试
def buggy_function(x):
    pdb.set_trace()  # 在这里设置断点
    result = x / 0  # 会报错
    return result

# 使用 logging
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)

logger.debug("调试信息")
logger.info("一般信息")
logger.warning("警告信息")
logger.error("错误信息")
```


## 第六部分：性能优化

### 6.1 性能分析（Profiling）

```python
import cProfile
import pstats

def slow_function():
    total = 0
    for i in range(10000):
        total += sum(range(100))
    return total

# 使用 cProfile 分析
def profile_function():
    profiler = cProfile.Profile()
    profiler.enable()
    slow_function()
    profiler.disable()
    
    stats = pstats.Stats(profiler)
    stats.sort_stats('cumtime')
    stats.print_stats(10)

# 使用 timeit 测量执行时间
import timeit
print(timeit.timeit('slow_function()', globals=globals(), number=10))
```

### 6.2 代码优化技巧

```python
# 1. 使用局部变量
def optimized_sum(data):
    local_sum = 0
    local_append = local_sum  # 错误示例
    # 正确：直接使用 sum()
    return sum(data)

# 2. 列表推导 vs 循环
# 慢速
squares = []
for i in range(1000):
    squares.append(i**2)

# 快速
squares = [i**2 for i in range(1000)]

# 3. 使用 set 进行快速查找
# 慢速 (O(n))
items = [1, 2, 3, 4, 5]
if 3 in items:  # O(n)
    pass

# 快速 (O(1))
items_set = {1, 2, 3, 4, 5}
if 3 in items_set:  # O(1)
    pass

# 4. 字符串拼接
# 慢速
result = ""
for s in ["a", "b", "c"]:
    result += s  # 每次创建新字符串

# 快速
result = "".join(["a", "b", "c"])
```

### 6.3 内存优化

```python
import sys
from array import array

# 使用生成器代替列表（节省内存）
def read_large_file(filename):
    with open(filename, 'r') as f:
        for line in f:
            yield line

# 使用 array 存储数值
# 列表: 每个元素是 Python 对象，内存开销大
list_data = [1, 2, 3, 4, 5]
print(sys.getsizeof(list_data))  # 较大

# array: 存储原始数据类型，内存高效
array_data = array('i', [1, 2, 3, 4, 5])
print(sys.getsizeof(array_data))  # 较小

# 使用 __slots__ 减少对象内存
class Person:
    __slots__ = ['name', 'age']
    def __init__(self, name, age):
        self.name = name
        self.age = age
```


## 第七部分：Python 生态与应用

### 7.1 Web 开发

Python 主流的 Web 框架：

- **Django**：全功能框架，适合大型项目
- **Flask**：轻量级框架，灵活易用
- **FastAPI**：高性能异步框架，支持自动生成 API 文档

```python
# FastAPI 示例
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello, World!"}

@app.get("/users/{user_id}")
def read_user(user_id: int):
    return {"user_id": user_id, "name": f"User {user_id}"}
```

### 7.2 数据分析与科学计算

核心库：

- **NumPy**：数值计算
- **Pandas**：数据处理与分析
- **Matplotlib**：数据可视化
- **SciPy**：科学计算

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# NumPy
arr = np.array([1, 2, 3, 4, 5])
print(arr.mean())  # 3.0

# Pandas
df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35],
    'salary': [50000, 60000, 70000]
})
print(df.describe())

# Matplotlib
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.title("Square Function")
plt.show()
```

### 7.3 机器学习与人工智能

核心库：

- **scikit-learn**：传统机器学习
- **TensorFlow** / **PyTorch**：深度学习

```python
from sklearn.linear_model import LinearRegression
import numpy as np

# 简单线性回归
X = np.array([[1], [2], [3], [4]])
y = np.array([2, 4, 6, 8])

model = LinearRegression()
model.fit(X, y)

print(model.predict([[5]]))  # 预测值接近 10
```

### 7.4 自动化与脚本

Python 是自动化任务的利器：

```python
import os
import shutil
from pathlib import Path

# 文件和目录操作
def organize_files(directory):
    """按扩展名整理文件"""
    for file in Path(directory).iterdir():
        if file.is_file():
            ext = file.suffix[1:] or "no_extension"
            ext_dir = Path(directory) / ext
            ext_dir.mkdir(exist_ok=True)
            shutil.move(str(file), str(ext_dir / file.name))

# 定时任务
import schedule
import time

def job():
    print("执行定时任务")

schedule.every().day.at("10:30").do(job)
schedule.every(1).hours.do(job)

while True:
    schedule.run_pending()
    time.sleep(1)
```

### 7.5 网络爬虫

```python
import requests
from bs4 import BeautifulSoup

def scrape_website(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    
    # 提取标题
    title = soup.title.text if soup.title else "无标题"
    
    # 提取所有段落
    paragraphs = [p.text for p in soup.find_all('p')]
    
    return {
        'title': title,
        'paragraphs': paragraphs[:5]  # 只返回前5段
    }

# result = scrape_website("https://example.com")
```


## 第八部分：最佳实践与设计模式

### 8.1 代码质量

遵循以下原则编写高质量代码：

- **PEP 8 规范**：一致的代码风格
- **类型提示**：使用 type hints 提高代码可读性
- **文档字符串**：为函数、类、模块编写文档
- **代码复用**：DRY（Don't Repeat Yourself）原则
- **高内聚低耦合**：模块化设计

```python
from typing import List, Optional, Dict

def process_data(data: List[int], multiplier: Optional[float] = None) -> Dict[str, float]:
    """
    处理数据列表，计算统计信息。
    
    Args:
        data: 整数列表
        multiplier: 可选乘数
    
    Returns:
        包含统计信息的字典
    """
    if not data:
        return {"mean": 0.0, "max": 0.0, "min": 0.0}
    
    if multiplier:
        data = [x * multiplier for x in data]
    
    return {
        "mean": sum(data) / len(data),
        "max": max(data),
        "min": min(data)
    }
```

### 8.2 常用设计模式

```python
# 单例模式（使用元类实现见 3.6 节）

# 工厂模式
class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "汪汪"

class Cat(Animal):
    def speak(self):
        return "喵喵"

class AnimalFactory:
    @staticmethod
    def create(animal_type: str) -> Animal:
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
        raise ValueError(f"未知动物类型: {animal_type}")

# 策略模式
from typing import Protocol

class SortStrategy(Protocol):
    def sort(self, data: List[int]) -> List[int]: ...

class BubbleSort:
    def sort(self, data: List[int]) -> List[int]:
        # 冒泡排序实现
        return sorted(data)  # 简化示例

class QuickSort:
    def sort(self, data: List[int]) -> List[int]:
        return sorted(data)  # 简化示例

class Sorter:
    def __init__(self, strategy: SortStrategy):
        self.strategy = strategy
    
    def sort(self, data: List[int]) -> List[int]:
        return self.strategy.sort(data)
```

