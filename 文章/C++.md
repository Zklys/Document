- **极度要求你有C语言基础，这里只会说明C++对比C语言独特的语法**
- 而且是及其基础不涉及任何高级语法的C++笔记
# 从Hello World开始
依据传统我们仍然从输出Hello World看起。
```cpp
#include <iostream>

int main(){
  std::cout<<"Hello World."<<std::endl;
  return 0;
}
```
`C++`的代码风格与`C`截然不同，最显著的就是标准库的区别。
- C++的标准库是`iostream`而C是`stdio.h`。
在`C++`中可以使用`cout`进行输出每一个输出都是用`<<`进行连接的。
- `cout`和`endl`都是定义在标准库但是声明在标准命名空间中的。
  - `endl`是一个能被`cout`识别的换行符。 
在使用命名空间时我们用`::`表示命名空间的属于关系。
```cpp
using namespace std;//引入std中的所有命名
using std::cout;//全局引入cout的命名
```
还可以使用`using`进行全局的引入。
- 命名空间是为了防止在协同开发时造成的命名冲突。
- 不推荐全局引入，因为`std`中的命名庞杂，可能在开发中与根本不会用到的命名进行冲突。
# 输入
在C++中我们可以使用`cin`进行输出，`cin`也是标准命名空间中的。
```cpp
int a=0;
std::cin>>a;
```
- `cin`使用`>>`链接变量。
`C++`在很大程度上兼容了C语言的语法。所以如果你不想使用`cout`和`cin`，你仍然可以使用`printf`和`scanf`进行输入和输出操作。
# 在C++中使用C的头文件
在C++中，将C语言的头文件进行了特殊处理。
例如标准库`stdio.h`被改为了`cstdio`。
```c
#include <cstdio>
```
通过这种方式来区分C语言头文件和C++的头文件
# auto类型
`auto`关键字是让编译器自动推断类型。
```cpp
auto a=10;
```
在编译时会自动推断为`int`。
但更常见的用法是用于复杂类型的自动推理。
# 命名空间
创建命名空间
```cpp
namespace abc{
  class node{

  };
}
```
这样就算创建了，并且在里面声明了一个类。
```cpp
abc::node
using abc::node
using namespace abc
```
引入方式与标准命名空间并无差别。
- 通过命名空间的形式能够有效避免命名冲突的问题。
# 枚举
```cpp
enum Color { RED, GREEN, BLUE };  // 枚举量默认从0开始
Color c = RED;
```
也可以显式指定值。
```cpp
enum Code {a=10,b,c=1,d};
Code cod=b; 
```
这时，`b=11`、`d=2`。
# C++风格的内存管理
在C++中申请与释放内存使用的是`new`和`delete`。
```cpp
int *p=new int;
*p=10;
delete p;
p=nullptr;
```
`nullptr`是C++中对于空指针的安全类型。
# 引用
C++还允许创建一种名为引用的类型。
```cpp
int a=10;
int &c=a;
```
`c`就是对`a`的引用。
- 引用无独立内存空间，编译器不会为引用单独分配内存，它共享原变量的地址空间
- 引用类型必须与绑定变量类型完全一致：
# 函数重载
有效的函数重载必须满足：
- 同名函数
	- 参数类型不同：`void func(int)`与`void func(double)`
	- 参数数量不同：`void func(int)`与`void func(int, int)`
	- 参数顺序不同：`void func(int, char)`与`void func(char, int)`
# C++类与对象
- 面向对象的四大特性：封装、继承、多态、抽象。
类是现代面向对象编程思维中重要的思想，将一切事物抽象为具体的代码行为。
```cpp
class code{
  public:

  private:

  protected:

};
```
这就是简单的一个类。
- 在类中数据分为3中权限，`public`(公开)，`private`(隐私)，`protected`(保护)。
  - `public`完全开放访问权限。
  - `private`仅类内部和友元可访问。
  - `protected`类内和派生类可访问。
