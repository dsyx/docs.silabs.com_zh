> You are viewing documentation for version: [2.3.1](https://docs.silabs.com/openthread/2.3.1/openthread-multiprotocol-overview/) | [Version History](https://docs.silabs.com/openthread/2.3.1/version-history)

---

- [Developing with OpenThread](developing-with-openthread.md)
- [Getting Started](getting-started.md)
- [Fundamentals](fundamentals.md)
- [OpenThread Developer's Guide](openthread-developer's-guide.md)
    - [Developing and Debugging](developing-and-debugging.md)
    - [OpenThread Border Router](openthread-border-router.md)
    - [Coexistence](coexistence.md)
    - **[Multiprotocol](multiprotocol.md)**
    - [Bootloading](bootloading.md)
    - [Non-Volatile Memory](non-volatile-memory.md)
    - [Security](security.md)
    - [Performance](performance.md)
- [API Reference Guide](https://docs.silabs.com/openthread/2.3.1/openthread-api/)

---

# OpenThread in Multiprotocol Applications <!-- omit from toc -->

本节提供了多协议应用程序的相关背景信息，以及在多协议应用程序中使用 OpenThread 的详细信息，包括动态多协议和并发多协议模型。

- [Multiprotocol Fundamentals](../Documents/UG103.16/ug103-16-multiprotocol-fundamentals.pdf)：描述了四种多协议模式，讨论了在针对多协议实现选择协议时的注意事项，并回顾了 Radio Scheduler，这是动态多协议解决方案所必需的组件之一。
- [Dynamic Multiprotocol User's Guide](../Documents/UG305/ug305-dynamic-multiprotocol-users-guide.pdf)：介绍了如何实现动态多协议解决方案。
- [Dynamic Multiprotocol Development with Bluetooth and OpenThread](../Documents/AN1265/an1265-openthread-bluetooth-dynamic-multiprotocol-gsdk-v3x.pdf)：提供了使用 Bluetooth 和 OpenThread 开发动态多协议应用程序的详细信息。
- [Running Zigbee, OpenThread, and Bluetooth Concurrently on a Linux Host with a Multiprotocol Co-Processor](../Documents/AN1333/an1333-concurrent-protocols-with-802-15-4-rcp.pdf)：描述了如何在 Linux 主机处理器上运行 Zigbee EmberZNet、OpenThread 和 Bluetooth 网络栈的任何组合，并与具有多协议和多 PAN 支持的单个 EFR32 Radio Coprocessor（RCP）进行交互。它还描述了如何在 EFR32 上将 Zigbee 协议栈作为网络协处理器（NCP）与 OpenThread RCP 一起运行。
- [Running Zigbee and OpenThread Concurrently on a System-on-Chip](../Documents/AN1418/an1418-concurrent-mp-soc.pdf)：描述了如何在片上系统（SoC）上运行 Zigbee、OpenThread 网络栈以及 Zigbee 应用层的组合。
