好的，收到。我们直接跳过JDK安装、环境变量配置和IDE下载等环节，**从编写第一个Java代码的结构开始**进入教学。

以下是调整后的**纯Java入门核心知识点总结（从基础语法起步）**，章节已重新编号，适合直接用于课堂讲义。

---

# 📘 Java入门核心知识点总结（从基础语法开始）

## 教学总目标
- **认知**：理解Java程序的运行流程（编译 → 执行）及面向对象思想。
- **技能**：能熟练使用IDE编写、调试、运行Java程序，能读懂常见报错信息。
- **核心**：扎实掌握语法基础、面向对象三大特征、核心API及异常处理。


## 第一部分：Java程序结构与基础语法（第1-4课时）

> **教学提示**：此阶段需配合大量简单的控制台练习题，让学生形成肌肉记忆。第一节课直接展示一个完整的类结构。

### 1.1 第一个Java程序与基本结构
- **类的定义**：`public class 类名 {}`（类名必须与文件名一致）。
- **主方法（入口）**：`public static void main(String[] args) {}` —— 这是JVM启动时调用的唯一入口。
- **输出语句**：`System.out.println("内容");`（换行）与 `System.out.print("内容");`（不换行）。
- **注释**：单行 `//`、多行 `/* */`、文档注释 `/** */`。

### 1.2 变量与数据类型
- **基本数据类型（8种）**：
  - 整数：`byte`, `short`, `int`（默认）, `long`（注意 `L` 后缀）。
  - 浮点：`float`（注意 `F` 后缀）, `double`（默认）。
  - 字符：`char`（单引号，本质是Unicode码）。
  - 布尔：`boolean`（只有 `true`/`false`，**绝对不能**与0/1互转）。
- **引用数据类型**：类（如String）、接口、数组。
- **类型转换**：自动类型提升（隐式，小转大）与强制类型转换（显式，大转小，可能丢失精度）。
- **标识符与关键字**：命名规则（字母、下划线、美元符开头，不能是关键字）与规范（驼峰命名法）。

### 1.3 运算符
- **算术运算符**：`+ - * / %`（重点讲 `%` 取余，以及 `+` 在字符串拼接中的作用）。
- **赋值运算符**：`=` 与扩展赋值 `+=`、`-=` 等（隐含强制类型转换）。
- **比较运算符**：`==`（极易与赋值混淆，需反复敲打）、`!=`。
- **逻辑运算符**：`&&`（短路与）、`||`（短路或）、`!`（取反）——重点讲短路效果。
- **三元运算符**：`条件 ? 值1 : 值2`（简化 if-else 赋值）。

### 1.4 流程控制（重中之重）
- **顺序结构**：代码自上而下运行。
- **分支结构**：
  - `if` / `else if` / `else`（用于区间或复杂条件判断）。
  - `switch` / `case`（用于定值判断，注意 `break` 穿透问题；Java 14+ 支持 `->` 和 `yield` 写法）。
- **循环结构**：
  - `for`（明确循环次数，如遍历数组）。
  - `while`（不明确循环次数，如读取文件直到结束）。
  - `do-while`（先执行一次再判断，至少执行一次）。
  - 控制关键字：`break`（跳出当前层循环）、`continue`（跳过本次循环，进入下次）。


## 第二部分：数组与方法（第5-8课时）

### 2.1 数组（Array）
- **声明与初始化**：
  - 静态初始化：`int[] arr = {1, 2, 3};`
  - 动态初始化：`int[] arr = new int[5];`（默认值为0）。
- **内存分析（核心难点）**：
  - **栈内存（Stack）**：存储局部变量和引用变量名（保存地址值）。
  - **堆内存（Heap）**：存储实际的对象和数据（new出来的东西）。
  - 画出数组赋值的内存图，解释引用传递与值传递的区别。
- **常见操作**：遍历（普通for / 增强for）、求最值、反转、查找。
- **多维数组**：以二维数组为例（本质是数组的数组，如 `int[][] a = new int[3][4];`）。
- **工具类**：`Arrays` 的常用方法（`toString` 打印、`sort` 排序、`copyOf` 复制）。

