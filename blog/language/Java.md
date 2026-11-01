# Java 核心知识点

## 第一部分：Java 程序结构与基础语法

### 1.1 程序结构

- 类定义：`public class 类名 {}`，类名必须与文件名一致。
- 主方法：`public static void main(String[] args) {}`，JVM 启动时调用的唯一入口。
- 输出：`System.out.println()` 换行，`System.out.print()` 不换行。
- 注释：单行 `//`、多行 `/* */`、文档注释 `/** */`。

### 1.2 变量与数据类型

- 基本数据类型 8 种：
  - 整数：`byte`、`short`、`int`（默认）、`long`（`L` 后缀）。
  - 浮点：`float`（`F` 后缀）、`double`（默认）。
  - 字符：`char`，单引号，本质是 Unicode 码。
  - 布尔：`boolean`，只有 `true` / `false`，不能与 0/1 互转。
- 引用数据类型：类（如 `String`）、接口、数组。
- 类型转换：隐式提升（小转大）；强制转换（大转小，可能丢失精度）。
- 标识符：字母、下划线、`$` 开头，不能是关键字；命名用驼峰。

### 1.3 运算符

- 算术 `+ - * / %`：`%` 取余，`+` 可拼接字符串。
- 赋值 `=` 与扩展赋值 `+=`、`-=` 等，扩展赋值隐含强制类型转换。
- 比较 `==`、`!=`：`==` 判等，勿与赋值混淆；逻辑 `&&`（短路与）、`||`（短路或）、`!`（取反）。
- 三元 `条件 ? 值1 : 值2`，简化 if-else 赋值。

### 1.4 流程控制

- 顺序结构：代码自上而下执行。
- `if` / `else if` / `else` 判断区间或复合条件；`switch` / `case` 判断定值，注意 `break` 穿透，Java 14+ 支持 `->` 与 `yield`。
- 循环：`for` 次数明确；`while` 次数不明；`do-while` 先执行后判断，至少执行一次。
- `break` 跳出当前层循环；`continue` 跳过本次进入下次。

## 第二部分：数组与方法

### 2.1 数组

- 静态初始化 `int[] arr = {1, 2, 3};`；动态初始化 `int[] arr = new int[5];`，元素取默认值 0。
- 栈存局部变量和引用变量名（地址值），堆存 `new` 出的对象和数据。
- 基本类型传值，引用类型传地址，经引用修改数据会影响原数组。
- 操作：遍历（普通 for / 增强 for）、求最值、反转、查找。
- 二维数组 `int[][] a = new int[3][4];`，本质是数组的数组。
- `Arrays`：`toString` 打印、`sort` 排序、`copyOf` 复制。

### 2.2 方法

- 五要素：修饰符 + 返回值类型 + 方法名 + 参数列表 + 方法体。
- 调用时实参传形参，基本类型传值，引用类型传地址。
- 重载：同类中方法名相同、参数列表不同（个数、顺序、类型），与返回值类型无关。
- 可变参数 `int... nums`：本质是数组，必须放在参数列表最后。
- 递归：方法自己调用自己，必须设出口，否则抛 `StackOverflowError`。

## 第三部分：面向对象编程

### 3.1 类与对象

- 类是抽象设计，对象是具体实例；属性即成员变量，行为即成员方法。
- `new 类名()` 在堆中开辟空间，把地址返回给栈中的引用变量。

### 3.2 构造方法

- 方法名与类名一致，没有返回值类型。
- 未写构造器时系统提供无参构造；一旦写了带参构造，默认无参构造不再提供，建议手动补上。
- `this` 区分成员变量与局部变量；`this(...)` 调用本类其他构造器，必须在第一行。

### 3.3 三大特征

#### 封装

- `private` 修饰属性，隐藏内部细节。
- 提供公共 `getter` / `setter` 访问，可在方法内做逻辑校验。

#### 继承

- `extends` 继承，单继承可多层；子类拥有父类非 `private` 的属性和方法。
- 重写：方法名、参数、返回值相同，访问权限不能更小，建议加 `@Override`。
- `super()` 调用父类构造器须在第一行；`super` 访问父类被覆盖的成员。

#### 多态

- 三要素：继承 + 重写 + 父类引用指向子类对象，如 `Animal a = new Cat();`。
- 动态绑定：编译时看左边（父类），运行时看右边（子类重写的方法）。
- 向下转型需强制转换，先用 `instanceof` 做安全判断。

