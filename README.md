# bk-Kernel_RT-16.2 — Xiaomi Pad 5 (nabu)

[![LICENSE](https://img.shields.io/badge/License-GPLv2-blue.svg)](COPYING)

**bk-Kernel_RT-16.2** 是基于 Linux 4.14 的预抢占实时内核，专为 **Xiaomi Pad 5 (nabu)** 设计，支持 **Android 15 ~ 16 (HyperOS)**。

> 内核作者：[@零音Rei](https://www.coolapk.com) · CoolApk @零音Rei  
> 感谢他的付出，我只是个搬运工，不要把我当成作者了 😄

---

## 版本信息

| 项目 | 值 |
|------|------|
| 内核版本 | **4.14.336** |
| 内核代号 | **bk-Kernel_RT-16.2-r1** |
| 基线版本 | 4.14.190-bk-Kernel_RT-16.2-r2 |
| 支持设备 | Xiaomi Pad 5 (nabu) |
| 支持系统 | Android 15 ~ 16 HyperOS |
| 内核类型 | PREEMPT_RT (实时抢占) |

---

## 特性

### 核心改进

- **Linux 主线安全更新** — 合并 linux-stable 4.14.190 → 4.14.336，包含数百个上游安全修复与稳定性补丁
- **实时抢占 (PREEMPT_RT)** — 低延迟调度，适用于交互与多媒体场景
- **KernelSU** — 内核级 Root 方案，支持模块管理
- **DroidSpaces** — 集成桌面模式扩展，提升大屏生产力

### 性能与优化

- **CPU 调度优化** — 修复 cpuset 配置，提升系统动画流畅度
- **内存管理增强** — 改进内存回收策略，优化多任务切换体验
- **ZRAM 回写支持** — 启用 ZRAM 写入块设备，更高效的内存压缩交换
- **灰烬模式** — 极端高负载场景下自动介入，提升系统响应流畅度（仿照水龙内核做法）
- **游戏专项优化** — 针对游戏场景调整调度与内存策略，自行测试体验

### 系统集成

- **PBRP 内置** — 内核内置 PBRP（OrangeFox/TWRP 分支）恢复环境
- **AnyKernel3 刷入** — 支持通过 TWRP / OrangeFox 刷入，保持 boot 分区兼容
- **HyperOS 兼容** — 修复 HyperOS 自定义功能服务属性，确保系统功能完整

---

## 刷入说明

### 前置要求

- 设备已解锁 Bootloader
- 已安装第三方 Recovery（TWRP / OrangeFox / PBRP）
- 当前系统为 HyperOS Android 15 ~ 16

### 刷入步骤

1. 从 [Releases](https://github.com/lietxia/android_kernel_xiaomi_nabu/releases) 下载最新刷机包（`bk-Kernel_nabu-A16-Hyper-*.zip`）
2. 进入 Recovery 模式
3. 刷入下载的 ZIP 包（AnyKernel3 格式）
4. 重启系统

> **注意**：切换回其他内核（如龙核）时，**不建议直接还原 boot 分区**（有卡一风险），请通过 TWRP 刷入对应内核的 AnyKernel3 包来还原。

### 刷后建议

- 不建议使用第三方调度工具接管内核，保持内核默认调度策略即可
- 如需调试，可开启内核调试接口查看运行状态

---

## 更新日志

### 4.14.336_bk-Kernel_RT-16.2-r1（首个正式版）

相较于上个版本（4.14.190-bk-Kernel_RT-16.2-r2），主要修复和优化了以下内容：

**修复**
- 修复 cpuset 配置，优化系统动画流畅度
- 修复内存管理相关问题

**优化**
- 内存管理优化，改进多任务切换体验
- ZRAM 回写块支持，提升内存压缩效率
- 游戏场景专项优化

**新增**
- 灰烬模式 — 极端高负载下提升系统流畅度
- 集成 DroidSpaces 桌面模式扩展
- 内核内置 PBRP 恢复环境
- Android 16 兼容性优化

**其他**
- 合并 linux-stable 4.14.190 → 4.14.336（529 个上游提交）
- 同步上游安全补丁与稳定性修复
- …… 其他优化与调整，详见提交历史

---

## 已知问题

- **键盘与手写笔连接异常** — 蓝牙输入设备连接暂时存在问题，后续版本修复
- **小核簇调度器** — 小核簇调速器锁定为 Performance，小核将持续工作在最高频率（下个版本修复，当前对功耗影响不大）

---

## 构建说明

### 前置条件

- Ubuntu 24.04 (CI 环境) 或等效 Linux 发行版
- AOSP Clang r547379
- GCC 4.9 交叉编译工具链（aarch64 + arm）
- pahole v1.25

### 本地构建

```bash
# 安装依赖
sudo apt install bc bison build-essential ccache cpio curl flex git \
  libelf-dev libssl-dev python3 rsync unzip zip zlib1g-dev

# 设置工具链路径（按实际路径修改）
export CLANG_DIR=/path/to/clang-r547379
export GCC64_DIR=/path/to/aarch64-linux-android-4.9
export GCC32_DIR=/path/to/arm-linux-androideabi-4.9
export PAHOLE=/path/to/pahole

# 编译
./bk_build/build.sh
```

### GitHub Actions 构建

仓库已配置 GitHub Actions CI，可在 Actions 页面手动触发 `Build and release nabu kernel` workflow 自动完成编译与发布。

---

## 致谢

- [@零音Rei](https://www.coolapk.com) — 内核作者与维护者
- 水龙内核 — 灰烬模式灵感来源
- [KernelSU](https://github.com/tiann/KernelSU) — 内核级 Root 方案
- [DroidSpaces](https://github.com/droidspaces) — 桌面模式扩展
- [AnyKernel3](https://github.com/osm0sis/AnyKernel3) — 刷机包模板
- linux-stable 维护团队 — 长期支持的内核安全更新

---

## 许可证

本项目基于 **GNU General Public License v2 (GPLv2)** 发布，详见 [COPYING](COPYING)。