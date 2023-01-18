# UG103.6: Bootloader Fundamentals (Rev. 2.0) <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1 Standalone Bootloader](#11-standalone-bootloader)
  - [1.2 Application Bootloader](#12-application-bootloader)
- [2. About the Gecko Bootloader](#2-about-the-gecko-bootloader)
  - [2.1 Features](#21-features)
    - [2.1.1 Field-Updateable](#211-field-updateable)
      - [Series 1](#series-1)
      - [Series 2](#series-2)
    - [2.1.2 Secure Boot](#212-secure-boot)
    - [2.1.3 Signed GBL Update Image File](#213-signed-gbl-update-image-file)
    - [2.1.4 Encrypted GBL Update File](#214-encrypted-gbl-update-file)
- [3. Memory Space for Bootloading](#3-memory-space-for-bootloading)
- [4. Design Decisions](#4-design-decisions)

---

本文档介绍了 Silicon Labs 网络设备的引导加载（Bootloading）。描述了 Standalone Bootloader 和 Application Bootloader 的概念，并讨论了它们的相对优势和劣势。此外，还介绍了每种方法的设计和实现细节。最后，描述了 bootloader 的文件格式。

# 1. Introduction

Bootloader 是一个存储在预留闪存中的程序，它可以初始化设备、更新固件映像，并可能执行一些完整性检查。无论是通过串行通信还是 OTA（Over The Air）方式，都可以按需进行固件映像更新。生产级编程通常在产品制造过程中完成，但有时会希望能够在生产完成后进行重新编程。更重要的是，这能够在设备部署后更新具有新特性和错误修复的固件。这使得更新固件映像成为可能。

Silicon Labs 支持不使用 Bootloader 的设备，但这些设备需要外部硬件来更新固件，如 Debug Adapter（Silicon Labs ISA3 或 WSTK（Wireless Starter Kit））或第三方的 SerialWire/JTAG 编程设备。没有 Bootloader 的设备在部署后无法使用受支持的方式更新固件，因此 Silicon Labs 强烈推荐使用 Bootloader。

在 2017 年 3 月，Silicon Labs 推出了 Gecko Bootloader，这是一个可通过 Simplicity Studio IDE 配置的代码库，用于生成可与各种 Silicon Labs 协议栈一起使用的 Bootloader。Gecko Bootloader 可以与 EFM32 和 EFR32 Series 1 及之后的设备一起使用。在 2021 年 12 月，Gecko Bootloader 被重构为一个基于组件的设计，并与 GSDK（Gecko SDK Suite） 4.0 一起发布。这个新版本记录在 *UG489: Silicon Labs Gecko Bootloader User’s Guide for GSDK 4.0 and Higher* 以及其他文档中。用于旧版本的文档随其各自的 SDK 安装。

Gecko Bootloader 使用一个定制的更新映像文件格式。Gecko Bootloader 生成的 Application Bootloader 使用的更新映像文件是 GBL（Gecko BootLoader）文件，这在 *UG489: Silicon Labs Gecko Bootloader User’s Guide for GSDK 4.0 and Higher* 中进行了描述。

引导加载一个固件更新映像有两种方式。第一种是 OTA，即通过无线网络，如下图所示。

<figure>
  <img src="images/Figure%201.1.%20OTA%20Bootloading%20Use%20Case.png"
       alt="Figure 1.1. OTA Bootloading Use Case"
       style="display:block; margin: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 1.1. OTA Bootloading Use Case</figcaption>
</figure>

第二种是通过设备的硬连线链路。下图展示使用 UART（Universal Asynchronous Receiver/Transmitter）、SPI（Serial Protocol Interface）或 USB（Universal Serial Bus）的 SoC（System on Chip）与使用 UART 或 SPI 的 NCP（Network Coprocessor）的串行引导加载用例。

<figure>
  <img src="images/Figure%201.2.%20Serial%20Bootloaders%20Use%20Cases.png"
       alt="Figure 1.2. Serial Bootloaders Use Cases"
       style="display:block; margin: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 1.2. Serial Bootloaders Use Cases</figcaption>
</figure>

Silicon Labs 网络设备以两种不同的模式使用 Bootloader 来执行固件更新：Standalone（也称为 Standalone Bootloader）和 Application（也称为 Application Bootloader）。Application Bootloader 又进一步地划分为使用外部存储和使用本地存储（用于下载更新映像）。这两种 Bootloader 类型将在接下来的两节中讨论。

本文档中描述的固件更新情况假定为源节点（通过串行或 OTA 链路将固件映像发送到目标的设备）通过其他方式获取新固件。例如，如果本地 ZigBee 网络上的一个设备已连接到以太网网关，则该设备可以通过互联网获取或接收这些固件更新。固件更新过程的这一必要部分取决于系统，这超出了本文档的范围。

## 1.1 Standalone Bootloader

Standalone Bootloader 是使用一个外部通信接口（如 UART 或 SPI）来获取应用程序映像的程序。Standalone 固件更新是一个单阶段的过程，这允许将应用程序映像放入闪存中来覆盖现有的应用程序映像，而无需应用程序本身的参与。Standalone Bootloader 与在闪存中正在运行的应用程序之间几乎没有交互。通常，应用程序与 Bootloader 交互的唯一时间是它请求重新引导到 Bootloader。一旦 Bootloader 运行，它就会通过物理连接（如 UART 或 SPI）或 OTA 方式接收包含（新）固件映像的固件更新包。

启动固件更新过程后，新代码将覆盖现有的栈和应用程序代码。若在此过程中发生任何错误，则无法恢复应用程序，并且必须重新开始该过程。有关将 Gecko Bootloader 配置为 Standalone Bootloader 的信息，请参阅 *UG489: Silicon Labs Gecko Bootloader User’s Guide for GSDK 4.0 and Higher*。

## 1.2 Application Bootloader

Application Bootloader 在正在运行的应用程序下载完更新映像文件后开始固件更新过程。Application Bootloader 期望映像存放在可访问的外部存储器中或主闪存（如果芯片具有足够的存储空间来支持此本地存储模型）中。

Application Bootloader 依赖应用程序来获取新的固件映像。应用程序可以以任何便捷的方式（UART、OTA 等）下载映像，但必须将其存储在一个称为下载空间（Download Space）的区域中。下载空间通常是外部存储器设备（如 EEPROM 或 dataflash），但在使用 Application Bootloader 的本地存储变体时，它也可以是芯片内部闪存的一部分。存储好新映像后，Application Bootloader 将会被调用以验证新映像并将其从下载空间复制到闪存中。

由于 Application Bootloader 不参与映像的获取，并且映像在固件更新过程开始之前下载，因此下载错误不会对正在运行的映像产生负面影响。下载过程可以随时重新开始或暂停。可以在开始固件更新过程之前验证所下载映像的完整性，以防止损坏或无功能的映像被应用。

可以将 Gecko Bootloader 配置为接受一个多升级映像的列表，以尝试验证和应用。这允许 Gecko Bootloader 存储更新映像的备份副本，如果第一个映像损坏，它可以访问该副本。

注意，EmberZNet NCP 平台不使用 Application Bootloader，因为应用程序代码驻留在 Host 上而不是直接驻留在 NCP 上。取而代之的是，充当串行协处理器的设备将使用 Standalone Bootloader，该 Bootloader 旨在通过与预期的 NCP 固件使用的相同串行接口接受代码。然而，Host 的应用程序（与 NCP 位于不同的 MCU）可以使用任何合适的引导加载方案。

有关 Application Bootloader 的详情，可以参阅 *UG489: Silicon Labs Gecko Bootloader User’s Guide for GSDK 4.0 and Higher*。

# 2. About the Gecko Bootloader

Silicon Labs Gecko Bootloader 是一个可配置的代码库，可以与所有较新的 Silicon Labs Gecko MCU 和 Wireless MCU 一起使用。它使用称为 GBL 文件的特殊格式的更新映像文件。Gecko Bootloader 在 Series 1 设备上采用一个两阶段的设计，其中最小的 First Stage Bootloader 用于更新 Main Bootloader。

在 Series 2 设备上，First Stage Bootloader 被 SE（Secure Engine，安全引擎）取代，Gecko Bootloader 仅包含 Main Bootloader。SE 可以是基于硬件的，也可以是虚拟的（软件）。如果基于硬件，那么实现可能具有或不具有 SV（Secure Vault，安全保险库）。在本文档中，将使用以下约定。

* HSE - Hardware Secure Engine, either with or without Secure Vault if not specified
* VSE - Virtual Secure Engine
* SE - Secure Engine (either HSE or VSE, in general)

具有 First Stage Bootloader 或 SE 可以对 Main Bootloader 进行现场更新，包括添加新功能、更改通信协议、添加新安全特性和修复程序等。Gecko Bootloader 由三个组件组成：

* **Core** ：Bootloader 核心包含两个 Bootloader 阶段的主要功能。它还包含写入内部主闪存、执行 Bootloader 更新以及重置（标记合适的重置原因）到应用程序的功能。
* **Driver** ：不同的引导加载应用需要不同的硬件驱动程序以供 Bootloader 的其他组件使用。
* **Component/Plugin** ：对于不同配置可选或可选择的 Main Bootloader 的所有部分都作为组件（在 GSDK 4.0 及更高版本中）或插件（先前版本中）实现。每个组件/插件都有一个通用的头文件，以及一个或多个实现。当前版本包含用于 UART 和 SPI 通信协议、SPI 闪存、内部闪存和不同加密操作等功能的组件。

## 2.1 Features

Gecko Bootloader 特性包括：

* 可现场更新（Field-updateable）
* 安全启动（Secure boot）
* 已签署的 GBL 固件更新映像文件（Signed GBL firmware update image file）
* 已加密的 GBL 固件更新映像文件（Encrypted GBL firmware update image file）

这些特性在随后的小节中进行了总结，并在 *UG489: Silicon Labs Gecko Bootloader User’s Guide for GSDK 4.0 and Higher* 中详细地描述。有关使用 Gecko Bootloader 的特定于协议的信息可在以下文档中找到：

* *AN1084: Using the Gecko Bootloader with EmberZNet*
* *UG235.06: Bootloading and OTA with Silicon Labs Connect SDK v2.x*
* *UG435.06: Bootloading and OTA with Silicon Labs Connect SDK v3.x*
* *AN1086: Using the Gecko Bootloader with Silicon Labs Bluetooth Applications*

### 2.1.1 Field-Updateable

#### Series 1

在 EFM32 和 EFR32 Series 1 设备上，Gecko Bootloader 的现场更新功能由一个两阶段的设计提供，该 Bootloader 具有 First Stage 和 Main Stage。Bootloader 的 First Stage 是不可现场更新的，并且只能通过读写内部闪存中的固定地址来更新 Main Bootloader。要执行 Main Bootloader 更新，正在运行的 Main Bootloader 会验证 Bootloader 更新映像的完整性和真实性，然后将其写入到内部闪存，并发出一个重新引导以进入到 First Stage Bootloader。First Stage Bootloader 会先验证 Main Bootloader 更新映像的完整性，然后再将其复制到 Main Bootloader 的位置上，从而完成更新。

#### Series 2

在 Series 2 设备上，Gecko Bootloader 的现场更新功能由 SE 提供。要执行 Main Bootloader 更新，正在运行的 Main Bootloader 会验证 Bootloader 更新映像的完整性和真实性，将其写入到内部闪存，并请求 SE 安装该更新。在将 Main Bootloader 更新映像复制到 Main Bootloader 位置并完成更新之前，SE 可选地验证 Main Bootloader 更新映像的真实性。可以使用相同的机制来更新 SE 自身。

### 2.1.2 Secure Boot

安全启动（Secure Boot）旨在防止不受信任的映像在设备上运行。启用安全启动后，Bootloader 会使用非对称加密技术在每次引导时强制执行应用程序映像的加密签名验证。使用的签名算法是 ECDSA-P256-SHA256。公钥在制造期间写入到设备，而私钥保密。这确保了应用程序是由可信方创建和签署的。

### 2.1.3 Signed GBL Update Image File

除了安全启动之外，Gecko Bootloader 还支持强制执行更新映像文件的加密签名验证。这允许 Bootloader 和应用程序在开始更新过程之前验证应用程序或 Bootloader 的更新是否来自受信任的源。使用的签名算法是 ECDSA-P256-SHA256。公钥与安全引导的密钥相同，在制造期间写入到设备，而私钥不分发。这可确保 GBL 文件是由可信方创建和签署的。

### 2.1.4 Encrypted GBL Update File

GBL 更新文件也可以加密，以防止窃听者获取明文固件映像。使用的加密算法是 AES-CTR-128，加密密钥在制造期间写入到设备。

# 3. Memory Space for Bootloading

Series 1 设备上的 Gecko Bootloader 的 First Stage 占用单个闪存页。在使用 2 kB 闪存页的设备上（如 EFR32MG1），这意味着 First Stage 占用 2 kB。

Main Bootloader 的大小取决于所需的功能。在典型的 Bootloader 配置中，Series 1 设备的 Main Bootloader 占用 14 kB 闪存，使 Bootloader 的总大小达到 16 kB。

Silicon Labs 建议为 Series 1 和 EFR32xG21 设备的 Bootloader 保留 16 kB，为 EFR32xG22 设备保留 24 kB。

在 EFR32xG1 设备上（Mighty Gecko、Flex Gecko 和 Blue Gecko 系列），Bootloader 位于主闪存中：

* First stage bootloader @ 0x0
* Main bootloader @ 0x800
* Application @ 0x4000

在 EFR32xG12 及更高版本的 Series 1 设备上，Bootloader 位于信息块（Information Block）的 Bootloader 区域中：

* Application @ 0x0
* First stage bootloader @ 0x0FE10000
* Main bootloader @ 0x0FE10800

在 EFR32xG21 上，Main Bootloader 位于主闪存中：

* Main bootloader @ 0x0
* Application @ 0x4000

在 EFR32xG22 上，Main Bootloader 位于主闪存中：

* Main bootloader @ 0x0
* Application @ 0x6000

在 EFR32xG23 上，Main Bootloader 位于主闪存中：

* Main bootloader @ 0x08000000
* Application @ 0x080060000

在 EFR32xG24 上，Main Bootloader 位于主闪存中：

* Main bootloader @ 0x08000000
* Application @ 0x080060000

# 4. Design Decisions

部署哪种类型的 Bootloader 取决于许多因素。请注意，平台类型和可用的闪存可能会限制 Bootloader 的选择。

与此相关的一些问题是：

* 设备从何处获得新的更新映像？是否通过网络协议进行的 OTA？是否使用单独的接口连接到互联网？
* 设备是否有外部存储器芯片来存储新的更新映像？如果没有，是否有足够的内部闪存来存储最大的预期应用程序映像（当前的和新下载的副本）？
* 如果设备通过 OTA 接收新的映像，它是否与持有下载映像的服务器相隔多跳？
* 需要什么样的映像安全性？
* 将使用哪种通信驱动程序（在单协议的情况下）？
* 用例是否需要多个协议？

Gecko Bootloader 平台的可配置设计意味着开发人员可以创建 Bootloader 以适应几乎任何的设计选择。有关详细信息，请参阅 *UG489: Silicon Labs Gecko Bootloader User’s Guide for GSDK 4.0 and Higher*。