### 3.4 抽象类与接口

- `abstract` 类不能实例化，但有构造器供子类调用，用于抽取共性的非完整设计。
- `interface`：JDK 8 前全是抽象方法，JDK 8 增加 `default` 和 `static`，JDK 9 增加 `private`。
- 接口体现规范与能力；类单继承，可 `implements` 多个接口。

### 3.5 特殊关键字

- `static`：类级别，类加载时初始化；静态方法不能直接访问非静态成员，此时还没有对象。
- `final`：类不可继承、方法不可重写、变量变常量（基本类型值不变，引用类型地址不变）。
- `package` / `import`：管理物理路径，解决类名冲突。

## 第四部分：常用核心 API

### 4.1 字符串

- `String` 不可变，依赖字符串常量池；每次拼接都产生新对象，频繁拼接效率极低。
- 方法：`length()`、`charAt()`、`indexOf()`、`substring()`、`trim()`、`split()`、`replace()`、`equals()`；比较内容必须用 `equals()`，不能用 `==`。
- `StringBuilder` 线程不安全、效率高，日常首选；`StringBuffer` 线程安全、效率稍低。

### 4.2 包装类

- 8 种基本类型各有包装类：`int` → `Integer`，`char` → `Character`。
- 自动装箱 `Integer i = 128;`，自动拆箱 `int a = i;`。
- 字符串转基本类型：`Integer.parseInt("123")`、`Double.parseDouble("3.14")`。

### 4.3 时间日期（Java 8+）

- `java.time` 包：`LocalDate` 日期、`LocalTime` 时间、`LocalDateTime` 日期时间。
- `DateTimeFormatter` 负责格式化与解析，线程安全，优于 `SimpleDateFormat`。

### 4.4 Math 与 Random

- `Math.random()` 生成 [0.0, 1.0)。
- `Random.nextInt(n)` 生成 [0, n)。

## 第五部分：集合框架

- 数组长度固定，集合长度可变；`Collection` 是单列集合，`Map` 是双列键值对集合。

| 接口 | 实现类 | 底层结构 | 特点 |
| --- | --- | --- | --- |
| List | `ArrayList` | 数组 | 有序可重复，查询快、增删慢 |
| List | `LinkedList` | 双向链表 | 有序可重复，增删快、查询慢 |
| Set | `HashSet` | 哈希表（数组 + 链表 / 红黑树） | 无序不可重复，靠 `hashCode()` 与 `equals()` 判重 |
| Set | `TreeSet` | 红黑树 | 无序不可重复，自然排序或 `Comparator` 定制排序 |
| Map | `HashMap` | 哈希表 | 键值对，允许 `null` 键值，线程不安全、高效 |
| Map | `LinkedHashMap` | 哈希表 + 链表 | 键值对，保持插入顺序 |
| Map | `Properties` | 哈希表 | 键值对，加载 `.properties` 配置文件 |

- 遍历：普通 `for`（带索引）、增强 `for`（`for (类型 变量 : 集合)`）。
- `Iterator` 遍历时不能直接 `remove`，必须用迭代器自身的 `remove`。
- Lambda：`集合.forEach(System.out::println)`。

## 第六部分：异常处理

### 6.1 异常体系

- `Throwable` 分为 `Error` 与 `Exception`。
- `Error`：JVM 底层严重问题，如 `OutOfMemoryError`、`StackOverflowError`，程序无法处理。
- `RuntimeException` 是非受检异常，由逻辑错误导致，如 `NullPointerException`、`ArrayIndexOutOfBoundsException`、`ArithmeticException`，编译时不强制处理。
- 非 `RuntimeException` 是受检异常，必须显式处理，如 `IOException`、`SQLException`、`ClassNotFoundException`。

### 6.2 处理机制

- `throws` 跟在方法声明后，抛给调用者处理；`throw` 在方法体内手动抛出异常对象。
- `try-catch-finally`：`finally` 无论如何都会执行，常用于关流、关连接。
- `try-with-resources`（Java 7+）：`try()` 中的资源自动调用 `close()`。

## 第七部分：I/O 流与文件操作

### 7.1 File 类

- 代表文件或目录的路径名；操作：判断是否存在、创建与删除文件或目录、获取文件名与大小、列出目录内容。