- 在类中不使用权限修饰，则默认为`private`。
## 类的使用
```cpp
#include <iostream>
class code{
  public:
    int age=10;
};

int main(){
  code cd;
  std::cout<<cd.age;
  return 0;
}
```
我们可以使用如上代码来访问类`code`中的公共成员`age`并进行输出。
- 我们也可以在类中定义函数。
```cpp
#include <iostream>
class code{
  public:
    int age=10;
    void code_out(){
      std::cout<<"code_out";
    }
    void code_out1();
};
void code::code_out1(){
  std::cout<<"code_out";
}

int main(){
  code cd;
  std::cout<<cd.age;
  cd.code_out();
  cd.code_out1();
  return 0;
}
```
在类中同样可以类内声明，类外实现。也可以全部都在类内。
## 构造函数与析构函数
在类创建对象时编译器通常会自动为我们创建一些函数，构造函数和析构函数,就是编译器自动为我们创建的函数的其中两个。
- 构造函数在类创建对象时自动调用。**名称与类名相同，没有返回值。**
- 析构函数会在销毁对象时自动调用。**名称由~+类名，没有参数，没有返回值。**
我们可以显式地书写构造和析构函数，在我们写了构造和析构函数之后，编译器通常不会再为我们自动生成函数了。
```cpp
#include <iostream>
class code{
  public:
    void code_out();
    code(int n);
    code();
  private:
    int age=10;
};

int main(){
  code cd;
  cd.code_out();
  code cd2(21);
  cd2.code_out();
  return 0;
}

code::code(int n){
  age=n;
}

code::code(){}  //这个构造函数目前不需要任何行为

void code::code_out(){
  std::cout<<"age = "<<age<<std::endl;
}
```
我们在重写构造函数之后还重载了构造函数，令我们在创建对象的时候直接修改`age`的值。
- 第一个对象我们并没有传入值所以调用的是无参的构造函数。
- 第二个对象我们传入了`21`的值所以在创建的同时对于`cd2`的`age`的值已经被修改为`21`。
```cpp
#include <iostream>
class code{
  public:
    void code_out();
    code(int n);
    code();
    ~code();
  private:
    int age=10;
};

int main(){
  code cd;
  cd.code_out();
  code cd2(21);
  cd2.code_out();
  return 0;
}

code::~code(){}

code::code(int n){
  age=n;
}

code::code(){}

void code::code_out(){
  std::cout<<"age = "<<age<<std::endl;
}
```
这里我们重写了析构函数，但是目前析构函数没有任何的行为。或许你可以让它输出一句语句。
- **析构函数是严格不能有参数的。**
## 类的继承
类是可以继承其他类的数据的，被继承的类被称为父类(或者基类，无论什么名字，理解就好)，继承的类为子类(派生类，名字随意)

