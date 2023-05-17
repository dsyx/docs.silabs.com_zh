<p style="background: margin: 24px 0; padding: 8px; background: #f6f6f6; color: #555; font-size: 14pt;">文档版本：2.2</p>

# OpenThread

## OpenThread 文档

[Silicon Labs 文档](https://docs.silabs.com/openthread/latest/#silicon-labs-documentation) | [发布说明](https://www.silabs.com/documents/public/release-notes/open-thread-release-notes-2.2.0.0.pdf) | [开源文档](https://openthread.io/guides)

## 什么是 OpenThread ？

由 Google 发布的 OpenThread 是：

* 一个 Thread 网络协议的开源实现。Google Nest 发布的 OpenThread，使得 Nest 产品中使用的技术可以更广泛地提供给开发者，以加速互联家庭产品的开发。
* 与 OS 和平台无关，窄平台抽象层和小内存占用使其具有高度可移植性。它同时支持 SoC（System-On-Chip，片上系统）和 NCP（Network Co-Processor，网络协处理器）设计。
* 一个 Thread Certified Component（认证组件），实现了 Thread 1.1.1 Specification 中定义的所有特性，包括所有的 Thread 网络层（IPv6、6LoWPAN、IEEE 802.15.4 with MAC security、Mesh Link Establishment、Mesh Routing）和设备角色，以及 Border Router 的支持。

Thread 的更多信息可在 threadgroup.org 中找到。Thread 是 Thread Group, Inc 的注册商标。

## 谁支持 OpenThread ？

## 入门

所有的终端用户文档和指南都位于 openthread.io。如果你想做一些事情，比如...

了解更多关于 OpenThread 的特性和增强功能、在你的产品中使用 OpenThread、了解如何构建和配置一个 Thread 网络、将 OpenThread 移植到一个新的平台、在 OpenThread 之上构建一个应用程序、使用 OpenThread 认证一个产品... 那么 [openthread.io](https://openthread.io/) 就是一个合适的地方了。

注意：对于中国用户，终端用户文档可在 openthread.google.cn 中获得。

如果你有兴趣为 OpenThread 做贡献，请继续阅读。

## 贡献

我们希望你能为 OpenThread 做出贡献，帮助它变得更好！更多信息请参阅我们的[贡献指南](https://github.com/openthread/openthread/blob/master/CONTRIBUTING.md)。

贡献者需要遵守我们的[行为准则](https://github.com/openthread/openthread/blob/master/CODE_OF_CONDUCT.md)和[编码约定与风格指南](https://github.com/openthread/openthread/blob/master/STYLE_GUIDE.md)。

## 版本管理

OpenThread 遵循语义化版本管理准则，以保证发布周期的透明度，并保持向后的兼容性。OpenThread 的版本管理独立于 Thread 协议规范的版本，但会明确指出它当前支持的规范版本。

## 许可

OpenThread 是在 BSD 3-Clause 许可证下发布的。可参阅 LICENSE 文件以了解更多信息。

请只在准确引用此软件发行版时使用 OpenThread 的名称和标志。不要以暗示你被 Nest、Google 或 The Thread Group 认可或与之有关联的方式使用这些标志。

## 需要帮助 ？

有很多途径可以获得对 OpenThread 的支持：

* Bugs and feature requests —— [提交到问题追踪器](https://github.com/openthread/openthread/issues)
* Stack Overflow —— [使用 openthread 标签发布问题](https://stackoverflow.com/questions/tagged/openthread)
* Google Groups —— [在 openthread-users 中讨论和发布消息](https://groups.google.com/forum/#!forum/openthread-users)
* openthread-users Google Group 是用户讨论 OpenThread 并与 OpenThread 团队直接互动的推荐场所。

## Silicon Labs 文档

### 入门

* [x] [Getting Started with Simplicity Studio 5 and the Gecko SDK](../SimplicityStudio/Simplicity%20Studio%205%20Users%20Guide/Getting%20Started.md) —— 描述了下载开发工具和包含 Silicon Labs OpenThread 的 Gecko SDK。介绍了 Simplicity Studio 5 的界面。
* [x] [QSG170: Silicon Labs OpenThread Quick Start Guide](QSG170/Silicon%20Labs%20OpenThread%20Quick-Start%20Guide.md) —— 提供关于使用 Silicon Labs OpenThread stack 配置、构建和安装应用程序的基本信息。
* [x] [UG103.11: Thread Fundamentals](UG103.11/Thread%20Fundamentals.md) —— 针对刚接触 OpenThread 的读者，包含了 Thread 的技术背景、技术概述，并描述了实施 Thread 解决方案时需要考虑的 Thread 的一些关键特性。
* [x] [UG103.01: Wireless Networking Fundamentals](../Others/UG103.1/Wireless%20Networking%20Application%20Development%20Fundamentals.md) —— 针对刚接触无线网络的读者，介绍了无线网络的一些基本概念。这些概念在其他基础知识文档中都有提及。

### 使用 OpenThread 进行开发

* [x] [AN1256: Using the Silicon Labs RCP with the OpenThread Border Router](AN1256/Using%20the%20Silicon%20Labs%20RCP%20with%20the%20OpenThread%20Border%20Router.md) —— 描述了使用 OpenThread Border Router GitHub 仓库和 Silicon Labs OpenThread Radio Co-Processor（RCP）应用程序来创建一个 Thread Border Router。
* [x] [AN1350: Single-Band Proprietary Sub-GHz Support with OpenThread](AN1350/Single-Band%20Proprietary%20Sub-GHz%20Support%20with%20OpenThread.md) —— 描述了如何使用 Silicon Labs OpenThread SDK、Simplicity Studio 5 和一个兼容的主板配置 OpenThread 应用，使其在 proprietary sub-GHz 频段上运行。它还提供了该特性支持的 proprietary radio PHY 的详细信息。
* [x] [AN1264: Using OpenThread with FreeRTOS](AN1264/Using%20Open%20Thread%20with%20FreeRTOS.md) —— 描述了如何用 FreeRTOS 构建 OpenThread 应用程序。
* [x] [AN1372: Configuring OpenThread Applications for Thread 1.3](AN1372/Configuring%20OpenThread%20Applications%20for%20Thread%201.3.md) —— 提供了配置 OpenThread SoC 和 Border Router 应用程序以使用 Thread 1.3 特性的说明。
* [x] [UG162: Simplicity Commander Reference Guide](../Others/UG162/Simplicity%20Commander%20Reference%20Guide.md) —— 描述了如何以及何时使用 Simplicity Commander 的命令行接口。

### 多协议

* [x] [UG103.16: Multiprotocol Fundamentals](../Others/UG103.16/Multiprotocol%20Fundamentals.md) —— 介绍了四种多协议模式，讨论了为多协议实现选择协议时的考虑因素，并检阅了动态多协议解决方案的必要组件：Radio Scheduler。
* [x] [AN1265: Dynamic Multiprotocol Development with Bluetooth and OpenThread in GSDK v3.x](../Others/AN1265/Dynamic%20Multiprotocol%20Development%20with%20Bluetooth%20and%20OpenThread%20in%20GSDK%20v3.x.md) —— 提供了使用 Bluetooth 和 OpenThread 开发动态多协议应用的细节。
* [x] [UG305: Dynamic Multiprotocol User's Guide](../Others/UG305/Dynamic%20Multiprotocol%20User's%20Guide.md) —— 描述了如何实现动态多协议解决方案。
* [ ] [AN1333: Running Zigbee, OpenThread, and Bluetooth Concurrently on a Linux Host with a Multiprotocol Co-Processor]() —— 描述了如何在 Linux 主机处理器上运行 Zigbee EmberZNet、OpenThread 和 Bluetooth 网络栈的任何组合，与支持多协议和 multi-PAN 的单个 EFR32 RCP 进行交互，以及如何在 EFR32 上作为 Zigbee NCP 与 OpenThread RCP 一起运行。

### 共存

* [ ] [UG103.17: Wi-Fi Coexistence Fundamentals]() —— 介绍了改善 2.4GHz IEEE 802.11b/g/n Wi-Fi 和其他 2.4GHz 无线电，如 Bluetooth、Bluetooth Mesh、Bluetooth Low Energy，以及 IEEE 802.15.4-based 无线电，如 Zigbee 和 OpenThread 共存的方法。
* [ ] [AN1017: Zigbee and Thread Coexistence with Wi-Fi]() —— 详细介绍了 Wi-Fi 对 Zigbee 和 Thread 的影响，以及改善共存的方法。首先，介绍了在 Zigbee/Thread 和 Wi-Fi 无线电之间没有直接互动的情况下改善共存的方法。然后，介绍了 Silicon Labs 的 PTA（Packet Traffic Arbitration，分组流量总裁）支持，以协调共处的 Zigbee/Thread 和 Wi-Fi 无线电的 2.5 GHz 射频流量（仅适用于 EFR32MG）。
* [ ] [AN1294: Configuring Antenna Diversity for OpenThread]() —— 描述了如何使用 Simplicity Studio 5 中的项目配置器和组件编辑器来配置 OpenThread 应用中的天线分集。

### 安全

* [ ] [UG103.05: IoT Endpoint Security Fundamentals]() —— 介绍了实施 IoT（Internet of Things，物联网）系统时必须考虑的安全概念。它以 ioXt 联盟的八项安全原则为结构，明确划分了 Silicon Labs 为支持端点安全而提供的解决方案，以及在 Silicon Labs 框架之外你必须做的事情。
* [ ] [AN1329: Using Silicon Labs Secure Vault Features with OpenThread]() —— 描述了如何在 OpenThread 应用中利用 Secure Vault 特性。它专注于特定的 PSA 特性，并强调这些特性是如何集成到 OpenThread 栈中的。
* [ ] [AN1190: Series 2 Secure Debug]() —— 描述了如何锁定和解锁 EFR32 Gecko Series 2 设备的调试权限。描述了调试访问的多个方面，包括安全调试解锁。还包括用于锁定和解锁调试访问的 DCI（Debug Challenge Interface，调试质询接口）和 SE（Secure Engine，安全引擎）邮箱接口。
* [ ] [AN1222: Production Programming of Series 2 Devices]() —— 提供了在生产环境中对 Series 2 设备进行编程、供应和配置的详细信息。涵盖了 Series 2 设备的安全引擎子系统，该系统运行易于升级的 SE（Secure Engine，安全引擎）或 VSE（Virtual Secure Engine，虚拟安全引擎）固件。
* [ ] [AN1247: Anti-Tamper Protection Configuration and Use]() —— 展示如何在 EFR32 Series 2 设备上使用 Secure Vault 对防篡改模块进行编程、供应和配置。
* [ ] [AN1268: Authenticating Silicon Labs Devices using Device Certificates]() —— 如何使用安全设备证书和签名通过 Secure Vault 认证 EFR32 Series 2 设备。
* [ ] [AN1271: Secure Key Storage]() —— 解释了如何使用 Secure Vault 安全地“包裹” EFR32 Series 2 设备中的密钥，以便它们可以被存储在非易失性存储器中。
* [ ] [AN1303: Programming Series 2 Devices Using the Debug Challenge Interface (DCI) and Serial Wire Debug (SWD)]() —— 介绍了如何通过 DCI 和 SWD 供应和配置 Series 2 设备。
* [ ] [AN1311: Integrating Crypto Functionality Using PSA Crypto Compared to Mbed TLS]() —— 描述了与 Mbed TLS 相比如何使用 PSA Crypto 将加密功能集成到应用中。

### 引导加载

* [x] [UG103.06: Bootloader Fundamentals](../GeckoBootloader/UG103.6/Bootloader%20Fundamentals.md) —— 介绍了 Silicon Labs 网络设备的引导加载。讨论了 Gecko Bootloader 以及 legacy Ember bootloader 和 legacy Bluetooth bootloader，并介绍了它们所使用的文件格式。
* [x] [UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher](../GeckoBootloader/UG489/Silicon%20Labs%20Gecko%20Bootloader%20User%E2%80%99s%20Guide%20for%20GSDK%204.0%20and%20Higher.md) —— 描述了针对 EFR32 SoC 和 NCP 的 Silicon Labs Gecko Bootloader 的高级实现，并提供了如何在 GSDK 4.0 及以上版本中开始使用 Gecko Bootloader 与 Silicon Labs 无线协议栈的信息。
* [ ] [AN1326: Transitioning to the Updated Gecko Bootloader in GSDK 4.0 and Higher]() —— GSDK 4.0 中引入的 Gecko Bootloader v2.x 与 Gecko Bootloader v1.x 相比包含了许多变化。本文档描述了版本之间的差异，包括如何在 Simplicity Studio 5 中配置新的 Gecko Bootloader。
* [x] [AN1218: Series 2 Secure Boot with RTSL](../GeckoBootloader/AN1218/Series%202%20Secure%20Boot%20with%20RTSL.md) —— 包含了在 Series 2 设备上配置和使用带有硬件信任根和安全加载器的安全启动的详细信息，包括如何供应签名密钥。这是 UG266: Silicon Labs Gecko Bootloader User's Guide 的配套文档。


### 非易失性数据存储

* [x] [UG103.07: Non-Volatile Data Storage Fundamentals](../Others/UG103.7/Non-Volatile%20Data%20Storage%20Fundamentals.md) —— 介绍了使用闪存的非易失性数据存储以及为 Silicon Labs 微控制器和 SoC 提供的三种不同的存储实现：Simulated EEPROM、PS Store 和 NVM3。
* [x] [AN1135: Using Third Generation Non-Volatile Memory (NVM3) Data Storage](../Others/AN1135/Using%20Third%20Generation%20Non-Volatile%20Memory%20(NVM3)%20Data%20Storage.md) —— 解释了如何在各种协议实现中使用 NVM3 作为非易失性数据存储。

### 测试

* [ ] [AN718: Manufacturing Test Overview]() —— 提供了对将射频测试和表征集成到标准测试流程中的不同选项的高级描述。它适用于从早期原型开发阶段过渡到制造生产环境并需要制造测试帮助的客户。
* [ ] [AN700.1: Manufacturing Test Guidelines for the EFR32]() —— 详细介绍了将射频测试和表征集成到标准测试流程中的不同选择。
* [ ] [Performance Results for Multi-PAN RCP for OpenThread and Zigbee]() —— 总结了针对并发 multiprotocol/multi-PAN 的 RCP（在主机处理器上运行 OpenThread 和 Zigbee），同时进行 Thread 和 Zigbee 吞吐量性能测试的结果。

### 性能

* [ ] [AN1142: Mesh Network Performance Comparison]() —— 检阅 Zigbee、Thread 和 Bluetooth mesh 网络，评估它们在性能和行为上的差异。
* [ ] [AN1141: Thread Mesh Network Performance]() —— 详细介绍了测试 Thread 网状网络性能的方法；结果旨在为设计实践和原则以及预期的现场性能结果提供指导。
