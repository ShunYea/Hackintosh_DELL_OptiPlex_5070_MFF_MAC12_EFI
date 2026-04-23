# Hackintosh_DELL_OptiPlex_5070_MFF
Dell OptiPlex 5070 Hackintosh OpenCore EFI
- CPU用的是i9-9900T，网卡是很苹果的博通BCM943224PCIEBT2，最早当时用的是12点几版本忘记了，后面小版本一直升级，目前是最新的12.7.6，貌似官方也升级了。
- 修改自某位大佬的版本，也是忘记出处了，因为当时下载了很多，不知道是哪个版本能用的，就一直沿用下来。
- 功能基本是正常的，目前没有发现异常，没有其他外设，没法测试隔空啥的。稳定性也很好，用了大概一年半，没有大的问题。
- 准备要想升级到更高的版本了，虽然舍不得12版本的流程，据说高版本卡得一批。但是微信等啥的4.0版本必须得MacOS 13版本以上了，所以没办法只能跟着升级，所以这里仅仅作为最经典最稳定的12.7.6版本的EFI的存档，万一哪天需要回滚到这个老版本，就可以直接用了。

## 配置 ##
- CPU：i9-9900T
- 网卡：黑苹果博通BCM943224PCIEBT2
- MacOS：12.7.6
- OpenCore 1.0.0

## BIOS设置 ##
这个很重要：
- General - Advanced Boot Options 取消勾选 Enable Legacy Option ROMs
- System Configuration - SATA Operation 选择 AHCI
- Video - Primary Display 选择 Intel HD Graphics
- Secure Boot - Secure Boot Enable 取消勾选 Secure Boot Enable
- Intel Software Guard Extensions - Intel SGX Enable 选择 Disabled
- Virtualization Support - Virtualization 勾选 Enable Intel Virtualization Technology
- Virtualization Support - VT for Direct I/O 取消勾选 Enable VT for Direct I/O
- Power Management - Block Sleep 勾选（非常重要，重置了BIOS后这个没设置，一直造成只要一睡眠就重启……）

使用grub工具进命令行设置：
- 关闭CFG Lock：setup_var 0x5BE 0x0
- 设置64M预分配显存：setup_var 0x8DC 0x2


# 🍏 Hackintosh | DELL OptiPlex 5070 MFF EFI 存档

> **写在前面**：这是一份基于 macOS Monterey 12.7.6 版本的经典稳定版 EFI 存档。由于微信 4.0 等常用软件强制要求 macOS 13 及以上系统，后续只能跟随升级。为了防止高版本体验不佳（据说比较卡顿），特此留存这份经过一年半实测、稳定性极佳的配置，万一需要回滚老版本即可直接使用。

## 📝 项目说明

* **系统流变**：最早从 macOS 12 的早期版本开始使用，一路跟随小版本更新至最新的 12.7.6（官方近期也有推送更新）。
* **EFI 来源**：基础版本修改自网络大佬分享（因测试过多个版本，最初出处已不可考），在此基础上打磨后一直沿用至今。
* **运行状态**：核心功能基本正常，稳定性极佳，未出现过重大问题。*(注：因无其他苹果外设，隔空投送等特定生态功能暂未深度测试)*

---

## ⚙️ 硬件与系统配置

| 核心部件 / 软件 | 型号 / 版本 | 备注说明 |
| :--- | :--- | :--- |
| **处理器 (CPU)** | Intel Core i9-9900T | |
| **无线网卡** | 博通 BCM943224PCIEBT2 | 经典的黑苹果免驱卡 |
| **操作系统** | macOS Monterey 12.7.6 | 养老稳定版 |
| **引导工具** | OpenCore 1.0.0 | |

---

## 🔧 BIOS 核心设置

> ⚠️ **注意**：BIOS 设置对于黑苹果的成功引导至关重要，请仔细核对以下每一项。

| 菜单路径 | 设置项 | 目标状态 | 详细说明 |
| :--- | :--- | :--- | :--- |
| **General** | Advanced Boot Options | ❌ **取消勾选** | 禁用 *Enable Legacy Option ROMs* |
| **System Configuration**| SATA Operation | 💽 **AHCI** | 更改硬盘模式为 AHCI |
| **Video** | Primary Display | 🖥️ **Intel HD Graphics**| 强制使用 Intel 核显 |
| **Secure Boot** | Secure Boot Enable | ❌ **取消勾选** | 关闭安全启动 (*Secure Boot*) |
| **Intel SGX** | Intel SGX Enable | ❌ **Disabled** | 禁用 SGX 扩展 |
| **Virtualization Support**| Virtualization | ✅ **勾选** | 开启 *Intel Virtualization Technology* |
| **Virtualization Support**| VT for Direct I/O | ❌ **取消勾选** | 关闭 VT-d 功能 |
| **Power Management** | Block Sleep | ✅ **勾选** | 🚨 **极重要**：防止睡眠后自动重启（重置 BIOS 后极易漏设该项）|

---

## 💻 进阶设置 (GRUB 命令行)

请使用 GRUB 工具进入命令行环境，执行以下命令进行底层参数修改：

```bash
# 1. 关闭 CFG Lock (解锁 MSR 0xE2 寄存器)
setup_var 0x5BE 0x0

# 2. 设置 64M 预分配显存 (DVMT Pre-Allocated)
setup_var 0x8DC 0x2
