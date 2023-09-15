> You are viewing documentation for version: [2.3.1](https://docs.silabs.com/openthread/2.3.1/openthread-developing-debugging-overview/) | [Version History](https://docs.silabs.com/openthread/2.3.1/version-history)

---

- [Developing with OpenThread](developing-with-openthread.md)
- [Getting Started](getting-started.md)
- [Fundamentals](fundamentals.md)
- [OpenThread Developer's Guide](openthread-developer's-guide.md)
    - **[Developing and Debugging](developing-and-debugging.md)**
    - [OpenThread Border Router](openthread-border-router.md)
    - [Coexistence](coexistence.md)
    - [Multiprotocol](multiprotocol.md)
    - [Bootloading](bootloading.md)
    - [Non-Volatile Memory](non-volatile-memory.md)
    - [Security](security.md)
    - [Performance](performance.md)
- [API Reference Guide](https://docs.silabs.com/openthread/2.3.1/openthread-api/)

---

# Developing and Debugging Silicon Labs OpenThread Applications <!-- omit from toc -->

这些页面提供了有关开发和调试 OpenThread 应用程序的详细信息。内容包括：

- [Single-Band Proprietary Sub-GHz Support with OpenThread](../Documents/AN1350/an1350-openthread-single-band-proprietary-sub-ghz.pdf)：介绍如何使用 Silicon Labs OpenThread SDK 和 Simplicity Studio 5 以及兼容主板来配置 OpenThread 应用程序以在专有的 sub-GHz 频段上运行。它还提供了有关此特性支持的专有无线电 PHY 的详细信息。
- [Using OpenThread with Free RTOS](../Documents/AN1264/an1264-open-thread-with-free-rtos.pdf)：描述如何使用 FreeRTOS 构建 OpenThread 应用程序。
- [Configuring OpenThread Applications for Thread 1.3](../Documents/AN1372/an1372-configuring-for-thread-1-3.pdf)：提供有关配置 OpenThread SoC 和 Border Router 应用程序以使用 Thread 1.3 特性的说明。

## Development Tools <!-- omit from toc -->

**Simplicity Studio and the Simplicity IDE**：Simplicity Studio® 是所有 Silicon Labs 技术、SoC 和模块的统一开发环境。它为您提供了访问目标设备特定的 Web 和 SDK 资源、软件和硬件配置工具的途径，以及一个集成的开发环境（IDE），其中包括行业标准的代码编辑器、编译器和调试器。请访问 [silabs.com Simplicity Studio 页面](https://www.silabs.com/developers/simplicity-studio) 以下载工具并获取更多信息。

**Network Analyzer**：Simplicity Studio® 5（SSv5）的 Network Analyzer 可以让你调试复杂的无线系统。该工具捕获无线网络活动的踪迹，可以实时或在以后详细检查。有关更多信息，请参阅 Simplicity Studio 5 User's Guide 中的 [Network Analyzer 部分](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-tools-network-analyzer/)。

**Wireshark**：提供了 [Windows/Mac 用户](https://www.wireshark.org/download.html) 和 [Linux 用户](https://www.wireshark.org/docs/wsug_html_chunked/ChBuildInstallUnixInstallBins.html) 的下载说明。Simplicity Studio® 5 支持在 Silicon Labs 设备上运行的应用程序与 Wireshark 之间的[实时交互](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-testing-and-debugging/using-wireshark)。

**Energy Profiler**：Simplicity Studio® 5（SSv5）的 Energy Profiler 使您能够可视化单个设备、一个目标系统上的多个设备或一组交互式无线设备的能耗，以分析和改进这些系统的功耗性能。当前能耗的实时信息与程序计数器相关联，提供高级的能耗软件监控功能。它还与 Network Analyzer 网络分析工具进行了基本级别的集成。有关更多信息，请参阅 Simplicity Studio 5 User's Guide 中的 [Energy Profiler 部分](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-tools-energy-profiler/)。

**Simplicity Commander**：Simplicity Commander 是一个在生产环境中使用的通用工具。它通过一个简单的命令行界面（CLI）来调用，也可以进行脚本化。Simplicity Commander 使客户能够完成配置和构建应用程序和引导加载程序以及将映像刷写到其设备等关键任务。Simplicity Commander 可通过 Simplicity Studio 获取，也可以通过[特定于系统的安装程序](https://www.silabs.com/developers/mcu-programming-options#programming)进行下载。[Simplicity Commander User's Guide](../Documents/UG162/ug162-simplicity-commander-reference-guide.pdf) 提供了更多信息。

**Silicon Labs Configurator (SLC)**：SLC 提供了对应用程序配置和生成功能的命令行访问。[Software Project Generation and Configuration with SLC-CLI](../Documents/UG520/ug520-software-project-generation-configuration-with-slc-cli.pdf) 提供了有关下载和使用 SLC-CLI 工具的说明。
