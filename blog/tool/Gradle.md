# Gradle

## 1. 简介

Gradle 是基于 JVM 的自动化构建工具，脚本用 Groovy 或 Kotlin DSL 编写，负责编译、测试、打包、发布。

- **声明式与命令式结合**：DSL 定义构建逻辑，代码实现复杂逻辑；**依赖管理**：动态版本、传递性依赖、冲突解决
- **构建性能**：增量构建、构建缓存、并行执行；**多语言**：Java、Android、Kotlin、C++、Swift
- **插件机制**：官方与社区插件丰富，也可自定义；**多项目构建**：模块化开发与代码复用

Android 的官方构建系统是 Gradle，Spring Boot 等项目也已迁移到 Gradle。

## 2. Gradle 入门

### 2.1 什么是 Gradle

Gradle 是运行在 JVM 上的自动化构建工具，自动完成编译、测试、打包、发布。
作用有三：项目构建（标准的、跨平台的自动化构建方式）、依赖管理（管理 jar 包、避免版本冲突）、统一开发结构（标准的项目目录）。
脚本用 Groovy 或 Kotlin 编写，表达能力强，也足够灵活。

### 2.2 安装

**方式一：手动安装**

1. 从 [gradle.org](https://gradle.org) 下载二进制包，解压到指定目录（如 `/opt/gradle`）。
2. 配置环境变量：
   ```bash
   export GRADLE_HOME=/opt/gradle
   export PATH=$PATH:$GRADLE_HOME/bin
   ```
3. 验证安装：`gradle -v`

**方式二：包管理器**

- macOS: `brew install gradle`
- Ubuntu/Debian: `sudo apt install gradle`
- Windows: `choco install gradle`（Chocolatey）

### 2.3 Gradle Wrapper

Wrapper 是自动下载并使用指定 Gradle 版本的脚本，保证所有开发者和 CI 环境版本一致。
```bash
gradle wrapper --gradle-version 8.5
```

生成 `gradlew`（Unix/Linux/macOS 脚本）、`gradlew.bat`（Windows 脚本）、`gradle/wrapper/gradle-wrapper.jar`、`gradle/wrapper/gradle-wrapper.properties`（指定版本与下载地址）。团队统一用 `./gradlew` 或 `gradlew`，不再直接调用 `gradle`。

### 2.4 第一个项目

`gradle init` 生成项目骨架：
```bash
mkdir my-project && cd my-project
gradle init --type java-application
```

项目结构：
```text
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

## 3. 核心概念

### 3.1 项目（Project）

项目（Project）是一个构建单元，可以是 JAR 包、Web 应用或 Android APK。一次构建由一个或多个项目组成，每个项目都有一个 `build.gradle` 脚本。

### 3.2 任务（Task）

任务（Task）是一个构建步骤，如编译、打包、测试。Task 之间用 `dependsOn` 定义依赖，形成有向无环图（DAG），保证执行顺序。

### 3.3 构建脚本（Build Script）

构建脚本定义项目如何构建。每个项目有一个 `build.gradle(.kts)`，用于应用插件、声明依赖、配置任务。

### 3.4 插件（Plugin）

插件是组织和复用构建逻辑的主要方式。应用插件会执行代码，创建任务、配置属性、扩展项目功能。

### 3.5 构建生命周期

构建分三个阶段：初始化（发现项目结构，解析 `settings.gradle`）、配置（执行构建脚本，构建任务图）、执行（按任务依赖关系执行任务）。

## 4. 构建脚本

### 4.1 Groovy DSL vs Kotlin DSL

构建脚本可用两种 DSL：

| 特性 | Groovy DSL | Kotlin DSL |
|------|------------|------------|
| 文件扩展名 | `.gradle` | `.gradle.kts` |
| 语法 | 动态类型，灵活 | 静态类型，更安全 |
| IDE 支持 | 基础补全 | 智能提示、代码补全、重构 |
| 学习曲线 | 较低 | 稍高（需 Kotlin 基础） |

Kotlin DSL 有语法高亮、代码补全和声明导航，编辑体验更好；从 Android Studio Giraffe 起，新项目默认使用 Kotlin DSL。

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

`settings.gradle(.kts)` 定义多项目构建的结构，声明包含哪些子项目。单项目构建保持简单：
```kotlin
// settings.gradle.kts
rootProject.name = "my-project"
```

### 4.3 build.gradle 文件

`build.gradle(.kts)` 是核心构建脚本，包含插件、项目元信息、仓库、依赖和任务配置：
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

`sourceCompatibility` / `targetCompatibility` 设置 Java 版本；`application` 配置可执行应用的主类；`java` 配置 Java 编译选项。

## 5. 依赖管理

### 5.1 仓库配置

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

| 配置类型 | 说明 |
|----------|------|
| `implementation` | 编译时依赖，不暴露给消费者，减少编译依赖泄漏 |
| `api` | 编译时依赖，暴露给消费者（传递性依赖） |
| `compileOnly` | 仅编译时需要，运行时不需要（如容器提供的 API） |
| `runtimeOnly` | 仅运行时需要，编译时不需要（如 JDBC 驱动） |
| `testImplementation` | 测试编译时依赖 |
| `testRuntimeOnly` | 测试运行时依赖 |

`api` 有传递性：A 依赖 B，B 用 `api` 依赖 C，则 A 也能访问 C。`implementation` 没有传递性。

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

Gradle 6.8 引入版本目录，在根项目的 `gradle/libs.versions.toml` 中集中管理第三方依赖版本：
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

内置类型有 `Copy`（复制文件）、`Jar`（创建 JAR）、`Zip`（创建 ZIP）、`Delete`（删除文件）、`Exec`（执行外部命令）、`JavaCompile`（编译 Java 源码）、`Test`（执行测试）。
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

内置类型不够用时自定义任务类型，并用 `@Input`、`@OutputFile`、`@OutputDirectory` 标记输入输出，才能受益于增量构建：
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

### 6.5 增量构建

增量构建检查任务的输入和输出是否变化，都没有变化就跳过该任务，因此必须正确定义输入输出：
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

插件可以创建任务、配置项目属性、添加依赖、扩展 Gradle 功能。

### 7.2 应用插件

**使用 plugins DSL（推荐）**：
```kotlin
plugins {
    id("java")
    id("application")
    id("org.springframework.boot") version "3.0.0"
}
```
**使用 apply 方法（旧方式）**：
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
**最佳实践**是写约定插件（Convention Plugin）或二进制插件，而不是脚本插件；约定插件封装通用构建逻辑，便于在多项目构建中共享。

## 8. 多项目构建

### 8.1 多项目结构

多项目构建由一个根项目和一个或多个子项目组成：
```text
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

在 `settings.gradle(.kts)` 中用 `include()` 声明子项目：
```kotlin
rootProject.name = "my-project"
include("app", "core", "util")
```

### 8.3 子项目配置

每个子项目有自己的 `build.gradle(.kts)`，也可以在根项目统一配置所有子项目：
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

`buildSrc` 是根目录下的特殊目录，存放构建逻辑，Gradle 会自动识别；适合定义共享配置或命令式构建逻辑（如自定义任务、插件）：
```text
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

## 9. Gradle 命令行

### 9.1 基本命令

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

命令行上不必写任务全名，给出能唯一识别任务的部分名称即可，例如 `gradle che` 可能已足以识别 `check` 任务。

## 10. 性能优化

### 10.1 Gradle 守护进程

守护进程（Daemon）在后台运行，提升构建速度，Gradle 默认启用。在 `gradle.properties` 中：
```properties
org.gradle.daemon=true
```

或通过环境变量：
```bash
export GRADLE_OPTS="-Dorg.gradle.daemon=true"
```

### 10.2 并行执行

并行执行让 Gradle 并发执行不同项目的任务，线程数按 CPU 核数自动确定。在 `gradle.properties` 中：
```properties
org.gradle.parallel=true
```

或在命令行中：`gradle build --parallel`

### 10.3 构建缓存

构建缓存重用先前构建的输出。在 `gradle.properties` 中：
```properties
org.gradle.caching=true
```

缓存分本地和远程两种。远程缓存在 CI 中尤其有效：CI 构建把输出推送到远程缓存，开发者和下游 CI 从中拉取。

### 10.4 配置缓存

配置缓存让任务图在首次运行时计算并存储，后续构建跳过配置阶段直接执行。在 `gradle.properties` 中：
```properties
org.gradle.configuration-cache=true
```

配置缓存从 Gradle 8.1 起稳定；Gradle 9.0 计划将其设为首选执行模式，10.0 默认启用。

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

测试报告自动生成在 `build/reports/tests/` 下，`testReport` 任务可自定义报告配置。

### 11.3 与 Jenkins 集成

1. **安装 Gradle 插件**：在 Jenkins 插件管理界面搜索并安装 Gradle 插件
2. **配置 Gradle**：在“系统管理”→“全局工具配置”中指定 Gradle 安装路径
3. **创建 Job**：新建 Job，源码管理选择 Git
4. **构建步骤**：添加 Gradle 构建步骤，指定要执行的任务

## 12. 最佳实践与常见问题

### 12.1 最佳实践

1. **始终使用 Gradle Wrapper**：保证所有环境和开发者版本一致
2. **使用 Kotlin DSL**：更好的 IDE 支持和类型安全
3. **用版本目录集中管理依赖**：在 `libs.versions.toml` 统一版本
4. **合理使用 `implementation` 和 `api`**：减少不必要的依赖暴露
5. **启用构建缓存和配置缓存**：提升构建速度
6. **用 `buildSrc` 共享构建逻辑**：避免重复配置
7. **为自定义任务定义输入/输出**：利用增量构建
8. **定期升级 Gradle 版本**：获取性能改进和新特性

### 12.2 常见问题

**Q: 构建速度慢怎么办？** A: 检查是否启用了守护进程、并行执行、构建缓存和配置缓存，并用 `gradle build --scan` 生成构建扫描报告分析瓶颈。

**Q: 依赖冲突如何解决？** A: 用 `gradle dependencies` 查看依赖树，再用 `exclude` 排除冲突依赖，或用 `resolutionStrategy` 强制指定版本。

**Q: Groovy DSL 和 Kotlin DSL 如何选择？** A: 新项目用 Kotlin DSL，IDE 支持和类型安全更好；现有 Groovy 项目可逐步迁移。

**Q: 多模块项目如何共享配置？** A: 用根项目的 `subprojects` 或 `allprojects` 块统一配置，或用 `buildSrc` 定义约定插件。
