# KernelSU 代码地图

## 仓库目录结构

```
KernelSU/
├── 📁 docs/                    # 多语言 README 文件
├── 🔧 kernel/                  # 内核模块 (核心)
│   ├── 📄 ksu.c               # 主模块入口
│   ├── 📄 core_hook.c         # 系统调用钩子
│   ├── 📄 manager.c           # 管理器验证
│   ├── 📄 allowlist.c         # 应用白名单
│   ├── 📄 module_api.c        # 模块 API
│   ├── 📄 apk_sign.c          # APK 签名验证
│   ├── 📁 include/            # 头文件
│   ├── 📁 selinux/            # SELinux 相关
│   └── 📄 Makefile            # 内核构建配置
├── 📱 manager/                 # Android 管理器应用
│   ├── 📁 app/src/            # 应用源码
│   ├── 📄 build.gradle.kts    # Gradle 构建配置
│   └── 📁 gradle/             # Gradle 包装器
├── 🛠️ userspace/              # 用户空间工具
│   ├── 🦀 ksud/               # 守护进程 (Rust)
│   │   ├── 📄 Cargo.toml      # Rust 项目配置
│   │   ├── 📁 src/            # Rust 源码
│   │   └── 📁 bin/            # 二进制文件
│   └── 📁 su/                 # su 命令实现
├── 📚 website/                 # 文档网站
│   └── 📁 docs/               # 多语言文档
│       ├── 📁 zh_CN/          # 中文文档
│       ├── 📁 guide/          # 英文指南
│       └── 📁 [其他语言]/
└── ⚙️ scripts/                # 构建脚本
```

## 核心文件详解

### 🔧 内核模块 (`kernel/`)

| 文件 | 功能 | 重要程度 |
|------|------|----------|
| `ksu.c` | 主模块入口点 | ⭐⭐⭐⭐⭐ |
| `core_hook.c` | 系统调用钩子实现 | ⭐⭐⭐⭐⭐ |
| `manager.c` | 验证管理器应用合法性 | ⭐⭐⭐⭐ |
| `allowlist.c` | 管理应用白名单 | ⭐⭐⭐ |
| `module_api.c` | 提供模块 API 接口 | ⭐⭐⭐⭐ |
| `apk_sign.c` | APK 数字签名验证 | ⭐⭐⭐ |
| `ksud.c` | 与用户空间守护进程通信 | ⭐⭐⭐⭐ |

### 📱 Android 管理器 (`manager/app/`)

```
app/src/main/
├── 📄 AndroidManifest.xml     # 应用清单
├── 📁 java/                   # Java/Kotlin 源码
│   └── 📁 me/weishu/kernelsu/ # 主包名
├── 📁 res/                    # 资源文件
│   ├── 📁 layout/             # 布局文件
│   ├── 📁 values/             # 字符串等资源
│   └── 📁 drawable/           # 图标图片
└── 📁 cpp/                    # JNI C++ 代码
```

### 🦀 用户空间守护进程 (`userspace/ksud/`)

| 文件 | 功能 |
|------|------|
| `src/main.rs` | 程序入口点 |
| `src/module.rs` | 模块管理逻辑 |
| `src/event.rs` | 事件处理系统 |
| `Cargo.toml` | Rust 项目配置 |

### 📚 文档系统 (`website/docs/`)

| 目录 | 内容 |
|------|------|
| `zh_CN/` | 中文文档 |
| `guide/` | 英文指南 |
| `zh_TW/` | 繁体中文 |
| `ja_JP/` | 日语文档 |

## 代码流程图

### 启动流程
```
1. Android 系统启动
   ↓
2. 内核加载 KernelSU 模块 (kernel/ksu.c)
   ↓
3. 注册系统调用钩子 (kernel/core_hook.c)
   ↓
4. 启动用户空间守护进程 (userspace/ksud)
   ↓
5. 等待管理器应用连接 (manager/)
```

### 权限检查流程
```
1. 应用请求 su 权限
   ↓
2. 内核钩子拦截系统调用 (core_hook.c)
   ↓
3. 检查应用是否在白名单 (allowlist.c)
   ↓
4. 验证调用者身份 (manager.c)
   ↓
5. 授予或拒绝权限
```

### 模块安装流程
```
1. 用户通过管理器安装模块
   ↓
2. 管理器调用守护进程 API (ksud)
   ↓
3. 守护进程处理模块文件 (module.rs)
   ↓
4. 通过内核 API 挂载模块 (module_api.c)
   ↓
5. 模块生效
```

## 编程语言分布

| 语言 | 用途 | 文件位置 |
|------|------|----------|
| **C** | 内核模块 | `kernel/*.c` |
| **Kotlin/Java** | Android 应用 | `manager/app/src/` |
| **Rust** | 系统工具 | `userspace/ksud/src/` |
| **Markdown** | 文档 | `docs/`, `website/` |
| **Shell** | 构建脚本 | `scripts/` |

## 关键概念理解

### 1. 内核钩子 (Kernel Hooks)
- **位置**: `kernel/core_hook.c`
- **作用**: 拦截系统调用，实现权限控制
- **技术**: kprobe 机制

### 2. OverlayFS 模块系统
- **位置**: `kernel/module_api.c`, `userspace/ksud/src/module.rs`
- **作用**: 无侵入式系统修改
- **技术**: Linux OverlayFS

### 3. APK 签名验证
- **位置**: `kernel/apk_sign.c`
- **作用**: 确保管理器应用的合法性
- **技术**: APK v2 签名验证

### 4. 进程间通信
- **位置**: `kernel/ksud.c`, `userspace/ksud/`
- **作用**: 内核与用户空间通信
- **技术**: Unix socket, netlink

## 学习建议

### 🚀 快速入门路径
1. **阅读文档** → `website/docs/zh_CN/`
2. **了解架构** → `BEGINNER_GUIDE_CN.md`
3. **查看管理器** → `manager/app/src/`
4. **研究守护进程** → `userspace/ksud/src/`
5. **深入内核** → `kernel/`

### 🎯 专项学习路径

#### Android 应用开发者
```
manager/app/ → 学习 UI 设计和权限管理
↓
userspace/ksud/ → 了解系统服务
↓
kernel/manager.c → 理解安全验证
```

#### 系统程序员
```
userspace/ksud/ → 学习 Rust 系统编程
↓
kernel/core_hook.c → 理解系统调用拦截
↓
kernel/module_api.c → 掌握模块管理
```

#### 内核开发者
```
kernel/ksu.c → 模块初始化
↓
kernel/core_hook.c → 钩子机制
↓
kernel/selinux/ → 安全策略
```

## 常用调试技巧

### 内核调试
```bash
# 查看内核日志
dmesg | grep ksu

# 加载调试信息
echo 8 > /proc/sys/kernel/printk
```

### 应用调试
```bash
# Android 日志
adb logcat | grep KernelSU

# 应用信息
adb shell dumpsys package me.weishu.kernelsu
```

### 系统调试
```bash
# 进程信息
ps aux | grep ksud

# 文件系统
mount | grep overlay
```

开始探索 KernelSU 的精彩世界吧！ 🚀