> You are viewing documentation for version: [2.3.1](https://docs.silabs.com/openthread/2.3.1/openthread-bootloading-overview/) | [Version History](https://docs.silabs.com/openthread/2.3.1/version-history)

---

- [Developing with OpenThread](developing-with-openthread.md)
- [Getting Started](getting-started.md)
- [Fundamentals](fundamentals.md)
- [OpenThread Developer's Guide](openthread-developer's-guide.md)
    - [Developing and Debugging](developing-and-debugging.md)
    - [OpenThread Border Router](openthread-border-router.md)
    - [Coexistence](coexistence.md)
    - [Multiprotocol](multiprotocol.md)
    - **[Bootloading](bootloading.md)**
    - [Non-Volatile Memory](non-volatile-memory.md)
    - [Security](security.md)
    - [Performance](performance.md)
- [API Reference Guide](https://docs.silabs.com/openthread/2.3.1/openthread-api/)

---

# Bootloading Embedded Applications <!-- omit from toc -->

引导加载（Bootloading）允许您在您的设备上更新应用程序固件映像。本节提供使用 Silicon Labs Gecko Bootloader 进行引导加载的相关背景信息。

- [Bootloader Fundamentals](../Documents/UG103.06/ug103-06-fundamentals-bootloading.pdf)：介绍了用于 Silicon Labs 网络设备的引导加载。讨论了 Gecko Bootloader 以及 legacy Ember 和 Bluetooth bootloaders，并描述了每种 bootloader 使用的文件格式。
- [Gecko Bootloader User's Guide](../Documents/UG489/ug489-gecko-bootloader-user-guide-gsdk-4.pdf)：描述了针对 EFR32 SoCs 和 NCPs 的 Silicon Labs Gecko Bootloader 的高级实现，并提供如何在 GSDK 4.0 及更高版本中开始使用 Gecko Bootloader 与 Silicon Labs 无线协议栈的相关信息。
- [Series 2 Secure Boot with RTSL](../Documents/AN1218/an1218-secure-boot-with-rtsl.pdf)：包含了在 Series 2 设备上配置和使用 Secure Boot with hardware Root of Trust 和 Secure Loader 的相关详细信息，包括如何配置签名密钥。这是 UG266: Silicon Labs Gecko Bootloader User's Guide 的附属文档。
- [Transitioning to the Updated Gecko Bootloader in GSDK 4.0 and Higher](../Documents/AN1326/an1326-gecko-bootloader-transitioning-guide.pdf)：Gecko Bootloader v2.x 是在 GSDK 4.0 中引入的，与 Gecko Bootloader v1.x 相比，有许多变化。此文档描述了这两个版本之间的差异，包括如何在 Simplicity Studio 5 中配置新的 Gecko Bootloader。
