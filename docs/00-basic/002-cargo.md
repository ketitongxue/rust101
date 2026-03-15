# 包管理工具 cargo

## 创建项目

```Bash
$ cargo new world_hello

    **Creating** binary (application) `world_hello` package

**note**: see more `Cargo.toml` keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
```

## 运行项目

1. cargo run

```Bash
$ cargo run
   Compiling world_hello v0.1.0 (/Users/sunfei/development/rust/world_hello)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43s
     Running `target/debug/world_hello`
Hello, world!
```

2. 手动编译和运行项目

编译（debug 模式）

```Bash
$ cargo build
    Finished dev [unoptimized + debuginfo] target(s) in 0.00s
```

运行

```Bash
$ ./target/debug/world_hello
Hello, world!
```

高性能模式编译和运行（生产环境使用）

```Bash
cargo run --release
cargo build --release
```

## 快速验证代码正确性

```Bash
$ cargo check
    Checking world_hello v0.1.0 (/Users/sunfei/development/rust/world_hello)
    Finished dev [unoptimized + debuginfo] target(s) in 0.06s
```

## Cargo.toml 和 Cargo.lock

`Cargo.toml` 和 `Cargo.lock` 是 `cargo` 的核心文件，它的所有活动均基于此二者。

- `Cargo.toml` 是 `cargo` 特有的**项目数据描述文件**。它存储了项目的所有元配置信息，如果 Rust 开发者希望 Rust 项目能够按照期望的方式进行构建、测试和运行，那么，必须按照合理的方式构建 `Cargo.toml`。
    
- `Cargo.lock` 文件是 `cargo` 工具根据同一项目的 `toml` 文件生成的**项目依赖详细清单**，因此我们一般不用修改它，只需要对着 `Cargo.toml` 文件撸就行了。
    

> 什么情况下该把 `Cargo.lock` 上传到 git 仓库里？
> 当你的项目是一个可运行的程序时，就上传 `Cargo.lock`，如果是一个依赖库项目，那么就把它添加到 `.gitignore` 中。

## Cargo.toml 详解

### Package 配置段落

`package` 中记录了项目的描述信息，典型的如下：

```TOML
[package]
name = "world_hello"
version = "0.1.0"
edition = "2024"

[dependencies]
```

`name` 字段定义了项目名称，`version` 字段定义当前版本，新项目默认是 `0.1.0`，`edition` 字段定义了我们使用的 Rust 大版本。

在 `Cargo.toml` 文件中，`edition` 字段用于指定项目使用的 **Rust** **版本（Edition）**。

简单来说，它是 Rust 团队用来引入**不兼容语言变更**（如添加新关键字）的一种机制，同时又不会导致整个生态系统发生“分裂”。

---

为什么需要 Edition？

在大多数编程语言中，引入像 `async` 或 `await` 这样的新关键字会破坏现有的代码（如果旧代码中有人用这些词做变量名）。

Rust 通过 Edition 解决了这个问题：

- 向后兼容性：编译器支持所有已发布的 Edition。你可以让使用 2021 Edition 的库和使用 2024 Edition 的库在一起编译，它们能够完美协作。

- **平滑演进：** Rust 每三年发布一个新 Edition（如 2015, 2018, 2021, 2024），允许语言引入更简洁的语法和强大的特性，而不会强制要求开发者立即重写旧代码。

### 项目依赖

使用 `cargo` 工具的最大优势就在于，能够对该项目的各种依赖项进行方便、统一和灵活的管理。

在 `Cargo.toml` 中，主要通过各种依赖段落来描述该项目的各种依赖项：

- 基于 Rust 官方仓库 `crates.io`，通过版本说明来描述

- 基于项目源代码的 git 仓库地址，通过 URL 来描述

- 基于本地项目的绝对路径或者相对路径，通过类 Unix 模式的路径来描述

这三种形式具体写法如下：

```TOML
[dependencies]
rand = "0.3"
hammer = { version = "0.5.0"}
color = { git = "https://github.com/bjz/color-rs" }
geometry = { path = "crates/geometry" }
```

## 依赖下载问题（网络问题）

下载依赖库的地址是 [crates.io](https://crates.io/)，是由 Rust 官方搭建的镜像下载和管理服务。

1. 开启命令行或全局翻墙
2. 修改 Rust 的下载镜像为国内的镜像地址

为了使用 `crates.io` 之外的注册服务，我们需要对 `$HOME/.cargo/config.toml` ($CARGO_HOME 下) 文件进行配置，添加新的服务提供商，有两种方式可以实现：增加新的镜像地址和覆盖默认的镜像地址。

1. 增加新的镜像地址
	1. **在** **`crates.io`** **之外添加新的注册服务**，在 `$HOME/.cargo/config.toml` （如果文件不存在则手动创建一个）中添加以下内容
    ```TOML
    [registries]
    ustc = { index = "https://mirrors.ustc.edu.cn/crates.io-index/" }
    ```

     这种方式只会新增一个新的镜像地址，因此在引入依赖的时候，需要指定该地址，例如在项目中引入 `time` 包，你需要在 `Cargo.toml` 中使用以下方式引入:

    ```TOML
    [dependencies]
    time = {  registry = "ustc" }
    ```

      在重新配置后，初次构建可能要较久的时间，因为要下载更新 `ustc` 注册服务的索引文件，由于文件比较大，需要等待较长的时间。

        此处有两点需要注意：

    2. cargo 1.68 版本开始支持稀疏索引，不再需要完整克隆 crates.io-index 仓库，可以加快获取包的速度，如：
    
    ```TOML
    [source.ustc]
    registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
    ```
    
    3. cargo search 无法使用镜像

2. 覆盖默认的镜像地址

      更推荐这种方式，因为增加新的镜像地址在项目大了后，实在是很麻烦，全部修改后，万一以后不用这个镜像了，你又要全部修改成其它的。

      而覆盖默认的镜像地址这种方式，则不需要修改 `Cargo.toml` 文件，**因为它是直接使用新注册服务来替代默认的** **`crates.io`**。

      在 `$HOME/.cargo/config.toml` 添加以下内容：

    ```TOML
    [source.crates-io]
    replace-with = 'ustc'
    
    [source.ustc]
    registry = "git://mirrors.ustc.edu.cn/crates.io-index"
    ```

     首先，创建一个新的镜像源 `[source.ustc]`，然后将默认的 `crates-io` 替换成新的镜像源: `replace-with = 'ustc'`。

     简单吧？只要这样配置后，以往需要去 `crates.io` 下载的包，会全部从科大的镜像地址下载，速度刷刷的... 我的 300M 大刀（宽带）终于有了用武之地。

      这里强烈推荐大家在学习完后面的基本章节后，看一下 **[Cargo 使用指南章节](https://course.rs/cargo/intro.html)****，对于你的Rust 之旅会有莫大的帮助！

### 常用国内镜像源

科大镜像：https://mirrors.ustc.edu.cn/crates.io-index/

字节跳动：

```TOML
[source.crates-io]
replace-with = 'rsproxy'

[source.rsproxy]
registry = "https://rsproxy.cn/crates.io-index"

# 稀疏索引，要求 cargo >= 1.68
[source.rsproxy-sparse]
registry = "sparse+https://rsproxy.cn/index/"

[registries.rsproxy]
index = "https://rsproxy.cn/crates.io-index"

[net]
git-fetch-with-cli = true
```

## .cargo/config.toml

```Rust
[build]
target-dir = "/Users/keti/.target"
```