### 2.2 方法（Method）
- **方法定义五要素**：修饰符 + 返回值类型 + 方法名 + 参数列表 + 方法体。
- **方法调用**：实参传递给形参（值传递——基本类型传值，引用类型传地址）。
- **方法重载（Overload）**：同一类中，方法名相同，参数列表**必须不同**（个数、顺序、类型），与返回值类型无关。
- **可变参数**：`int... nums`（本质是数组，必须放在参数列表最后）。
- **递归（难点）**：方法自己调用自己。必须设定递归出口，防止 `StackOverflowError`。


## 第三部分：面向对象编程（OOP）—— 核心精髓（第9-16课时）

> **教学提示**：这是Java的灵魂，务必结合现实例子（汽车、学生、银行卡）来讲，让学生从“执行者”思维转变为“设计者”思维。

### 3.1 类与对象
- **类（抽象概念）** vs **对象（具体实例）** —— “汽车设计图”与“真正跑起来的汽车”。
- **属性（成员变量）** 与 **行为（成员方法）**。
- **对象的创建**：`new 类名()` 在堆中开辟空间，并返回地址给栈中的引用变量。

### 3.2 构造方法（Constructor）
- 构造方法名必须与类名一致，**没有返回值类型**。
- 若未写任何构造器，系统默认提供无参构造；若写了带参构造，系统将不再提供默认无参构造（建议手动补上）。
- `this` 关键字：区分成员变量与局部变量；调用本类其他构造器（`this(...)` 必须在第一行）。

### 3.3 面向对象三大特征

#### 封装 (Encapsulation)
- 使用 `private` 修饰属性，隐藏内部细节。
- 提供公共的 `getter` / `setter` 方法供外界访问（可在方法内添加逻辑校验）。

#### 继承 (Inheritance)
- 使用 `extends` 关键字（Java单继承，但可以多层继承）。
- 子类拥有父类非 `private` 的属性和方法。
- **方法重写 (Override)**：子类对父类方法进行重新实现（要求方法名、参数、返回值相同，访问权限不能更小）。建议加 `@Override` 注解。
- `super` 关键字：调用父类构造器（`super()` 必须在第一行）或父类被覆盖的成员。

#### 多态 (Polymorphism)
- **三要素**：继承关系 + 方法重写 + 父类引用指向子类对象（如 `Animal a = new Cat();`）。
- **动态绑定**：编译时看左边（父类），运行时看右边（子类重写的方法）。
- 向下转型（强制转换）与 `instanceof` 关键字（转型前的安全判断）。

### 3.4 抽象类与接口
- **抽象类（`abstract`）**：不能实例化，但有构造器（供子类调用），用于抽取共性的非完整设计。
- **接口（`interface`）**：JDK 8 前全是抽象方法；JDK 8 支持 `default` 和 `static` 方法；JDK 9 支持 `private` 方法。接口体现“规范/能力”。
- **区别对比**：类只能单继承，但可以多实现（`implements` 多个接口）。

### 3.5 特殊关键字深度解析
- `static`：属于类级别（类加载时初始化）。静态方法中不能直接访问非静态成员（因为还没有对象）。
- `final`：修饰类（断子绝孙）、修饰方法（不可重写）、修饰变量（变成常量，基本类型值不可变，引用类型地址不可变）。
- `package` 与 `import`：物理路径管理，解决类名冲突。


## 第四部分：常用核心API（第17-19课时）

### 4.1 字符串（String / StringBuilder / StringBuffer）
- **String 的不可变性（重点）**：字符串常量池机制，每次拼接都会产生新对象，频繁拼接效率极低。
- **String 常用方法**：`length()`, `charAt()`, `indexOf()`, `substring()`, `trim()`, `split()`, `replace()`, `equals()`（**切记**：比较内容必须用 `equals`，不要用 `==`）。
- **可变字符串**：`StringBuilder`（线程不安全，效率高，日常开发首选）与 `StringBuffer`（线程安全，效率稍低）。

### 4.2 包装类（Wrapper Class）
- 八种基本类型对应的包装类（如 `int` -> `Integer`, `char` -> `Character`）。
- **自动装箱/拆箱**：`Integer i = 128;`（装箱） 和 `int a = i;`（拆箱）。
- **字符串转基本类型**：`Integer.parseInt("123")`、`Double.parseDouble("3.14")`。

