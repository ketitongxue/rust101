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