### 7.2 流的分类

- 按流向：输入流读入内存，输出流写出磁盘。
- 按单位：字节流处理一切数据（图片、视频、音频），字符流只处理纯文本。
- 四大基类：`InputStream` / `OutputStream`（字节），`Reader` / `Writer`（字符）。
- 节点流是基础，处理流包装节点流；缓冲流 `BufferedInputStream` / `BufferedOutputStream`、`BufferedReader` / `BufferedWriter`（`readLine()` 按行读）。
- 序列化：对象写入文件或网络传输；类须实现 `Serializable` 标记接口，显式声明 `serialVersionUID`。
- `ObjectOutputStream` 序列化，`ObjectInputStream` 反序列化。

## 第八部分：多线程基础

- 进程是资源分配单位，线程是 CPU 调度执行的最小单位。
- 创建线程：继承 `Thread` 重写 `run()`；实现 `Runnable` 实现 `run()`（推荐，避免单继承限制）；实现 `Callable` 配合 `FutureTask`，可拿返回值。
- 生命周期：新建 → 就绪 → 运行 → 阻塞 / 等待 / 计时等待 → 终止。
- 线程安全：多线程操作共享数据导致错乱，用 `synchronized` 同步块或同步方法加锁。
- `volatile` 保证可见性和有序性，不保证原子性。

## 第九部分：泛型进阶

### 9.1 泛型类、接口与方法

- 泛型类与泛型接口：`class Box<T> {}`。
- 泛型方法在返回值前必须有 `<T>`；仅使用了泛型参数的方法不算泛型方法。

### 9.2 通配符

- `<?>` 无界：表示未知类型，只能读不能写，`null` 除外。
- `<? extends Number>` 上界：`Number` 或其子类，可读不可写，无法确定具体子类型。
- `<? super Integer>` 下界：`Integer` 或其父类，可写不可读，读出来只能装进 `Object`。
- PECS 原则：生产者用 `extends`，消费者用 `super`。

### 9.3 类型擦除

- 编译期做类型检查，运行时 JVM 并不知道泛型，泛型信息擦除为原始类型。
- 影响：无法 `new T()`、无法对泛型类型用 `instanceof`、泛型类的静态变量是共享的。
- 桥接方法：编译器为保证多态自动生成的方法。

## 第十部分：Lambda 表达式与函数式接口

### 10.1 Lambda 语法

- `(参数列表) -> { 方法体 }`；简写：参数类型可省，单参数可省小括号，单条语句可省 `{}` 和 `return`。

### 10.2 函数式接口

- 只有一个抽象方法的接口，可有 `default` 和 `static` 方法，标注 `@FunctionalInterface`。

| 接口 | 输入输出 | 抽象方法 |
| --- | --- | --- |
| `Function<T, R>` | 输入 T，输出 R | `apply` |
| `Consumer<T>` | 输入 T，无输出 | `accept` |
| `Supplier<T>` | 无输入，输出 T | `get` |
| `Predicate<T>` | 输入 T，输出 boolean | `test` |

- 变种：`BiFunction`、`UnaryOperator`、`IntPredicate` 等。

### 10.3 方法引用

- 四种写法：`类名::静态方法`、`对象::实例方法`、`类名::实例方法`、`类名::new`（构造器引用）。

## 第十一部分：Stream 流式编程

### 11.1 创建 Stream

- 集合：`collection.stream()` 或 `collection.parallelStream()`；数组：`Arrays.stream(array)`。
- 直接创建：`Stream.of(1, 2, 3)`、`Stream.iterate(0, n -> n + 1).limit(10)`。

### 11.2 中间操作（惰性求值）

- 筛选与切片：`filter`、`limit`、`skip`、`distinct`。
- 映射：`map` 一对一；`flatMap` 一对多，把多个流扁平化为一个流。
- 排序：`sorted`，自然排序或定制 `Comparator`。

### 11.3 终止操作（立即执行）

- 匹配与查找：`allMatch`、`anyMatch`、`noneMatch`、`findFirst`、`findAny`。
- 归约：`reduce` 把元素反复结合为单一结果，如求和。
- 收集：`collect` 配合 `Collectors.toList()`、`Collectors.toSet()`、`Collectors.toMap()`。
- 分组与分区：`groupingBy` 按字段分组，支持多级；`partitioningBy` 分 true / false 两组。
- 统计：`summarizingInt` 等，取总数、和、平均、最大、最小。

