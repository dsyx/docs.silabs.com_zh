> You are viewing documentation for version: [2.3.1](https://docs.silabs.com/openthread/2.3.1/openthread-security-overview/) | [Version History](https://docs.silabs.com/openthread/2.3.1/version-history)

---

- [Developing with OpenThread](developing-with-openthread.md)
- [Getting Started](getting-started.md)
- [Fundamentals](fundamentals.md)
- [OpenThread Developer's Guide](openthread-developer's-guide.md)
    - [Developing and Debugging](developing-and-debugging.md)
    - [OpenThread Border Router](openthread-border-router.md)
    - [Coexistence](coexistence.md)
    - [Multiprotocol](multiprotocol.md)
    - [Bootloading](bootloading.md)
    - [Non-Volatile Memory](non-volatile-memory.md)
    - **[Security](security.md)**
    - [Performance](performance.md)
- [API Reference Guide](https://docs.silabs.com/openthread/2.3.1/openthread-api/)

---

# Security <!-- omit from toc -->

Silicon Labs根据您使用的部件以及您的应用和生产需求提供一系列安全功能。除了可用的安全功能之外，本节还描述了与OpenThread相关的安全问题。

- [IoT Security Fundamentals](../Documents/UG103.05/ug103-05-fundamentals-security.pdf)：介绍了在实现 IoT（Internet of Things）系统时必须考虑的安全概念。使用 ioXt Alliance 的八大安全原则作为结构，清晰地界定了 Silicon Labs 提供的支持端点安全的解决方案，以及您在 Silicon Labs 框架之外必须做的工作。
- [Using Silicon Labs Secure Vault Features with OpenThread](../Documents/AN1329/an1329-using-secure-vault-openthread.pdf)：描述了如何在 OpenThread 应用程序中利用 Secure Vault 特性。重点介绍了特定的 PSA 特性，以及这些特性如何集成到 OpenThread 协议栈中。
- [Series 2 Secure Debug](../Documents/AN1190/an1190-efr32-secure-debug.pdf)：描述了如何锁定和解锁 EFR32 Gecko Series 2 设备的调试访问。描述了调试访问的多个方面，包括安全调试解锁。还包括了用于锁定和解锁调试访问的 DCI（Debug Challenge Interface）和 SE（Secure Engine）Mailbox Interface。
- [Production Programming of Series 2 Devices](../Documents/AN1222/an1222-efr32xg2x-production-programming.pdf)：提供了在生产环境中对 Series 2 设备进行编程、预配和配置的相关详细信息。涵盖了 Series 2 设备的 Secure Engine Subsystem，它运行易于升级的 SE（Secure Engine）或 VSE（Virtual Secure Engine）固件。
- [Anti-Tamper Protection Configuration and Use](../Documents/AN1247/an1247-efr32-secure-vault-tamper.pdf)：展示了如何使用 Secure Vault 在 EFR32 Series 2 设备上编程、预配和配置防篡改模块。
- [Authenticating Silicon Labs Devices using Device Certificates](../Documents/AN1268/an1268-efr32-secure-identity.pdf)：展示了如何使用 Secure Vault 对 EFR32 Series 2 设备进行身份验证，使用安全设备证书和签名。
- [Secure Key Storage](../Documents/AN1271/an1271-efr32-secure-key-storage.pdf)：解释了如何在 EFR32 Series 2 设备中使用 Secure Vault 安全地“包装”密钥，以便将其存储在非易失性存储中。
- [Programming Series 2 Devices Using the DCI and SWD](../Documents/AN1303/an1303-efr32-dci-swd-programming.pdf)：描述了如何通过 DCI 和 SWD 为 Series 2 设备预配和配置。
- [Integrating Crypto Functionality with PSA Crypto vs. Mbed TLS](../Documents/AN1311/an1311-mbedtls-psa-crypto-porting-guide.pdf)：描述了如何使用 PSA Crypto 与 Mbed TLS 将加密功能集成到应用程序中。
