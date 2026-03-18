## `rustup`在 Linux 或 macOS 上安装

如果您使用的是 Linux 或 macOS，请打开终端并输入以下命令：

```rust
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

## `rustup`在 Windows 上安装

在 Windows 系统上，需要访问[https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install)并按照说明安装 Rust。

## 安装确认

```rust
rustc --version
```

可以看到最新稳定版本的版本号、提交哈希值和提交日期，例如：

```rust
rustc 1.93.0 (254b59607 2026-01-19)
```

## 更新和卸载

### 更新

```rust
rustup update
```

### 卸载

```rust
rustup self uninstall
```

## 环境配置

### IDE

vscode、rustrover

### 插件
- crates: Rust 包管理
- Even Better TOML: TOML 文件支持
- Better Comments: 优化注释显示
- Error Lens: 错误提示优化
- GitLens: Git 增强
- Github Copilot: 代码提示
- indent-rainbow: 缩进显示优化
- Prettier - Code formatter: 代码格式化
- REST client: REST API 调试
- rust-analyzer: Rust 语言支持
- Rust Test lens: Rust 测试支持
- Rust Test Explorer: Rust 测试概览
- TODO Highlight: TODO 高亮
- vscode-icons: 图标优化
- YAML: YAML 文件支持


### 安装 cargo generate

cargo generate 是一个用于生成项目模板的工具。它可以使用已有的 github repo 作为模版生成新的项目。

```bash
cargo install cargo-generate
```

在我们的课程中，新的项目会使用 `tyr-rust-bootcamp/template` 模版生成基本的代码：

```bash
cargo generate tyr-rust-bootcamp/template
```

### 安装 pre-commit

pre-commit 是一个代码检查工具，可以在提交代码前进行代码检查。

```bash
pipx install pre-commit
```

```bash
pip 和 pipx 什么时候用哪个？

用 pip

pip install requests pandas numpy
- 安装库，供代码导入使用
- 项目依赖（通常在 requirements.txt 或虚拟环境中）

用 pipx

pipx install pre-commit                                                       
pipx install poetry                                                           
pipx install black
- 安装命令行工具/应用程序
- 工具直接运行，不作为库导入
```

安装成功后运行 `pre-commit install` 即可。

### 安装 Cargo deny

Cargo deny 是一个 Cargo 插件，可以用于检查依赖的安全性。

```bash
cargo install --locked cargo-deny
```

### 安装 typos

typos 是一个拼写检查工具。

```bash
cargo install typos-cli
```

### 安装 git cliff

git cliff 是一个生成 changelog 的工具。

```bash
cargo install git-cliff
```

### 安装 cargo nextest

cargo nextest 是一个 Rust 增强测试工具。

```bash
cargo install cargo-nextest --locked
```

### 修改编译文件生成位置

```bash
vi ~/.cargo/config.toml
[build]

target-dir = "$HOME/.target"
```