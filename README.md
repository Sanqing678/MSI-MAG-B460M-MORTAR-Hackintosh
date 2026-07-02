# 🖥️ MSI MAG B460M MORTAR · OpenCore Hackintosh

**Intel Core i5-10400F · AMD Radeon RX 5500 · Comet Lake-S · B460 · macOS Sequoia 15 · OpenCore 1.0.x**

<div align="center">

![CPU](https://img.shields.io/badge/CPU-i5_10400F_Comet_Lake-0071C5?style=flat-square)
![GPU](https://img.shields.io/badge/GPU-RX_5500_Navi_14-ED1C24?style=flat-square)
![RAM](https://img.shields.io/badge/RAM-32_GB_DDR4-8B5CF6?style=flat-square)
![OpenCore](https://img.shields.io/badge/OpenCore-1.0.x-9cf?style=flat-square)
![Chipset](https://img.shields.io/badge/Chipset-B460-00A3E0?style=flat-square)
![macOS](https://img.shields.io/badge/macOS-Sequoia_15-0071BC?style=flat-square)
![Status](https://img.shields.io/badge/Status-Working-brightgreen?style=flat-square)

**[🖥️ 硬件配置](#-硬件配置) · [⚙️ BIOS 设置](#%EF%B8%8F-bios-设置) · [✅ 驱动状态](#-驱动状态) · [📦 EFI 配置](#-efi-配置) · [🚀 快速开始](#-快速开始)**

</div>

---

## 🖥️ 硬件配置

| 组件 | 型号 | 规格 | 驱动情况 |
|:-----|:-----|:-----|:--------:|
| **型号** | MSI MAG B460M MORTAR | MS-7C82 | ✅ |
| **主板** | MSI MAG B460M MORTAR | Intel B460 (Comet Lake-S) | ✅ |
| **CPU** | Intel Core i5-10400F | 6C/12T @ 2.90-4.30 GHz | ✅ 睿频正常 |
| **独显** | AMD Radeon RX 5500 | 4 GB (Navi 14, 1002-7340) | ✅ 原生支持 |
| **内存** | DDR4 | 32 GB | ✅ |
| **系统盘** | WD_BLACK SN750 | 2 TB NVMe SSD | ✅ |
| **数据盘** | GLOWAY STK1.5TS3-S7 | 1.5 TB SATA SSD | ✅ |
| **声卡** | Realtek ALC1200 | HDA: 10EC-0B00 | ✅ |
| **有线网卡** | Realtek RTL8125 2.5GbE | 10EC-8125 | ✅ |
| **显示器** | 27M2U-D | 3840×2160 4K @ HDMI | ✅ |

> **注意**：i5-10400F 为无核显版本（F 后缀），必须使用独立显卡输出。RX 5500 (Navi 14) 在 macOS 中原生支持。

---

## ✅ 驱动状态

| 功能 | 状态 | 功能 | 状态 |
|:-----|:---:|:-----|:---:|
| **RX 5500** | ✅ 原生驱动 | **Metal** | ✅ |
| **Realtek ALC1200 声卡** | ✅ | **Realtek RTL8125 2.5GbE** | ✅ |
| **USB 2.0/3.0** | ✅ | **NVMe SSD** | ✅ |
| **CPU 睿频** | ✅ | **XCPM 电源管理** | ✅ |
| **4K 3840×2160** | ✅ | **睡眠/唤醒** | ✅ |
| **NVRAM** | ✅ | **SATA SSD** | ✅ |

---

## ⚙️ BIOS 设置

| 设置 | 值 | 说明 |
|:-----|:---:|:-----|
| **Secure Boot** | Disabled | macOS 不支持 |
| **Fast Boot** | Disabled | 避免启动问题 |
| **VT-x** | Enabled | 虚拟机支持 |
| **VT-d** | Disabled | 或添加 `dart=0` |
| **UEFI Boot** | Enabled | |
| **SATA Mode** | AHCI | |
| **XHCI Hand-off** | Enabled | USB 兼容 |
| **Above 4G Decoding** | Enabled | |

> 进入 MSI BIOS 方法：开机按 Del

---

## 📦 EFI 配置

### Kexts 清单

```
EFI/OC/Kexts/
├── Lilu.kext                   ✅ 内核补丁框架
├── VirtualSMC.kext             ✅ SMC 模拟
├── AppleALC.kext               ✅ 声卡驱动
├── RealtekRTL8125.kext         ✅ 2.5GbE 有线网卡驱动
├── USBPorts.kext               ✅ USB 定制映射
├── SMCProcessor.kext           ✅ CPU 传感器
├── SMCSuperIO.kext             ✅ 风扇/温度
├── RestrictEvents.kext         ✅ 事件限制
└── FeatureUnlock.kext          ✅ Sidecar/AirPlay 解锁
```

### SSDT 热补丁

```
EFI/OC/ACPI/
├── SSDT-PLUG.aml       ✅ CPU 电源管理 (XCPM)
├── SSDT-EC.aml         ✅ EC 仿冒 (Comet Lake 必需)
├── SSDT-USBX.aml       ✅ USB 电源属性
├── SSDT-SBUS.aml       ✅ SMBus 支持
├── SSDT-MCHC.aml       ✅ MCHC 仿冒
├── SSDT-HPET.aml       ✅ HPET IRQ 修复
└── SSDT-RTC0.aml       ✅ RTC 兼容性
```

---

## 🚀 快速开始

### 1️⃣ BIOS 配置

1. 开机按 Del 进入 BIOS
2. 按上述表格配置
3. 按 F10 保存退出

### 2️⃣ EFI 部署

```bash
diskutil list
sudo diskutil mount diskXs1
cp -R EFI /Volumes/EFI/
```

### 3️⃣ 生成三码（重要！）

使用 GenSMBIOS 或 OCAT 生成自己的 SMBIOS 序列号（建议 iMac20,1），不要直接使用本仓库的三码。

### 4️⃣ 安装 macOS Sequoia 15

1. 制作 macOS Sequoia 安装 U 盘
2. 替换 U 盘 EFI 分区中的 EFI 文件夹
3. 从 USB 启动 → 选择「Install macOS」
4. 按提示完成安装（约 2-3 次重启）

---

## 🔧 注意事项

- **i5-10400F** 是**无核显版本**（F 后缀），必须使用独立显卡，核显部分已禁用
- **AMD RX 5500** (Navi 14) 在 macOS 中**原生支持**，无需任何补丁
- **Realtek RTL8125 2.5GbE** 需使用 `RealtekRTL8125.kext`（非普通 RTL8111 驱动）
- Realtek ALC1200 声卡 layout-id 已配置
- MSI 主板推荐使用 SMBIOS **iMac20,1**
- 微星 B460M BIOS 界面友好，一般无需额外 DVMT 或 CFG Lock 修改
- ⚠️ **自行生成三码后再使用，不要直接使用本仓库的序列号**

---

## 📄 许可证

MIT License — 仅供学习和研究

> ⚠️ 在非 Apple 硬件上安装 macOS 可能违反 Apple EULA

---

<p align="center">
  <b>为 Hackintosh 社区贡献 ❤️</b><br/>
  <sub>MSI MAG B460M MORTAR · OpenCore 1.0.x · macOS Sequoia 15</sub>
</p>