### 4.3 时间日期（Java 8+ 新版API）
- 推荐使用 `java.time` 包：`LocalDate`（日期）、`LocalTime`（时间）、`LocalDateTime`（日期时间）。
- 格式化与解析：`DateTimeFormatter`（线程安全，比 `SimpleDateFormat` 更优秀）。

### 4.4 Math 与 Random
- `Math.random()` 生成 [0.0, 1.0) 随机数。
- `Random` 类的 `nextInt(n)` 生成 [0, n) 整数。


## 第五部分：集合框架（第20-22课时）

> **教学提示**：对比数组的弊端（长度固定），引出集合。重点讲 **怎么存**、**怎么取**、**选哪个**。

- **Collection（单列集合）**：
  - **List（有序、可重复）**：
    - `ArrayList`：底层数组，查询快、增删慢（适合读多写少）。
    - `LinkedList`：底层双向链表，增删快、查询慢（适合写多读少）。
  - **Set（无序、不可重复）**：
    - `HashSet`：底层哈希表（数组+链表/红黑树），依赖 `hashCode()` 和 `equals()` 保证唯一性。
    - `TreeSet`：底层红黑树，可对元素进行自然排序或定制排序（`Comparator`）。
- **Map（双列集合，键值对）**：
  - `HashMap`：重点中的重点。允许 `null` 键值，线程不安全（高效）。
  - `LinkedHashMap`：保持插入顺序。
  - `Properties`：常用来加载 `.properties` 配置文件。
- **集合遍历方式**：
  1. 普通 `for` 循环（带索引）。
  2. 增强 `for` 循环（`for(类型 变量 : 集合)`）。
  3. 迭代器 `Iterator`（注意遍历时不能直接 `remove`，需用迭代器的 `remove`）。
  4. Lambda 表达式 `forEach`（`集合.forEach(System.out::println)`）。


## 第六部分：异常处理（第23-24课时）

### 6.1 异常体系（Throwable）
- **Error（错误）**：JVM底层严重问题（如 `OutOfMemoryError`、`StackOverflowError`），程序无法处理。
- **Exception（异常）**：
  - **RuntimeException（运行时异常 / 非受检异常）**：程序员逻辑错误导致（如 `NullPointerException`、`ArrayIndexOutOfBoundsException`、`ArithmeticException`），编译时不强制处理。
  - **非RuntimeException（编译时异常 / 受检异常）**：必须显式处理（如 `IOException`、`SQLException`、`ClassNotFoundException`）。

### 6.2 异常处理机制
- **抛出**：
  - `throws`：跟在方法声明后面，表示“此方法可能有问题，调用者处理”。
  - `throw`：在方法体内部手动制造异常对象。
- **捕获**：
  - `try-catch-finally`：`finally` 块无论如何都会执行（常用于关闭IO流、数据库连接）。
  - **最佳实践**：使用 `try-with-resources`（Java 7+），放在 `try()` 括号内的资源会自动调用 `close()`。


## 第七部分：I/O流与文件操作（第25-27课时）

### 7.1 File 类
- 代表文件或目录的路径名。常用操作：判断是否存在、创建/删除文件/目录、获取文件名/大小、列出目录下的所有文件。

### 7.2 流的分类与使用
- **按流向**：输入流（读入内存） vs 输出流（写出到磁盘）。
- **按单位**：字节流（万能，处理图片、视频、音频） vs 字符流（只能处理纯文本）。
- **四大抽象基类**：`InputStream` / `OutputStream`（字节） 和 `Reader` / `Writer`（字符）。
- **节点流（基础）**与**处理流（包装）**：
  - 高效缓冲流：`BufferedInputStream` / `BufferedOutputStream`（字节缓冲） 和 `BufferedReader` / `BufferedWriter`（字符缓冲，可以按行读 `readLine()`）。
- **对象序列化**：将对象写入文件或网络传输。需实现 `Serializable` 接口（标记接口），并显式声明 `serialVersionUID`（版本号）。使用 `ObjectOutputStream` 序列化，`ObjectInputStream` 反序列化。


## 第八部分：多线程基础（入门了解）（第28-29课时）

> **教学提示**：入门阶段掌握“怎么创建线程”和“什么是锁”即可，无需深入JMM和AQS。