| 继承类别 |          基类          |          子类          |
| :--: | :------------------: | :------------------: |
| 公有继承 | `public`，`protected` | `public`,`protected` |
| 私有继承 | `public`，`protected` |      `private`       |
| 保护继承 | `public`，`protected` |     `protected`      |
```cpp
#include <iostream>
class code{
  public:
    void code_out();
    code(int n);
    code();
    ~code();
  private:
    int age=10;
};

class aode:public code{

};

int main(){
  aode ad;
  ad.code_out();
  return 0;
}

code::~code(){}

code::code(int n){
  age=n;
}

code::code(){}

void code::code_out(){
  std::cout<<"age = "<<age<<std::endl;
}
```
在程序中我们声明了类`aode`用公有继承的方式继承了`code`。
- 所以我们使用`aode`创建的对象同样可以调用`code`的公共函数，但是无法直接调用`code`的私有成员`age`。
我们可以使用虚函数来在子类中重写父类中的函数。
```cpp
#include <iostream>
class code{
  public:
    virtual void code_out();
    code(int n);
    code()=default;
    ~code()=default;
  private:
    int age=10;
};

class aode:public code{
  public:
    void code_out();
};

int main(){
  aode ad;
  ad.code_out();
  return 0;
}

void aode::code_out() {
    std::cout<<std::endl;
}

code::code(int n){
  age=n;
}

void code::code_out(){
  std::cout<<"age = "<<age<<std::endl;
}
```
如图我们使用`virtual`将函数`code_out`在`code`中声明为虚函数，同时在`aode`类中重写了`code_out`函数。
- 令函数直接等于`default`就相当于声明此函数为默认不做任何行为。
```cpp
#include <iostream>
class code{
  public:
    virtual void code_out();
    virtual void code_outa()=0;
    code(int n);
    code()=default;
    ~code()=default;
  private:
    int age=10;
};

class aode:public code{
  public:
    void code_out();
    void code_outa();
};

int main(){
  aode ad;
  ad.code_out();
  return 0;
}

void aode::code_outa(){}

void aode::code_out() {
    std::cout<<std::endl;
}

code::code(int n){
  age=n;
}

void code::code_out(){
  std::cout<<"age = "<<age<<std::endl;
}
```
- **未定义的虚函数为纯虚函数，强制子类进行重写覆盖。**
- 至此，我们说完的类的四大特性：封装、继承、多态、抽象。
# 模板
## 函数模版
函数模板是C++泛型编程的核心，允许编写与数据类型无关的通用代码。编译器会根据调用时提供的具体类型自动生成对应函数（称为实例化）。
- 代码复用：避免为不同类型重写相同逻辑。
- 类型安全：强类型检查优于宏定义。
- 灵活性：支持自定义类型（需满足模板操作约束）。
```cpp
template <typename T>  // 模板声明：T为类型占位符
T maxNumber(T a, T b) {
  return (a > b) ? a : b;
}
```
当调用`maxNumber(3, 5)`时，编译器生成`int maxNumber(int, int)`。
- `?:`是C++中的三元比较运算符，
- 若 `a > b` 成立，则返回 `a`，否则返回 `b`
```cpp
template <typename T1, typename T2, ...> 
返回类型 函数名(参数列表) { ... }
```
`typename`或`class`关键字等价。
- 如`template <class T>`
## 类模版
之前我们提到过模版的概念，类也可以使用模版。
```cpp
#include <iostream>
template <class T>
class code{
  public:
    void code_out();
    code(T n);
    code()=default;
    ~code()=default;
  private:
    T age=10;
};

int main(){
  code<int> cd(21);
  cd.code_out();
  return 0;
}
template <class T>
code<T>::code(T n){
  age=n;
}
template <class T>
void code<T>::code_out(){
  std::cout<<"age = "<<age<<std::endl;
}
```
模版类需要在生命对象时传入数据类型。
- 这是泛式编程的主要概念。
# 运算符重载
运算符重载允许开发者为自定义类型（类或结构体）定义运算符的行为，
- 运算符重载本质上是通过特殊函数实现，`返回类型 operator运算符符号(参数列表) { ... }`

运算符重载可以采用成员函数和非成员函数的方式。
- 常见运算符的重载规则

|运算符类型|是否应为成员函数|是否需const|示例签名|
|:-------:|:-------------:|:--------:|:------:|
|算术运算符(+)  |推荐非成员|✅|T operator+(const T&, const T&)|
|比较运算符(==) |任意形式|✅|bool operator==(const T&) const
|赋值运算符（=） |必须成员|❌|T& operator=(const T&)
|复合赋值（+=）  |必须成员|❌|T& operator+=(const T&)
|下标运算符（[]）|必须成员|分情况|T& operator[] (int)(非const版)
|类型转换        |必须成员|✅|operator double() const
|自增/自减（++） |必须成员|❌|T& operator++()(前置)|
|流操作符（<<）  |必须非成员|❌|ostream& operator<<(ostream&, const T&)
- AI整理(因为我懒。)
```cpp
class Vector {
private:
    double x, y;
public:
    Vector(double x, double y) : x(x), y(y) {}
    
    // 成员函数形式：+=
    Vector& operator+=(const Vector& rhs) {
        x += rhs.x;
        y += rhs.y;
        return *this;
    }
    
    // 非成员函数形式：+（需友元访问私有成员）
    friend Vector operator+(const Vector& lhs, const Vector& rhs);
};

Vector operator+(const Vector& lhs, const Vector& rhs) {
    return Vector(lhs.x + rhs.x, lhs.y + rhs.y);
}

// 使用
Vector v1(1, 2), v2(3, 4);
Vector v3 = v1 + v2;  // 调用 operator+
v1 += v2;             // 调用 operator+=
```
- 来看AI示例(因为我不知道怎么写示例，~~理直气壮~~)
在类`Vector`中先创建了两个私有成员`x`和`y`，然后用构造函数的初始化列表来初始化成员。
- 初始化列表就是`Vector(double a, double b) : x(a), y(b)`这样的方式，让成员直接初始化为传入参数的值，`x=a y=b`。
然后重载了运算符`+=`。
- this指针是指向当前对象本身的指针。它本身由编译器隐式生成。
在`v1 += v2;`中，就相当于是`v1`调用函数，将`v2`传入令`const Vector& rhs=v2`就相当于`v1.operator+=(v2)`这样的方式，最后返回`v1`本身。

