> You are viewing documentation for version: [2.3.1](https://docs.silabs.com/openthread/2.3.1/openthread-coexistence-overview/) | [Version History](https://docs.silabs.com/openthread/2.3.1/version-history)

---

- [Developing with OpenThread](developing-with-openthread.md)
- [Getting Started](getting-started.md)
- [Fundamentals](fundamentals.md)
- [OpenThread Developer's Guide](openthread-developer's-guide.md)
    - [Developing and Debugging](developing-and-debugging.md)
    - [OpenThread Border Router](openthread-border-router.md)
    - **[Coexistence](coexistence.md)**
    - [Multiprotocol](multiprotocol.md)
    - [Bootloading](bootloading.md)
    - [Non-Volatile Memory](non-volatile-memory.md)
    - [Security](security.md)
    - [Performance](performance.md)
- [API Reference Guide](https://docs.silabs.com/openthread/2.3.1/openthread-api/)

---

# Coexistence <!-- omit from toc -->

本节描述了实施托管式共存以改善 2.4 GHz IEEE 802.11b/g/n Wi-Fi 与其他 2.4 GHz 无线电的共存，这些无线电包括基于 IEEE 802.15.4 的无线电，如 Zigbee® 和 OpenThread。

- [Wi-Fi Coexistence Fundamentals](../Documents/UG103.17/ug103-17-wi-fi-coexistence-fundamentals.pdf)：介绍了改善 2.4 GHz IEEE 802.11b/g/n Wi-Fi 与其他 2.4 GHz 无线电（如 Bluetooth、Bluetooth Mesh、Bluetooth Low Energy 以及基于 IEEE 802.15.4 的无线电，如 Zigbee 和 OpenThread）共存的方法。
- [Zigbee and OpenThread Coexistence with Wi-Fi](../Documents/AN1017/an1017-coexistence-with-wifi.pdf)：详细说明了 Wi-Fi 对 Zigbee 和 Thread 的影响以及改善共存的方法。首先，描述了在 Zigbee/Thread 和 Wi-Fi 无线电之间没有直接交互的情况下改善共存的方法。其次，描述了 Silicon Labs 的 Packet Traffic Arbitration（PTA）可用于协调 2.5 GHz 射频流量，以实现 Zigbee/Thread 和 Wi-Fi 无线电的共存（仅适用于 EFR32MG）。
- [Configuring Antenna Diversity for OpenThread](../Documents/AN1294/an1294-configuring-antenna-diversity-for-openthread.pdf)：描述了如何使用 Simplicity Studio 5 中的 Project Configurator 和 Component Editor 配置 OpenThread 应用程序中的天线多样性。