- **进程 vs 线程**：进程是资源分配单位，线程是CPU调度执行的最小单位。
- **创建线程的三种方式**：
  1. 继承 `Thread` 类，重写 `run()` 方法。
  2. 实现 `Runnable` 接口，实现 `run()` 方法（**推荐**，因为Java单继承，实现接口更灵活）。
  3. 实现 `Callable` 接口，配合 `FutureTask`（可以获取线程执行结果的返回值）。
- **线程生命周期（状态）**：新建 → 就绪（可运行） → 运行 → 阻塞/等待/计时等待 → 终止。
- **线程安全问题**：多个线程操作共享数据时出现数据错乱。
  - 解决方案：加锁（`synchronized` 同步代码块或同步方法）。
- **简单了解**：`volatile` 关键字保证可见性和有序性（不保证原子性）。

---

# 📘 Java进阶核心知识点总结（教学专用大纲）

## 进阶教学总目标
- **编码能力**：熟练使用 Lambda 与 Stream API 写出简洁高效的代码；能基于 JUC 包解决多线程安全问题。
- **原理剖析**：理解 JVM 内存模型与类加载机制；能阅读常用集合（如 HashMap）的底层源码。
- **设计能力**：掌握常用设计模式，理解 Spring 框架底层的核心思想（IoC、AOP）的实现基础。


## 第一部分：泛型进阶（第1-2课时）

> **教学提示**：入门阶段只学了泛型基本使用（`List<String>`），进阶阶段必须讲透**通配符**和**类型擦除**，这是看懂框架源码的基础。

### 1.1 泛型类、接口与方法
- 回顾泛型类/接口的定义（`class Box<T>{}`）。
- 泛型方法：区分**泛型方法**与**使用了泛型参数的方法**（前者返回值前必须有 `<T>`）。

### 1.2 通配符（Wildcard）—— 核心难点
- **无界通配符**：`<?>`（表示未知类型，只能读不能写，除了 `null`）。
- **上界通配符**：`<? extends Number>`（表示类型是 Number 或其子类，**可读不可写**，因为无法确定具体子类型）。
- **下界通配符**：`<? super Integer>`（表示类型是 Integer 或其父类，**可写不可读**，因为读出来只能装进 `Object`）。
- **PECS 原则（Producer Extends, Consumer Super）**：生产者（提供数据）用 `extends`，消费者（消费数据）用 `super`。这是面试常考点。

### 1.3 类型擦除（Type Erasure）
- **核心概念**：编译期进行类型检查，但运行时 JVM 并不知道泛型（泛型信息被擦除为原始类型 `Raw Type`）。
- 带来的影响：无法 `new T()`、无法 `instanceof` 泛型类型、泛型类中的静态变量是共享的。
- 桥接方法（Bridge Method）：编译器为了保证多态，自动生成的方法。


## 第二部分：Lambda 表达式与函数式接口（第3-4课时）

> **教学提示**：这是 Java 8 最大的变革，学生需要从“面向对象”思维切换为“函数式编程”思维（传递行为，而不是传递对象）。

### 2.1 Lambda 语法
- 标准语法：`(参数列表) -> { 方法体 }`。
- 简写规则：参数类型可省略、单参数可省略小括号、单条语句可省略 `{}` 和 `return`。

### 2.2 函数式接口（`@FunctionalInterface`）
- 定义：**只有一个抽象方法**的接口（可以有 `default` 和 `static` 方法）。
- Java 内置四大核心函数式接口（必须掌握）：
  - **`Function<T, R>`**：输入 T，输出 R（`apply`）。
  - **`Consumer<T>`**：输入 T，无输出（`accept`）。
  - **`Supplier<T>`**：无输入，输出 T（`get`）。
  - **`Predicate<T>`**：输入 T，输出 boolean（`test`）。
- 其他变种：`BiFunction`、`UnaryOperator`、`IntPredicate` 等。

### 2.3 方法引用（Method Reference）
- 语法糖，让 Lambda 更简洁：`类名::静态方法`、`对象::实例方法`、`类名::实例方法`、`类名::new`（构造器引用）。


## 第三部分：Stream 流式编程（第5-7课时）

> **教学提示**：重点讲 **“流水线思想”** —— 数据源 → 中间操作（惰性求值） → 终止操作（立即执行）。必须配合大量数据过滤、分组、统计的练习。

### 3.1 Stream 的创建
- 集合创建：`collection.stream()` 或 `collection.parallelStream()`。
- 数组创建：`Arrays.stream(array)`。
- 直接创建：`Stream.of(1,2,3)`、`Stream.iterate(0, n -> n+1).limit(10)`。

