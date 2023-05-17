> You are viewing documentation for version: [2.1 (latest)](https://docs.silabs.com/mcu-bootloader/2.1/) | [2.1](https://docs.silabs.com/mcu-bootloader/2.1) | [Version History](https://docs.silabs.com/mcu-bootloader/2.1/_version_history)
---

# Gecko Bootloader API Reference

[Additional Gecko Bootloader Documentation](https://docs.silabs.com/mcu-bootloader/2.1/#additional-gecko-bootloader-documentation) | [Release Notes](https://www.silabs.com/documents/public/release-notes/gecko-platform-release-notes-4.1.3.0.pdf) | [Downloads](https://www.silabs.com/products/development-tools/software/simplicity-studio)

要在你的应用程序中使用应用接口，请在你的应用程序中包含 api/btl_interface.h 头文件，并添加以下文件来进行构建：

* api/btl_interface.c
* api/btl_interface_storage.c

更多信息请参见 Application Interface 文档。

### Bootloader Components

Bootloader 由多个部分组成：

### Core

Bootloader Core 包含两个 Bootloader 阶段的主要功能。它还包含写入内部 Main Flash、执行 Bootloader 升级的功能，以及标记合适的重置原因来重置到应用程序。更多信息请参见 Core 文档。

### Driver

不同的 Bootloader 需要不同的硬件驱动，以便被 Bootloader 的其他组件使用。更多信息请参见 Driver 文档。

### Plugin

Bootloader 的所有部分，无论是可选的还是可替换的，都以插件的形式实现。每个插件都有一个通用的头文件，以及一个或多个实现。目前的版本包含了 UART 和 SPI 通信协议、SPI Flash 存储、内部 Flash 存储和不同的加密操作等功能的插件。关于不同插件的更多信息，请参见 Plugin 文档。

## Additional Gecko Bootloader Documentation

以下是适用于 Gecko SDK Suite 中大多数协议的 SDK 的 Gecko Bootloader 文档。一些协议的 SDK 还包括与该协议有关的特定信息。这些信息也包括在内。

### Developing with the Gecko Bootloader

* [x] [UG103.06: Bootloader Fundamentals](UG103.6/Bootloader%20Fundamentals.md) —— 介绍了 Silicon Labs 网络设备的引导加载。讨论了 Gecko Bootloader 以及 Legacy Ember Bootloader 和 Bluetooth Bootloader，并介绍了它们各自使用的文件格式。
* [x] [UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher](UG489/Silicon%20Labs%20Gecko%20Bootloader%20User%E2%80%99s%20Guide%20for%20GSDK%204.0%20and%20Higher.md) —— 描述了 Silicon Labs Gecko Bootloader 在 EFR32 SoC 和 NCP 上的高级实现，并提供有关如何在 GSDK 4.0 及更高版本中使用 Gecko Bootloader 与 Silicon Labs 无线协议栈的信息。
* [ ] [AN1326: Transitioning to the Updated Gecko Bootloader in GSDK 4.0 and Higher]() —— Gecko Bootloader v2.x 是在 GSDK 4.0 中引入的，与 Gecko Bootloader v1.x 相比有许多变化。本文档描述了这些版本之间的差异，包括如何在 Simplicity Studio 5 中配置新的 Gecko Bootloader。
* [x] [AN1218: Series 2 Secure Boot with RTSL](AN1218/Series%202%20Secure%20Boot%20with%20RTSL.md) —— 包含有关在 Series 2 设备上使用硬件 Root of Trust 和 Secure Loader 配置和使用 Secure Boot 的详细信息，包括如何提供 Signing Key。这是 [UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher]() 的附属文档。

### Protocol-Specific Information

* [ ] [AN1086: Using the Gecko Bootloader with Silicon Labs Bluetooth Applications]() —— 包含有关将 Gecko Bootloader 与 Silicon Labs Bluetooth 应用程序结合使用的详细信息。它是 [UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher]() 中提供的通用 Gecko Bootloader 实现信息的一个补充。
* [ ] [AN1085: Using the Gecko Bootloader with Silicon Labs Connect]() —— 包含有关将 Silicon Labs Gecko Bootloader 与 Connect 结合使用的详细信息。它是 [UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher]() 中提供的通用 Gecko Bootloader 实现信息的一个补充。
* [ ] [AN1084: Using the Gecko Bootloader with Zigbee EmberZNet]() —— 包含有关将 Silicon Labs Gecko Bootloader 与 EmberZNet 结合使用的详细信息。它是 [UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher]() 中提供的通用 Gecko Bootloader 实现信息的一个补充。
