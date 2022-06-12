# surface-pro-4-hackintosh-monterey

Surface Pro 4 + macOS 12 Monterey，无触摸屏/蓝牙/摄像头（Wi-Fi 使用 USB），其他功能完整

## ⚠ WWDC22 和 Ventura ⚠

看起来 Apple 对 Skylake 的支持已经走到了终点。不出意外的话 Monterey 将会成为本项目支持的最后一代 macOS。

如果你有任何想法，可以在 [Issue](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/issues) 区交流。

## ⭐ 现已支持 Monterey ⭐

经过数天的尝试，本项目现已支持直升 macOS 12 Monterey。

测试的过程非常复杂，也修改了非常多的参数，因此不在此处叙述了，如果有任何问题和想法可以开 [Issue](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/issues) 交流。

<img width="1000" alt="Monterey" src="https://user-images.githubusercontent.com/20179549/165629179-52eb9fea-d871-4d90-922a-ab8a95ee318d.png">

## 兼容性

版本历史和更新记录请参见 [Releases](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/releases)。

v3 是最后一个在 Big Sur 上经过测试的版本。后续版本理论上仍支持 Big Sur，但我已无法再进行测试。

v4 开始同时支持 Big Sur 和 Monterey。

## 硬件

组件|描述
-|-
型号|Surface Pro 4 8G 256G
CPU|Intel Core i5-6300U 2.5GHz
显卡|Intel HD Graphics 520 2GB
声卡|Realtek ALC3269（布局 3）
USB 无线网卡|COMFAST CF-811AC（自购）

## 目标对应机型

官方名称|MacBook Pro（13 英寸，2016 年，两个雷雳 3 端口）
-|-
标识符|MacBookPro13,1
部件号|MLL42xx/A、MLUQ2xx/A

## 来源 & 鸣谢

组件|来源于
-|-
[macOS](https://www.apple.com.cn/macos)|[Apple](https://www.apple.com.cn)
[OpenCore](https://github.com/acidanthera/OpenCorePkg)|[acidanthera](https://github.com/acidanthera)
DSDT|[bigsadan/surface-pro-4-hackintosh](https://github.com/bigsadan/surface-pro-4-hackintosh)
键盘 & 触摸板等|[VoodooI2C (基础)](https://github.com/VoodooI2C)，[Xiashangning/BigSurface](https://github.com/Xiashangning/BigSurface)
音频|[AppleALC](https://github.com/acidanthera/AppleALC) + 布局 3
无线网卡驱动|[chris1111/Wireless-USB-OC-Big-Sur-Adapter](https://github.com/chris1111/Wireless-USB-OC-Big-Sur-Adapter)

## 食用

1. 在 [Releases](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/releases) 里下载本项目的最新稳定版（点击 `Source code (zip)` 即可下载）

1. 按照 [Dortania 的官方教程](https://dortania.github.io/OpenCore-Install-Guide/installer-guide) 制作 macOS 启动盘

1. 把 EFI 文件夹粘贴到 U 盘任何一个空分区的根目录里，具体参考 Dortania 的官方教程（注意 U 盘文件系统要能够被 Surface UEFI 识别）

1. Surface 进 UEFI，关掉 Boot Lock、Secure Boot 和 TPM 等，引导顺序将 U 盘置首

1. 插上 U 盘，安装 macOS

1. Reset NVRAM（如果某天突然发现有任何功能变得不正常了，在尝试修复之前记得先 Reset NVRAM）

1. 开始愉快的水果之旅

## 注意事项

### Wi-Fi

如果你和我一样买了 `COMFAST CF-811AC` 的 USB 无线网卡或其他支持的网卡的话，Wi-Fi 是需要单独下载 [这个](https://github.com/chris1111/Wireless-USB-OC-Big-Sur-Adapter/releases) App 进行使用的。下载安装之后重启就可以在任务栏上找到图标了。

### 触摸板

在触摸板设置中关闭 `Force click and haptic feedback`，否则使用起来会有些奇怪（Voodoo 触摸板驱动会把触摸板按下当作 `Force Click`）

### 更新

默认配置禁用了 SIP。使用 Apple 提供的更新极有可能损坏系统。若要恢复更新，你可能需要重新启用 SIP。参考：[Disabling SIP](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/post-issues.html#disabling-sip)

**注意：非常不推荐通过系统更新直接升级系统。推荐手动下载系统安装程序进行安装升级。**

禁用更新可能会同时影响 App Store 内的应用更新。请自行权衡利弊。

**更新：有小伙伴反映在默认配置禁用了 SIP 的情况下仍然收到了 macOS 12.4 的更新。非常不推荐直接安装。如有需要，请手动下载安装程序。**

### 外接显示屏变紫色

下载 [patch-edid.rb](https://gist.github.com/adaugherity/7435890) 文件并运行，然后按照 [Writing to the macOS system partition](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/post-issues.html#writing-to-the-macos-system-partition) 的步骤将系统分区映射为可写分区，最后将生成的 `DisplayVendorID` 文件夹复制到 `/System/Library/Displays/Contents/Resources/Overrides` 目录下覆盖原文件。记得备份原来的文件。

**注意：修改系统分区文件可能会破坏系统完整性（不会损坏系统），从而无法收到 Apple 更新。修改文件时记得备份。**

另注意，目测每接一个新的显示屏都需要跑一遍上述步骤。

### DSDT

现 repo 里的 DSDT 主要是为了电源管理功能而存在的，如果发现与 DSDT 相关的问题（如 [#3](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/issues/3)）可自行打新的 DSDT 并替换。

### 成功安装，但在 OOBE 时重启

可以参考食用 [fix-tsc](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/tree/fix-tsc) 分支。[这个](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/tree/dda837c4bf170b88a8168fc2adc656aacd90aea9)、[这个](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/tree/0997f86718c328fdee8e2e03658a87d74dab9ed8) 和 [这个](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/tree/c8741d380ee2ea9ac10d098bebe657a99f6a44a6) 版本都可以试一试。也可尝试 [打了 TSC 补丁的 v4](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/tree/3675685f6eb66e0f0a5f7cefb8c8c5c31c48f8f2) 和 [打了 TSC 补丁的 v3](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/tree/e4bd74951c716cdd0f4cc8abefe156da4e6ff219)。更多信息请参考 [#5](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/issues/5)。

## 下载 & 更新记录

请参见 [Releases](https://github.com/ilharp/surface-pro-4-hackintosh-monterey/releases)

## LICENSE

All files and codes in this repo are **only for archiving**, and their copyright **belongs to the original author**.