### 3.2 中间操作（Intermediate Operations）—— 惰性的
- **筛选与切片**：`filter`、`limit`、`skip`、`distinct`。
- **映射**：`map`（一对一）、`flatMap`（一对多，将多个流扁平化为一个流）。
- **排序**：`sorted`（自然排序 / 定制 Comparator）。

### 3.3 终止操作（Terminal Operations）—— 触发执行
- **匹配与查找**：`allMatch`、`anyMatch`、`noneMatch`、`findFirst`、`findAny`。
- **归约**：`reduce`（将流元素反复结合，得到单一结果，如求和）。
- **收集**：`collect(Collectors.toList())`、`Collectors.toSet()`、`Collectors.toMap()`。
  - 分组：`Collectors.groupingBy`（按字段分组，支持多级分组）。
  - 分区：`Collectors.partitioningBy`（分为 true/false 两组）。
  - 统计：`summarizingInt` 等（获取总数、和、平均、最大、最小）。

### 3.4 并行流（Parallel Stream）
- `parallelStream()` 利用 Fork/Join 框架底层分治。
- **注意事项**：线程安全问题、性能测试（并不是所有场景都快，数据量小反而慢）。


## 第四部分：并发编程进阶 —— JUC 并发工具包（第8-13课时）

> **教学提示**：入门只学了 `synchronized`。进阶阶段必须全面引入 `java.util.concurrent`（JUC），这是企业面试的绝对重点。

### 4.1 线程池（ThreadPool）
- **为什么要用线程池**：避免频繁创建/销毁线程的开销，统一管理线程资源。
- **创建方式**：
  - `Executors` 工厂类的便捷方法（`newFixedThreadPool`、`newCachedThreadPool`、`newSingleThreadExecutor`、`newScheduledThreadPool`）。
  - **阿里规约强制**：不要用 `Executors`，要手动创建 `ThreadPoolExecutor`（因为前者队列无界可能导致 OOM）。
- **核心参数（7大参数）**：核心线程数、最大线程数、存活时间、时间单位、阻塞队列（`ArrayBlockingQueue`、`LinkedBlockingQueue`、`SynchronousQueue`）、线程工厂、拒绝策略（`AbortPolicy`、`CallerRunsPolicy`、`DiscardPolicy`、`DiscardOldestPolicy`）。

### 4.2 锁机制（Lock）
- `ReentrantLock`（可重入锁）与 `synchronized` 的区别（可中断、可尝试加锁、可设置公平锁、支持多条件绑定）。
- `ReentrantReadWriteLock`（读写锁）：读读共享、读写互斥、写写互斥（适合读多写少）。

### 4.3 并发容器（Concurrent Collections）
- **`ConcurrentHashMap`（重中之重）**：
  - JDK 1.7 分段锁（Segment） vs JDK 1.8+ CAS + `synchronized`（锁住链表头节点/红黑树根）。
  - 对比 `Hashtable`（全表锁）和 `Collections.synchronizedMap`。
- **`CopyOnWriteArrayList`**：写时复制（适合读多写少，牺牲一致性保证并发安全）。
- **`BlockingQueue`（阻塞队列）**：`ArrayBlockingQueue`、`LinkedBlockingQueue`、`SynchronousQueue`（生产者-消费者模式的绝佳实现）。

### 4.4 并发工具类（同步辅助类）
- **`CountDownLatch`（倒计时门闩）**：等待多个线程完成后再执行（如：等待所有玩家准备就绪）。
- **`CyclicBarrier`（循环栅栏）**：多个线程互相等待，达到屏障数后一起执行（如：旅行团等所有人到齐再出发）。
- **`Semaphore`（信号量）**：控制同时访问的并发数（如：限流）。
- **`CompletableFuture`（异步编程）**（Java 8+）：
  - 实现回调式编程（`thenApply`、`thenAccept`、`thenRun`）。
  - 组合多个异步任务（`thenCombine`、`allOf`、`anyOf`）。
  - 相比 `Future.get()` 阻塞，`CompletableFuture` 支持非阻塞回调，是当前异步编程的主流。

