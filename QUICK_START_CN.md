# KernelSU 快速上手指南

## 第一次接触 KernelSU？从这里开始！

### 1. 克隆和探索仓库 (你已经完成了！)

```bash
# 仓库已经在这里: /home/runner/work/KernelSU/KernelSU
cd /home/runner/work/KernelSU/KernelSU
ls -la  # 查看目录结构
```

### 2. 了解项目结构

```bash
# 查看主要组件
echo "=== 内核模块 (C语言) ==="
ls kernel/

echo "=== Android 管理器 (Kotlin/Java) ==="
ls manager/

echo "=== 用户空间工具 (Rust) ==="
ls userspace/

echo "=== 文档网站 ==="
ls website/docs/
```

### 3. 阅读核心文档

```bash
# 中文文档
cat docs/README_CN.md

# 模块开发指南
cat website/docs/zh_CN/guide/module.md
```

### 4. 尝试构建组件

#### 4.1 构建 Android 管理器
```bash
cd manager/
# 检查 Gradle 环境
./gradlew --version

# 构建调试版本 (需要 Android SDK)
# ./gradlew assembleDebug
```

#### 4.2 检查用户空间组件 (Rust)
```bash
cd ../userspace/ksud/
# 查看 Rust 项目结构
cat Cargo.toml
ls src/
```

#### 4.3 查看内核模块
```bash
cd ../../kernel/
# 查看内核 Makefile
cat Makefile
# 查看主要源文件
ls *.c *.h
```

### 5. 学习核心概念

#### 什么是 KernelSU？
- **内核级 Root**: 在 Linux 内核层面实现 root 功能
- **模块系统**: 基于 OverlayFS 的模块挂载
- **权限管理**: 精细化的应用权限控制

#### 与 Magisk 的区别
- **Magisk**: 用户空间修改，容易被检测
- **KernelSU**: 内核空间实现，更难被检测

### 6. 实际操作建议

#### 对于 Android 开发者
1. 先研究 `manager/app/` 目录下的 Android 应用
2. 了解 KernelSU 的 UI 和功能
3. 学习如何与内核模块通信

#### 对于系统开发者
1. 研究 `kernel/` 目录下的内核代码
2. 理解系统调用钩子机制
3. 学习内核模块开发

#### 对于 Rust 开发者
1. 查看 `userspace/ksud/` 的 Rust 代码
2. 了解用户空间守护进程的工作原理
3. 学习与内核模块的交互

### 7. 开发环境建议

#### 必需工具
- **Git**: 版本控制
- **文本编辑器**: VS Code, Vim 等

#### Android 开发
- **Android Studio**: 构建管理器应用
- **Android SDK**: Android 开发工具包
- **Java/Kotlin**: 编程语言知识

#### 内核开发
- **Linux 系统**: 内核编译环境
- **交叉编译工具链**: ARM/x86 编译器
- **C 语言**: 内核编程语言

#### Rust 开发
- **Rust 工具链**: cargo, rustc
- **交叉编译支持**: Android 目标平台

### 8. 学习资源

#### 官方资源
- **网站**: https://kernelsu.org/
- **中文文档**: https://kernelsu.org/zh_CN/
- **Telegram**: @KernelSU

#### 技术文档
- **Linux 内核**: https://www.kernel.org/doc/
- **Android 开发**: https://developer.android.com/
- **Rust 语言**: https://www.rust-lang.org/learn

### 9. 下一步计划

选择你感兴趣的方向：

#### 🔨 实用方向
- [ ] 学习安装和使用 KernelSU
- [ ] 尝试现有的 KernelSU 模块
- [ ] 为自己的设备编译内核

#### 📱 应用开发方向
- [ ] 研究管理器应用源码
- [ ] 学习 Android 权限管理
- [ ] 开发 KernelSU 插件应用

#### 🧠 内核开发方向
- [ ] 学习 Linux 内核编程
- [ ] 理解系统调用钩子
- [ ] 研究 SELinux 绕过技术

#### 🦀 系统工具方向
- [ ] 学习 Rust 系统编程
- [ ] 研究用户空间守护进程
- [ ] 开发命令行工具

### 10. 实际动手

现在就开始你的第一个任务：

```bash
# 选择一个你感兴趣的目录
cd manager/    # Android 开发者
# 或者
cd kernel/     # 内核开发者  
# 或者
cd userspace/  # Rust 开发者

# 开始阅读代码！
find . -name "*.java" -o -name "*.kt" -o -name "*.c" -o -name "*.rs" | head -10
```

记住：**不要害怕提问！** KernelSU 社区非常活跃，大家都愿意帮助新人。

开始你的 KernelSU 之旅吧！ 🚀