# 友元
友元是 C++ 中的一种声明机制，它不是类的成员函数，而是通过 friend 关键字授予外部实体访问权限。当一个函数或类被声明为另一个类的友元时，它可以直接读写该类的私有和保护成员，而无需通过公共接口（如 getter/setter 方法）。这打破了类的封装性，但适用于需要高效访问的场景，例如运算符重载或跨类协同工作时。
- 介绍来自AI(我自己实在想不出来友元该怎么介绍了。)
**示例代码是我自己写的**
```cpp
#include <iostream>
class code{
  public:
    void code_out();
    friend void out(code &cod);
  private:
    int age=10;
};

int main(){
  code cd;
  out(cd);
  return 0;
}

void out(code &cod){
  std::cout<<cod.age;
}
void code::code_out(){
  std::cout<<"age = "<<age<<std::endl;
}
```
- 这是友元函数，函数本身仍然是外部函数，能够通过友元特性访问类中的隐私成员。
```cpp
#include <iostream>
class code{
  public:
    void code_out();
    friend void out(code &cod);
    friend class aode;
  private:
    int age=10;
};

class aode{
  public:
    void aode_out(code &cod);
};

int main(){
  code cd;
  out(cd);
  aode ad;
  ad.aode_out(cd);
  return 0;
}

void aode::aode_out(code &cod){
  std::cout<<cod.age;
}

void out(code &cod){
  std::cout<<cod.age;
}
void code::code_out(){
  std::cout<<"age = "<<age<<std::endl;
}
```
- 如上我们还可以创建友元类，以便于在其他类中访问类的都私有参数。
# 异常
```cpp
#include <iostream>
#include <stdexcept>
using namespace std;

void riskyOperation(int value) {
    if (value < 0) {
        throw invalid_argument("Value cannot be negative"); // 抛出标准异常
    }
    if (value > 100) {
        throw runtime_error("Value exceeds limit"); // 抛出不同类型异常
    }
    cout << "Operation successful: " << value << endl;
}

int main() {
    try {
        riskyOperation(-5);  // 触发异常
        riskyOperation(150); // 不会执行到这里
    }
    catch (const invalid_argument& e) {
        cerr << "Invalid argument: " << e.what() << endl;
    }
    catch (const runtime_error& e) {
        cerr << "Runtime error: " << e.what() << endl;
    }
    catch (...) {  // 捕获所有其他异常
        cerr << "Unknown exception occurred" << endl;
    }
    return 0;
}
```

- `throw`在`riskyOperation`中检测到错误时抛出异
- `try`块包含可能抛出异常的代码后续紧跟一个或多个`catch`块
- `catch`块按顺序匹配异常类型`catch (...)`捕获所有未处理的异常
- `what()`方法获取错误信息
# 文件操作
C++主要通过`<fstream>`库实现文件操作
- `ofstream`输出文件流（写操作）
- `ifstream`输入文件流（读操作）
- `fstream`双向文件流（读写操作）
```cpp
fstream file;
file.open("路径/文件名", 打开模式);
```
可以使用如上的方式来打开文件。
打开文件的同时也有很多模式。
- `ios::in`读模式
- `ios::out`写模式（自动创建文件）
- `ios::app`追加模式
- `ios::binary`二进制模式
```cpp
#include <fstream>
#include <string>
using namespace std;

int main() {
    // 写文件
    fstream outFile("data.txt", ios::out);
    outFile << "姓名: 张三\n年龄: 25";  // 写入数据
    outFile.close();

    // 读文件
    fstream inFile("data.txt", ios::in);
    string line;
    while(getline(inFile, line)) {  // 逐行读取
        cout << line << endl;       // 输出: 姓名: 张三 → 年龄: 25
    }
    inFile.close();
    return 0;
}
```
- 也可以使用如`ios::out|ios::in`的方式来同时具备多种代开方式。

ok！C++的基础就到此为止了。我用及其简略的语言叙述了一个非现代化的C++，比如智能指针，RAII，异步，多线程，交叉编译之类的，统统没有说到！！！
- ~~因为我懒~~
- 如果要继续深入，就去看[C++版的数据结构](DataStruct.md)。