### 4.5 原子类（Atomic）
- `AtomicInteger`、`AtomicBoolean`、`AtomicReference` 等。
- 底层原理：**CAS（Compare-And-Swap，比较并交换）** + `volatile` + `Unsafe` 类。
- CAS 的三大问题：ABA 问题（通过 `AtomicStampedReference` 解决）、循环时间长开销大、只能保证一个共享变量原子性。


## 第五部分：反射（Reflection）与动态代理（第14-16课时）

> **教学提示**：反射是 Java 动态性的基石，是 Spring IOC、AOP 的底层原理。讲课时要让学生感受到“框架是如何在不知道具体类的情况下工作的”。

### 5.1 反射的核心类
- `Class` 类：获取类的三种方式（`Class.forName`、`类名.class`、`对象.getClass()`）。
- 通过反射获取构造器（`Constructor`）、方法（`Method`）、字段（`Field`），并动态创建对象、调用方法、修改属性（甚至 `private` 成员）。

### 5.2 反射的应用场景
- 运行时动态加载配置文件中的类（如 JDBC 加载驱动 `Class.forName("com.mysql.cj.jdbc.Driver")`）。
- 通用工具类（JSON 序列化/反序列化框架）。
- **破坏封装性**（跳过编译期检查，可以修改 `final` 和 `private`）。

### 5.3 动态代理（Dynamic Proxy）
- **JDK 原生动态代理**：基于接口（`Proxy.newProxyInstance` + `InvocationHandler`）。
- **CGLIB 动态代理**：基于子类继承（无需接口，通过字节码技术生成子类）。
- **对比**：JDK 代理只能代理接口，CGLIB 能代理普通类（但不能代理 `final` 类）。
- **应用**：Spring AOP（面向切面编程）的底层实现。


## 第六部分：注解（Annotation）与自定义注解（第17课时）

### 6.1 元注解（Meta-Annotation）
- `@Target`（作用目标：类、方法、字段、参数等）。
- `@Retention`（生命周期：源码级 `SOURCE`、编译期 `CLASS`、运行期 `RUNTIME`——**框架开发常用 `RUNTIME`**）。
- `@Documented`、`@Inherited`。

### 6.2 自定义注解与解析
- 定义注解 `@interface` + 属性（`String value() default "";`）。
- **运行时解析**：结合反射（`getAnnotation`）在运行时读取注解信息。
- **编译期解析**：结合 `AbstractProcessor`（注解处理器，如 Lombok 的 `@Getter` 原理）。


## 第七部分：Java NIO 与网络编程（第18-20课时）

> **教学提示**：虽然日常业务多用 Netty，但理解 NIO（New IO / Non-blocking IO）是理解高并发 IO 模型的基础。

### 7.1 NIO 三大核心
- **Buffer（缓冲区）**：数据的容器（`ByteBuffer`、`CharBuffer`），核心属性（capacity、position、limit）。
- **Channel（通道）**：数据传输通道（`FileChannel`、`SocketChannel`、`ServerSocketChannel`），双向读写。
- **Selector（选择器）**：多路复用器，单线程管理多个 Channel（对应操作系统的 `select`/`epoll` 模型）。

### 7.2 阻塞 IO（BIO） vs 非阻塞 IO（NIO） vs 异步 IO（AIO）
- **BIO**：同步阻塞，一个连接一个线程（适合连接数少的场景）。
- **NIO**：同步非阻塞，一个线程管理多个连接（适合连接多、数据量小的场景，如聊天服务器）。
- **AIO**：异步非阻塞，基于回调（Java 7+，但目前应用不如 Netty 广泛）。

### 7.3 网络编程基础回顾与进阶
- 基于 `Socket` / `ServerSocket` 的 BIO 通信模型。
- 基于 `SocketChannel` / `ServerSocketChannel` + `Selector` 的 NIO 模型（简单实现群聊系统）。


## 第八部分：JVM 内存模型与类加载机制（第21-24课时）

> **教学提示**：这是 Java 程序员进阶的“分水岭”，虽然枯燥，但对于排查 OOM、性能调优至关重要。教学时多结合实际例子（如死循环导致栈溢出、大对象导致堆溢出）。