### 11.4 并行流

- `parallelStream()` 底层用 Fork/Join 分治；注意线程安全，数据量小时反而更慢，需实测。

## 第十二部分：JUC 并发工具包

### 12.1 线程池

- 避免频繁创建销毁线程的开销，统一管理线程资源。
- `Executors` 提供 `newFixedThreadPool`、`newCachedThreadPool`、`newSingleThreadExecutor`、`newScheduledThreadPool`。
- 阿里规约禁用 `Executors`，须手动创建 `ThreadPoolExecutor`，因其队列无界可能 OOM。
- 7 个核心参数：核心线程数、最大线程数、存活时间、时间单位、阻塞队列、线程工厂、拒绝策略。
- 阻塞队列：`ArrayBlockingQueue`、`LinkedBlockingQueue`、`SynchronousQueue`；拒绝策略：`AbortPolicy`、`CallerRunsPolicy`、`DiscardPolicy`、`DiscardOldestPolicy`。

### 12.2 锁机制

- `ReentrantLock` 可重入锁，相比 `synchronized` 可中断、可尝试加锁、可设公平锁、支持多条件绑定。
- `ReentrantReadWriteLock` 读写锁：读读共享、读写互斥、写写互斥，适合读多写少。

### 12.3 并发容器

- `ConcurrentHashMap`：JDK 1.7 用分段锁 `Segment`；JDK 1.8+ 用 CAS + `synchronized`，锁链表头节点或红黑树根。
- 优于 `Hashtable`（全表锁）和 `Collections.synchronizedMap`。
- `CopyOnWriteArrayList`：写时复制，适合读多写少，牺牲一致性保证并发安全。
- `BlockingQueue`：`ArrayBlockingQueue`、`LinkedBlockingQueue`、`SynchronousQueue`，生产者-消费者模式的实现。

### 12.4 同步辅助类

- `CountDownLatch` 倒计时门闩：等待多个线程完成后执行。
- `CyclicBarrier` 循环栅栏：线程互相等待，达到屏障数后一起执行。
- `Semaphore` 信号量：控制同时访问的并发数，用于限流。
- `CompletableFuture`（Java 8+）：回调 `thenApply`、`thenAccept`、`thenRun`；组合 `thenCombine`、`allOf`、`anyOf`。
- 相比 `Future.get()` 的阻塞，`CompletableFuture` 支持非阻塞回调，是异步编程主流。

### 12.5 原子类

- `AtomicInteger`、`AtomicBoolean`、`AtomicReference` 等。
- 底层原理：CAS（比较并交换）+ `volatile` + `Unsafe` 类。
- CAS 三问题：ABA（用 `AtomicStampedReference` 解决）、循环开销大、只保证一个共享变量的原子性。

## 第十三部分：反射与动态代理

### 13.1 反射核心类

- `Class` 三种获取方式：`Class.forName`、`类名.class`、`对象.getClass()`。
- 通过 `Constructor`、`Method`、`Field` 动态创建对象、调用方法、修改属性，包括 `private` 成员。

### 13.2 应用场景

- 运行时加载配置文件中的类，如 JDBC 驱动 `Class.forName("com.mysql.cj.jdbc.Driver")`。
- 通用工具类，如 JSON 序列化与反序列化。
- 破坏封装：跳过编译期检查，可修改 `final` 和 `private` 成员。

### 13.3 动态代理

- JDK 原生：基于接口，`Proxy.newProxyInstance` + `InvocationHandler`。
- CGLIB：基于子类继承，字节码生成子类，无需接口，但不能代理 `final` 类。
- JDK 只能代理接口，CGLIB 可代理普通类；两者是 Spring AOP 的底层实现。

## 第十四部分：注解与自定义注解

### 14.1 元注解

- `@Target`：作用目标，类、方法、字段、参数等。
- `@Retention`：生命周期，`SOURCE` 源码级、`CLASS` 编译期、`RUNTIME` 运行期，框架开发常用 `RUNTIME`。
- `@Documented`、`@Inherited`。

### 14.2 自定义注解与解析

- `@interface` 定义，属性如 `String value() default "";`。
- 运行时解析：结合反射 `getAnnotation`。
- 编译期解析：结合 `AbstractProcessor`，如 Lombok `@Getter`。

