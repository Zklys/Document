# Gradle 构建工具：从入门到进阶完全指南
## 1. 前言：为什么选择 Gradle

在 Java 生态中，Apache Ant 和 Apache Maven 曾是主流的构建工具，但它们逐渐暴露出一些问题：Ant 灵活性虽高但缺乏依赖管理，需要手动维护所有 jar 包及其位置；Maven 采用“约定优于配置”的理念，使用 XML 编写 pom.xml，但配置冗长繁琐，构建过程相对僵化，缺乏并行执行能力。

Gradle 的出现彻底改变了这一局面。它是一个基于 JVM 的开源自动化构建工具，结合了 Ant 的灵活性和 Maven 的依赖管理功能。Gradle 的主要优势包括：

- **声明式与命令式结合**：既可以通过 DSL 简洁定义构建逻辑，也能用代码实现复杂逻辑
- **依赖管理强大**：支持动态版本、传递性依赖和冲突解决
- **性能卓越**：增量构建、构建缓存、并行执行，大幅提升大型项目的构建速度
- **多语言支持**：不仅限于 Java，还能构建 Android、Kotlin、C++、Swift 等项目
- **灵活的插件机制**：丰富的官方和社区插件，也可自定义插件
- **多项目构建**：支持模块化开发和代码重用

如今，Gradle 不仅是 Android 的官方构建系统，越来越多的 Java 项目（如 Spring Boot）也迁移到了 Gradle。


## 2. Gradle 入门

### 2.1 什么是 Gradle

简单来说，Gradle 就是一个运行在 JVM 上的自动化的项目构建工具，用来帮助我们自动完成编译、测试、打包、发布等构建任务。对于开发者来说，Gradle 的主要作用有三个：

1. **项目构建**：提供标准的、跨平台的自动化项目构建方式
2. **依赖管理**：方便快捷地管理项目依赖的资源（jar 包），避免版本冲突
3. **统一开发结构**：提供标准的、统一的项目结构

Gradle 构建脚本使用 Groovy 或 Kotlin 语言编写，表达能力非常强，也足够灵活。

### 2.2 Gradle 的安装

**方式一：手动安装**