### 8.1 JVM 内存区域（运行时数据区）
- **程序计数器**：当前线程执行的字节码行号（线程私有，唯一不会 OOM 的区域）。
- **虚拟机栈（Stack）**：存储局部变量、操作栈、方法出口（线程私有，`StackOverflowError` / `OutOfMemoryError`）。
- **本地方法栈**：为 `native` 方法服务。
- **堆（Heap）**：**所有线程共享**，存储对象实例和数组。分代（年轻代、老年代、元空间/永久代）。GC 主要发生在这里。
- **方法区（元空间 Metaspace）**：存储类元信息、常量、静态变量（JDK 8 后由本地内存实现）。

### 8.2 类加载机制
- **加载（Loading）**：将 `.class` 文件读入内存。
- **链接（Linking）**：验证（Verify）→ 准备（Prepare，静态变量赋默认值）→ 解析（Resolve，符号引用转直接引用）。
- **初始化（Initialization）**：执行 `<clinit>()`，静态变量赋初始值、执行静态代码块。
- **类加载器（ClassLoader）**：
  - **启动类加载器（Bootstrap）**：加载 `rt.jar`（C++ 实现）。
  - **扩展类加载器（Extension）**：加载 `lib/ext/*.jar`。
  - **应用程序类加载器（AppClassLoader）**：加载 classpath 下的类。
  - **双亲委派模型（Parent Delegation Model）**：向上委派查找，向下委托加载（保证核心类库的安全，防止用户自定义 `java.lang.String` 破坏系统）。

### 8.3 垃圾回收（GC）入门
- **可达性分析算法（根搜索）**：GC Roots（虚拟机栈引用的对象、静态变量引用的对象等）作为起点。
- **四种引用类型**：强引用（`Strong`）、软引用（`Soft`，内存不足时回收）、弱引用（`Weak`，下次 GC 必回收）、虚引用（`Phantom`，跟踪回收）。
- **常用垃圾回收器了解**：Serial、Parallel、CMS、G1（JDK 9 默认）、ZGC（低延迟）。


## 第九部分：常用设计模式（Java 实战角度）（第25-27课时）

> **教学提示**：不要只讲理论，**必须结合 JDK 源码或实际框架场景**讲解，让学生明白“为什么这样设计”。

- **创建型**：
  - **单例模式（Singleton）**：饿汉式（线程安全）、懒汉式（双重检查锁 `DCL` + `volatile`）、静态内部类式（推荐）、枚举式（防反射破坏）。
  - **工厂模式（Factory）**：简单工厂、工厂方法、抽象工厂（结合 `Calendar.getInstance()` 讲解）。
  - **建造者模式（Builder）**：链式调用（结合 Lombok 的 `@Builder` 或 `StringBuilder` 源码）。
- **结构型**：
  - **代理模式（Proxy）**：结合动态代理章节，静态代理 vs 动态代理（AOP 基础）。
  - **装饰者模式（Decorator）**：结合 IO 流（`new BufferedInputStream(new FileInputStream(...))`）。
  - **适配器模式（Adapter）**：结合 `Arrays.asList()` 或 `InputStreamReader`。
- **行为型**：
  - **策略模式（Strategy）**：结合 `Comparator` 接口（排序策略）。
  - **模板方法模式（Template Method）**：结合 `AbstractList` 或 Spring 的 `JdbcTemplate`。
  - **观察者模式（Observer）**：结合 Swing 事件监听器或 Spring 事件机制。


## 第十部分：Java 版本新特性速览（JDK 8 ~ 21）（第28-29课时）

> **教学提示**：现今企业主流已过渡到 Java 11 / 17 / 21，适当补充新语法，让学生不至于进入公司看不懂代码。

- **JDK 9**：模块化（`module-info.java`）、`JShell` 交互式工具。
- **JDK 10**：局部变量类型推断（`var`）。
- **JDK 11（LTS）**：`String` 新增方法（`isBlank`、`strip`、`repeat`、`lines`）、`HttpClient` 标准化。
- **JDK 12-13**：`switch` 表达式增强（预览，`->` 不用 `break`）。
- **JDK 14**：`switch` 正式版 + `yield` 返回值；`Records`（纯数据载体类，替代 Lombok 的部分功能）、`instanceof` 模式匹配（预览）。
- **JDK 17（LTS）**：`Sealed Classes`（密封类，限制子类）、`Records` 和 `instanceof` 正式版。
- **JDK 21（LTS）**：虚拟线程（Virtual Threads，极大提升并发吞吐量）、结构化并发、顺序集合。