## 第十五部分：Java NIO 与网络编程

### 15.1 NIO 三大核心

- `Buffer`：数据容器，如 `ByteBuffer`、`CharBuffer`，属性为 capacity、position、limit。
- `Channel`：传输通道，如 `FileChannel`、`SocketChannel`、`ServerSocketChannel`，双向读写。
- `Selector`：多路复用，单线程管理多个 Channel，对应 `select` / `epoll` 模型。

### 15.2 BIO / NIO / AIO

- BIO：同步阻塞，一个连接一个线程，适合连接少。
- NIO：同步非阻塞，一个线程管理多个连接，适合连接多、数据量小，如聊天服务器。
- AIO：异步非阻塞，基于回调，Java 7+，应用不如 Netty 广泛。

### 15.3 网络编程模型

- BIO：`Socket` / `ServerSocket`；NIO：`SocketChannel` / `ServerSocketChannel` + `Selector`。

## 第十六部分：JVM 内存模型与类加载机制

### 16.1 运行时数据区

- 程序计数器：当前线程的字节码行号，线程私有，唯一不会 OOM 的区域。
- 虚拟机栈：局部变量、操作栈、方法出口，线程私有，可能抛 `StackOverflowError` 或 `OutOfMemoryError`。
- 本地方法栈：为 `native` 方法服务。
- 堆：所有线程共享，存对象实例和数组，分年轻代、老年代、元空间/永久代，GC 主要发生在这里。
- 方法区（元空间 Metaspace）：类元信息、常量、静态变量，JDK 8 后由本地内存实现。

### 16.2 类加载机制

- 加载：把 `.class` 文件读入内存。
- 链接：验证 → 准备（静态变量赋默认值）→ 解析（符号引用转直接引用）。
- 初始化：执行 `<clinit>()`，静态变量赋初始值、执行静态代码块。
- 类加载器：启动类加载器 Bootstrap 加载 `rt.jar`（C++ 实现），扩展类加载器 Extension 加载 `lib/ext/*.jar`，应用程序类加载器 AppClassLoader 加载 classpath 下的类。
- 双亲委派：向上委派查找，向下委托加载，保证核心类库安全，防止自定义 `java.lang.String` 破坏系统。

### 16.3 垃圾回收（GC）

- 可达性分析：以 GC Roots 为起点，如虚拟机栈引用的对象、静态变量引用的对象。
- 四种引用：强引用、软引用（Soft，内存不足时回收）、弱引用（Weak，下次 GC 必回收）、虚引用（Phantom，跟踪回收）。
- 回收器：Serial、Parallel、CMS、G1（JDK 9 默认）、ZGC（低延迟）。

## 第十七部分：常用设计模式

- 创建型：
  - 单例：饿汉式线程安全、懒汉式（双重检查锁 + `volatile`）、静态内部类式（推荐）、枚举式（防反射破坏）。
  - 工厂：简单工厂、工厂方法、抽象工厂，如 `Calendar.getInstance()`。
  - 建造者：链式调用，如 Lombok `@Builder`、`StringBuilder`。
- 结构型：
  - 代理：静态代理与动态代理，AOP 的基础。
  - 装饰者：IO 流，如 `new BufferedInputStream(new FileInputStream(...))`。
  - 适配器：如 `Arrays.asList()`、`InputStreamReader`。
- 行为型：
  - 策略：如 `Comparator`。
  - 模板方法：如 `AbstractList`、Spring `JdbcTemplate`。
  - 观察者：如 Swing 事件监听器、Spring 事件机制。

## 第十八部分：Java 版本新特性（JDK 8 ~ 21）

- JDK 9：模块化 `module-info.java`、`JShell` 交互式工具。
- JDK 10：局部变量类型推断 `var`。
- JDK 11（LTS）：`String` 新增 `isBlank`、`strip`、`repeat`、`lines`；`HttpClient` 标准化。
- JDK 12-13：`switch` 表达式增强（预览），`->` 不用 `break`。
- JDK 14：`switch` 正式版加 `yield`；`Records` 纯数据载体类；`instanceof` 模式匹配（预览）。
- JDK 17（LTS）：`Sealed Classes` 密封类；`Records`、`instanceof` 模式匹配转正。
- JDK 21（LTS）：虚拟线程、结构化并发、顺序集合。