1. 从 [gradle.org](https://gradle.org) 下载最新版本的 Gradle 二进制包
2. 解压到指定目录（如 `/opt/gradle`）
3. 配置环境变量：
   ```bash
   export GRADLE_HOME=/opt/gradle
   export PATH=$PATH:$GRADLE_HOME/bin
   ```
4. 验证安装：`gradle -v`

**方式二：使用包管理器**

- macOS: `brew install gradle`
- Ubuntu/Debian: `sudo apt install gradle`
- Windows: 使用 Chocolatey `choco install gradle`

### 2.3 Gradle Wrapper

Gradle 官方推荐使用 Gradle Wrapper（包装器）来执行任何 Gradle 构建。Wrapper 是一个脚本，它会自动下载并使用指定版本的 Gradle，确保所有开发者和 CI 环境使用相同的 Gradle 版本，避免版本不一致的问题。

使用 `gradle wrapper` 命令可以生成 Wrapper 文件：
```bash
gradle wrapper --gradle-version 8.5
```

生成的文件包括：
- `gradlew`（Unix/Linux/macOS 可执行脚本）
- `gradlew.bat`（Windows 可执行脚本）
- `gradle/wrapper/gradle-wrapper.jar`
- `gradle/wrapper/gradle-wrapper.properties`（指定 Gradle 版本和下载地址）

此后，团队所有成员都应使用 `./gradlew`（Unix）或 `gradlew`（Windows）来代替直接使用 `gradle` 命令。

### 2.4 第一个 Gradle 项目

使用 `gradle init` 命令可以快速生成项目骨架：

```bash
mkdir my-project && cd my-project
gradle init --type java-application
```

生成的项目结构如下：
```
my-project/
├── build.gradle          # 主构建脚本
├── settings.gradle       # 项目配置
├── gradle/
│   └── wrapper/
├── gradlew
├── gradlew.bat
└── src/
    ├── main/java/        # 主代码
    └── test/java/        # 测试代码
```


## 3. Gradle 核心概念

### 3.1 项目（Project）

在 Gradle 中，**项目（Project）** 代表一个需要构建的单元，可以是一个 JAR 包、一个 Web 应用或一个 Android APK。一个 Gradle 构建可以由一个或多个项目组成。每个项目都有一个 `build.gradle` 构建脚本。

### 3.2 任务（Task）

**任务（Task）** 是 Gradle 的核心概念，每个 Task 代表一个构建步骤，如编译、打包、测试等。Task 之间可以通过 `dependsOn` 定义依赖关系，形成有向无环图（DAG），确保构建顺序正确。

### 3.3 构建脚本（Build Script）

构建脚本用于定义项目如何构建。每个项目都有一个 `build.gradle(.kts)` 文件，用于应用插件、声明依赖、配置任务等。

### 3.4 插件（Plugin）

插件是组织构建逻辑并在项目间重用构建逻辑的主要方法。将插件应用到项目会执行代码，这些代码可以创建任务、配置属性，并以其他方式扩展项目的功能。

### 3.5 构建生命周期

Gradle 构建的执行包含三个阶段：

1. **初始化（Initialization）**：发现项目结构，解析 `settings.gradle` 文件
2. **配置（Configuration）**：执行构建脚本，构建任务图
3. **执行（Execution）**：按照任务依赖关系执行任务


## 4. Gradle 构建脚本详解

### 4.1 Groovy DSL vs Kotlin DSL

Gradle 支持两种 DSL（领域特定语言）来编写构建脚本：

| 特性 | Groovy DSL | Kotlin DSL |
|------|------------|------------|
| 文件扩展名 | `.gradle` | `.gradle.kts` |
| 语法 | 动态类型，灵活 | 静态类型，更安全 |
| IDE 支持 | 基础补全 | 智能提示、代码补全、重构 |
| 学习曲线 | 较低 | 稍高（需 Kotlin 基础） |

Kotlin DSL 具有语法突出显示、代码补全和声明导航功能，可提供更好的编辑体验。从 Android Studio Giraffe 开始，新项目默认使用 Kotlin DSL。

**Groovy DSL 示例**：
```groovy
plugins {
    id 'java'
}

group = 'com.example'
version = '1.0.0'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.apache.commons:commons-lang3:3.12.0'
}
```

**Kotlin DSL 示例**：
```kotlin
plugins {
    java
}

group = "com.example"
version = "1.0.0"

repositories {
    mavenCentral()
}

dependencies {
    implementation("org.apache.commons:commons-lang3:3.12.0")
}
```

### 4.2 settings.gradle 文件

`settings.gradle(.kts)` 文件用于定义多项目构建的结构，包括哪些子项目是其一部分。对于单项目构建，该文件可以保持简单：

```kotlin
// settings.gradle.kts
rootProject.name = "my-project"
```

### 4.3 build.gradle 文件

`build.gradle(.kts)` 文件是项目的核心构建脚本，包含：

```kotlin
// 应用插件
plugins {
    java
    application
}

// 项目元信息
group = "com.example"
version = "1.0.0"

// 仓库配置
repositories {
    mavenCentral()
}

// 依赖声明
dependencies {
    implementation("org.apache.commons:commons-lang3:3.12.0")
    testImplementation("org.junit.jupiter:junit-jupiter:5.10.0")
}

// 任务配置
tasks.test {
    useJUnitPlatform()
}
```

### 4.4 常用配置

- **sourceCompatibility / targetCompatibility**：设置 Java 版本
- **application**：配置可执行应用的主类
- **java**：配置 Java 编译选项


## 5. 依赖管理

依赖管理是构建工具最重要的特性之一。Gradle 对依赖管理提供了出色的支持，只需在构建文件中编写几行代码，Gradle 就会在内部完成所有繁重的配置管理工作。

### 5.1 仓库配置

Gradle 支持多种类型的仓库：

```kotlin
repositories {
    mavenCentral()           // Maven 中央仓库
    mavenLocal()             // 本地 Maven 仓库
    google()                 // Google 仓库（Android）
    jcenter()                // JCenter 仓库（已废弃）
    maven { url = uri("https://repo.example.com") }  // 自定义仓库
    ivy { url = uri("https://repo.example.com") }    // Ivy 仓库
    flatDir { dirs("libs") } // 本地目录
}
```

### 5.2 依赖配置类型

Gradle 提供了多种依赖配置来管理不同范围的依赖：

| 配置类型 | 说明 |
|----------|------|
| `implementation` | 编译时依赖，不会暴露给模块的消费者，有助于减少编译依赖泄漏 |
| `api` | 编译时依赖，会暴露给模块的消费者（传递性依赖） |
| `compileOnly` | 仅编译时需要，运行时不需要（如容器提供的 API） |
| `runtimeOnly` | 仅运行时需要，编译时不需要（如 JDBC 驱动） |
| `testImplementation` | 测试编译时依赖 |
| `testRuntimeOnly` | 测试运行时依赖 |

**`implementation` vs `api` 的区别**：
- `api` 依赖具有传递性：模块 A 依赖于 B，B 通过 `api` 依赖于 C，则 A 也能访问 C
- `implementation` 依赖不具有传递性

### 5.3 依赖声明

**外部模块依赖**：
```kotlin
dependencies {
    implementation("org.springframework:spring-core:6.0.0")
    implementation("com.google.guava:guava:32.0.0-jre")
}
```

**项目依赖**（多项目构建中）：
```kotlin
dependencies {
    implementation(project(":core"))
    implementation(project(":util"))
}
```

**文件依赖**：
```kotlin
dependencies {
    implementation(fileTree("libs") { include("*.jar") })
    implementation(files("libs/my-library.jar"))
}
```

### 5.4 版本目录（Version Catalog）

Gradle 6.8 引入了版本目录（Version Catalog）功能，允许在 `libs.versions.toml` 文件中集中管理所有第三方依赖的版本信息。

在根项目的 `gradle/` 目录下创建 `libs.versions.toml` 文件：

```toml
[versions]
spring = "6.0.0"
guava = "32.0.0-jre"
junit = "5.10.0"

[libraries]
spring-core = { group = "org.springframework", name = "spring-core", version.ref = "spring" }
guava = { group = "com.google.guava", name = "guava", version.ref = "guava" }
junit-jupiter = { group = "org.junit.jupiter", name = "junit-jupiter", version.ref = "junit" }

[bundles]
spring-bundle = ["spring-core"]
```

在构建脚本中使用：
```kotlin
dependencies {
    implementation(libs.spring.core)
    implementation(libs.guava)
    testImplementation(libs.junit.jupiter)
}
```

### 5.5 依赖冲突解决

Gradle 提供了多种机制来解决依赖冲突：

1. **自动版本选择**：Gradle 默认选择最高版本
2. **排除特定依赖**：
   ```kotlin
   implementation("com.example:library:1.0.0") {
       exclude(group = "org.unwanted", module = "unwanted-module")
   }
   ```
3. **强制使用特定版本**：
   ```kotlin
   configurations.all {
       resolutionStrategy {
           force("com.google.guava:guava:32.0.0-jre")
       }
   }
   ```
4. **依赖约束**：
   ```kotlin
   dependencies {
       constraints {
           implementation("com.google.guava:guava:32.0.0-jre")
       }
   }
   ```


## 6. 任务（Task）深入

### 6.1 任务定义与配置

**定义任务**：
```kotlin
// Kotlin DSL
tasks.register("hello") {
    doLast {
        println("Hello, Gradle!")
    }
}

// 或使用 tasks.create（立即创建，不延迟）
tasks.create("hello") {
    doLast {
        println("Hello, Gradle!")
    }
}
```

**配置已有任务**：
```kotlin
tasks.jar {
    archiveFileName.set("my-app.jar")
    manifest {
        attributes["Main-Class"] = "com.example.Main"
    }
}
```

### 6.2 任务依赖

```kotlin
tasks.register("taskA") {
    doLast { println("Task A") }
}

tasks.register("taskB") {
    dependsOn("taskA")
    doLast { println("Task B") }
}

// 运行 gradle taskB 会先执行 taskA，再执行 taskB
```

### 6.3 任务类型

Gradle 提供了许多内置任务类型，包括：

- `Copy`：复制文件
- `Jar`：创建 JAR 文件
- `Zip`：创建 ZIP 文件
- `Delete`：删除文件
- `Exec`：执行外部命令
- `JavaCompile`：编译 Java 源代码
- `Test`：执行测试

```kotlin
tasks.register<Copy>("copyResources") {
    from("src/main/resources")
    into("build/resources")
}

tasks.register<Jar>("customJar") {
    archiveFileName.set("custom.jar")
    from(sourceSets.main.get().output)
}
```

### 6.4 自定义任务

当内置任务类型无法满足需求时，可以创建自定义任务类型：

```kotlin
abstract class GreetingTask : DefaultTask() {
    @get:Input
    abstract val message: Property<String>

    @TaskAction
    fun greet() {
        println(message.get())
    }
}

// 注册自定义任务
tasks.register<GreetingTask>("greet") {
    message.set("Hello from custom task!")
}
```

开发自定义任务时，建议使用 `@Input`、`@OutputFile`、`@OutputDirectory` 等注解标记输入和输出，以便自动受益于增量构建。

### 6.5 增量构建

Gradle 的增量构建功能会检查任务的输入和输出是否有变化，如果都没有变化，则跳过该任务。要实现增量构建，任务需要正确定义输入和输出：

```kotlin
abstract class IncrementalTask : DefaultTask() {
    @get:InputDirectory
    abstract val inputDir: DirectoryProperty

    @get:OutputDirectory
    abstract val outputDir: DirectoryProperty

    @TaskAction
    fun process() {
        // 只有输入或输出发生变化时才会执行
    }
}
```


## 7. 插件系统

### 7.1 插件的作用

插件是组织构建逻辑并在项目间重用构建逻辑的主要方法。插件可以：
- 创建任务
- 配置项目属性
- 添加依赖
- 扩展 Gradle 功能

### 7.2 应用插件

**使用 plugins DSL（推荐）** ：
```kotlin
plugins {
    id("java")
    id("application")
    id("org.springframework.boot") version "3.0.0"
}
```

**使用 apply 方法（旧方式）** ：
```kotlin
apply(plugin = "java")
```

### 7.3 常用核心插件

| 插件 ID | 说明 |
|---------|------|
| `java` | Java 项目构建（编译、测试、打包） |
| `java-library` | Java 库项目（支持 `api` 配置） |
| `application` | 可执行 Java 应用 |
| `war` | Web 应用打包 |
| `groovy` | Groovy 项目 |
| `scala` | Scala 项目 |
| `checkstyle` | 代码风格检查 |
| `pmd` | 代码静态分析 |
| `jacoco` | 代码覆盖率报告 |

### 7.4 自定义插件开发

如果 Gradle 或社区没有提供项目所需的特定功能，创建自定义插件是一个解决方案。

**步骤一：创建 Plugin 类**：

```kotlin
abstract class SamplePlugin : Plugin<Project> {
    override fun apply(project: Project) {
        project.tasks.register("customTask") {
            doLast {
                println("Hello from custom plugin!")
            }
        }
    }
}
```

**步骤二：应用插件**：
```kotlin
// build.gradle.kts
apply<SamplePlugin>()
```

**开发插件的最佳实践**是创建**约定插件（Convention Plugin）** 或**二进制插件**，而不是脚本插件。约定插件将通用的构建逻辑封装起来，便于在多项目构建中共享。


## 8. 多项目构建

随着项目的发展，通常会将其拆分为更小、更集中的模块。Gradle 通过多项目构建支持这一点。

### 8.1 多项目结构

一个多项目构建由一个**根项目**和一个或多个**子项目**组成：

```
my-project/
├── settings.gradle.kts          # 声明子项目
├── build.gradle.kts             # 根项目构建逻辑（可选）
├── app/
│   └── build.gradle.kts         # 应用模块
├── core/
│   └── build.gradle.kts         # 核心逻辑
└── util/
    └── build.gradle.kts         # 工具代码
```

### 8.2 settings.gradle 配置

在 `settings.gradle(.kts)` 中使用 `include()` 声明子项目：

```kotlin
rootProject.name = "my-project"
include("app", "core", "util")
```

### 8.3 子项目配置

每个子项目都有自己的 `build.gradle(.kts)` 文件。可以使用根项目的构建脚本统一配置所有子项目：

```kotlin
// 根项目的 build.gradle.kts
subprojects {
    apply(plugin = "java")
    
    repositories {
        mavenCentral()
    }
    
    dependencies {
        testImplementation("org.junit.jupiter:junit-jupiter:5.10.0")
    }
}
```

### 8.4 项目间依赖

子项目之间可以相互依赖：

```kotlin
// app/build.gradle.kts
dependencies {
    implementation(project(":core"))
    implementation(project(":util"))
}
```

### 8.5 buildSrc 与约定插件

`buildSrc` 是 Gradle 项目根目录下的一个特殊目录，包含所有构建逻辑。Gradle 会自动识别 `buildSrc` 目录：

```
my-project/
├── buildSrc/
│   ├── build.gradle.kts
│   └── src/main/kotlin/
│       └── shared-build-conventions.gradle.kts
├── settings.gradle.kts
├── app/
│   └── build.gradle.kts
└── core/
    └── build.gradle.kts
```

`buildSrc` 是定义和维护共享配置或命令式构建逻辑（如自定义任务或插件）的好地方。


## 9. Gradle 命令行

### 9.1 基本命令

命令行的基本结构：
```bash
gradle [taskName...] [--option-name...]
```

**常用命令**：
| 命令 | 说明 |
|------|------|
| `gradle tasks` | 列出所有可用任务 |
| `gradle build` | 构建项目（编译、测试、打包） |
| `gradle clean` | 清理构建输出 |
| `gradle test` | 执行测试 |
| `gradle jar` | 打包 JAR 文件 |
| `gradle run` | 运行应用（application 插件） |
| `gradle -v` / `gradle --version` | 显示版本信息 |

### 9.2 常用选项

| 选项 | 说明 |
|------|------|
| `--parallel` | 启用并行执行 |
| `--build-cache` | 启用构建缓存 |
| `--no-build-cache` | 禁用构建缓存 |
| `--configuration-cache` | 启用配置缓存 |
| `--no-daemon` | 不使用守护进程 |
| `--daemon` | 使用守护进程 |
| `--info` / `--debug` | 输出更详细的日志 |
| `-q` / `--quiet` | 安静模式 |
| `-x` / `--exclude-task` | 排除指定任务 |

### 9.3 任务名称缩写

在命令行上指定任务时，不必提供任务的全名。可以提供足以唯一标识任务的任务名称的一部分。例如，`gradle che` 可能足以让 Gradle 识别 `check` 任务。


## 10. 性能优化

### 10.1 Gradle 守护进程

Gradle 守护进程（Daemon）在后台运行，可以显著提升构建速度。Gradle 默认启用守护进程，也可以通过配置强制启用：

在 `gradle.properties` 中：
```
org.gradle.daemon=true
```

或通过环境变量：
```bash
export GRADLE_OPTS="-Dorg.gradle.daemon=true"
```

### 10.2 并行执行

并行执行允许 Gradle 并发执行来自不同项目的任务，优化 CPU 利用率：

在 `gradle.properties` 中：
```
org.gradle.parallel=true
```

或在命令行中：`gradle build --parallel`

Gradle 会根据 CPU 核数自动确定最佳并行线程数。

### 10.3 构建缓存

构建缓存通过重用先前构建的输出，显著提升构建速度：

在 `gradle.properties` 中：
```
org.gradle.caching=true
```

构建缓存支持本地缓存和远程缓存。远程缓存在 CI 环境中尤其有效——CI 构建将任务输出推送到远程缓存，开发者和下游 CI 可以从远程缓存拉取。

### 10.4 配置缓存

配置缓存是 Gradle 最受期待的功能之一。启用配置缓存后，任务图在首次运行时计算并存储，后续构建直接跳过配置阶段，进入执行阶段。

在 `gradle.properties` 中：
```
org.gradle.configuration-cache=true
```

配置缓存从 Gradle 8.1 开始成为稳定功能。Gradle 团队计划在 Gradle 9.0 中将其设为首选执行模式，在 Gradle 10.0 中默认启用。


## 11. 测试与持续集成

### 11.1 测试配置

Gradle 在独立的（forked）JVM 中执行测试，与主构建过程隔离。

**配置 JUnit 5**：
```kotlin
dependencies {
    testImplementation("org.junit.jupiter:junit-jupiter:5.10.0")
}

tasks.test {
    useJUnitPlatform()
}
```

**配置集成测试**：
```kotlin
sourceSets {
    create("integrationTest") {
        compileClasspath += sourceSets.main.get().output + configurations.testRuntimeClasspath.get()
        runtimeClasspath += output + compileClasspath
    }
}

tasks.register<Test>("integrationTest") {
    testClassesDirs = sourceSets["integrationTest"].output.classesDirs
    classpath = sourceSets["integrationTest"].runtimeClasspath
}
```

### 11.2 测试报告

Gradle 会自动生成测试报告，位于 `build/reports/tests/` 目录下。可以使用 `testReport` 任务自定义报告配置。

### 11.3 与 Jenkins 集成

在 Jenkins 上执行 Gradle 构建只需几个步骤即可完成设置：

1. **安装 Gradle 插件**：在 Jenkins 的插件管理界面搜索并安装 Gradle 插件
2. **配置 Gradle**：在“系统管理”→“全局工具配置”中指定 Gradle 安装路径
3. **创建 Job**：创建新的 Jenkins Job，在源码管理中选择 Git
4. **构建步骤**：添加 Gradle 构建步骤，指定要执行的任务


## 12. 最佳实践与常见问题

### 最佳实践

1. **始终使用 Gradle Wrapper**：确保所有环境和开发者使用相同的 Gradle 版本
2. **使用 Kotlin DSL**：获得更好的 IDE 支持和类型安全
3. **使用版本目录集中管理依赖**：在 `libs.versions.toml` 中统一管理版本
4. **合理使用 `implementation` 和 `api`**：减少不必要的依赖暴露
5. **启用构建缓存和配置缓存**：显著提升构建速度
6. **使用 `buildSrc` 共享构建逻辑**：避免重复配置
7. **为自定义任务定义输入/输出**：利用增量构建
8. **定期升级 Gradle 版本**：获取性能改进和新特性

### 常见问题

**Q: 构建速度慢怎么办？**
A: 检查是否启用了 Gradle 守护进程、并行执行、构建缓存和配置缓存。使用 `gradle build --scan` 生成构建扫描报告，分析瓶颈。

**Q: 依赖冲突如何解决？**
A: 使用 `gradle dependencies` 查看依赖树，然后使用 `exclude` 排除冲突依赖，或使用 `resolutionStrategy` 强制指定版本。

**Q: Groovy DSL 和 Kotlin DSL 如何选择？**
A: 新项目推荐使用 Kotlin DSL，它提供更好的 IDE 支持和类型安全。现有 Groovy 项目可以逐步迁移。

**Q: 多模块项目如何共享配置？**
A: 使用根项目的 `subprojects` 或 `allprojects` 块统一配置，或使用 `buildSrc` 定义约定插件。


## 13. 总结

Gradle 作为新一代的构建系统，凭借其高效的性能、灵活的 DSL、强大的依赖管理和丰富的插件生态，已成为 Java 和 Android 开发的首选构建工具。

从入门到进阶，本文涵盖了 Gradle 的各个方面：
- **入门**：安装配置、Gradle Wrapper、第一个项目
- **核心概念**：Project、Task、Plugin、构建生命周期
- **构建脚本**：Groovy DSL vs Kotlin DSL、settings.gradle、build.gradle
- **依赖管理**：仓库配置、依赖类型、版本目录、冲突解决
- **任务系统**：任务定义、依赖、类型、自定义任务、增量构建
- **插件系统**：插件应用、常用插件、自定义插件开发
- **多项目构建**：结构配置、项目间依赖、buildSrc
- **性能优化**：守护进程、并行执行、构建缓存、配置缓存
- **测试与 CI**：测试配置、测试报告、Jenkins 集成

掌握 Gradle 意味着拥有更高的工作效率和更强的项目掌控能力。无论你是初入编程世界的新手，还是经验丰富的开发老手，学习 Gradle 都是一项极具价值的投资。