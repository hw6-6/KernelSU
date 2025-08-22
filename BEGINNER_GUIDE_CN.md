# KernelSU 代码仓库初学者指南

## 项目概述

KernelSU 是一个基于 Android 内核的 root 解决方案，与传统的 Magisk 等用户空间 root 方案不同，KernelSU 在内核层面实现 root 功能。

### 主要特性
- **内核级 su**：在内核层面提供 root 权限管理
- **OverlayFS 模块系统**：基于 OverlayFS 的模块挂载机制
- **应用配置**：精细化的应用权限控制

## 仓库结构分析

### 1. `/kernel/` 目录 - 内核模块 (核心组件)
这是 KernelSU 的核心，包含所有内核空间的代码：

```
kernel/
├── ksu.c           # 主要的内核模块入口
├── core_hook.c     # 核心系统调用钩子
├── manager.c       # 管理器应用验证
├── allowlist.c     # 应用白名单管理
├── module_api.c    # 模块 API 接口
├── apk_sign.c      # APK 签名验证
└── selinux/        # SELinux 相关代码
```

**技术栈**: C 语言，Linux 内核模块
**授权**: GPL-2.0

### 2. `/manager/` 目录 - Android 管理应用
用户界面应用，用于管理 KernelSU：

```
manager/
├── app/            # Android 应用源码
├── build.gradle.kts # 构建配置
└── gradle/         # Gradle 包装器
```

**技术栈**: Kotlin/Java，Android SDK，Gradle
**授权**: GPL-3.0

### 3. `/userspace/` 目录 - 用户空间组件
包含用户空间的守护进程和工具：

```
userspace/
├── ksud/           # KernelSU 守护进程
└── su/             # su 命令实现
```

**技术栈**: Rust (基于 CI 配置推断)

### 4. `/website/` 目录 - 文档网站
官方文档和指南：

```
website/docs/
├── zh_CN/          # 中文文档
├── guide/          # 英文指南
└── ...             # 其他语言文档
```

### 5. `/docs/` 目录 - README 文件
多语言的项目介绍文件

## 构建系统

### GitHub Actions 工作流
项目使用 GitHub Actions 进行自动化构建：

- `build-manager.yml` - 构建 Android 管理器应用
- `build-ksud.yml` - 构建用户空间守护进程
- `build-kernel-*.yml` - 构建不同 Android 版本的内核
- `deploy-website.yml` - 部署文档网站

### 支持的架构
- `arm64-v8a` (ARM 64位)
- `x86_64` (Intel 64位)

### 支持的 Android 版本
- Android 12 (API 31)
- Android 13 (API 33) 
- Android 14 (API 34)
- 支持 GKI 2.0 内核 (5.10+)

## 初学者学习路径

### 第一步：理解概念
1. **阅读中文文档**: `website/docs/zh_CN/guide/`
   - 安装指南
   - 模块开发指南
   - 与 Magisk 的区别

2. **理解架构差异**:
   - 传统 root：在用户空间修改
   - KernelSU：在内核空间实现

### 第二步：环境搭建
1. **Android 开发环境**:
   ```bash
   cd manager/
   ./gradlew build
   ```

2. **内核开发环境** (需要 Linux):
   ```bash
   cd kernel/
   make
   ```

### 第三步：代码学习顺序
1. **从管理器开始** (`manager/app/`)
   - 最容易理解的 Android 应用代码
   - 了解用户界面和功能

2. **学习模块系统** (`website/docs/zh_CN/guide/module.md`)
   - 理解模块的工作原理
   - 学习模块开发

3. **深入内核代码** (`kernel/`)
   - 需要 Linux 内核编程基础
   - 理解系统调用钩子机制

### 第四步：实践项目
1. **编译管理器应用**
2. **创建简单的 KernelSU 模块**
3. **研究现有模块的源码**

## 开发工具推荐

### Android 开发
- **Android Studio** - 管理器应用开发
- **ADB** - 设备调试

### 内核开发  
- **交叉编译工具链** - ARM/x86 内核编译
- **GDB** - 内核调试
- **QEMU** - 虚拟机测试

### 代码编辑
- **VS Code** - 通用代码编辑
- **CLion** - C/C++ 开发
- **IntelliJ IDEA** - Kotlin/Java 开发

## 常见问题

### Q: 我需要什么基础知识？
A: 
- **基础**: Android 开发经验
- **进阶**: Linux 内核编程知识
- **高级**: 系统调用和内核模块开发

### Q: 如何开始贡献代码？
A:
1. Fork 仓库
2. 选择感兴趣的模块 (推荐从管理器应用开始)
3. 阅读相关文档
4. 提交 Pull Request

### Q: 如何调试内核模块？
A:
- 使用 `printk` 输出调试信息
- 通过 `dmesg` 查看内核日志
- 使用 QEMU 虚拟机进行安全测试

## 学习资源

### 官方资源
- **官网**: https://kernelsu.org/
- **中文指南**: https://kernelsu.org/zh_CN/
- **Telegram 群组**: @KernelSU

### 相关技术文档
- **Linux 内核编程**: https://www.kernel.org/doc/
- **Android 系统开发**: https://source.android.com/
- **OverlayFS 文档**: https://www.kernel.org/doc/Documentation/filesystems/overlayfs.txt

## 下一步建议

1. **立即开始**: 阅读 `docs/README_CN.md` 了解项目基本信息
2. **尝试编译**: 克隆仓库并尝试编译管理器应用  
3. **加入社区**: 加入 Telegram 群组与其他开发者交流
4. **选择方向**: 根据你的兴趣选择专注的模块 (应用层/内核层)

## 总结

KernelSU 是一个技术含量较高的项目，涉及 Android 系统的多个层面。作为初学者，建议从管理器应用和文档开始，逐步深入到内核代码。记住，这是一个活跃的开源项目，不要害怕提问和寻求帮助！