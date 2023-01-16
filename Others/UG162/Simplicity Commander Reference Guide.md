# UG162: Simplicity Commander Reference Guide (Rev. 2.4) <!-- omit in toc -->

- [1. 引言](#1-引言)
- [2. 文件格式概述](#2-文件格式概述)
  - [2.1 Motorola S-record (s37) 文件格式](#21-motorola-s-record-s37-文件格式)
  - [2.2 Update Image 文件格式](#22-update-image-文件格式)
  - [2.3 Intel HEX-32 文件格式](#23-intel-hex-32-文件格式)
- [3. 一般信息](#3-一般信息)
  - [3.1 安装 Simplicity Commander](#31-安装-simplicity-commander)
  - [3.2 命令行语法](#32-命令行语法)
  - [3.3 一般选项](#33-一般选项)
    - [3.3.1 Help (--help)](#331-help---help)
    - [3.3.2 Version (--version)](#332-version---version)
    - [3.3.3 Device (--device \<device name\>)](#333-device---device-device-name)
    - [3.3.4 J-Link 连接选项](#334-j-link-连接选项)
    - [3.3.5 调试接口配置](#335-调试接口配置)
    - [3.3.6 图形用户界面](#336-图形用户界面)
    - [3.3.7 Timestamp (--timestamp)](#337-timestamp---timestamp)
  - [3.4 输出和退出状态](#34-输出和退出状态)
- [4. EFR32 Custom Tokens](#4-efr32-custom-tokens)
  - [4.1 引言](#41-引言)
  - [4.2 Custom Token Groups](#42-custom-token-groups)
  - [4.3 创建 Custom Token Groups](#43-创建-custom-token-groups)
  - [4.4 定义 Tokens](#44-定义-tokens)
  - [4.5 存储区域](#45-存储区域)
  - [4.6 Token 文件格式说明](#46-token-文件格式说明)
  - [4.7 使用 Custom Token 文件](#47-使用-custom-token-文件)
  - [4.8 在任意位置使用 Custom Token 文件](#48-在任意位置使用-custom-token-文件)
- [5. 安全概述](#5-安全概述)
  - [5.1 安全储存](#51-安全储存)
  - [5.2 访问证书](#52-访问证书)
  - [5.3 Challenge 和命令签署](#53-challenge-和命令签署)
- [6. Simplicity Commander 命令](#6-simplicity-commander-命令)
  - [6.1 设备刷写命令](#61-设备刷写命令)
    - [6.1.1 刷写映像文件](#611-刷写映像文件)
    - [6.1.2 使用 IP 地址的刷写且不验证和重置](#612-使用-ip-地址的刷写且不验证和重置)
    - [6.1.3 刷写多个文件](#613-刷写多个文件)
    - [6.1.4 修补闪存](#614-修补闪存)
    - [6.1.5 使用输入文件进行修补](#615-使用输入文件进行修补)
    - [6.1.6 刷写 Tokens](#616-刷写-tokens)
  - [6.2 闪存验证命令](#62-闪存验证命令)
  - [6.3 存储器读取命令](#63-存储器读取命令)
    - [6.3.1 打印闪存内容](#631-打印闪存内容)
    - [6.3.2 将闪存内容转储到文件](#632-将闪存内容转储到文件)
  - [6.4 Token 命令](#64-token-命令)
    - [6.4.1 打印 Tokens](#641-打印-tokens)
    - [6.4.2 将 Tokens 转储到文件](#642-将-tokens-转储到文件)
    - [6.4.3 从映像文件中转储 Tokens](#643-从映像文件中转储-tokens)
    - [6.4.4 从 Token Groups 生成 C 头文件](#644-从-token-groups-生成-c-头文件)
  - [6.5 转换和修改文件命令](#65-转换和修改文件命令)
    - [6.5.1 合并两个文件](#651-合并两个文件)
    - [6.5.2 定义特定字节](#652-定义特定字节)
    - [6.5.3 定义 Tokens](#653-定义-tokens)
    - [6.5.4 转储文件内容](#654-转储文件内容)
    - [6.5.5 为安全启动签署应用程序](#655-为安全启动签署应用程序)
    - [6.5.6 使用硬件安全模块为安全启动签署应用程序](#656-使用硬件安全模块为安全启动签署应用程序)
    - [6.5.7 使用由硬件安全模块创建的签名为安全启动签署签署应用程序](#657-使用由硬件安全模块创建的签名为安全启动签署签署应用程序)
    - [6.5.8 为 Gecko Bootloader 添加 CRC32](#658-为-gecko-bootloader-添加-crc32)
    - [6.5.9 使用中间证书为安全启动签署应用程序](#659-使用中间证书为安全启动签署应用程序)
    - [6.5.10 添加 Trust Zone 解密密钥](#6510-添加-trust-zone-解密密钥)
    - [6.5.11 从 ELF 文件中提取 Sections](#6511-从-elf-文件中提取-sections)
  - [6.6 EBL 命令](#66-ebl-命令)
    - [6.6.1 打印 EBL 信息](#661-打印-ebl-信息)
    - [6.6.2 EBL 密钥生成](#662-ebl-密钥生成)
    - [6.6.3 EBL 文件创建](#663-ebl-文件创建)
    - [6.6.4 EBL 文件解析](#664-ebl-文件解析)
    - [6.6.5 来自 AAT 的存储器使用情况信息](#665-来自-aat-的存储器使用情况信息)
  - [6.7 GBL 命令](#67-gbl-命令)
    - [6.7.1 GBL 文件创建](#671-gbl-文件创建)
    - [6.7.2 带压缩的 GBL 文件创建](#672-带压缩的-gbl-文件创建)
    - [6.7.3 创建用于引导加载程序升级的 GBL 文件](#673-创建用于引导加载程序升级的-gbl-文件)
    - [6.7.4 创建用于安全元素升级的 GBL 文件](#674-创建用于安全元素升级的-gbl-文件)
    - [6.7.5 从应用程序创建已签署和已加密的 GBL 升级映像文件](#675-从应用程序创建已签署和已加密的-gbl-升级映像文件)
    - [6.7.6 创建与硬件安全模块一起使用的局部已签署和已加密的 GBL 升级文件](#676-创建与硬件安全模块一起使用的局部已签署和已加密的-gbl-升级文件)
    - [6.7.7 使用硬件安全模块创建已签署的 GBL 文件](#677-使用硬件安全模块创建已签署的-gbl-文件)
    - [6.7.8 GBL 文件解析](#678-gbl-文件解析)
    - [6.7.9 GBL 密钥生成](#679-gbl-密钥生成)
    - [6.7.10 生成签署密钥](#6710-生成签署密钥)
    - [6.7.11 使用硬件安全模块生成签署密钥](#6711-使用硬件安全模块生成签署密钥)
    - [6.7.12 使用硬件安全模块创建已签署的 GBL 文件](#6712-使用硬件安全模块创建已签署的-gbl-文件)
    - [6.7.13 从 ELF 文件创建 GBL 文件](#6713-从-elf-文件创建-gbl-文件)
    - [6.7.14 使用未加密的安全元素升级文件创建已加密的 GBL 文件](#6714-使用未加密的安全元素升级文件创建已加密的-gbl-文件)
    - [6.7.15 创建具有版本依赖的 GBL 文件](#6715-创建具有版本依赖的-gbl-文件)
  - [6.8 Kit 实用命令](#68-kit-实用命令)
    - [6.8.1 固件升级](#681-固件升级)
    - [6.8.2 Kit 信息探测](#682-kit-信息探测)
    - [6.8.3 适配器重置命令](#683-适配器重置命令)
    - [6.8.4 适配器调试模式命令](#684-适配器调试模式命令)
    - [6.8.5 列出适配器 IP 配置命令](#685-列出适配器-ip-配置命令)
    - [6.8.6 适配器 DHCP 命令](#686-适配器-dhcp-命令)
    - [6.8.7 设置静态 IP 配置命令](#687-设置静态-ip-配置命令)
  - [6.9 设备擦除命令](#69-设备擦除命令)
    - [6.9.1 擦除芯片](#691-擦除芯片)
    - [6.9.2 擦除区域](#692-擦除区域)
    - [6.9.3 擦除地址范围内的页](#693-擦除地址范围内的页)
  - [6.10 设备锁定和保护命令](#610-设备锁定和保护命令)
    - [6.10.1 调试锁定](#6101-调试锁定)
    - [6.10.2 调试解锁](#6102-调试解锁)
    - [6.10.3 写保护闪存范围](#6103-写保护闪存范围)
    - [6.10.4 写保护闪存区域](#6104-写保护闪存区域)
    - [6.10.5 禁用写保护](#6105-禁用写保护)
  - [6.11 设备实用命令](#611-设备实用命令)
    - [6.11.1 设备信息命令](#6111-设备信息命令)
    - [6.11.2 设备重置命令](#6112-设备重置命令)
    - [6.11.3 设备恢复命令](#6113-设备恢复命令)
    - [6.11.4 设备 Z-Wave QR Code 命令](#6114-设备-z-wave-qr-code-命令)
  - [6.12 外部 SPI 闪存命令](#612-外部-spi-闪存命令)
    - [6.12.1 擦除外部 SPI 闪存命令](#6121-擦除外部-spi-闪存命令)
    - [6.12.2 读取外部 SPI 闪存命令](#6122-读取外部-spi-闪存命令)
    - [6.12.3 写外部 SPI 闪存命令](#6123-写外部-spi-闪存命令)
  - [6.13 高级能量监视器命令](#613-高级能量监视器命令)
    - [6.13.1 测量时间窗口内的平均电流](#6131-测量时间窗口内的平均电流)
    - [6.13.2 将电流测量记录为时间序列数据](#6132-将电流测量记录为时间序列数据)
    - [6.13.3 触发事件开始记录](#6133-触发事件开始记录)
  - [6.14 串行线输出读取命令](#614-串行线输出读取命令)
    - [6.14.1 配置 SWO 速度](#6141-配置-swo-速度)
    - [6.14.2 读取 SWO 直到超时](#6142-读取-swo-直到超时)
    - [6.14.3 读取 SWO 直到找到标记](#6143-读取-swo-直到找到标记)
    - [6.14.4 转储 Hex 编码的 SWO 输出](#6144-转储-hex-编码的-swo-输出)
  - [6.15 NVM3 命令](#615-nvm3-命令)
    - [6.15.1 从设备读取 NVM3 数据](#6151-从设备读取-nvm3-数据)
    - [6.15.2 解析 NVM3 数据](#6152-解析-nvm3-数据)
    - [6.15.3 在文件中初始化 NVM3 区域](#6153-在文件中初始化-nvm3-区域)
    - [6.15.4 使用文本文件写入 NVM3 数据](#6154-使用文本文件写入-nvm3-数据)
    - [6.15.5 使用 CLI 选项写入 NVM3 数据](#6155-使用-cli-选项写入-nvm3-数据)
  - [6.16 CTUNE 命令](#616-ctune-命令)
    - [6.16.1 CTUNE 获取命令](#6161-ctune-获取命令)
    - [6.16.2 CTUNE 设置命令](#6162-ctune-设置命令)
    - [6.16.3 CTUNE 自动设置命令](#6163-ctune-自动设置命令)
  - [6.17 安全命令](#617-安全命令)
    - [6.17.1 获取设备状态](#6171-获取设备状态)
    - [6.17.2 生成密钥对](#6172-生成密钥对)
    - [6.17.3 将公钥写到设备](#6173-将公钥写到设备)
    - [6.17.4 从设备读取公钥](#6174-从设备读取公钥)
    - [6.17.5 配置锁定选项](#6175-配置锁定选项)
    - [6.17.6 锁定调试访问](#6176-锁定调试访问)
    - [6.17.7 安全调试解锁](#6177-安全调试解锁)
    - [6.17.8 禁用篡改](#6178-禁用篡改)
    - [6.17.9 使用安全元素的设备擦除](#6179-使用安全元素的设备擦除)
    - [6.17.10 禁用设备擦除](#61710-禁用设备擦除)
    - [6.17.11 Roll Challenge](#61711-roll-challenge)
    - [6.17.12 生成示例授权文件](#61712-生成示例授权文件)
    - [6.17.13 生成访问证书](#61713-生成访问证书)
    - [6.17.14 生成未签署的命令文件](#61714-生成未签署的命令文件)
    - [6.17.15 生成示例配置文件](#61715-生成示例配置文件)
    - [6.17.16 写入用户配置](#61716-写入用户配置)
    - [6.17.17 读取用户配置](#61717-读取用户配置)
    - [6.17.18 获取安全储存路径](#61718-获取安全储存路径)
    - [6.17.19 写入 AES 解密密钥](#61719-写入-aes-解密密钥)
    - [6.17.20 读取设备证书](#61720-读取设备证书)
    - [6.17.21 保险库设备证明](#61721-保险库设备证明)
  - [6.18 实用命令](#618-实用命令)
    - [6.18.1 密钥生成](#6181-密钥生成)
    - [6.18.2 生成签署密钥](#6182-生成签署密钥)
    - [6.18.3 Token 密钥](#6183-token-密钥)
    - [6.18.4 生成证书](#6184-生成证书)
    - [6.18.5 签署证书](#6185-签署证书)
    - [6.18.6 验证签名](#6186-验证签名)
    - [6.18.7 应用程序信息](#6187-应用程序信息)
    - [6.18.8 从 ELF 文件打印 Section Header 信息](#6188-从-elf-文件打印-section-header-信息)
  - [6.19 OTA 命令](#619-ota-命令)
    - [6.19.1 创建 OTA 引导加载程序文件](#6191-创建-ota-引导加载程序文件)
    - [6.19.2 创建 Null OTA 文件](#6192-创建-null-ota-文件)
    - [6.19.3 打印 OTA 文件信息](#6193-打印-ota-文件信息)
    - [6.19.4 签署 OTA 文件](#6194-签署-ota-文件)
    - [6.19.5 为外部签署创建 OTA 文件](#6195-为外部签署创建-ota-文件)
    - [6.19.6 外部签署 OTA 文件](#6196-外部签署-ota-文件)
    - [6.19.7 验证 OTA 文件的签名](#6197-验证-ota-文件的签名)
  - [6.20 Post-Build 命令](#620-post-build-命令)
    - [6.20.1 执行项目 Post-Build 文件](#6201-执行项目-post-build-文件)
  - [6.21 RPS 命令](#621-rps-命令)
    - [6.21.1 从二进制映像创建 RPS 文件](#6211-从二进制映像创建-rps-文件)
    - [6.21.2 从 ELF 映像创建 RPS 文件](#6212-从-elf-映像创建-rps-文件)
    - [6.21.3 从 Hex/s37 映像创建 RPS 文件](#6213-从-hexs37-映像创建-rps-文件)
- [7. 软件版本历史](#7-软件版本历史)

---

本文档描述了如何以及何时使用 Simplicity Commander 的 CLI（Command-Line Interface，命令行接口）。Simplicity Commander 支持所有 EFR32 Wireless SoCs、EFR32 Wireless SoC modules（如 MGM111 或 MGM12P）、EFM32 MCU families 和 EM3xx Wireless SOCs。目前不支持 EFM8 MCU families。

本文档适用于软件工程师、硬件工程师和发布工程师。Silicon Labs 建议您检阅本文档以熟悉 CLI 命令及其预期用途。您可以参考本文档的特定章节以根据需要访问操作信息。本文档还包括示例，以便您了解 Simplicity Commander 的实际操作。

本文档随 Simplicity Commander version 1.14 一起更新。请参阅 [7. 软件修订历史](#7-软件版本历史) 以获取当前版本及先前版本的应用程序的新命令和已修改命令的列表。

# 1. 引言

Simplicity Commander 是一个用于生产环境的单一、通用工具。可以使用一个简单的 CLI 来调用它，也可以编写脚本来使用它。客户能够使用 Simplicity Commander 来完成这些基本任务：

* 刷新他们自己的应用程序。
* 配置他们自己的应用程序。
* 为生产创建二进制文件。

Simplicity Commander 旨在支持 Silicon Labs Wireless STK 和 STK 平台。

本文档的主要目标读者是熟悉 EFR32 和 EM3xx 编程的软件工程师、硬件工程师和发布工程师。本参考指南描述了如何使用 Simplicity Commander CLI。它提供了有关 Simplicity Commander 和 Silicon Labs 引导加载程序（Bootloader）支持的文件格式的一般信息，并包括了有关使用 Simplicity Commander 命令、选项和参数的详细信息。它还包括示例命令行输入和输出，以便您更好地了解如何有效地使用 Simplicity Commander。

# 2. 文件格式概述

Simplicity Commander 使用不同的文件格式：.bin、.s37、.ebl、.gbl 和 .hex。每种文件格式的用途略有不同。Simplicity Commander 支持的文件格式总结如下。

## 2.1 Motorola S-record (s37) 文件格式

Silicon Labs 使用 Simplicity Studio 作为其 IDE（Integrated Development Environment，集成开发环境），并利用了适用于 ARM 平台的 IAR Embedded Workbench。该工具组合生成 Motorola S-record 文件（特别是 s37）作为其输出。（有关 Motorola S-record 文件格式的更多信息，请参阅 [http://en.wikipedia.org/wiki/S_record](http://en.wikipedia.org/wiki/S_record)）。在 Silicon Labs 开发中，s37 文件包含有关内置固件的编程数据，并且通常只代表固件的一个单独的部分 —— 应用程序固件或引导加载程序固件 —— 但不是两者都。可以使用 Simplicity Commander 的 `flash` 命令将 s37 格式的应用程序映像加载到受支持的目标设备中。s37 格式可以表示设备中任意闪存（Flash）字节的任意组合。Simplicity Commander 的 `convert` 命令可以用来读取多个 s37 文件和 hex 文件；将多个文件组合成一个文件以输出一个 s37 文件；并修改文件的各个字节。

## 2.2 Update Image 文件格式

Update Image 文件提供了一种高效且容错的映像格式，可以与 Silicon Labs 引导加载程序一起使用以更新应用程序，而无需特殊的编程设备。支持两种映像格式：GBL（Gecko Bootloader）格式，用于 Silicon Labs Gecko Bootloader（用于 EFR32 设备）和 EBL（Ember Bootloader）格式，用于 legacy Ember bootloaders。请参阅 *UG103.6: Application Development Fundamentals: Bootloading* 以了解有关在不同平台上使用这些映像文件格式和引导加载程序的更多详细信息。

Update Image 文件由 Simplicity Commander 的 `gbl create` 或 `ebl create` 命令生成。这些格式只能表示固件映像；它们不能用于保存 Simulated EEPROM token 数据（如 *AN703: Using Simulated EEPROM Version 1 and Version 2 for the EM35x and EFR32 Series 1 SoC Platforms* 所述）。GBL 升级文件可能包含在主闪存之外刷写的数据（GBL upgrade files may contain data that gets flashed outside the main flash.）。

引导加载程序可以通过 OTA（over-the-air）或受支持的外设接口（如串口）接收 Update Image 文件，并就地重新编程闪存。Update Image 文件通常用于后期开发和现场升级已制造的设备。

在开发期间，应使用 .s37 或 .hex 文件格式将引导加载程序加载到设备上。如果使用了支持现场（in-field）引导加载程序升级的 Gecko Bootloader，则可以使用 GBL Update Image 执行引导加载程序升级。对于其他引导加载程序或文件格式，请勿尝试将引导加载程序映像作为更新映像加载到设备上。

## 2.3 Intel HEX-32 文件格式

生产编程使用标准的 Intel HEX-32 文件格式。EFR32 芯片的正常开发过程涉及使用 s37 和 ebl 文件格式创建和编程映像。s37 和 ebl 文件旨在保存应用程序、引导加载程序、制造数据和其他要在开发过程中编程的信息。然而，s37 和 ebl 文件并不是为了保存整个芯片的单个映像的。例如，通常情况下，有一个针对引导加载程序的 s37 文件、一个针对应用程序的 s37 文件，一个针对制造数据的 s37 文件。由于生产编程主要是安装一个包含所有必要代码和信息的单一完整映像，因此使用的文件格式是 Intel HEX-32 格式。虽然 s37 和 hex 文件在功能上是相同的 —— 它们只是定义地址和放置在这些地址上的数据 —— Silicon Labs 在概念上将它们区别开来，即单个 hex 文件包含单个完整映像，该映像通常由多个 s37 文件导出。您可以使用 Simplicity Commander 的 `convert` 命令来读取多个 hex 文件和 s37 文件；将多个文件组合成一个文件以输出一个 hex 文件；并修改文件的各个字节。

**注意**：Simplicity Commander 能够以相同的方式处理 s37 和 hex 文件。可以使用 s37 文件执行的所有功能都可以使用 hex 文件执行。最后，关于生产编程，Simplicity Commander 的 `flash` 命令允许开发者将各种源加载到物理芯片上。`convert` 命令可用于将各种源合并到最终映像文件中，并在必要时修改该映像中的各个字节。

下表总结了 Simplicity Commander 使用的不同文件格式的输入和输出。

<table style="margin-left: auto; margin-right: auto;">
<caption style="white-space: nowrap;">Table 2.1. File Format Summary</caption>
<thead>
  <tr>
    <th rowspan="2"></th>
    <th colspan="5" style="text-align: center;">Inputs</th>
    <th colspan="5" style="text-align: center;">Outputs</th>
    <th rowspan="2" style="text-align: center; vertical-align: bottom">rps</th>
  </tr>
  <tr>
    <th style="text-align: center;">ebl</th>
    <th style="text-align: center;">s37</th>
    <th style="text-align: center;">hex</th>
    <th style="text-align: center;">bin</th>
    <th style="text-align: center;">chip</th>
    <th style="text-align: center;">ebl</th>
    <th style="text-align: center;">s37</th>
    <th style="text-align: center;">hex</th>
    <th style="text-align: center;">bin</th>
    <th style="text-align: center;">chip</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="text-align: center;">flash</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;"></td>
  </tr>
  <tr>
    <td style="text-align: center;">readmem</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
  </tr>
  <tr>
    <td style="text-align: center;">convert</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
  </tr>
  <tr>
    <td style="text-align: center;">ebl create</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
  </tr>
  <tr>
    <td style="text-align: center;">ebl parse</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
  </tr>
  <tr>
    <td style="text-align: center;">rps create</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;">X</td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;"></td>
    <td style="text-align: center;">X</td>
  </tr>
</tbody>
</table>

# 3. 一般信息

## 3.1 安装 Simplicity Commander

您可以使用 Simplicity Studio 或通过下载以下独立版本之一然后完成安装来安装 Simplicity Commander：

[https://www.silabs.com/documents/public/software/SimplicityCommander-Linux.zip](https://www.silabs.com/documents/public/software/SimplicityCommander-Linux.zip)

[https://www.silabs.com/documents/public/software/SimplicityCommander-Mac.zip](https://www.silabs.com/documents/public/software/SimplicityCommander-Mac.zip)

[https://www.silabs.com/documents/public/software/SimplicityCommander-Windows.zip](https://www.silabs.com/documents/public/software/SimplicityCommander-Windows.zip)

## 3.2 命令行语法

要执行 Simplicity Commander 命令，请启动一个 Windows 命令窗口，然后切换到 Simplicity Commander 目录。Simplicity Commander 中的一般命令行结构如下所示：

```text
commander [command] [options] [arguments]
```

* `commander` 是工具的名称。
* `command` 是指一个 Simplicity Commander 支持的命令，如 `flash`、`readmem`、`convert` 等。特定于命令的帮助提供了有关每个命令的附加信息。
* `option` 是指一个修饰命令操作的关键字。选项前面有 --（双破折号），如每个命令所述。一些命令有单字符的短版本，即前面有 -（单破折号）。有关单破折号的简写，请参阅特定于命令的帮助。
* `argument` 是 Simplicity Commander 启动时提供给它的信息项。当命令接受一个或多个输入文件时，通常会使用一个参数。
* 方括号表示*可选*参数，如本例所示：`commander flash [filename(s)] [options]`
* 尖括号表示*必需*参数，如本例所示：`commander readmem --output <filename>`

## 3.3 一般选项

### 3.3.1 Help (--help)

显示所有 Simplicity Commander 命令的帮助以及每个命令的命令特定帮助。

**命令行语法**

```sh
$ commander --help
```

**命令行用法输出**

Simplicity Commander help 显示所有 Simplicity Commander 命令的列表。下图就是一个例子。

<figure>
  <img src="images/Figure%203.1.%20Simplicity%20Commander%20Help.png"
       alt="Figure 3.1. Simplicity Commander Help"
       style="display:block; margin-left: auto; margin-right: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 3.1. Simplicity Commander Help</figcaption>
</figure>

要显示特定 Simplicity Commander 命令的帮助，请输入命令名称，后跟 `--help`。

**命令行输入示例**

```sh
$ commander flash --help
```

**命令行输出示例**

在下图中展示 Simplicity Commander 的 flash 命令的帮助。

<figure>
  <img src="images/Figure%203.2.%20Simplicity%20Commander%20Flash%20Command%20Help.png"
       alt="Figure 3.2. Simplicity Commander Flash Command Help"
       style="display:block; margin-left: auto; margin-right: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 3.2. Simplicity Commander Flash Command Help</figcaption>
</figure>

### 3.3.2 Version (--version)

显示 Simplicity Commander、J-Link DLL 和 EMDLL 的版本信息，以及检测到的 USB 设备列表。如果您将此选项与另一个命令或命令/选项结合使用，Simplicity Commander 会在执行任何命令之前显示此额外信息。

**命令行语法**

```sh
$ commander --version
```

**命令行用法输出**

Simplicity Commander version 显示版本信息。下图就是一个例子。

<figure>
  <img src="images/Figure%203.3.%20Simplicity%20Commander%20Version%20Information.png"
       alt="Figure 3.3. Simplicity Commander Version Information"
       style="display:block; margin-left: auto; margin-right: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 3.3. Simplicity Commander Version Information</figcaption>
</figure>

### 3.3.3 Device (--device \<device name\>)

为命令指定一个目标设备。如果提供此选项，则不会使用自动检测到的目标设备。在某些情况下，例如将 `convert` 与 `--token` 选项一起使用时，此选项是必需的。

为方便起见，Simplicity Commander 会尝试解析 `--device` 选项，使得通常不需要完整的部件号作为命令输入。例如，Simplicity Commander 将 `commander --device EFR32` 解释为表示所选设备是 EFR32，这会影响此特定设备的内存布局和可用特性。作为另一个示例，Simplicity Commander 将 `--device EFR32F256` 解释为具有 256 kB 闪存的 EFR32。

支持并建议总是使用完整的部件号，例如 `--device EFR32MG1P233F256GM48`。

**命令行语法**

```sh
$ commander <command> --device <device name>
```

**命令行输入示例**

```sh
$ commander device info --device Cortex M3
```

### 3.3.4 J-Link 连接选项

使用以下选项来选择一个 J-Link 设备以连接并用于需要连接到 kit 或调试器的任何操作。您可以通过 IP（使用 `--ip` 选项）或通过 USB（使用 `--serialno` 选项）进行连接，如以下示例所示。您一次只能使用其中一个选项。如果未提供任何选项，Simplicity Commander 会尝试连接到唯一的通过 USB 连接的 J-Link 适配器。

**命令行语法**

```sh
$ commander <command> --serialno <J-Link serial number>
```

**命令行输入示例**

```sh
$ commander adapter probe --serialno 440050184
```

**命令行用法**

```sh
$ commander <command> --ip <IP address>
```

**命令行输入示例**

```sh
$ commander adapter probe --ip 10.7.1.27
```

### 3.3.5 调试接口配置

在将调试器连接到目标设备时，使用 `--tif` 和 `--speed` 选项配置目标接口和时钟速度。

Simplicity Commander 支持使用 SWD（Serial Wire Debug，串行线调试）或 JTAG（Joint Test Action Group，联合测试行动组）作为目标接口。当前支持的所有 Silicon Labs 硬件都可以与 SWD 配合使用，而有些还可以与 JTAG 配合使用。自定义的硬件可能需要使用 JTAG。

可用的最大时钟速度通常取决于调试适配器、目标设备以及两者之间的物理连接。Silicon Labs kits 通常支持高达 1000 – 8000 kHz 的速度，具体取决于 kit 的型号。如果选择的时钟速度高于适配器支持的速度，时钟速度将回落到使用它支持的最高速度。如果调试连接不稳定或者在使用带有较长调试电缆的自定义硬件或电气连接不太理想而根本无法工作时，您可能需要选择较低的时钟速度。

如果未使用 `--tif` 和 `--speed` 选项，则默认配置为 SWD 和 4000 kHz。

**命令行语法**

```sh
$ commander <command> [--tif <target interface>] [--speed <speed in kHz>]
```

**命令行输入示例**

```sh
$ commander device info --tif SWD --speed 1000
```

**命令行输出示例**

```sh
Setting debug interface speed to 1000 kHz
Setting debug interface to SWD
Part Number    : EFR32BG1P332F256GJ43
Die Revision   : A2
Production Ver : 138
Flash Size     : 256 kB
SRAM Size      : 32 kB
Unique ID      : 000b57fffe0934e3
DONE
```

### 3.3.6 图形用户界面

显示一个供 Simplicity Commander 实验室使用的 GUI（Graphical User Interface，图形用户界面）。GUI 可在实验室中用于以下典型任务：

* 刷写设备映像
* 升级 Silicon Labs kit 固件和配置
* 设置设备的锁定特性

**命令行语法**

```sh
$ commander
```

### 3.3.7 Timestamp (--timestamp)

添加一个时间戳到 Simplicity Commander 的输出。

**命令行语法**

```sh
$ commander <command> --timestamp
```

**命令行用法输出**

显示来自 Simplicity Commander 的所有输出的时间戳。

**命令行输入示例**

```sh
$ commander device reset --timestamp
```

**命令行输出示例**

Simplicity Commander 显示设备重置命令的时间戳。

```text
17:00:39.194 Resetting chip...
DONE
```

## 3.4 输出和退出状态

Simplicity Commander 的退出状态可能采用几个不同的值。当操作成功完成时，Simplicity Commander 的退出状态为 0。发生任何错误时，退出状态为非零值。

Simplicity Commander 定义了以下的退出状态码。

| Exit Status | Description                                                                                                                                                               |
| :---------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0           | No error occured                                                                                                                                                          |
| -1          | Input error. For example, this could be a missing command line option, non-existent command, or an invalid filename.                                                      |
| -2          | Run time error. Used whenever anything goes wrong when executing the command. Examples include not being able to connect to a debug adapter or flash verification failed. |

**注意**：某些操作系统会将退出状态显示为一个无符号整数。在这些系统上，-1 会被解释为 255，-2 为 254，以此类推。

如果该应用程序崩溃，操作系统本身可能会创建其他退出码。退出码将始终是非零的，并且不受 Simplicity Commander 的控制。

除退出状态外，所有错误和潜在错误条件都在 Simplicity Commander 的输出中指示。所有错误都以前缀“ERROR:”显示。所有警告都以前缀“WARNING:”显示。

Simplicity Commander 的任何输出都将始终以“DONE”结尾。这并不表示操作成功，它仅表示执行已完成。

Windows 中的错误示例如下。

```bat
C:\>commander device info -s 440000000
ERROR: Unable to connect with device with given serial number
ERROR: Could not open J-Link connection.
DONE

C:\>echo %errorlevel%
-2
```

# 4. EFR32 Custom Tokens

## 4.1 引言

Simplicity Commander 支持为读取和写入定义 custom token groups。Custom tokens 与 manufacturing tokens 类似，但它的定义和位置是可配置的，以便满足不同的要求。

Simplicity Commander 有两种不同的方法来查找和使用 custom token 定义文件。为了使 Simplicity Commander 以与 regular token group 相同的方式处理 custom token 文件，该文件必须放置在特定位置，如 [4.2 Custom Token Groups](#42-custom-token-groups) 中所述。

另一个选择是使用 `--tokendefs` 命令行选项来替代 `--tokengroup` 选项。使用此方法，Simplicity Commander 在任意位置使用 token 定义文件，例如，在版本控制下。有关详细信息，请参阅 [4.8 在任意位置使用 Custom Token 文件](#48-在任意位置使用-custom-token-文件)。

## 4.2 Custom Token Groups

为了使 Simplicity Commander 像处理 regular token groups 一样处理 custom token 文件，文件必须放在特定的 `tokens` 文件夹中，并且文件名必须遵循一个特有的语法。

tokens 文件夹的位置和初始化取决于所使用的操作系统。

在 Windows 和 Linux 上，`tokens` 文件夹包含在 zip 文件中，并与安装目录中的可执行文件一起放置。

在 Mac OS X 上，首次在命令行上运行 `commander` 时，会在用户的主目录中自动生成名为 `~/Library/SimplicityCommander/tokens/` 的文件夹。例如，运行 `commander --help` 足以确保创建包含文件的文件夹。在这个 `tokens` 文件夹中，有一个名为 `tokens-example-efr32.json` 的文件。此文件提供了 Simplicity Commander 当前支持的 token 类型和位置的示例。

文件名的语法是 `tokens-<group name>-<architecture>.json`。`<group name>` 是 custom token group 的名称，可以是任意字符串。`<architecture>` 是描述 token 定义适用于哪些设备的字符串。下表列出了支持的体系结构字符串。

| Architecture | Devices                            |
| :----------- | :--------------------------------- |
| `efr32`      | All Series 1 EFR32 devices         |
| `efr32xg2`   | All Series 2 EFR32 devices         |
| `em3xx`      | All EM3xx devices                  |
| `efm32`      | All EFM32 devices (Series 0 and 1) |
| `ezr32`      | All EZR32 devices                  |

例如，要为 EFR32 Series 1 设备定义 token group myapp，文件名应该为 `tokens-myapp-efr32.json`。

## 4.3 创建 Custom Token Groups

要定义一个 custom token group，请使用以下命名约定将 `tokens-example-efr32.json` 复制到同一个目录中的一个新文件：`tokens-<groupname>-efr32.json`。

例如：`tokens-myapp-efr32.json`。

要验证 Simplicity Commander 可以看到新文件，请运行：

```sh
$ commander tokendump --help
```

您的 token group 的名称（例如，“myapp”）应被列为受支持的 token group，如：

```text
--tokengroup <tokengroup> which set of tokens to use. Supported: myapp, znet
```

## 4.4 定义 Tokens

JSON 文件中的每个 token 具有以下属性。

<table>
<thead>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>name</td>
    <td>
      The name of the token, which is used as an identifier when dumping or writing tokens.
    </td>
  </tr>
  <tr>
    <td>page</td>
    <td style="background-color: rgb(146, 208, 80);">
      The named memory region to use for the token. For more information, see section <a href="#45-存储区域">4.5 Memory Regions</a>.
    </td>
  </tr>
  <tr>
    <td>offset</td>
    <td style="background-color: rgb(0, 176, 240);">
      The offset in number of bytes from the start of the memory region at which to place the token.
    </td>
  </tr>
  <tr>
    <td>sizeB</td>
    <td>
      The size of the token in bytes.
      <ul>
        <li><span style="color: rgb(217, 30, 42);">A token of size 1 is interpreted as an unsigned 8-bit integer.</span></li>
        <li><span style="color: rgb(86, 85, 86);">A token of size 2 is interpreted as an unsigned 16-bit integer.</span></li>
        <li><span style="color: rgb(161, 185, 46);">A token of size 4 is interpreted as an unsigned 32-bit integer.</span></li>
        <li><span style="color: rgb(226, 127, 38);">Any other size is interpreted as a byte array of the given size.</span></li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>string</td>
    <td>
      Optional boolean. If this property is <code>true</code>, the token is interpreted as a zero terminated ASCII string instead of a byte array. The maximum string length is <code>sizeB - 1</code> because one byte is reserved for the zero terminator.
    </td>
  </tr>
  <tr>
    <td>description</td>
    <td style="background-color: rgb(0, 176, 240);">
      A plain text description of the token. This property is currently only used for documentation of the JSON file.
    </td>
  </tr>
</tbody>
</table>

## 4.5 存储区域

以下值是“page”选项中的有效数据：

USERDATA

User data page 是一个单独的闪存页，用于持久化的数据和配置。禁用调试锁时**不会**擦除 user data page。但是，它可以通过特定的页擦除来擦除。

User data page 位于地址 0x0FE00000。在 Series 1 EFR32 设备上为 2 kB，在 Series 2 EFR32 设备上为 1 kB。

LOCKBITSDATA

在 Series 1 EFR32 设备上，芯片自身使用 lock bits page 来配置闪存写入锁、调试锁、AAP 锁等。但是，此页的最后 1.5 kB 未被设备自身使用，并且具有在禁用调试锁时会被擦除的重要属性。通过 MSC 进行的常规批量擦除 —— 通常是通过执行 `commander device masserase` 或 `commander flash --masserase` 命令 —— 不会擦除 lock bits page。

在 Series 1 EFR32 设备上，lock bits page 位于地址 0x0FE04000，大小为 2 kB。此页中的 token 必须在这些设备上使用至少 0x200 的偏移量；否则，可能会与芯片功能发生冲突。

在 Series 2 EFR32 设备上，没有物理的 lock bits page。取而代之的是，LOCKBITSPAGE 区域被定义为主闪存块中最后一个闪存页的前 2 kB。这保持了向后兼容性，同时仍然确保在调试解锁期间擦除设备时擦除该区域中的任何数据。

## 4.6 Token 文件格式说明

Token 文件声明了在芯片上哪些值是为 manufacturing tokens 编程的。行由以下形式之一组成：

```text
<token-name> : <data>
<token-name> : !ERASE!
```

使用 token 文件时请遵循以下准则：

* 省略的 tokens 保持原样，不会在芯片上编程。
* Token 名称不区分大小写。
* 所有整数值都被解释为 BIG-endian 格式的十六进制数，并且必须以“0x”为前缀。
* 空白行和以 #（井号）开头的行将被忽略。
* 字节数组以十六进制格式给出，没有前导的“0x”。
* 数据集指定为 !ERASE! 的 token 将为全 0xFF。
* Token 数据可以是三种主要形式之一：字节数组、整数或字符串。
* 字节数组是一系列所需长度的十六进制数。
* 整数是 BIG-endian 十六进制数字，必须以“0x”为前缀。
* 字符串数据是一组带引号的 ASCII 字符。

## 4.7 使用 Custom Token 文件

请参阅 [4.1 引言](#41-引言) 以了解 custom token 文件的定义以及它们应该放置到哪些地方以便 Simplicity Commander 可以自动找到它们。要使用一个位于 `tokens` 文件夹中的 custom token 文件，请使用与 JSON 文件名相对应的 `--tokengroup` 选项来运行 Simplicity Commander。例如，如果文件名为 `tokens-myapp-efr32.json`，请使用此选项：

```text
--tokengroup myapp
```

要创建一个文本文件来作为 `flash` 或 `convert` 命令的输入，最简单的方法是从一个设备中转储当前数据开始。

例如：

```sh
$ commander tokendump -s 440050148 --tokengroup myapp --outfile mytokens.txt
```

然后可以修改 `mytokens.txt` 以包含所需的内容，然后以如下方式在刷写设备或创建映像时使用：

```sh
$ commander flash -s 440050148 --tokengroup myapp --tokenfile mytokens.txt
```

为了能够从应用程序中读取 custom token 数据，Simplicity Commander 提供了 `tokenheader` 命令，该命令生成一个可包含在应用程序中的 C 头文件。有关详细信息，请参阅 [6.4.4 从 Token Groups 生成 C 头文件](#644-从-token-groups-生成-c-头文件)。

## 4.8 在任意位置使用 Custom Token 文件

在某些情况下，将 custom token 定义文件放在文件系统中的某个位置会更方便（例如，如果它处于版本控制之下）。Simplicity Commander 通过 `--tokendefs` 选项支持此功能，该选项引用文件系统中任意位置的一个 JSON 文件。使用它来代替 `--tokengroup` 选项。

例如：

```sh
$ commander tokendump --tokendefs my_tokens.json --outfile mytokens.txt
$ commander flash --tokendefs my_tokens.json --tokenfile mytokens.txt
```

# 5. 安全概述

本章介绍了 Simplicity Commander 中的基本安全特性。

## 5.1 安全储存

安全储存（Security Store）是储存由 Simplicity Commander 中的 security 命令生成和使用的所有文件的位置。您可以使用 `commander security getpath` 命令找到安全储存的路径。除非 `--nostore` 选项与安全命令一起使用，否则 Simplicity Commander 将储存在安全储存中看到的所有密钥、证书和配置文件。文件的说明如下所示。

* **access_certificate.bin** – certificate delegating permission to unlock debug access of a device.
* **archive folder** – folder used to store all outdated files (for example, all files in the challenge folder are moved here when a challenge is rolled).
* **cert_key.pem** – private key used to sign unlock token.
* **cert_pubkey.pem** – public key used in certificate. Public key corresponding to *cert_key.pem*.
* **certificate_authorization.json** – configuration file used to define authorizations given by access certificate. May be edited.
* **challenge_xxx folder** – folder used to store files related to a challenge.
    * **unlock_payload_xxx.bin** – payload used to unlock secure debug access.
    * **unlock_command_to_be_signed_dd_mm_yyyy.bin** – command token that needs to be signed with *cert_key.pem*
* **command_key.pem** – private command key used to sign access certificate.
* **command_pubkey.pem** – public command key stored on device. Public key corresponding to *command_key.pem*.
* **user_configuration.json** – configuration file used in write config. May be edited.

运行 `commander security unlock` 命令时，Simplicity Commander 将使用所有可用的文件来尝试解锁调试访问。如果缺少任何内容，那么将要求您提供文件以作为命令的一个选项。除非使用 `--nostore` 选项，否则文件将储存在安全储存中。

## 5.2 访问证书

访问证书（Access Certificate）用于将对单个设备的访问权委托给另一个密钥，该密钥称为证书密钥。此方案支持将命令密钥保存在安全位置的安全模型，而证书密钥可用于更宽松的安全实践。

访问证书包含它适用的设备的序列号、它授予访问权限的操作的描述以及公共证书密钥。访问证书的概要如下图所示。

设备序列号唯一地标识每个设备。可以通过执行 `commander security status` 命令来显示它。`certificate_authorizations.json` 文件设置证书的授权。当前版本的 Simplicity Commander 不支持对授权文件进行任何修改，但会在以后的版本中提供。证书中的证书公钥对应的证书私钥用于生成解锁调试访问所需的签名。有关详细信息，请参阅 [5.3 Challenge 和命令签署](#53-challenge-和命令签署)。通过使用与写入设备的公共命令密钥相对应的私有命令密钥对证书进行签署来验证证书。证书的签署可以通过将未签署的证书传递给包含私钥的 HSM（Hardware Security Module，硬件安全模块）或使用 `--command-key` 选项将私钥提供给 Simplicity Commander（用于开发）来完成。

<figure>
  <img src="images/Figure%205.1.%20Access%20Certificate.png"
       alt="Figure 5.1. Access Certificate"
       style="display:block; margin-left: auto; margin-right: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 5.1. Access Certificate</figcaption>
</figure>

## 5.3 Challenge 和命令签署

需要签署以创建一个有效解锁命令的数据部分称为 *challenge*。安全元素（Secure Element）生成此随机数据。它保持不变，直到它被 [security rollchallenge](#61711-roll-challenge) 命令更新为新的随机值。

通过更新 challenge，任何现有的命令签署都将实际上失效，因为签名包含的数据部分已更改。这允许设备所有者在有限的时间内向其他人提供调试访问。

命令签名是通过对包含下图中黄色数据字段的二进制文件进行签署来创建的；Simplicity Commander 使用访问证书中公钥对应的私钥设置解锁命令 ID、命令参数和 security challenge。

[`security gencommand`](#61714-生成未签署的命令文件) 命令创建一个包含这些元素的文件，但不包含签名。如果证书私钥对用户不可用，则必须从另一方（例如 HSM）获得签名。如果用户持有证书私钥，Security Commander 可以使用 [`security unlock`](#6177-安全调试解锁) 命令创建已签署的解锁命令。通过将命令签名和访问证书传递给 Debug Challenge 接口，调试接口将暂时解锁，直到下次上电或引脚复位。

<figure id="figure_5_2">
  <img src="images/Figure%205.2.%20Unlock%20Command%20Signature.png"
       alt="Figure 5.2. Unlock Command Signature"
       style="display:block; margin-left: auto; margin-right: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 5.2. Unlock Command Signature</figcaption>
</figure>

# 6. Simplicity Commander 命令

本节包括以下有关使用每个 Simplicity Commander 命令的信息：

* 命令行语法
* 命令行输入示例
* 命令行输出示例

如果命令行语法与命令行输入示例相同，则仅包括前者。

Simplicity Commander 命令分为以下类别：

* [6.1 设备刷写命令](#61-设备刷写命令)
* [6.2 闪存验证命令](#62-闪存验证命令)
* [6.3 存储器读取命令](#63-存储器读取命令)
* [6.4 Token 命令](#64-token-命令)
* [6.5 转换和修改文件命令](#65-转换和修改文件命令)
* [6.6 EBL 命令](#66-ebl-命令)
* [6.7 GBL 命令](#67-gbl-命令)
* [6.8 Kit 实用命令](#68-kit-实用命令)
* [6.9 设备擦除命令](#69-设备擦除命令)
* [6.10 设备锁定和保护命令](#610-设备锁定和保护命令)
* [6.11 设备实用命令](#611-设备实用命令)
* [6.12 外部 SPI 闪存命令](#612-外部-spi-闪存命令)
* [6.13 高级能量监视器命令](#613-高级能量监视器命令)
* [6.14 串行线输出读取命令](#614-串行线输出读取命令)
* [6.15 NVM3 命令](#615-nvm3-命令)
* [6.16 CTUNE 命令](#616-ctune-命令)
* [6.17 安全命令](#617-安全命令)
* [6.18 实用命令](#618-实用命令)
* [6.19 OTA 命令](#619-ota-命令)
* [6.20 Post-Build 命令](#620-post-build-命令)
* [6.21 RPS 命令](#621-rps-命令)

## 6.1 设备刷写命令

本节中的命令都需要有效的调试连接才能与设备通信。在运行 `flash` 命令时，您通常总是会使用 J-Link 连接选项之一，但为了使它们简短明了，有意将其排除在大多数示例之外。

### 6.1.1 刷写映像文件

从指定地址开始，将指定文件名中的映像刷写到目标设备。地址值会被解释为十六进制数。受影响的字节将在写入前被擦除。如果映像包含任何局部闪存页，这些页将从设备中读取并在擦除页和写回之前用映像内容修补。写入后，受影响的闪存区域被读回并进行比较。最后，使用引脚复位来复位芯片，以开始执行代码。要连接的调试器由 J-Link 序列号（`--serialno` 选项）指示。`--binary` 选项可用于将所有文件类型解释为平面二进制文件，绕过对 GBL、S-record 或 Intel Hex 文件的任何解析。例如，您可以使用它来测试使用内部存储引导加载程序的固件升级。`--include-section` 和 `--exclude-section` 选项可以在刷写 ELF 文件时使用。

**命令行语法**

```sh
$ commander flash <filename> --address <address> --serialno <serial number> [--binary --include-section <section> --exclude-section <section>]
```

**命令行输入示例**

```sh
$ commander flash blink.bin --address 0x0 --serialno 440012345
```

连接到序列号为 440012345 的 J-Link 调试器，并将 blink.bin 中的映像刷写到目标设备，从地址 0 开始。

**命令行输出示例**

```text
Flashing blink.s37.
Flashing 2812 bytes, starting at address 0x00000000
Resetting...
Uploading flash loader...
Waiting for flashloader to become ready...
Erasing flash...
Flashing...
Verifying written data...
Resetting...
Finished!
DONE
```

### 6.1.2 使用 IP 地址的刷写且不验证和重置

使用指定的 IP 地址将指定文件名中的映像刷写到目标设备。刷写后不验证闪存中的数据，并且刷写后设备处于停机状态。

**命令行语法**

```sh
$ commander flash <filename> --ip <IP> --halt --noverify
```

**命令行输入示例**

```sh
$ commander flash blink.s37 --ip 10.7.1.27 --halt --noverify
```

使用 IP 地址 10.7.1.27 将 blink.s37 中的映像刷写到目标设备。刷写后不验证闪存中的数据，并且刷写后设备处于停机状态。

**命令行输出示例**

```text
Flashing blink.s37.
Flashing 2812 bytes, starting at address 0x00000000
Resetting...
Uploading flash loader...
Waiting for flashloader to become ready...
Erasing flash...
Flashing...
Finished!
DONE
```

### 6.1.3 刷写多个文件

将映像刷写到目标设备。任何重叠数据都被视为错误。

**命令行语法**

```sh
$ commander flash <filename> <filename>
```

**命令行输入示例**

```sh
$ commander flash blink.s37 userpage.hex
```

将 blink.s37 和 userpage.hex 中的映像刷写到目标设备。

**命令行输出示例**

```text
Adding file blink.s37...
Adding file userpage.hex...
Flashing 2812 bytes, starting at address 0x00000000
Resetting...
Uploading flash loader...
Waiting for flashloader to become ready...
Erasing flash...
Flashing...
Verifying written data...
Finished!
Flashing 2048 bytes, starting at address 0x0fe00000
Resetting...
Uploading flash loader...
Waiting for flashloader to become ready...
Erasing flash...
Flashing...
Verifying written data...
Resetting...
Finished!
DONE
```

### 6.1.4 修补闪存

将指定字节写入闪存。受影响的页将从设备中读取并在擦除页和写回之前使用此数据进行修补。当您使用 `--patch` 选项时，修补存储数据会被解释为无符号整数。可选的 `length` 参数用于定义字节数，最多 8 个字节。如果未指定长度，则默认为修补 1 个字节。

**命令行语法**

```sh
$ commander flash --patch <address>:<data>[:length]
```

**命令行输入示例**

```sh
$ commander flash --patch 0x120:0xAB --patch 0x3200:0xA5A5:2
```

将指定字节 0xAB 写入到地址 0x120，将 0xA5A5 写入到地址 0x3200。受影响的页将从设备中读取并在擦除页和写回之前使用此数据进行修补。

**命令行输出示例**

```text
Patching 0x00000120 = 0xAB...
Patching 0x00003200 = 0xA5A5...
Flashing 2048 bytes, starting at address 0x00000000
Resetting...
Uploading flash loader...
Waiting for flashloader to become ready...
Erasing flash...
Flashing...
Verifying written data...
Finished!
Flashing 2048 bytes, starting at address 0x00003000
Resetting...
Uploading flash loader...
Waiting for flashloader to become ready...
Erasing flash...
Flashing...
Verifying written data...
Resetting...
Finished!
DONE
```

### 6.1.5 使用输入文件进行修补

刷写指定的应用程序，同时修补映像文件和设备的闪存。如果文件名在文件内，则在写入映像之前修补这些字节。

**命令行语法**

```sh
$ commander flash <filename> --patch <address>:<data>[:length] --patch <address>:<data>[:length]
```

**命令行输入示例**

```sh
$ commander flash blink.s37 --patch 0x123:0x00FF0001:4 --patch 0x0FE00004:0x00
```

刷写 blink 应用程序，同时修补映像文件和设备的闪存。因为 0x123 在文件内，所以这些字节在写入映像之前被修补。此外，user page 将从设备中读取并在擦除页和写回之前使用此数据进行修补。

**命令行输出示例**

```text
Flashing blink.s37.
Patching 0x00000123 = 00FF0001...
Patching 0x0FE00004 = 00...
Flashing 4096 bytes, starting at address 0x00000000
Resetting...
Uploading flash loader...
Waiting for flashloader to become ready...
Erasing flash...
Flashing...
Verifying written data...
Finished!
Flashing 2048 bytes, starting at address 0x0fe00000
Resetting...
Uploading flash loader...
Waiting for flashloader to become ready...
Erasing flash...
Flashing...
Verifying written data...
Finished!
DONE
```

### 6.1.6 刷写 Tokens

本节描述如何使用新值从文本文件和/或命令行选项中刷写一个或多个 token(s)。Manufacturing tokens 是 Simplicity Commander 唯一支持的 token 类型；不支持 simulated EEPROM tokens。有关 manufacturing tokens 的更多信息，请参阅 *AN961: Bringing Up Custom Nodes for the EFR32MG and EFR32FG Families*。

`--tokengroup` 选项定义了使用哪组 tokens。Simplicity Commander 目前内置了对 `znet` token group 的支持。

Silicon Labs 建议使用 `tokendump` 命令从设备或映像文件中生成 token 文件，然后修改此文件以与 `--tokenfile` 选项一起使用。

**命令行语法**

```sh
$ commander flash --tokengroup <token group> -–token <TOKEN_NAME:value> –-tokenfile <filename>
```

**命令行输入示例**

```sh
$ commander flash --tokengroup znet --token TOKEN_MFG_STRING:"IoT Inc"
```

设置 token `MFG_STRING` 的值为 `IoT Inc`。`TOKEN_` 前缀是可选的，即 `TOKEN_MFG_STRING` 和 `MFG_STRING` 是等价的。

**命令行输入示例**

```sh
$ commander flash --tokengroup znet --tokenfile tokens.txt
```

设置在 tokens.txt 中指定的 tokens。处理文件中的所有 tokens，如果发现重复，将被视为错误。

**命令行输入示例**

```sh
$ commander flash --tokengroup znet --tokenfile tokens.txt --token TOKEN_MFG_STRING:"IoT Inc"
```

设置在 tokens.txt 中指定的 tokens。此外，将 `MFG_STRING` 设置为给定的值。处理命令行中指定的所有文件和 tokens，如果发现重复项，将被视为错误。

根据所使用的操作系统和 shell，可能需要一些转义符才能正确指定字符串。例如，在 Windows 7 Professional 命令提示符窗口的命令行中，执行以下命令：

```sh
$ commander flash --tokengroup znet --token "TOKEN_MFG_STRING:\"IoT Inc\""
```

**命令行输出示例**

```text
Flashing 2048 bytes to 0x0fe00000
Resetting...
Uploading flash loader...
Waiting for flashloader to become ready...
Erasing flash...
Flashing...
Verifying written data...
Resetting...
Finished!
DONE
```

## 6.2 闪存验证命令

`verify` 命令根据一组 files、tokens 和/或 patch 选项验证设备的内容，而无需向闪存写入任何内容。它的工作原理与 `flash` 命令的验证步骤类似，但实际上并没有先刷写。例如，`verify` 命令可用于验证微控制器上的应用程序是否符合您的预期。

**命令行语法**

`flash` 命令的所有选项和示例也适用于 `verify` 命令。除了 `--halt`、`--masserase` 和 `--noverify` 选项。

```sh
$ commander verify [filename] [filename ...] [patch options] [token options]
```

**命令行输入示例**

```sh
$ commander verify myimage.hex
```

**命令行输出示例**

```text
Parsing file myimage.hex...
Verifying 52000 bytes at address 0x00000000...OK!
Verifying 2048 bytes at address 0x0fe00000...OK!
DONE
```

## 6.3 存储器读取命令

`readmem` 命令从设备读取数据，可以将其存储到文件或以人类可读的格式打印。要从设备读取的位置和长度由 `--range` 和 `--region` 选项定义。您可以组合一个或多个范围和区域来读取闪存中的多个不同区域并将其组合到一个文件中。

**注意**：与 `flash` 一样，本节中的命令都需要有效的调试连接才能与设备通信。在执行 `readmem` 时，您通常总是会使用 J-Link 连接选项之一，但为了使示例简短明了，将其排除在示例之外。

`--range` 选项支持两种不同的范围格式：

* 第一种是 `<startaddress>:<endaddress>`，如 `--range 0x4000:0x6000`。该范围是非包含的，这意味着从 0x4000 到 0x5FFF 的所有字节都被读出。
* 第二种是 `<startaddress>:+<length>`，它需要一个地址来开始读取，以及要读取的字节数。例如，上一个示例的等效命令行输入是 `--range 0x4000:+0x2000`。

`--region` 选项需要带 @ 前缀的已命名闪存区域。下面列出了与 `--region` 选项一起使用的有效区域。

* Series 0 **EFM32**, **EZR32**, **EFR32**: `@mainflash`, `@userdata`, `@lockbits`, `@devinfo`
* Series 1 **EFM32**, **EFR32**: `@mainflash`, `@userdata`, `@lockbits`, `@devinfo`, `@bootloader`
* Series 2 **EFM32**, **EFR32**: `@mainflash`, `@userdata`, `@devinfo`
* **EM3xx**: `@mfb`, `@cib`, `@fib`

### 6.3.1 打印闪存内容

指定要从闪存读取的存储器范围并打印数据。

**命令行语法**

```sh
$ commander readmem --range <startaddress>:<endaddress>
```

或

**命令行语法**

```sh
$ commander readmem --range <startaddress>:+<length>
```

**命令行输入示例**

```sh
$ commander readmem --range 0x100:+128
```

从闪存地址 0x100 开始读取 128 字节并打印结果到标准输出。

**命令行输出示例**

```text
Reading 128 bytes from 0x00000100...
{address: 0 1 2 3 4 5 6 7 8 9 A B C D E F}
00000100: 12 F0 40 72 11 00 DF F8 C0 24 90 42 07 D2 DF F8
00000110: BC 24 90 42 03 D3 5F F0 80 72 11 00 01 E0 00 22
00000120: 11 00 DF F8 84 26 12 68 32 F0 40 72 0A 43 DF F8
00000130: 78 36 1A 60 70 47 80 B5 00 F0 90 FC FF F7 DD FF
00000140: 01 BD DF F8 70 16 09 68 08 00 70 47 38 B5 DF F8
00000150: 4C 06 00 F0 9F F9 05 00 ED B2 28 00 07 28 05 D0
00000160: 08 28 07 D1 00 F0 7C FC 04 00 0B E0 FF F7 E9 FF
00000170: 04 00 07 E0 40 F2 25 11 DF F8 3C 06 00 F0 B0 FC
DONE
```

### 6.3.2 将闪存内容转储到文件

读取指定 user page 的内容并将其存储到指定的文件中。文件格式将根据文件扩展名（.bin、.hex 或 .s37）自动检测。（有关文件格式的详细信息，请参阅 [2. 文件格式概述](#2-文件格式概述)）。

**命令行语法**

```sh
$ commander readmem --region <@region> --outfile <filename>
```

**命令行输入示例**

```sh
$ commander readmem --region @userdata --outfile userpage.hex
```

读取名为 userdata 的区域的内容，并将其存储在名为 userpage.hex 的输出文件中。

**命令行输出示例**

```text
Reading 2048 bytes from 0x0fe00000...
Writing to userpage.hex...
DONE
```

## 6.4 Token 命令

`tokedump` 命令生成 token 数据的文本转储。它可以将使用与 `convert` 命令相同的命令行选项的（一组）文件作为输入，或者使用与 `readmem` 命令相同的命令行选项的微控制器作为输入。

`tokendump` 的输出可以打印到标准输出或使用 `--outfile` 选项写到输出文件。使用 `--outfile` 选项时写入的文件适合修改和重新用作 `flash`、`verify` 或使用 `--tokenfile` 选项的 `convert` 命令的输入。

`tokendump` 始终需要使用 `--tokengroup` 选项来选择 token group。Token group 是为特定栈或应用程序定义的一组 token。Simplicity Commander 仅支持 `znet` token group。

Manufacturing tokens 是 Simplicity Commander 唯一支持的 token 类型；不支持 simulated EEPROM tokens。有关 manufacturing tokens 的更多信息，请参阅 *AN961: Bringing Up Custom Nodes for the EFR32MG and EFR32FG Families*。

### 6.4.1 打印 Tokens

**命令行语法**

```sh
$ commander tokendump --tokengroup <token group> [--token <token name>]
```

**命令行输入示例**

```sh
$ commander tokendump --tokengroup znet --token TOKEN_MFG_STRING --token TOKEN_MFG_EMBER_EUI_64
```

从设备读取选定的 tokens 并将其打印到标准输出。

**命令行输出示例**

```text
#
# The token data can be in one of three main forms: byte-array, integer, or string.
# Byte-arrays are a series of hexadecimal numbers of the required length.
# Integers are BIG endian hexadecimal numbers.
# String data is a quoted set of ASCII characters.
#
MFG_STRING : "IoT_Inc"
# MFG_EMBER_EUI_64: F0B2030000570B00
DONE
```

### 6.4.2 将 Tokens 转储到文件

此示例的工作方式类似于 [6.4.1 打印 Tokens](#641-打印-tokens)，不同之处在于其将输出写到文件，该文件适合与 `--tokenfile` 选项（`flash`、`verify` 和 `convert` 命令）一起使用。

**命令行语法**

```sh
$ commander tokendump --tokengroup <token group> [--token <token name>] --outfile <filename>
```

**命令行输入示例**

```sh
$ commander tokendump --tokengroup znet --outfile tokens.txt
```

从设备读取所有 tokens 并将其输出到名为 tokens.txt 的文件。

**命令行输出示例**

```text
Writing tokens to tokens.txt...
DONE
```

### 6.4.3 从映像文件中转储 Tokens

如果将输入文件提供给 `tokedump` 命令，那么将从一个或多个文件中读取输入，而不是从设备中读取。

在这种情况下，必须提供 `--device` 选项，因为 token 的位置可能因设备系列而异。

**命令行语法**

```sh
$ commander tokendump <filename> --tokengroup <token group> --device <device> [--outfile <filename>]
```

**命令行输入示例**

```sh
$ commander tokendump blink.hex --tokengroup znet --device EFR32MG1P --outfile tokens.txt
```

**命令行输出示例**

```text
Parsing file blink.hex...
DONE
```

### 6.4.4 从 Token Groups 生成 C 头文件

`tokenheader` 命令根据 custom token group 生成一个简单的头文件。生成的头文件包含预处理器定义，用于指定每个 token 的位置和大小。

有关 custom tokens 的详细信息，请参阅 [4. EFR32 Custom Tokens](#4-efr32-custom-tokens)。

**命令行语法**

```sh
$ commander tokenheader --tokengroup <group name> --device <target device> <filename>
```

**命令行输入示例**

```sh
$ commander tokenheader --tokengroup myapp --device EFR32MG1P233F256 my_tokens.h
```

**命令行输出示例**

```text
Writing token header file: my_tokens.h
DONE
```

## 6.5 转换和修改文件命令

`convert` 命令执行映像文件转换和操作。它支持以下操作：

* 在文件格式之间转换。
* 合并多个映像文件。
* 提取映像子集。
* 修补字节。
* 设置 token 数据。

`convert` 命令可以将其输出写入到文件或以类似于 `readmem` 命令的人类可读格式将其打印到标准输出。写入文件时，会根据使用的文件扩展名自动检测文件格式。

`convert` 命令在没有任何 J-Link/debug 连接的情况下离线工作。该命令与设备无关，但使用 tokens 或 EBL 文件时除外。在这种情况下，您必须使用 `--device` 选项。

**命令行语法**

```sh
$ commander convert [infile1] [infile2 …] [options]
```

### 6.5.1 合并两个文件

将具有不同文件格式的两个文件转换为一个指定的输出文件。

**命令行语法**

```sh
$ commander convert <filename> <filename> [--address <address>] --outfile <filename>
```

**命令行输入示例**

```sh
$ commander convert blink.bin userpage.hex --address 0x0 --outfile blinkapp.s37
```

将 blink.bin 和 userpage.hex 合并为 blinkapp.s37。address 选项用于设置 .bin 文件的起始地址，因为 bin 文件不包含任何寻址信息。地址值会被解释为十六进制数。如果提供了多个 .bin 文件，则所有文件都使用相同的起始地址。如果这不是期望的，那么请考虑在单独的准备步骤中将 bin 文件转换为 s37 或 hex。

**命令行输出示例**

```text
Parsing file blink.bin...
Parsing file userpage.hex...
Writing to blinkapp.s37...
DONE
```

### 6.5.2 定义特定字节

与 `flash` 命令一样，`convert` 命令支持 `--patch` 选项，用于在任意地址设置任意无符号整数。

**命令行语法**

```sh
$ commander convert [filename] --patch <address>:<data>[:length] [--outfile <filename>]
```

**命令行输入示例**

```sh
$ commander convert blink.s37 --patch 0x0FE00000:0x12345:4 --outfile blink.hex
```

将 blink.s37 转换为 hex 格式，同时将 user page 的前四个字节定义为 0x00012345。这就像 `flash blink.s37 --patch 0x0FE00000:0x12345:4` 一样工作，但是是针对文件而不是写入到设备闪存。

**命令行输出示例**

```text
Parsing file blink.s37...
Patching 0x0FE00000 = 0x00012345...
Writing to blink.hex...
DONE
```

### 6.5.3 定义 Tokens

与 `flash` 命令一样，`convert` 命令支持 `--tokengroup`、`--token` 和 `--tokenfile` 选项，用于在执行文件转换时设置 token 数据。

**命令行语法**

```sh
$ commander convert [filename] --tokengroup <token group> [--tokenfile <filename>] [--token <token name>:<token data>] [--device <device>] [--outfile <filename>]
```

**命令行输入示例**

```sh
$ commander convert blink.s37 --tokengroup znet --tokenfile tokens.txt --device EFR32MG1P --outfile blink.hex
```

将 blink.s37 转换为 hex 格式，同时定义在 tokens.txt 和命令行中定义的 tokens。与 `flash` 的相应选项一样工作，但写入到文件而不是闪存。

**命令行输出示例**

```text
Parsing file blink.s37...
Writing to blink.hex...
DONE
```

### 6.5.4 转储文件内容

与 `readmem` 命令一样，如果没有给定输出文件，`convert` 命令将以人类可读的格式将其输出打印到标准输出。address 选项的值被解释为十六进制数。

**命令行语法**

```sh
$ commander convert <filename> [--address <bin file start address>]
```

**命令行输入示例**

```sh
$ commander convert blink.bin --address 0x0 userpage.hex
```

如果未使用 `--outfile` 选项，则数据将打印到标准输出，而不是写入到文件。

**命令行输出示例**

```text
Parsing file blink.bin...
Parsing file userpage.hex...
{address: 0 1 2 3 4 5 6 7 8 9 A B C D E F}
00000000: 10 04 00 20 B5 0A 00 00 57 08 00 00 8B 0A 00 00
00000010: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00000020: 00 00 00 00 00 00 00 00 00 00 00 00 97 0A 00 00
00000030: 00 00 00 00 00 00 00 00 D1 0A 00 00 13 06 00 00
00000040: D3 0A 00 00 D5 0A 00 00 D7 0A 00 00 D9 0A 00 00
00000050: DB 0A 00 00 DD 0A 00 00 DF 0A 00 00 E1 0A 00 00
00000060: E3 0A 00 00 E5 0A 00 00 E7 0A 00 00 E9 0A 00 00
00000070: EB 0A 00 00 ED 0A 00 00 EF 0A 00 00 F1 0A 00 00
<shortened data for documentation>
00000ac0: C5 0A 00 00 C0 46 C0 46 C0 46 C0 46 FF F7 CA FF
00000ad0: FE E7 FE E7 FE E7 FE E7 FE E7 FE E7 FE E7 FE E7
00000ae0: FE E7 FE E7 FE E7 FE E7 FE E7 FE E7 FE E7 FE E7
00000af0: FE E7 FE E7 00 36 6E 01 00 80 00 00
{address: 0 1 2 3 4 5 6 7 8 9 A B C D E F}
0fe00000: 45 23 01 00 FF FF FF FF FF FF FF FF FF FF FF FF
0fe00010: FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF
0fe00020: FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF
<shortened data for documentation>
0fe007e0: FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF
0fe007f0: FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF
DONE
```

### 6.5.5 为安全启动签署应用程序

签署应用程序以与安全启动引导加载程序一起使用。有关详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

**命令行语法**

```sh
$ commander convert <image file> --secureboot --keyfile <signing key> --outfile <signed image file>
```

**命令行输入示例**

```sh
$ commander convert example.s37 --secureboot --keyfile mykey --outfile example-signed.s37
```

此示例对名为 example.s37 的文件进行签署。

**命令行输出示例**

```text
Parsing file example.s37...
Image SHA256: 4591da45b6c40a424b81753001708061d5319197adec5188f4acc512cfb88e65
R = 8E417EB4CBC584218A8605FCF3E778F2A7810F2CAE190CB2EF4D0DF842829CC1
S = 5B095025FFD571699725107C4666C0B8B867370E990B73E74A0502CB9788DCA8
Writing to example-signed.s37...
DONE
```

### 6.5.6 使用硬件安全模块为安全启动签署应用程序

准备应用程序以使用 HSM（Hardware Security Module，硬件安全模块）进行签署，以与支持安全启动的引导加载程序一起使用。有关详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

**命令行语法**

```sh
$ commander convert <image file> --secureboot --extsign --outfile <image file for external signing>
```

**命令行输入示例**

```sh
$ commander convert example.s37 --secureboot --extsign --outfile example.s37.extsign
```

此示例以 HSM 可以在整个文件上创建签名的形式创建输出。可以使用 [6.5.7 使用由硬件安全模块创建的签名为安全启动签署签署应用程序](#657-使用由硬件安全模块创建的签名为安全启动签署签署应用程序) 中描述的命令再次将此签名写入文件。

**命令行输出示例**

```text
Parsing file example.s37...
Writing to example.s37.extsign...
DONE
```

### 6.5.7 使用由硬件安全模块创建的签名为安全启动签署签署应用程序

使用由 HSM 创建的签名签署应用程序以与安全启动引导加载程序一起使用。有关详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

**命令行语法**

```sh
$ commander convert <image file> --secureboot --signature <signature from external signing> --outfile <signed image file>
```

**命令行输入示例**

```sh
$ commander convert example.s37 --secureboot --signature example.s37.extsign.sig --outfile example-signed.s37
```

此示例使用从 HSM 获得的签名对映像文件 example.s37 进行签署，该签名使用 [6.5.6 使用硬件安全模块为安全启动签署应用程序](#656-使用硬件安全模块为安全启动签署应用程序) 中生成的 .extsign 文件。与此功能一起使用的输入文件（example.s37）必须与在 [6.5.6 使用硬件安全模块为安全启动签署应用程序](#656-使用硬件安全模块为安全启动签署应用程序) 中生成 .extsign 文件时使用的文件相同。

**命令行输出示例**

```text
Parsing file example.s37...
Parsing signature file example.s37.extsign.sig...
R = 8E417EB4CBC584218A8605FCF3E778F2A7810F2CAE190CB2EF4D0DF842829CC1
S = 5B095025FFD571699725107C4666C0B8B867370E990B73E74A0502CB9788DCA8
Writing to example-signed.s37...
Overwriting file: example-signed.s37...
DONE
```

### 6.5.8 为 Gecko Bootloader 添加 CRC32

此选项为映像添加一个 CRC32（32-bit 循环冗余校验），Gecko Bootloader 可以使用它在不使用安全启动时确保映像的完整性。此特性要求映像中存在 `ApplicationProperties_t` 结构。有关 `ApplicationProperties_t` 结构的更多详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

**命令行语法**

```sh
$ commander convert <image file> --crc --outfile <image file with CRC>
```

**命令行输入示例**

```sh
$ commander convert example.s37 --crc --outfile example-crc.s37
```

此示例向名为 example.s37 的映像文件添加一个校验和。

**命令行输出示例**

```text
Parsing file example.s37...
Appending CRC32 checksum...
Writing to example-crc.s37...
DONE
```

### 6.5.9 使用中间证书为安全启动签署应用程序

使用中间证书（Intermediary Certificate）签署应用程序以与安全启动引导加载程序一起使用。使用中间证书时，映像中必须存在 `ApplicationProperties_t` 结构。有关 `ApplicationProperties_t` 结构的更多详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

只有 Series 2 EFR32 设备支持通过中间证书进行安全启动验证。在使用中间证书签署引导加载程序之前，必须启用安全启动。有关启用安全启动的更多信息，请参阅 [6.17.16 写入用户配置](#61716-写入用户配置)。

有两种签署应用程序的方法：

* 直接提供与证书中嵌入的公钥相对应的私钥文件。
* 通过以 HSM 可以在整个文件上创建签名的形式生成输出，准备应用程序以使用 HSM 进行签署。然后可以通过将签名传递给 Simplicity Commander 将其写入文件，如下所述。

**注意**：Simplicity Commander 目前不支持为安全启动签署生成证书。这将在 Simplicity Commander 的未来版本中提供。

**命令行语法**

```sh
$ commander convert <image file> --secureboot --certificate <certificate file> --keyfile <keyfile> --outfile <signed image file>
```

```sh
$ commander convert <image file> --secureboot --certificate <certificate file> --extsign --outfile <image file for external signing>
```

```sh
$ commander convert <image file> --secureboot --certificate <certificate file> --signature <signature> --outfile <signed image file>
```

**命令行输入示例**

```sh
$ commander convert example.s37 --secureboot --certificate example_certificate.bin --keyfile public_certificate_key.pem --outfile example-signed.s37
```

此示例使用中间证书对映像文件 example.s37 进行签署。用于签署应用程序的密钥文件对应于证书中嵌入的公钥。Simplicity Commander 总是在签署应用程序之前验证密钥。

**命令行输出示例**

```text
Parsing file example.s37...
Private key matches public key in certificate.
R = 137EA7A19F6100E1EFA5C185CA952B67137D0597F4A89C7543BC5A49A7A6681E
S = C537A833018C3A23CF1EBDBAB04559482B0B5333A7C21556E6B42EDA1D1A5102
Writing to example-signed.s37...
DONE
```

### 6.5.10 添加 Trust Zone 解密密钥

将一个 AES（Advanced Encryption Standard，高级加密标准）加密/解密密钥添加到用于 TrustZone 安全密钥存储的引导加载程序映像。需要应用程序属性结构版本 1.2 或更高。

**命令行语法**

```sh
$ commander convert <image file> --aeskey <keyfile> --outfile <bootloader with decryption key>
```

**命令行输入示例**

```sh
$ commander convert my_bootloader.s37 --aeskey my_key.txt --outfile my_bootloader_with_key.s37
```

将解密密钥 `my_key.txt` 添加到名为 `my_bootloader_with_key.s37` 的引导加载程序映像。

**命令行输出示例**

```text
Parsing file my_bootloader.s37...
Writing to my_bootloader_with_key.s37...
DONE
```

### 6.5.11 从 ELF 文件中提取 Sections

从 ELF（Executable and Linkable Format，可执行和可链接格式）文件中提取 sections 并转换到指定的输出文件。如果 `--include-section` 和 `--exclude-section` 选项均未使用，Simplicity Commander 将提取所有 `.text` sections，以及地址不等于 0x0 的 `progbits` 类型的 sections。

**命令行语法**

```sh
$ commander convert <filename> [--include-section <section> --exclude-section <section>] --outfile <filename>
```

**命令行输入示例**

```sh
$ commander convert application.out --include-section text_apploader --outfile apploader.s37
```

从一个 ELF 文件的 `text_apploader` section 创建 S-record 文件。

**命令行输出示例**

```text
Parsing file application.out...
Writing to apploader.s37...
DONE
```

## 6.6 EBL 命令

### 6.6.1 打印 EBL 信息

从指定的 .ebl 文件中解析并打印 EBL 信息。

**命令行语法**

```sh
$ commander ebl print <filename>
```

**命令行输入示例**

```sh
$ commander ebl print example.ebl
```

**命令行输出示例**

```text
Found EBL Tag = 0x0000, length 140, [EBL Header]
  Version:      0x0201
  Signature:    0xE350 (Correct)
  Flash Addr:   0x00004000
  AAT CRC:      0x53BC1F4E
  AAT Size:     128 bytes
    HalAppBaseAddressTableType
      Top of Stack:       0x20006980
      Reset Vector:       0x000121F9
      Hard Fault Handler: 0x00012125
      Type:               0x0AA7
      HalVectorTable:     0x00004100
    Full AAT Size:     172
    Ember Version:     5.7.0.0
    Ember Build:       0
    Timestamp:         0x561E581F (Wed Oct 14, 2015 13:26:55 UTC [+0100])
    Image Info String: ''
    Image CRC:         0x2ACE0C5B
    Customer Version:  0x00000000
    Image Stamp:       0xF4271F50BA2E2FBA
Found EBL Tag = 0xFD03, length 1924, [Erase then Program Data]
  Flash Addr: 0x00004080
Found EBL Tag = 0xFD03, length 2052, [Erase then Program Data]
  Flash Addr: 0x00004800
(32 additional tags of the same type and length.)
Found EBL Tag = 0xFD03, length 1772, [Erase then Program Data]
  Flash Addr: 0x00015000
Found EBL Tag = 0xFC04, length    4, [EBL End Tag]
  CRC: 0xDBC82DA5
The CRC of this EBL file is valid (0xdebb20e3)
File has 0 bytes of end padding.
Calculated image stamp matches value found in AAT.
DONE
```

### 6.6.2 EBL 密钥生成

生成用于加密或解密的密钥文件，并将密钥文件输出到指定的文件。

**命令行语法**

```sh
$ commander ebl keygen --type aes-ccm --outfile <filename>
```

**命令行输入示例**

```sh
$ commander ebl keygen --type aes-ccm --outfile key.txt
```

**命令行输出示例**

```text
Using /dev/random for random number generation
Gathering sufficient entropy... (may take up to a minute)...
DONE
```

### 6.6.3 EBL 文件创建

从应用程序映像创建 EBL 文件并将输出写到指定的文件。可以选择使用由 `ebl keygen` 命令生成的密钥文件来加密 EBL 文件。

**命令行语法**

```sh
$ commander ebl create <eblfile> --app <filename> --device <part number> [--encrypt <keyfile>]
```

**命令行输入示例**

```sh
$ commander ebl create app.ebl.encrypted --app example.s37 --device EFR32F256 --encrypt key.txt
```

**命令行输出示例**

```text
Parsing file example .s37...
Parse .s37 format for flash
Flash Usage:
  Reserved for Bootloader:      0x00000000-0x00003fff (16384 bytes)
  CODE and Tables:              0x00004000-0x00014ddb (69084 bytes)
  CONST and INITC:              0x00014ddc-0x000184ab (14032 bytes)
  Available for future use:     0x000184ac-0x0003dfff (154452 bytes)
  Reserved for SIMEE:           0x0003e000-0x0003ffff (8192 bytes)

Usage Summary:
  262144 total bytes Flash, 107692 used, 154452 available

Setting AAT timestamp to current time: 0x586e1ec9
Create ebl image file
Wrote image stamp into AAT.
Encrypting EBL...
Unencrypted input file: ebl_plaintext_ux8544.ebl
Encrypt output file: app.ebl.encrypted
Randomly generating nonce
Using /dev/random for random number generation
Gathering sufficient entropy... (may take up to a minute)...
Created ENCRYPTED ebl image file
DONE
```

### 6.6.4 EBL 文件解析

解析 EBL 文件并将应用程序映像写到指定的文件。可以选择解密已加密的 EBL 文件。密钥文件必须与用于加密已加密 EBL 文件的密钥文件相同。

**命令行语法**

```sh
$ commander ebl parse <ebl filename> --app < filename> --device <part number> [--decrypt <key filename>]
```

**命令行输入示例**

```sh
$ commander ebl parse example.ebl.encrypted --app app.s37 --device EFR32F256 --decrypt ../aeskey
```

**命令行输出示例**

```text
Unencrypted output file: ebl_plaintext_L10567.ebl
Encrypt input file:      example.ebl.encrypted
MAC matches. Decryption successful.
Created DECRYPTED ebl image file
Parse .ebl format for flash
Create image file
Writing application to app.s37...
DONE
```

### 6.6.5 来自 AAT 的存储器使用情况信息

对于包含 AAT（Application Address Table，应用程序地址表）的应用程序，Simplicity Commander 可以分析应用程序的存储器使用情况。AAT 包含在 Zigbee 应用程序中。

RAM 使用情况仅适用于 EM3xx 应用程序。为 EFR32 构建的应用程序只能分析闪存使用情况。

**命令行语法**

```sh
$ commander ebl aat-usageinfo <filename> --device <part number>
```

**命令行输入示例**

```sh
$ commander ebl aat-usageinfo example.s37 --device EM357
```

**命令行输出示例**

```text
Parse .s37 format for flash

Approximate Usage Information:
RAM Usage:
  APPLICATION_CONFIGURATION_HEADER usage: 0x20000000-0x20000fc3 (4036 bytes)
  Available for future use:               0x20000fc4-0x2000195f (2460 bytes)
  Call Stack:                             0x20001960-0x200022bf (2400 bytes)
  Globals and Statics:                    0x200022c0-0x20002fe8 (3369 bytes)
  Alignment Overhead:                     0x20002fe9-0x20002fef (7 bytes)
  NO_INIT and Debug Channel:              0x20002ff0-0x20002fff (16 bytes)
Flash Usage:
  Reserved for Bootloader:                0x08000000-0x08001fff (8192 bytes)
  CODE and Tables:                        0x08002000-0x08011cdf (64736 bytes)
  CONST and INITC:                        0x08011ce0-0x08014263 (9604 bytes)
  Available for future use:               0x08014264-0x0802dfff (105884 bytes)
  Reserved for SIMEE:                     0x0802e000-0x0802ffff (8192 bytes)

Usage Summary:
  12288 total bytes RAM, 9828 used, 2460 available
  196608 total bytes Flash, 90724 used, 105884 available
DONE
```

## 6.7 GBL 命令

### 6.7.1 GBL 文件创建

从应用程序映像创建 GBL 文件并将输出写到指定的文件。可以选择使用由 `gbl keygen` 命令生成的密钥文件来加密 GBL 文件。

**命令行语法**

```sh
$ commander gbl create <gblfile> --app <filename> [--encrypt <keyfile>]
```

**命令行输入示例**

```sh
$ commander gbl create app.gbl.encrypted --app example.s37 --encrypt key.txt
```

**命令行输出示例**

```text
Parsing file example.s37...
Initializing GBL file...
Adding application to GBL...
Encrypting GBL...
Writing GBL file app.gbl.encrypted...
DONE
```

### 6.7.2 带压缩的 GBL 文件创建

从应用程序映像创建已压缩的 GBL 文件并将输出写到指定的文件。可以选择使用由 `gbl keygen` 命令生成的密钥文件来加密 GBL 文件。

目前支持的压缩算法有 `lz4` 和 `lzma`。目标设备上的引导加载程序必须支持解压所选定的压缩类型。

**命令行语法**

```sh
$ commander gbl create <gblfile> --app <filename> --compress <compression algorithm> [--encrypt <keyfile>]
```

**命令行输入示例**

```sh
$ commander gbl create app.gbl --app example.s37 --compress lz4
```

**命令行输出示例**

```text
Parsing file example.s37...
Initializing GBL file...
Adding application to GBL...
Compressing using lz4...
Writing GBL file app.gbl...
DONE
```

### 6.7.3 创建用于引导加载程序升级的 GBL 文件

从引导加载程序映像创建 GBL 文件并将输出写到指定的引导加载程序映像文件。有关详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

**命令行语法**

```sh
$ commander gbl create <gblfile> --bootloader <bootloader image file> [--encrypt <keyfile>]
```

**命令行输入示例**

```sh
$ commander gbl create bootloader.gbl --bootloader bootloader.s37
```

**命令行输出示例**

```text
Initializing GBL file...
Adding bootloader to GBL...
Writing GBL file bootloader.gbl...
DONE
```

### 6.7.4 创建用于安全元素升级的 GBL 文件

EFR32xG21 设备上的安全元素（Secure Element）可以使用 Silicon Labs 提供的安全元素升级二进制文件进行升级。此命令创建一个包含安全元素升级文件的 GBL 文件并将输出写到指定的 GBL 文件。有关详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

**命令行语法**

```sh
$ commander gbl create <gblfile> --seupgrade <secure element upgrade file> --app <application image>
```

**命令行输入示例**

```sh
$ commander gbl create se-upgrade.gbl --seupgrade secure-element-1.0.0.seu --app myapp.s37
```

**命令行输出示例**

```text
Parsing file myapp.s37...
Initializing GBL file...
Adding application to GBL...
Adding Secure Element upgrade image to GBL...
Writing GBL file se-upgrade.gbl...
DONE
```

### 6.7.5 从应用程序创建已签署和已加密的 GBL 升级映像文件

创建 GBL 文件，签署 GBL 文件，并加密 GBL 文件。有关详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

**命令行语法**

```sh
$ commander gbl create <gblfile> --app <app image file> --sign <signing key> [--encrypt <encryption key>]
```

**命令行输入示例**

```sh
$ commander gbl create example.gbl --app example.s37 --sign ecdsakey --encrypt aeskey
```

**命令行输出示例**

```text
Parsing file example.s37...
Initializing GBL file...
Adding application to GBL...
Encrypting GBL...
Signing GBL...
Image SHA256: 74b126bdbad680470487e32d7d7b3ec7f12b15d9988e028b26c2dd54f81dcfb7
R = 055A23A44CDEDA34506EE72F4530FE174CFC85F48933C1379C1360F8BC1AA75B
S = 1C9EF6C3F5CAA0D5B92ECC2569E4A8251F8561DAF52DE54D3E59591A5001B9EA
Writing GBL file example.gbl...
DONE
```

### 6.7.6 创建与硬件安全模块一起使用的局部已签署和已加密的 GBL 升级文件

通常不希望在创建 GBL 映像的计算机上保留用于本地签署的私钥。一个提高安全性的好方法是使用 HSM 来生成实际的签名。Simplicity Commander 使用一个三步流程来对其进行支持：

1. 使用 Simplicity Commander 创建一个用于外部签署的局部 GBL 文件。
2. 使用 HSM 创建一个局部 GBL 文件的 ECDSA（Elliptic Curve Digital Signature Algorithm，椭圆曲线数字签名算法）签名。
3. 使用 Simplicity Commander 使用来自 HSM 的签名对局部 GBL 文件进行签署，并创建一个完整的 GBL 文件。

本节介绍了步骤 1。步骤 2 特定于您使用的 HSM。步骤 3 在 [6.7.7 使用硬件安全模块创建已签署的 GBL 文件](#677-使用硬件安全模块创建已签署的-gbl-文件) 中进行了描述。有关详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

**命令行语法**

```sh
$ commander gbl create <output partial GBL file for external signing> --app <app image file> --extsign [--encrypt <encryption key>]
```

**命令行输入示例**

```sh
$ commander gbl create example.gbl.extsign --app example.s37 --extsign --encrypt aeskey
```

**命令行输出示例**

```text
Parsing file example.s37...
Initializing GBL file...
Adding application to GBL...
Encrypting GBL...
Preparing GBL for external signing...
Writing GBL file example.gbl.extsign...
DONE
```

### 6.7.7 使用硬件安全模块创建已签署的 GBL 文件

从如 [6.7.6 创建与硬件安全模块一起使用的局部已签署和已加密的 GBL 升级文件](#676-创建与硬件安全模块一起使用的局部已签署和已加密的-gbl-升级文件) 所述生成的 DER（Distinguished Encoding Rules）格式的局部 GBL 文件和 ECDSA 签名文件创建已签署的 GBL 文件。有关详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

Silicon Labs 建议您将 `--verify` 选项与由 HSM 使用的私钥相对应的公钥一起使用，以确保生成的 GBL 文件的完整性。

**命令行语法**

```sh
$ commander gbl sign <partial GBL file for external signing> --signature <signature from HSM> [--verify <public key file>] --outfile <signed GBL file>
```

**命令行输入示例**

```sh
$ commander gbl sign example.gbl.extsign --signature example.gbl.extsign.sig --verify ecdsakey.pub --outfile example-signed.gbl
```

**命令行输出示例**

```text
Reading GBL data from example.gbl.extsign...
Parsing signature file example.gbl.extsign.sig...
R = 2E73426A1052E12BFFFEFBA9BE2AA50CEA815B630C3CA878494EEF26088A5673
S = C218596DB9958AB30924B516953D2E5107644963B4CA128072AC965BE5C2992D
Writing signature to GBL...
Verifying GBL...
Image SHA256: 4d7325b09ade0ea272eb9895096c8137b18451f694a4eca9a5782f5c08dea03a
Q_X: 60BA97B850291456217C2149061AA344B32BBFB69A91A94BBF2F274744308D39
Q_Y: 41927DA5DB171E1C723C6B59C2BC88EDFF5A37014B0473775BA5B15921686ECA
R = 2E73426A1052E12BFFFEFBA9BE2AA50CEA815B630C3CA878494EEF26088A5673
S = C218596DB9958AB30924B516953D2E5107644963B4CA128072AC965BE5C2992D
Writing GBL file example-signed.gbl...
DONE
```

### 6.7.8 GBL 文件解析

解析 GBL 文件并将应用程序映像写到指定的文件。可以选择解密已加密的 GBL 文件。密钥文件必须与用于加密已加密 GBL 文件的密钥文件相同。

**命令行语法**

```sh
$ commander gbl parse <gbl filename> --app < filename> [--decrypt <key filename>]
```

**命令行输入示例**

```sh
$ commander gbl parse example.gbl.encrypted --app app.s37 --decrypt key.txt
```

**命令行输出示例**

```text
Reading GBL data...
Decrypting GBL...
Reading application...
Writing application to app.s37...
DONE
```

### 6.7.9 GBL 密钥生成

此命令已弃用。有关密钥生成的更多信息，请参阅 [6.18.1 密钥生成](#6181-密钥生成)。

### 6.7.10 生成签署密钥

此命令已弃用。有关生成签署密钥的更多信息，请参阅 [6.18.2 生成签署密钥](#6182-生成签署密钥)。

### 6.7.11 使用硬件安全模块生成签署密钥

此命令已弃用。有关使用硬件安全模块生成签署密钥的更多信息，请参阅 [6.18.3 Token 密钥](#6183-token-密钥)。

### 6.7.12 使用硬件安全模块创建已签署的 GBL 文件

> 注：与 [6.7.7 使用硬件安全模块创建已签署的 GBL 文件](#677-使用硬件安全模块创建已签署的-gbl-文件) 内容重复。

### 6.7.13 从 ELF 文件创建 GBL 文件

从 ELF 文件创建 GBL 文件并将输出写到指定文件。如果 `--include-section` 和 `--exclude-section` 选项均未使用，Simplicity Commander 将包含应用程序的所有 sections。

**命令行语法**

```sh
$ commander gbl create <gblfile> --app <application image file> [--include-section <section> --exclude-section <section>]
```

**命令行输入示例**

```sh
$ commander gbl create app.gbl --app app.out --exclude-section text_apploader --exclude-section text_signature
```

创建一个包含 ELF 应用程序的 GBL 文件，从应用程序中排除 `text_apploader` 和 `text_signature` sections。

**命令行输出示例**

```text
Parsing file app.out...
Initializing GBL file...
Adding application to GBL...
Encrypting GBL...
Writing GBL file app.gbl.encrypted...
DONE
```

### 6.7.14 使用未加密的安全元素升级文件创建已加密的 GBL 文件

创建包含未加密安全元素升级文件的已加密 GBL 文件，然后将输出写到指定的 GBL 文件。

**命令行语法**

```sh
$ commander gbl create <gblfile> --seupgrade <secure element upgrade file> --seunencrypted --app <application image> --encyrpt <AES key file>
```

**命令行输入示例**

```sh
$ commander gbl create se-upgrade.gbl --seupgrade secure-element.seu --seunencrypted --app myapp.s37 --encrypt aes-key.txt
```

使用文件加密区域之外的安全元素升级文件创建一个已加密的 GBL 文件。

**命令行输出示例**

```text
Parsing file myapp.s37...
Initializing GBL file...
Adding application to GBL...
Adding Secure Element upgrade image to GBL...
Encrypting GBL...
Writing GBL file se-upgrade.gbl...
DONE
```

### 6.7.15 创建具有版本依赖的 GBL 文件

在 GBL 文件中应用程序、引导加载程序和安全元素升级文件之间的任何版本依赖关系都可以使用 `--dep-app`、`--dep-boot` 和 `--dep-se` 选项来解决。

**命令行语法**

```sh
$ commander gbl create <gblfile> --seupgrade <secure element upgrade file> --app <application image> --dep-app <statement:version> --dep-se <staetment:version> --dep-boot <statement:version>
```

**依赖声明**

依赖声明可能是以下之一：

| Simplicity Commander Input | Statement             |
| :------------------------- | :-------------------- |
| g                          | Greater than          |
| geq                        | Greater than or equal |
| eq                         | Equal                 |
| leq                        | Less than or equal    |
| l                          | Less than             |

`--dep-app` 选项使用一个 uint32 作为版本输入，而 `--dep-se` 和 `--dep-boot` 选项采用 major.minor.patch 格式的版本输入。

**命令行输入示例**

```sh
$ commander gbl create se-upgrade.gbl --app myapp.s37 --seupgrade secure-element.seu --bootloader mybootloader.s37 --dep-app geq:0x01020002 --dep-boot l:0.5.7 --dep-se g:1.2.3
```

创建一个 GBL，其中应用程序版本必须大于或等于 0x01020002，引导加载程序版本必须小于 0.5.7，并且安全元素升级版本必须大于 1.2.3。

**命令行输出示例**

```text
Parsing file myapp.s37
Initializing GBL file...
Setting version dependency of Application to >= 0x00120002
Setting version dependency of Bootloader to < 0x00050007
Setting version dependency of SE upgrade image to > 0x00010203
Adding version dependencies to GBL...
Adding application to GBL...
Adding bootloader to GBL...
Adding Secure Element upgrade image to GBL...
Writing GBL file se-upgrade.gbl
DONE
```

## 6.8 Kit 实用命令

### 6.8.1 固件升级

将在 kit 的板控制器上运行的应用程序更新为 Silicon Labs 在 .emz 文件中提供的新版本。

**命令行语法**

```sh
$ commander adapter fwupgrade --serialno <J-Link serial number> <filename>
```

**命令行输入示例**

```sh
$ commander adapter fwupgrade -s 440050184 S1015B_wireless_stk_firmware_package_0v14p0b435.emz
```

**命令行输出示例**

```text
Checking manifest...
Checking if target is in bootloader...
Waiting for kit to restart...
Package is usable
Deleting previous firmware...
Installing files...
Resetting target...
Waiting for kit to restart...
Finished!
DONE
```

### 6.8.2 Kit 信息探测

检索已连接的 kit 的相关信息。列出 kit 的部件号和名称、已连接的板和固件版本的信息。

选项 `--kit`、`--boards` 和 `--firmware` 分别将输出限制为 kit 的信息、板列表、固件信息。

`VCOM Port` 行报告 kit 已由操作系统分配了哪个虚拟 COM 端口名称。在 Windows 上，它的格式为 COM\<number\>。在 Linux 和 macOS 上，该名称对应于 `/dev/` 文件夹中的一个特殊文件。例如，`VCOM Port: ttyACM0` 表示串口在 `/dev/ttyACM0` 处可用。此行并不总是可用的，可能会从输出中省略。

**命令行语法**

```sh
$ commander adapter probe --serialno <J-Link serial number> [--kit] [--boards] [--firmware]
```

**命令行输入示例**

```sh
$ commander adapter probe --serialno 440050184
```

**命令行输出示例**

```text
Kit Information:
=======================================
Kit Name        : EFR32 Mighty Gecko 2400/915 MHz Dual Band Wireless Starter Kit
Kit Part Number : WSTK6002A Rev. A00
J-Link Serial   : 440050184
Debug Mode      : MCU
AEM Supported   : 1
VCOM Supported  : 1
IP Supported    : 1
VCOM Port       : COM3
Firmware Information:
=======================================
FW Version      : 0v14p0b435
Board List:
=======================================
Name            : Wireless Starter Kit Mainboard
Part Number     : BRD4001A Rev. A01
Serial Number   : 152607557
Name            : EFR32MG 2400/915 MHz 19.5 dBm Dual Band Radio Board
Part Number     : BRD4150B Rev. B00
Serial Number   : 151300035
DONE
```

### 6.8.3 适配器重置命令

此命令重置适配器（Adapter）自身，导致重新启动。在正常操作期间通常不需要 `adapter reset` 命令。

可能会出现有关“Communication timed out”的错误，因为适配器有时会在它有时间应答命令之前重新启动。

**命令行语法**

```sh
$ commander adapter reset
```

**命令行输入示例**

```sh
$ commander adapter reset
```

**命令行输出示例**

```text
Communication timed out: Requested 76 bytes, received 0 bytes !
DONE
```

### 6.8.4 适配器调试模式命令

此命令设置或读取适配器的当前调试模式。除了 Wireless Pro Kits 的 MINI 和 Development Kits 的 TARGET 之外，支持的调试模式通常为 IN、OUT、MCU 和 OFF。有关其支持的调试模式的说明，请参阅您的 kit 的 *Quick Start Guide*。

**命令行语法**

```sh
$ commander adapter dbgmode [mode]
```

**命令行输入示例**

```sh
$ commander adapter dbgmode MCU
```

**命令行输出示例**

```text
Setting debug mode to MCU...
DONE
```

### 6.8.5 列出适配器 IP 配置命令

`adapter ip` 命令获取或设置适配器的 IP 配置。如果没有选项，将检索并显示当前配置。

**命令行语法**

```sh
$ commander adapter ip
```

**命令行输入示例**

```sh
$ commander adapter ip
```

**命令行输出示例**

```text
IP Address: 192.168.0.5/24
Gateway   : 192.168.0.1
DNS Server: 192.168.0.1
DONE
```

### 6.8.6 适配器 DHCP 命令

此命令设置适配器以使用 DHCP 自动检索 IP、gateway 和 DNS 地址。这是默认的配置。启用 DHCP 后，必须重新启动适配器才能使更改生效。

**命令行语法**

```sh
$ commander adapter ip --dhcp
```

**命令行输入示例**

```sh
$ commander adapter ip --dhcp
```

**命令行输出示例**

```text
Enabling DHCP. The adapter must be restarted to acquire a new IP address.
DONE
```

### 6.8.7 设置静态 IP 配置命令

此命令以 CIDR（Classless Inter-Domain Routing，无类别域间路由）表示法设置适配器的 IP 地址。

**命令行语法**

```sh
$ commander adapter ip --addr <IP address/prefix> [--gw <gateway address>] [--dns <dns server address>]
```

**命令行输入示例**

```sh
$ commander adapter ip --addr 192.168.1.5/24 --gw 192.168.1.1 --dns 192.168.1.1
```

**命令行输出示例**

```text
Setting IP Address: 192.168.1.5/24
Setting gateway   : 192.168.1.1
Setting DNS server: 192.168.1.1
DONE
```

## 6.9 设备擦除命令

### 6.9.1 擦除芯片

对支持的设备执行批量擦除。在 EFM32G 和 EFM32TG 上，所有页都会被擦除，这意味着会更慢。

**命令行语法**

```sh
$ commander device masserase
```

**命令行输出示例**

```text
Erasing chip...
DONE
```

### 6.9.2 擦除区域

擦除一个已命名的区域。有关 `--region` 选项的更多信息，请参阅 [6.2 闪存验证命令](#62-闪存验证命令)。

**命令行语法**

```sh
$ commander device pageerase --region <@region>
```

**命令行输入示例**

```sh
$ commander device pageerase --region @userdata
```

**命令行输出示例**

```text
Erasing range 0x0fe00000 - 0x0fe00800
DONE
```

### 6.9.3 擦除地址范围内的页

擦除受给定存储器范围影响的所有闪存页。如果给定范围与页边界不匹配，它将扩展为始终擦除整个页。

**命令行语法**

```sh
$ commander device pageerase --range <startaddress>:<endaddress>
```

**命令行输入示例**

```sh
$ commander device pageerase --range 0x200:0x6000
```

擦除页 0 到 11 或 0x0000 到 0x5FFF（假设页大小为 2 kB）的所有闪存。

**命令行输出示例**

```text
Erasing range 0x00000000 - 0x00006000
DONE
```

## 6.10 设备锁定和保护命令

### 6.10.1 调试锁定

锁定对设备调试接口的访问。此特性仅在 EFM32 和 EFR32 设备上受支持。从 Simplicity Commander 1.8 开始，不再需要 `--debug enable` 选项。

**命令行语法**

```sh
$ commander device lock [--debug enable]
```

**命令行输出示例**

```text
Locking debug access...
DONE
```

### 6.10.2 调试解锁

解锁对设备调试接口的访问。如果设备之前被锁定，这会触发批量擦除。

此特性仅在 EFM32 和 EFR32 设备上受支持。

**命令行语法**

```sh
$ commander device lock --debug disable
```

**命令行输出示例**

```text
ERROR: Could not get MCU information
Removing all locks/protection...
Unlocking debug access (triggers a mass erase)...
DONE
```

在 Simplicity Commander 1.8 中引入了另一种命令语法。

**命令行语法**

```sh
$ commander device unlock
```

**命令行输出示例**

```text
Unlocking debug access (triggers a mass erase)...
Chip successfully unlocked.
DONE
```

### 6.10.3 写保护闪存范围

保护受给定内存范围影响的所有闪存页以防止任何写入或擦除。闪存写保护的可用粒度取决于设备。有关详细信息，请参阅设备的参考手册。例如，对于 EFM32 和 EFR32 设备，写保护特性在闪存页上操作。在 EM3xx 设备上，这适用于 8 kB 或 16 kB 块。

对于所有设备，如果给定范围与设备支持的块大小不匹配，它将扩展到始终保护整个区域。

**命令行语法**

```sh
$ commander device protect --write --range <startaddress>:<endaddress>
```

**命令行输入示例**

```sh
$ commander device protect --write --range 0x0:0x4000
```

保护前 16 kB 中的所有闪存页以防止被擦除或写入。例如，可用于保护引导加载程序不被错误的应用程序代码修改。

**命令行输出示例**

```text
Write protecting range 0x00000000 - 0x00004000
DONE
```

### 6.10.4 写保护闪存区域

保护指定区域中的所有闪存页以防止被写入或擦除。

**命令行语法**

```sh
$ commander device protect --write --region @<region>
```

**命令行输入示例**

```sh
$ commander device protect --write --region @mainflash
```

保护整个 mainflash 以防止被写入或擦除。

**命令行输出示例**

```text
Write-protecting all pages in main flash.
DONE
```

### 6.10.5 禁用写保护

禁用所有页的写保护。

**命令行语法**

```sh
$ commander device protect --write --disable
```

**命令行输出示例**

```text
Disabling all write protection...
DONE
```

## 6.11 设备实用命令

### 6.11.1 设备信息命令

显示有关目标设备的详细信息。

**命令行语法**

```sh
$ commander device info
```

**命令行输出示例**

```text
Part Number    : EFR32MG1P233F256GM48
Die Revision   : A0
Production Ver : 0
Flash Size     : 256 kB
SRAM Size      : 32 kB
Unique ID      : 000b57000003b2f0
DONE
```

### 6.11.2 设备重置命令

使用引脚重置重置设备。

**命令行语法**

```sh
$ commander device reset
```

**命令行输出示例**

```text
Resetting chip...
DONE
```

### 6.11.3 设备恢复命令

在 EFM32 和 EFR32 设备上，此命令尝试恢复由于时钟、GPIO 引脚或类似配置错误而失去调试访问权限的设备。并非所有设备都支持恢复，在某些情况下需要与要恢复的设备对应的 kit，例如，一个 EFM32TG STK 来恢复 EFM32TG 设备。

在 EM3xx 设备上，此命令可用于从选项字节故障中恢复。

**命令行语法**

```sh
$ commander device recover
```

**命令行输出示例**

```text
Recovering "bricked" device...
DONE
```

### 6.11.4 设备 Z-Wave QR Code 命令

Z-Wave QR code 命令用于从所有 Z-Wave 设备中读取 QR code。QR code 为 90 字节，显示为 ASCII 字符，并存储在 TOKEN\_MFG\_ZW\_QR\_CODE manufacturing token 中。

QR code 是在初始化时在芯片中生成的。当 QR code 正确地初始化后，manufacturing token TOKEN\_MFG\_ZW\_INITIALIZED 的值从 0xFF 更改为 0x00。可选的 `--timeout` 选项用于指示 Simplicity Commander 应等待 QR code 初始化多长时间。如果没有给出时间，则默认为 5000 ms。

**命令行语法**

```sh
$ commander device zwave-qrcode [--timeout <timeout in ms>]
```

**命令行输入示例**

```sh
commander device zwave-qrcode --timeout 5000
```

**命令行输出示例**

```text
QR code: 900132782003515253545541424344453132333435212223242500100435301537022065520001000000300578
DONE
```

## 6.12 外部 SPI 闪存命令

Simplicity Commander 支持在有限选择的板和设备上读取、写入和擦除外部 SPI 闪存上的数据。目前支持以下配置：

* EFR32MG1x632 和 EFR32MG1x732 设备上的集成 SPI 闪存
* EFR32 radio 板上的 MX25 SPI 闪存

### 6.12.1 擦除外部 SPI 闪存命令

使用此命令擦除外部闪存上的数据。默认情况下，已擦除的范围会被回读以验证它是否确实已被擦除。可以通过包含 `--noverify` 选项来禁用此空白检查。

`extflash erase` 命令总是擦除完整的扇区（sector）。任何与所提供范围重叠的扇区都将被擦除。当前支持的所有闪存设备的扇区大小均为 4096 字节。例如，使用选项 `--range 0xE00:0x1100` 的擦除将实际上会擦除前两个扇区（相当于 `--range 0x0:0x2000`）。

**命令行语法**

```sh
$ commander extflash erase --range <range expression> [--noverify]
```

**命令行输入示例**

```sh
$ commander extflash erase --range 0x1000:0x3000
```

**命令行输出示例**

```text
Erasing 8192 bytes from 0x00001000 on external flash.
Resetting target...
Uploading flashloader...
Erasing external flash...
Verifying written data...
Waiting for flashloader to become ready...
Reading from external flash...
DONE
```

### 6.12.2 读取外部 SPI 闪存命令

使用此命令从外部闪存读取。

**命令行语法**

```sh
$ commander extflash read --range <range expression>
```

**命令行输入示例**

```sh
$ commander extflash read --range 0x0:+0x20
```

**命令行输出示例**

```text
Reading 32 bytes from 0x00002000 on external flash.
Resetting target...
Uploading flashloader...
Waiting for flashloader to become ready...
Reading from external flash...
{address:  0  1  2  3  4  5  6  7  8  9  A  B  C  D  E  F}
00002000: 48 65 6C 6C 6F 20 57 6F 72 6C 64 21 0A FF FF FF
00002010: FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF
DONE
```

### 6.12.3 写外部 SPI 闪存命令

使用此命令写入外部闪存。

受影响的闪存扇区中的任何现有内容都将在写入前被擦除。

与用于内部闪存的 `flash` 命令相比，`extflash write` 命令总是刷写给定文件的原始内容。如果给出 address 选项，则该值会被解释为十六进制数。例如，如果提供了 S-record 文件，则写入文件的 ASCII 内容；S-record 格式不会被解析并写到文件中指定的地址。

**命令行语法**

```sh
$ commander extflash write <filename> --address <start address>
```

**命令行输入示例**

```sh
$ commander extflash write myfile.txt --address 0x2000
```

**命令行输出示例**

```text
Flashing 13 bytes to 0x00002000 on external flash.
Resetting target...
Uploading flashloader...
Waiting for flashloader to become ready...
Erasing external flash...
Writing to external flash...
Verifying written data...
Waiting for flashloader to become ready...
Reading from external flash...
DONE
```

## 6.13 高级能量监视器命令

Simplicity Commander 支持从适配器的 AEM（Advanced Energy Monitor，高级能量监视器）读取和记录电流测量数据。

### 6.13.1 测量时间窗口内的平均电流

`aem measure` 命令测量一个时间窗口内的平均电流。`--windowlength` 以毫秒（ms）为单位，定义为当前样本将被测量和平均的持续时间。如果没有给出时间，默认为 100 ms。

**命令行语法**

```sh
$ commander aem measure [--windowlength <time in ms>]
```

**命令行输入示例**

```sh
$ commander aem measure --windowlength 200
```

**命令行输出示例**

```text
Averaged over 200 ms:
Current [mA]: 5.359
Power [mW]  : 17.763
Voltage [V] : 3.314
DONE
```

### 6.13.2 将电流测量记录为时间序列数据

`aem dump` 命令连续地测量电流并记录测量数据（电压和电流）。如果提供 `--outfile`，那么数据将记录在指定的输出文件中，否则数据将流式传输到终端窗口。如果提供 `--duration`，那么记录将在指定时间后停止，否则记录将一直继续。在这两种情况下，可以随时通过按 CTRL+C 来终止日志记录。可以传递 `--noheader` 选项以省略列标头，使其不包含在输出文件中。此处描述的选项也适用于与 `aem dump` 命令相关的其他选项，但为了简洁起见，将在下一节中省略。

如果提供了 `--datarate` 选项，那么当前的电流测量速率将设置为指定的速率。这将在等于提供的数据速率的倒数的时间内收集样本，并在将数据存储在输出日志（终端或指定的输出文件）中之前对这些样本进行平均。数据速率必须等于或大于 1 Hz，并且还需等于或小于相关适配器的 AEM 采样率。如果未提供 `--datarate`，那么该命令将默认为适配器的 AEM 采样率。

输出文件必须是 .txt 或 .csv 格式。

**命令行语法**

```sh
$ commander aem dump [--outfile <filename> --datarate <rate in Hz> --duration <time in s> --noheader]
```

**命令行输入示例**

```sh
$ commander aem dump --outfile output.csv --duration 10
```

此命令将记录 AEM 测量值 10 秒并将数据存储在“output.csv”中，包含一个列标头。

**命令行输出示例**

```text
Logging...
Send CTRL+C to abort.
Measurements written to file: 10000
Measurements written to file: 20000
<shortened data for documentation>
Measurements written to file: 90000
Measurements written to file: 100000

Closed file 'output.csv',
100000 measurements written to file.
DONE
```

### 6.13.3 触发事件开始记录

可以设置触发参数以在电流高于某个电流阈值（以 mA 为单位）或低于某个阈值时开始记录。可以包含 `--triggerabove` 和 `--triggerbelow` 选项以指定触发事件的类型。发出此命令时只能使用这些选项之一。

使用 `--triggertimeout` 选项，您可以指定命令在超时前等待触发事件的时间。此外，可以包含 `--pretrigger` 选项以允许在导致触发事件的指定时间窗口中记录测量值。如果实际触发事件发生在预触发时间的长度过去之前（在命令执行之后），则实际包含的预触发数据将是从命令执行到触发事件发生之前收集的数据。

**注意**：注意以下选项使用的单位。

**命令行语法**

```sh
$ commander aem dump [--triggerabove <current in mA> --triggerbelow <current in mA> --triggertimeout <timeout in s> --pretrigger <time in ms>]
```

**命令行输入示例**

```sh
$ commander aem dump --triggerabove 2 --triggertimeout 10 --pretrigger 250 --datarate 1000
```

此命令以 1000 Hz 的速率记录 AEM 测量值到控制台输出（无限期），从测量的电流大于 2 mA（毫安）时开始。实际触发事件之前的 250 ms（毫秒）内记录的数据也将被记录。如果不满足触发条件，命令将在 10 秒后超时。

**命令行输出示例**

```text
Logging...
Send CTRL+C to abort.
Waiting for trigger (current above 2 mA)...
Triggered at timestamp: 155119217 [us], 3.32649 seconds after sampling started.
154869217,1.00552,3.30057
154870217,1.00447,3.30057
154871217,1.00568,3.30057
<shortened data for documentation>
155117217,1.0006,3.3007
155118217,1.00196,3.30029
155119217,4.96464,3.29997
155120217,4.96765,3.29997
155121217,4.96612,3.30016
^C
Sampling was stopped by user.
DONE
```

## 6.14 串行线输出读取命令

Simplicity Commander 支持使用 `swo read` 命令读取和转储通过 SWO（Serial Wire Output，串行线输出）接收的数据。当命令被执行时，目标设备会被复位。然后该命令将读取并转储 SWO 数据，直到通过按 Ctrl+C 终止应用程序，或者满足后续描述的条件之一。

### 6.14.1 配置 SWO 速度

此命令以 Hz 为单位设置 SWO 的速度频率。默认的 SWO 速度为 875000 Hz。SWO 速度必须与目标应用程序使用的频率相匹配。

**命令行语法**

```sh
$ commander swo read [--swospeed <frequency in Hz>]
```

**命令行输入示例**

```sh
$ commander swo read --swospeed 1000000
```

**命令行输出示例**

```text
<data written by the target application at 1 MHz>
Got signal 2, exiting...
```

### 6.14.2 读取 SWO 直到超时

此命令设置适配器在超时之前在没有接收到数据的情况下等待的秒数。默认是永不超时。

**命令行语法**

```sh
$ commander swo read [--timeout <timeout in s>]
```

**命令行输入示例**

```sh
$ commander swo read --timeout 1
```

**命令行输出示例**

```text
<data written by the target application>
Timeout: No SWO output for 1 seconds.
DONE
```

### 6.14.3 读取 SWO 直到找到标记

如果使用 `--endmarker` 选项，那么命令将在 SWO 流中找到指定的字符串后终止。

**命令行语法**

```sh
$ commander swo read [--endmarker <end marker>]
```

**命令行输入示例**

```sh
$ commander swo read [--endmarker --finished--]
```

**命令行输出示例**

```text
<data written by the target application>
--finished--
DONE
```

### 6.14.4 转储 Hex 编码的 SWO 输出

如果使用 `--hex` 选项，那么所有输入和输出都将转换为一个十六进制字符串。如果目标转储二进制数据，这很有用。如果使用 `--hex` 选项，那么 `--endmarker` 也必须是 hex-encoded。

**命令行语法**

```sh
$ commander swo read [--hex] [--endmarker <hex encoded end marker>]
```

**命令行输入示例**

```sh
$ commander swo read --hex --endmarker 50415353
```

**命令行输出示例**

```text
0a5374617274696e6720746573742067726f757020434d550a434d553a333836323a546573745f434d555f4275675f363639393a50415353
DONE
```

## 6.15 NVM3 命令

Gecko SDK 中的 NVM3（Third Generation Non-Volatile Memory，第三代非易失性存储）模块提供了一种将数据存储在 EFM32 和 EFR32 设备上的非易失性存储器（闪存）中的方法。有关 NVM3 的更多详细信息，请参阅 *UG103.7: Non-Volatile Memory Fundamentals* 或 *AN1135: Using Third Generation NonVolatile Memory (NVM3) Data Storage in Dynamic Multiprotocol Applications*。

Simplicity Commander 支持从设备中读取 NVM3 数据区并解析 NVM3 数据以提取所存储的值。这在调试场景中非常有用，您可能需要找出已运行一段时间的应用程序所存储的状态。

### 6.15.1 从设备读取 NVM3 数据

此命令在设备的闪存中搜索 NVM3 区域并将内容转储到 .bin、.s37 或 .hex 格式的文件中。

可选的 `--range` 参数可用于指定 Simplicity Commander 应搜索 NVM3 数据的存储器范围。如果没有给出范围，则搜索整个闪存。

**命令行语法**

```sh
$ commander nvm3 read -o <outfile> [--range <startaddress>:<endaddress>]
```

**命令行输入示例**

```sh
$ commander nvm3 read -o my_nvm3_data.s37
```

扫描设备闪存并搜索有效的 NVM3 区域。找到后，将 NVM3 区域写到名为 `my_nvm3_data.s37` 的文件。

**命令行输出示例**

```text
Reading 24576 bytes from 0x000fa000...
Writing to my_nvm3_data.s37...
DONE
```

### 6.15.2 解析 NVM3 数据

此命令获取包含 NVM3 数据的映像文件并解析内容。解析所得的 NVM3 对象将打印到标准输出。

可选的 `--range` 参数可用于指定 Simplicity Commander 应搜索 NVM3 数据的存储器范围。如果没有给出范围，则搜索整个闪存。

可选的 `--key` 参数可用于指定要查找的特定 NVM3 键（key）。它可以多次使用以一次查找多个键。列出所有对象时，超过八个字节数据的对象将被截断。使用 `--key` 参数选择应显示其数据的对象。

**命令行语法**

```sh
$ commander nvm3 parse <file> [--range <startaddress>:<endaddress>] [--key <object key>]
```

**命令行输入示例**

```sh
$ commander nvm3 parse my_nvm3_data.s37
```

扫描给定文件并搜索有效的 NVM3 数据。找到后，数据将被解析并打印到标准输出。

**命令行输出示例**

```text
Parsing file my_nvm3_data.s37...
Found NVM3 range: 0x000FA000 - 0x00100000
All NVM3 objects:
    KEY -       TYPE -      SIZE - DATA
0x00001 -       Data -       4 B - 2A 00 00 00
0x00002 -       Data -      16 B - 73 36 57 CA 6B CE CF E2 (+ 8 more bytes)
0x00003 -    Counter -       4 B - 2

NVM3 erase count: 1

DONE
```

### 6.15.3 在文件中初始化 NVM3 区域

`nvm3 initfile` 命令在映像文件中创建一个空白的 NVM3 区域。例如，此特性可用于创建 `nvm3 set` 命令可以处理的文件，以创建可在生产期间写入的默认 NVM3 数据集。

必须给出 NVM3 区域的大小和位置，并且必须与使用 NVM3 区域的嵌入式应用程序中使用的大小和位置相匹配。

**命令行语法**

```sh
$ commander nvm3 initfile --address <location> --size <size in bytes> --device <target device part number> --outfile <image file>
```

**命令行输入示例**

```sh
$ commander nvm3 initfile --address 0xfa000 --size 0x6000 --device EFR32MG12P233F1024 --outfile my_nvm3_data.s37
```

这将创建一个 24 kB 的 NVM3 区域，其跨越闪存地址范围 0xfa000 - 0x100000。

**命令行输出示例**

```text
Placing NVM3 area at address 0x000fa000
Writing to my_nvm3_data.s37...
DONE
```

### 6.15.4 使用文本文件写入 NVM3 数据

`nvm3 set` 命令使用一个包含 NVM3 数据区域的映像文件并设置一个或多个 NVM3 对象的值。这些对象可能已经存在，在这种情况下会更新它们的值。如果该对象尚未存在，则会创建它。要写入的数据的定义可以作为文本文件（`--nvm3file`）或作为命令行参数（`--object` 和 `--counter`）传递。

通过 `--nvm3file` 选项传递的文本文件必须具有以下格式：

* 每行定义一个对象或计数器。
* 忽略空行。
* 忽略以 \# 开头的行。

文件中的每一行都必须具有以下语法：`<key>:<type>:<data>`。

`<key>` 是 NVM3 对象的键，它是嵌入式应用程序使用的唯一标识符。它的最大大小为 20 位（最大值 0xFFFFF）。

`<type>` 是 NVM3 对象的类型。它可以是以下两个值之一：`OBJ` 或 `CNT`。`OBJ` 表示普通字节数组。`CNT` 表示 NVM3 计数器类型（32-bit 无符号整数）。

`<data>` 是对象应该设置的值。对于计数器类型，该值会被解释为无符号整数，可以用 0x 作为前缀以指示十六进制值。字节数组总是被解析为十六进制，不应以 0x 作为前缀。

**示例文件**

```text
0x00001 : OBJ : 01020304AABBCCDD
0x01000 : CNT : 0x80
0x01001 : CNT : 42
```

该文件将 ID 为 0x1 的对象设置为一个八字节长的字节数组，内容如上。

ID 为 0x1000 的对象是一个值为 0x80（128）的计数器。ID 为 0x1001 的对象是一个值为 42 的计数器。

**命令行语法**

```sh
$ commander nvm3 set <input image file> --nvm3file <filename> --outfile <image file>
```

**命令行输入示例**

```sh
$ commander nvm3 set my_nvm3_data.s37 --nvm3file nvm3_objects.txt --outfile my_modified_nvm3_data.s37
```

`nvm3_objects.txt` 会被按照上述格式解析出 NVM3 对象。扫描给定的输入映像文件以查找有效的 NVM3 区域。文本文件中定义的对象会被写入 NVM3 区域，修改后的输出被写到输出映像文件。

**命令行输出示例**

```text
Parsing file my_nvm3_data.s37...
Found NVM3 range: 0x000FA000 - 0x00100000
Setting NVM3 object: 0x00001 = 01020304AABBCCDD
Setting NVM3 counter: 0x01000 = 128 (0x00000080)
Setting NVM3 counter: 0x01001 = 42 (0x0000002a)
Writing to my_modified_nvm3_data.s37...
DONE
```

### 6.15.5 使用 CLI 选项写入 NVM3 数据

在某些情况下，直接从命令行设置 NVM3 对象数据可能比使用文本文件更方便。在这种情况下，使用命令行选项 `--object` 和 `--counter`。

这两个选项都使用相同的语法：`<key>:<data>`。`<key>` 和 `<data>` 的定义与 [6.15.4 使用文本文件写入 NVM3 数据](#6154-使用文本文件写入-nvm3-数据) 一样。两种格式之间的唯一区别是 `<type>` 字段已被移除，因为它是由命令行选项名称给出的。

Simplicity Commander 会根据给定的 NVM3 区域大小自动找到正确的 NVM3\_MAX\_OBJECT\_SIZE。

**命令行语法**

```sh
$ commander nvm3 set <input image file> --object <key>:<data> --counter <key>:<data> --outfile <image file>
```

**命令行输入示例**

```sh
$ commander nvm3 set my_nvm3_data.s37 --object 0x1:01020304AABBCCDD --counter 0x1000:0x80 --counter 0x01001:42 --outfile my_modified_nvm3_data.s37
```

所有 `--object` 和 `--counter` 参数都按照上述的格式解析。扫描给定的输入映像文件以查找有效的 NVM3 区域。~~文本文件~~参数中定义的对象会被写入 NVM3 区域，修改后的输出被写到输出映像文件。

**命令行输出示例**

```text
Parsing file my_nvm3_data.s37...
Setting NVM3 object: 0x00001 = 01020304AABBCCDD
Setting NVM3 counter: 0x01000 = 128 (0x00000080)
Setting NVM3 counter: 0x01001 = 42 (0x0000002a)
Writing to my_modified_nvm3_data.s37...
DONE
```

## 6.16 CTUNE 命令

Wireless Gecko (EFR32™) 产品组合设备支持在软件中配置晶振负载电容。CTUNE（Crystal Oscillator Load Capacitor Tuning，晶振负载电容器调谐）值在基于 Wireless Gecko 的 modules 和 Silicon Labs WSTK（Wireless Starter Kit）radio boards 的生产测试期间进行调谐。对于 modules，每个设备的最佳值被写到闪存中的 DI（Device Information，设备信息）页。对于 radio boards，每个板的最佳值被写到一个 EEPROM，目标设备上运行的软件无法访问该 EEPROM，但 Simplicity Commander 可以读取。`ctune` 命令支持从这些位置读取存储的 CTUNE 值，以及写入和读取 CTUNE manufacturing token。

### 6.16.1 CTUNE 获取命令

此命令检索存储在 Device Info 页中的 CTUNE 值、存储在板上 EEPROM 中的值以及已写到 CTUNE manufacturing token 的值。这些值将会被显示。

**命令行语法**

```sh
$ commander ctune get
```

**命令行输入示例**

```sh
$ commander ctune get
```

**命令行输出示例**

```text
Getting CTUNE values from the Device Info page, stored in EEPROM on the board, and the MFG token.
DI:    Not set
Board: 346
Token: 346
DONE
```

**注意**：并非所有设备都将 CTUNE 值存储在 Device Info 页和板上的 EEPROM 中。如果是这种情况，该值将显示为“Not set”。

### 6.16.2 CTUNE 设置命令

该命令将 CTUNE manufacturing token 设置为 value 选项指定的值。

**命令行语法**

```sh
$ commander ctune set <value>
```

**命令行输入示例**

```sh
$ commander ctune set --value 346
```

**命令行输出示例**

```text
Setting CTUNE token to 346
DONE
```

### 6.16.3 CTUNE 自动设置命令

此命令从板上的 EEPROM 中检索 CTUNE 值，并将 CTUNE manufacturing token 设置为该值。

**命令行语法**

```sh
$ commander ctune autoset
```

**命令行输入示例**

```sh
$ commander ctune autoset
```

**命令行输出示例**

```text
Getting CTUNE value stored on the board...
Board: 346
Setting the CTUNE value...
```

## 6.17 安全命令

### 6.17.1 获取设备状态

此命令打印安全元素设备信息状态，包括：

* 固件版本 
* 序列号 
* 设备擦除状态 
* 安全调试解锁状态 
* 篡改状态 
* 安全启动状态

**命令行语法**

```sh
$ commander security status [--trustzone --verbose]
```

**命令行输入示例**

```sh
$ commander security status
```

**命令行输出示例**

```text
SE Firmware version : 1.1.3
Serial number       : 0000000000000000d0cf5efffe68a68b
Debug lock          : Disabled
Device erase        : Enabled
Secure debug unlock : Disabled
Tamper status       : OK
Secure boot         : Disabled
Boot status         : 0x20 - OK
DONE
```

`Debug lock` 指示调试访问是启用（锁定）还是禁用（解锁）。

`Device erase` 指示是否可以使用 `device erase` 命令重新获得调试访问权限。如果启用了设备擦除，那么这是可以的。如果设备被禁用，则不可能。

`Security debug unlock` Enabled 表示如果设备被锁定，可以使用 `security unlock` 命令重新获得调试访问权限。如果 `device erase` 和 `secure debug unlock` 均已禁用，则在设备被锁定时无法重新获得调试访问权限。

`Tamper status` 指示设备是否检测到篡改事件。OK 表示未检测到篡改事件。

`Secure boot` Enabled 意味着设备上运行的所有映像都必须使用与写到设备的公共签署密钥相对应的私有签署密钥进行签署。`Secure boot` Disabled 意味着映像不必使用签署密钥进行签署。

`Boot status` 显示安全启动是否失败或安全启动是否正常。

**命令行输入示例**

```sh
$ commander security status --trustzone
```

显示设备的 TrustZone 状态。

**命令行输出示例**

```text
SE Firmware version : 1.1.3
Serial number       : 0000000000000000d0cf5efffe68a68b
Debug lock          : Disabled
Device erase        : Enabled
Secure debug unlock : Disabled

Debug lock state: Unlocked

Non-secure, invasive debug lock     (DBGLOCK)  : Unlocked
Non-secure, non-invasive debug lock (NIDLOCK)  : Unlocked
Secure, invasive debug lock         (SPIDLOCK) : Unlocked
Secure, non-invasive debug lock     (SPNIDLOCK): Unlocked

Non-secure, invasive debug lock state     (DBGLOCK)  : Unlocked
Non-secure, non-invasive debug lock state (NIDLOCK)  : Unlocked
Secure, invasive debug lock state         (SPIDLOCK) : Unlocked
Secure, non-invasive debug lock state     (SPNIDLOCK): Unlocked

Tamper status   : OK
Secure boot     : Disabled
Boot status     : 0x20 - OK
DONE
```

`Debug lock state` 指示调试端口是已锁定还是已解锁。

`TrustZone` 调试锁配置由四种模式 `SPNIDLOCK`、`SPIDLOCK`、`NIDLOCK` 和 `DBGLOCK` 组成。上部的配置指定了 `security lock --trustzone` 命令锁定了哪个模式。下部的配置指定模式的实际状态，是否已解锁。

**命令行输入示例**

```sh
$ commander security status --verbose
```

显示安全状态的详细输出。

**命令行输出示例**

```text
SE Firmware version : 1.1.3
Serial number       : 0000000000000000d0cf5efffe68a68b
Debug lock          : Disabled
Device erase        : Enabled
Secure debug unlock : Disabled
Tamper status       : OK
Secure boot         : Disabled
Boot status         : 0x20 - OK
Verbose output: 00000000 00000000 00000000 FFFFFFFF 00000020 03020200 00000000 00000002 FFFFFFFF
DONE
```

`Verbose output` 是设备安全元素的全部输出。

### 6.17.2 生成密钥对

此命令已被弃用。有关如何生成密钥的更多信息，请参阅 [6.18.2 生成签署密钥](#6182-生成签署密钥) 和 [6.18.1 密钥生成](#6181-密钥生成)。

### 6.17.3 将公钥写到设备

**重要：这是一个一次性命令。它不能在每个设备上多次运行。**

这个一次性命令将设备永久锁定到这个密钥对。有两种不同的公钥可以写到设备。

* 命令密钥 —— 相应的私钥用于创建证书以执行安全调试解锁。
* 签署密钥 —— 相应的私钥必须对要在启用安全启动时在设备上运行的所有代码进行签署。

启用安全调试解锁后，锁定的设备可以通过创建由私有命令密钥签署的证书来临时解锁调试访问。

启用安全启动后，设备上运行的所有代码都必须由私钥签署。

**命令行语法**

```sh
$ commander security writekey [--command <public key PEM file>] [--sign <public key PEM file>]
```

**命令行输入示例**

```sh
$ commander security writekey --command command_public_key.pem
```

**命令行输出示例**

```text
Device has serial number 000000000000000014b457fffed50c35
================================================================================
Please look through any warnings before proceeding.
THIS IS A ONE-TIME command, all code to be run on the device must be signed by this key.
Type 'continue' and hit enter to proceed or Ctrl-C to abort:
================================================================================
continue
DONE
```

### 6.17.4 从设备读取公钥

此命令从设备中读取公钥。使用 [`commander security writekey`](#6173-将公钥写到设备) 命令可以将两种不同的公钥存储在设备上。

* 命令密钥 —— 相应的私钥用于创建证书以执行安全调试解锁或禁用篡改。
* 签署密钥 —— 相应的私钥必须对要在启用安全启动时在设备上运行的所有代码进行签署。

通过提供一个输出文件，密钥将被写入到文件。否则，密钥将作为字节数组打印到 CLI。

如果不使用可选的 `--nostore` 选项，密钥也将存储在 [安全储存](#51-安全储存) 中。

**命令行语法**

```sh
$ commander security readkey [--command] [--sign] [--outfile <filename>] [--nostore]
```

**命令行输入示例**

```sh
$ commander readkey --command --outfile command_public_key.pem
```

**命令行输出示例**

```text
Writing public key file in PEM format to key.pem...
DONE
```

### 6.17.5 配置锁定选项

`security lockconfig` 命令启用或禁用安全调试解锁。启用安全调试解锁后，可以通过 [`commander security unlock`](#6177-安全调试解锁) 命令临时解锁被锁定的设备。如果安全调试解锁被禁用，解锁锁定设备的唯一方法是运行 [`commander security erasedevice`](#6179-使用安全元素的设备擦除) 命令，这个前提是设备擦除尚未被禁用。如果设备擦除和安全调试解锁都被禁用，则无法解锁对锁定设备的调试访问。

**注意**：安全调试解锁必须在锁定设备之前启用。

**命令行语法**

```sh
$ commander security lockconfig --secure-debug-unlock <enable/disable>
```

**命令行输入示例**

```sh
$ commander security lockconfig --secure-debug-unlock enable
```

**命令行输出示例**

```text
Secure debug unlock was enabled.
DONE
```

### 6.17.6 锁定调试访问

`lock` 命令锁定设备上的调试接口。如果已启用安全调试解锁，则可以使用 `unlock` 命令解锁设备。如果设备擦除没有被禁用，调试访问也可以使用 [`commander security erasedevice`](#6179-使用安全元素的设备擦除) 命令解锁。但是，这也会触发设备上的批量擦除。

`--trustzone` 选项可用于锁定对特定 TrustZone 模式的调试访问。设置 TrustZone 调试锁的位掩码定义为 `<SPNIDLOCK, SPIDLOCK, NIDLOCK, DBGLOCK>`。如果该位设置为 1，将锁定对相应 TrustZone 模式的调试访问。将该位设置为 0 以使其保持打开状态。默认情况下，所有模式都是打开的。

**命令行语法**

```sh
$ commander security lock [--trustzone <xxxx>]
```

**命令行输入示例**

```sh
$ commander security lock
```

**命令行输出示例**

```text
Device is now locked.
DONE
```

**命令行输入示例**

```sh
$ commander security lock --trustzone 0011
```

对 TrustZone 模式 `DBGLOCK` 和 `NIDLOCK` 的调试访问将被锁定。

**命令行输出示例**

```text
Writing debug restriction bits:
DBGLOCK:   1
NIDLOCK:   1
SPIDLOCK:  0
SPNIDLOCK: 0
Device is now locked.
DONE
```

### 6.17.7 安全调试解锁

security unlock 命令会在不擦除闪存内容的情况下临时打开已锁定设备上的调试访问。运行 `commander security unlock` 命令时，Simplicity Commander 将使用安全储存和命令行选项中的所有可用文件来尝试解锁调试访问。如果缺少任何内容，那么将要求您提供该文件作为命令的选项。除非使用 `--nostore` 选项，否则作为命令行选项生成或提供的所有文件都会存储在安全储存中。

有关安全调试的更多信息，请参阅 *AN1190: EFR32xG21 Secure Debug*。

有几种不同的方法可以解锁调试访问，如下图所示。蓝色字段是动作，红色字段是制品。

<figure>
  <img src="images/Figure%206.1.%20Unlock%20Flow.png"
       alt="Figure 6.1. Unlock Flow"
       style="display:block; margin-left: auto; margin-right: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 6.1. Unlock Flow</figcaption>
</figure>

**命令行语法**

```sh
$ commander security unlock [--cert <signed access certificate> --cert-signature <signature> --command-signature <signature> --cert-privkey <keyfile> --cert-pubkey <keyfile> --command-key <keyfile> --nostore]
```

**命令行输入示例**

```sh
$ commander security unlock --command-key command_key.pem
```

此示例使用提供的命令密钥即时使用并生成证书和命令签名来签署证书。所有生成的文件和命令密钥都存储在安全储存中。

**命令行输出示例**

```text
Command public key stored in:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/command_pubkey.pem
Command private key stored in:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/command_key.pem
Authorization file written to Security Store:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/certificate_authorizations.json
Generating ECC P256 key pair...
Cert public key stored at:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/cert_pubkey.pem
Cert private key stored at:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/cert_key.pem
Command key matches public command key found on device. Signing certificate...
Certificate was signed with key:
test-cases/common/security_testfiles/command_key.pem
Successfully stored certificate
Certificate written to Security Store:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/access_certificate.bin
Created unsigned unlock command
Signed unlock command using
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/cert_key.pem
Secure debug successfully unlocked
Command unlock payload was stored in Security Store
DONE
```

**命令行输入示例**

```sh
$ commander security unlock --cert access_certificate.bin --cert-privkey cert_key.pem
```

此示例使用已签署的访问证书和与访问证书中的公钥对应的私有证书密钥解锁设备。证书和密钥存储在安全储存中。

**命令行输出示例**

```text
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/access_certificate.bin
Cert key written to Security Store:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/cert_pubkey.pem
Created unsigned unlock command
Signed unlock command using
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/cert_key.pem
Secure debug successfully unlocked
Command unlock payload was stored in Security Store
DONE
```

**命令行输入示例**

```sh
$ commander security unlock --cert-signature cert_signature.bin --command-signature command_signature.bin
```

此示例对访问证书和命令文件使用外部生成的签名。访问证书签名附加到证书并存储在安全储存中。命令签名根据证书中的公钥进行验证。

**命令行输出示例**

```text
Using certificate from Security Store:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/access_certificate.bin
Certificate in Security Store is not signed.
Moved existing file to:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/archive/access_certificate.bin
Signed certificate written to Security Store:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/access_certificate.bin
Command signature is valid
Secure debug successfully unlocked
Command unlock payload was stored in Security Store
```

**命令行输入示例**

```sh
$ commander security unlock
```

当使用当前的 challenge 解锁设备时，解锁有效负载将存储在安全储存中。下次运行解锁命令时，设备将直接使用解锁负载解锁。

**命令行输出示例**

```text
Unlocking with unlock payload:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/challenge_4329288395adfc4eea436e5d64dd296b/unlock_payload_0000000000111110.bin
DONE
```

### 6.17.8 禁用篡改

安全保险库（Secure Vault）产品能够检测某些类型的篡改事件并做出响应以减轻攻击。这提供了一个额外的保护层，以防止依赖于对产品进行物理篡改的攻击。

在执行此命令之前，必须在设备的 OTP（One-Time-Programmable，一次性可编程）设置中配置篡改源。有关如何完成此操作的更多信息，请参阅 [6.17.16 写入用户配置](#61716-写入用户配置)。

禁用篡改的过程遵循与 `security unlock` 命令相同的流程。有关该流程的更多信息，请参阅 [6.17.7 安全调试解锁](#6177-安全调试解锁)。

禁用篡改需要证书和已签署的 challenge。生成证书（包括篡改授权）并使用命令密钥对其进行签署。该证书包含一个公钥，必须使用相应的私钥来签署来自设备的 challenge 以禁用篡改源。`--disable-param` 选项确定要禁止的篡改源。如果未提供此选项，Simplicity Commander 将从证书中提取篡改授权并禁止证书允许的所有内容。如果证书不可用，所有源都将被禁止。

篡改源会被禁止，直到下一次上电复位（Power On Reset）。

**命令行语法**

```sh
$ commander security disabletamper [--disable-param <disable-mask> --cert <signed access certificate> --certsignature <signature> --commandsignature <signature> --cert-privkey <keyfile> --cert-pubkey <keyfile> --command-key <keyfile> --nostore]
```

**命令行输入示例**

```sh
$ commander security disabletamper --cert access_certificate.bin --cert-privkey cert_key.pem
```

**命令行输出示例**

```text
Using tamper parameters from certificate in Security Store: 0xffffffb6
Certificate written to Security Store:
/Users/matundal/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000000d6ffffead3617/access_certificate.bin
Cert key written to Security Store:
/Users/matundal/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000000d6ffffead3617/cert_pubkey.pem
Using tamper parameters from certificate in Security Store: 0xffffffb6
Created unsigned disable tamper command
Signed disable tamper command using
/Users/matundal/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000000d6ffffead3617/cert_key.pem
Tamper successfully disabled.
Command disable tamper payload was stored in Security Store

DONE
```

### 6.17.9 使用安全元素的设备擦除

此命令执行一个设备批量擦除并将调试配置重置为其初始已解锁状态。

系统的整个闪存和 RAM 都会被清除，除了安全元素中的 user data 页和 OTP commissioning 信息。

如果设备擦除已被禁用，则此命令不可用。

**注意**：设备擦除后，在设备被重置之前 DCI 接口不可用。

**命令行语法**

```sh
$ commander security erasedevice
```

**命令行输入示例**

```sh
$ commander security erasedevice
```

**命令行输出示例**

```text
Successfully erased device
DONE
```

### 6.17.10 禁用设备擦除

**重要：这是一个一次性命令。它不能被多次运行。**

此命令永久地禁用设备擦除。当禁用设备擦除时，[`commander security erasedevice`](#6179-使用安全元素的设备擦除) 命令不再可用。这意味着如果调试访问被锁定，则只有在设备被锁定之前启用了安全调试解锁才能打开调试访问。否则，无法重新获得调试访问权。该命令可以在设备被锁定后运行。

执行此命令需要用户确认，除非使用 `--noprompt` 选项。

**命令行语法**

```sh
$ commander security disabledeviceerase [--noprompt]
```

**命令行输入示例**

```sh
$ commander security disabledeviceerase
```

**命令行输出示例**

```text
================================================================================
THIS IS A ONE-TIME command which Permanently disables device erase.
If secure debug lock has not been set, there is no way to regain debug access to this device.
Type 'continue' and hit enter to proceed or Ctrl-C to abort:
================================================================================
continue
Disabled device erase successfully
DONE
```

### 6.17.11 Roll Challenge

此命令使安全元素 *roll* 或更新其 challenge 数据。Challenge 是在执行 unlock 命令之前必须从设备读取的随机数据。Roll challenge 会使现有的命令签名失效。有关详细信息，请参阅 [5.3 Challenge 和命令签署](#53-challenge-和命令签署)。

Challenge 在至少使用一次之前（即，通过运行 security unlock 命令或 disable tamper 命令）不能 roll。

**命令行语法**

```sh
$ commander security rollchallenge
```

**命令行输入示例**

```sh
$ commander security rollchallenge
```

**命令行输出示例**

```text
Challenge was rolled successfully.
DONE
```

### 6.17.12 生成示例授权文件

此命令生成要在证书中使用的默认授权文件。授权文件将存储在安全储存中，除非使用 `--nostore` 选项。

**Default Authorization File for Devices without Secure Vault**

```json
{
    "debug_authorizations":{
        "ENABLE_DEBUG_PORT": true
    }
}
```

**Default Authorization File for Devices with Secure Vault**

```json
{
    "debug_authorizations":{
        "ENABLE_DEBUG_PORT": true
    },
    "tamper_authorizations":{
        "FILTER_COUNTER": 1,
        "WATCHDOG": 1,
        "SE_RAM_CRC": 1,
        "SE_HARDFAULT": 1,
        "SOFTWARE_ASSERTION": 1,
        "SE_CODE_AUTH": 1,
        "USER_CODE_AUTH": 1,
        "MAILBOX_AUTH": 1,
        "DCI_AUTH": 1,
        "OTP_READ": 1,
        "AUTO_CODE_AUTH": 1,
        "SELF_TEST": 1,
        "TRNG_MONITOR": 1,
        "PRS0": 1,
        "PRS1": 1,
        "PRS2": 1,
        "PRS3": 1,
        "PRS4": 1,
        "PRS5": 1,
        "PRS6": 1,
        "PRS7": 1,
        "DECOUPLE_BOD": 1,
        "TEMP_SENSOR": 1,
        "VGLITCH_FALLING": 1,
        "VGLITCH_RISING": 1,
        "SECURE_LOCK": 1,
        "SE_DEBUG": 1,
        "DGLITCH": 1,
        "SE_ICACHE": 1
    }
}
```

**Debug Authorization**

Enable Debug Port 必须设置为 *true* 才能执行安全调试解锁。有关安全调试解锁的更多信息，请参阅 [6.17.7 安全调试解锁](#6177-安全调试解锁)。

**Tamper Authorizations**

Tamper Authorizations 指示可以禁止哪些源。默认情况下，所有源都可能被禁止。有关禁用篡改源的更多信息，请参阅 [6.17.8 禁用篡改](#6178-禁用篡改)。

**命令行语法**

```sh
$ commander security genauth [-o <filename>] [--nostore]
```

**命令行输入示例**

```sh
$ commander security genauth -o certificate_authorization.json --nostore
```

**命令行输出示例**

```text
Authorization file stored in:
certificate_authorization.json
DONE
```

### 6.17.13 生成访问证书

访问证书用于解锁调试访问或禁止设备上的篡改。有关详细信息，请参阅 [6.17.7 安全调试解锁](#6177-安全调试解锁) 或 [禁用篡改](#6178-禁用篡改)。提供给 Simplicity Commander 或由 Simplicity Commander 生成的证书和密钥存储在安全储存中，除非使用 `--nostore` 选项。 如果未使用 `--cert-pubkey` 或 `--authorization` 命令行选项，Simplicity Commander 会检查文件是否存储在安全储存中。如果文件不在安全储存中，Simplicity Commander 会生成一个可以编辑的默认授权文件。如果文件被编辑，则必须生成新的证书。如果未使用 `--cert-pubkey` 选项，那么 Simplicity Commander 还将生成一对证书密钥。如果生成了证书密钥，则不能使用 `--nostore` 选项。如果 `--command-key` 选项未在命令行上使用且其不位于安全存储中，则应使用 `--extsign` 选项让 Simplicity Commander 生成未签署的证书。要使用证书解锁调试访问，必须生成并提供一个证书签名。如果为其制作证书的设备已连接，Simplicity Commander 会直接检索设备序列号。

**注意**：在 Simplicity Commander 1.11.2 之前，未签署的证书是用全零代替签名创建的。这在 1.11.2 中得到修复，使其与使用 OpenSSL 等工具的外部签署兼容。

<figure>
  <img src="images/Figure%206.2.%20Access%20Certificate.png"
       alt="Figure 6.2. Access Certificate"
       style="display:block; margin-left: auto; margin-right: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 6.2. Access Certificate</figcaption>
</figure>

**命令行语法**

```sh
$ commander security gencert [--cert-pubkey <public key file>] [--authorization <auth-file>] [--command-key <private key file>][--extsign][--devserialno <serial number>] [-o <filename>] [--nostore]
```

**命令行输入示例**

```sh
$ commander security gencert --extsign
```

此示例生成一个未签署的证书，因为命令私钥未作为一个命令选项提供，且其也不位于安全储存中。公共证书密钥也没有提供，因此 Simplicity commander 生成一对证书密钥并将它们存储在安全储存中。还会生成默认的授权文件并将其存储在安全储存中。

**命令行输出示例**

```text
Authorization file written to Security Store:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/certificate_authorizations.json
Generating ECC P256 key pair...
Cert public key stored at:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/cert_pubkey.pem
Cert private key stored at:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/cert_key.pem
Successfully stored certificate
Created an unsigned certificate in Security Store:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/access_certificate.extsign
DONE
```

**命令行输入示例**

```sh
$ commander security gencert --cert-pubkey cert_pubkey.pem --authorization certificate_authorizations.json --command-key command_key.pem -o access_certificate.bin --nostore
```

在此示例中，生成证书所需的所有文件都作为命令行选项提供。设备序列号直接取自已连接的设备。该证书使用私有命令密钥签署，可用于解锁调试访问。

**命令行输出示例**

```text
Command key matches public command key found on device. Signing certificate...
Certificate was signed with key:
command_key.pem
DONE
```

**命令行输入示例**

```sh
$ commander security gencert
```

此示例使用已位于安全储存中的文件来生成一个已签署的证书。证书存储在安全储存中。

**命令行输出示例**

```text
Using authorizations from Security Store:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/certificate_authorizations.json
Using public key from Security Store:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/cert_pubkey.pem
Found command key in Security Store:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/command_key.pem
Certificate was signed with key:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/command_key.pem
DONE
```

### 6.17.14 生成未签署的命令文件

`commander security gencommand` 命令从设备中检索 [安全 challenge](#53-challenge-和命令签署) 并将其与其他数据一起存储在一个文件中，如 [Figure 5.2. Unlock Command Signature](#figure_5_2) 所述。使用私有证书密钥的此文件的签名可用作有效负载的部分以执行安全调试解锁。

除非使用 `--nostore` 选项，否则未签署的命令文件将存储在安全储存中。

如果用户拥有私有证书密钥，Simplicity Commander 会使用 [`commander security unlock`](#6177-安全调试解锁) 命令自动生成命令文件和签名。如果命令文件由外部过程（如 HSM）签署，则在执行 `commander security unlock` 命令时，需要将命令签名作为命令行选项传递。

**命令行语法**

```sh
$ commander security gencommand --action debug_unlock [-o <output file>] [--nostore]
```

**命令行输入示例**

```sh
$ commander security gencommand --action debug-unlock -o unlock_command_to_be_signed.bin --nostore
```

**命令行输出示例**

```text
Unsigned command file written to:
unlock_command_to_be_signed.bin
DONE
```

### 6.17.15 生成示例配置文件

此命令生成一个默认配置文件以与 `security_writeconfig` 命令一起使用。除非使用 `--nostore` 选项，否则文件将存储在安全储存中。

**Default Configuration File for Devices without Secure Vault**

```json
{
    "mcu_flags": {
        "SECURE_BOOT_ENABLE": true,
        "SECURE_BOOT_VERIFY_CERTIFICATE": false,
        "SECURE_BOOT_ANTI_ROLLBACK": true,
        "SECURE_BOOT_PAGE_LOCK_NARROW": false,
        "SECURE_BOOT_PAGE_LOCK_FULL": true
    }
}
```

**Default Configuration File for Devices with Secure Vault**

```json
{
    "mcu_flags": {
        "SECURE_BOOT_ENABLE": true,
        "SECURE_BOOT_VERIFY_CERTIFICATE": false,
        "SECURE_BOOT_ANTI_ROLLBACK": true,
        "SECURE_BOOT_PAGE_LOCK_NARROW": false,
        "SECURE_BOOT_PAGE_LOCK_FULL": true
    },
    "tamper_levels": {
        "FILTER_COUNTER": 0,
        "WATCHDOG": 4,
        "SE_RAM_CRC": 4,
        "SE_HARDFAULT": 4,
        "SOFTWARE_ASSERTION": 4,
        "SE_CODE_AUTH": 4,
        "USER_CODE_AUTH": 4,
        "MAILBOX_AUTH": 0,
        "DCI_AUTH": 0,
        "OTP_READ": 0,
        "AUTO_CODE_AUTH": 0,
        "SELF_TEST": 4,
        "TRNG_MONITOR": 0,
        "PRS0": 0,
        "PRS1": 0,
        "PRS2": 0,
        "PRS3": 0,
        "PRS4": 0,
        "PRS5": 0,
        "PRS6": 0,
        "PRS7": 0,
        "DECOUPLE_BOD": 4,
        "TEMP_SENSOR": 1,
        "VGLITCH_FALLING": 0,
        "VGLITCH_RISING": 0,
        "SECURE_LOCK": 4,
        "SE_DEBUG": 0,
        "DGLITCH": 0,
        "SE_ICACHE": 4
    },
    "tamper_filter" : {
        "FILTER_PERIOD": 0,
        "FILTER_THRESHOLD": 0,
        "RESET_THRESHOLD": 0
    },
    "tamper_flags": {
        "DGLITCH_ALWAYS_ON": false
    }
}
```

**MCU settings**

* **Secure Boot Enable** —— 如果设置了此选项，则在设备上启用安全启动。要求设备上运行的所有应用程序都已签署。
* **Secure Boot Verify Certificate** —— 如果设置了此选项，则设备上运行的应用程序都必须已使用了中间证书进行签署。即使未设置此选项，仍然可以使用证书进行签署。有关详细信息，请参阅 [6.5.9 使用中间证书为安全启动签署应用程序](#659-使用中间证书为安全启动签署应用程序)。
* **Secure Boot Anti Rollback** —— 如果设置了此选项，版本低于当前存储在闪存中的映像的应用程序映像将不会在设备上运行。
* **Secure Boot Page Lock Narrow** —— 安全启动过程验证的闪存页会被锁定，以防止通过 Root Code 以外的方式重新刷写。从 0 到应用程序安全启动签名所在页的页会被锁定，如果签名不在页边界上，则**不包括**最后一页。
* **Secure Boot Page Lock Full** —— 安全启动过程验证的闪存页会被锁定，以防止通过 Root Code 以外的方式重新刷写。从 0 到应用程序安全启动签名所在页的页会被锁定，如果签名不在页边界上，则**包括**最后一页。

**Tamper Levels**

不同的篡改源列在 tamper levels 下。默认配置是绝对最低的。Root Code 永远不会将篡改级别设置为低于默认配置的设置。下表列出了篡改级别。

<table style="margin-left: auto; margin-right: auto;">
<caption style="white-space: nowrap;">Table 6.1. Tamper Levels</caption>
<thead>
  <tr>
    <th>Tamper Level</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>1</td>
    <td>No action taken</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Generate SE interrupt</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Increment filter counter</td>
  </tr>
  <tr>
    <td>4</td>
    <td>System Reset</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Reserved</td>
  </tr>
  <tr>
    <td>6</td>
    <td>Reserved</td>
  </tr>
  <tr>
    <td>7</td>
    <td>Erase OTP (Makes the device unrecoverable; it will neve boot again.)</td>
  </tr>
</tbody>
</table>

**命令行语法**

```sh
$ commander security genconfig [-o <filename>] [--nostore]
```

**命令行输入示例**

```sh
$ commander security genconfig -o user_configuration.json --nostore
```

**命令行输出示例**

```text
Configuration file stored in:
user_configuration.json
DONE
```

### 6.17.16 写入用户配置

**重要：这是一个一次性命令。它不能被多次运行。**

`commander security writeconfig` 命令设置 Root Code 中配置文件中确定的配置。

通过此命令启用安全启动。在启用安全启动之前，您必须将公共签署密钥写到设备。有关将密钥写到设备的更多信息，请参阅 [6.17.3 将公钥写到设备](#6173-将公钥写到设备)。此外，必须生成一个配置文件，并且必须将 Secure Boot Enabled 标志设置为 true。如果没有提供配置文件，那么将生成一个默认配置。

在 Simplicity Commander 1.9 中，具有安全保险库的设备支持篡改配置。篡改配置决定了安全元素在发生篡改事件时的响应。有关配置文件和篡改配置的更多信息，请参阅 [6.17.15 生成示例配置文件](#61715-生成示例配置文件)。

有关安全启动的更多信息，请参阅 *AN1218: Series 2 Secure Boot with RTSL*。

有关篡改事件的更多信息，请参阅 [6.17.8 禁用篡改](#6178-禁用篡改)。

**命令行语法**

```sh
$ commander security writeconfig [--configfile <config file>] [--nostore] [--nopromt]
```

**命令行输入示例**

```sh
$ commander security writeconfig --configfile user_configuration.json
```

**命令行输出示例**

```text
================================================================================
THIS IS A ONE-TIME configuration: Please inspect file before confirming:
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b/user_configuration.json
Type 'continue' and hit enter to proceed or Ctrl-C to abort:
================================================================================
continue
DONE
```

### 6.17.17 读取用户配置

此命令从设备返回 OTP 设置。如果设备尚未使用 [6.17.16 写入用户配置](#61716-写入用户配置) 命令进行配置，则没有 OTP 设置可读。

**命令行语法**

```sh
$ commander security readconfig
```

**命令行输入示例**

```sh
$ commander security readconfig
```

**命令行输出示例**

```text
MCU Flags
Secure Boot                    : Enabled
Secure Boot Verify Certificate : Disabled
Secure Boot Anti Rollback      : Enabled
Secure Boot Page Lock Narrow   : Disabled
Secure Boot Page Lock Full     : Enabled

Tamper Levels
FILTER_COUNTER      : 0
WATCHDOG            : 4
SE_RAM_CRC          : 4
SE_HARDFAULT        : 4
SOFTWARE_ASSERTION  : 4
SE_CODE_AUTH        : 4
USER_CODE_AUTH      : 4
MAILBOX_AUTH        : 0
DCI_AUTH            : 0
OTP_READ            : 0
AUTO_CODE_AUTH      : 0
SELF_TEST           : 4
TRNG_MONITOR        : 0
PRS0                : 0
PRS1                : 0
PRS2                : 0
PRS3                : 0
PRS4                : 0
PRS5                : 0
PRS6                : 0
PRS7                : 0
DECOUPLE_BOD        : 4
TEMP_SENSOR         : 1
VGLITCH_FALLING     : 0
VGLITCH_RISING      : 0
SECURE_LOCK         : 4
SE_DEBUG            : 0
DGLITCH             : 0
SE_ICACHE           : 4

Tamper Filter
Filter Period   : 0
Filter Treshold : 0
Reset Treshold  : 0

Tamper Flags
Digital Glitch Detector Always On: Disabled

DONE
```

### 6.17.18 获取安全储存路径

获取安全储存的路径。如果连接了设备或提供了 `--deviceserialno` 选项，则返回设备特定的路径。否则，返回安全储存的路径。

**命令行语法**

```sh
$ commander security getpath [--deviceserialno <deviceserialno>]
```

**命令行输入示例**

```sh
$ commander security getpath
```

**命令行输出示例**

```text
/Users/example/Library/Preferences/SiliconLabs/commander/SecurityStore/device_0000000000000000d0cf5efffe68a68b
DONE
```

### 6.17.19 写入 AES 解密密钥

**重要：这是一个一次性命令。它不能在每个设备上多次运行。**

对称的 128-bit AES 密钥用于解密 GBL 文件。此密钥也称为 MFG_BOOTLOAD_AES_KEY。此设备上的所有加密映像都必须使用相同的 AES 密钥加密。

**命令行语法**

```sh
$ commander security writekey --decrypt <filename>
```

**命令行输入示例**

```sh
$ commander security writekey --decrypt key.txt
```

**命令行输出示例**

```text
Device has serial number 000000000000000014b457fffed50c35
================================================================================
Please look through any warnings before proceeding.
THIS IS A ONE-TIME command, all code to be run on the device must be signed by this key.
Type 'continue' and hit enter to proceed or Ctrl-C to abort:
================================================================================
continue
DONE
```

### 6.17.20 读取设备证书

此命令从设备中读取 X509 证书。可用的证书是：

* batch —— 每个制造批次都相同
* SE —— 每个设备唯一
* MCU —— 每个设备唯一

这些证书形成一个信任根证书链，其溯源到 Silicon Labs 颁发的 `Silicon Labs Root Certificate`。`SE` 和 `MCU Certificates` 由 `Batch Certificate` 颁发。`Batch Certificate` 由 `Factory Certificate` 颁发，`Factory Certificate` 由 `Silicon Labs Root Certificate` 颁发。

如果没有给出 outfile，则有关证书的关键信息将打印到命令行。可以通过提供 `outfile` 参数来完整读出证书。可用的编码是 `pem` 和 `der`。

**命令行语法**

```sh
$ commander security readcert <cert type> [--outfile <filename>]
```

**命令行输入示例**

```sh
$ commander security readcert batch
```

**命令行输出示例**

```text
Version            : 3
Subject            : CN=Batch 1001317 O=Silicon Labs Inc. C=US
Issuer             : CN=Factory O=Silicon Labs Inc. C=US
Valid From         : October 17 2019
Valid To           : September 16 2118
Signature algorithm: SHA256
Public Key Type    : ECDSA
Public key         : b0c113190bba3d1ee507d954e878957ad5cc8903ec7785525b8c0b2c2185514cd1421498487c5ea554801924468f8534e027e6496fcbdecef3659cd
DONE
```

**命令行输入示例**

```sh
$ commander security readcert se --outfile se_cert.pem
```

**命令行输出示例**

```text
Writing certificate to se_cert.pem...
DONE
```

### 6.17.21 保险库设备证明

设备证明（Attestation）用于以加密方式向远程方证明他们就是他们所说的系统，并确保他们正在与之交谈的设备与工厂生产的设备是同样的设备。

证明过程从验证证书链开始，其溯源到 Silicon Labs Root certificate。有关证书的更多信息，请参阅 [6.17.20 读取设备证书](#61720-读取设备证书)。

Attestation token 会被打印到命令行。Token 由下表中列出的多个声明（claims）组成。

| Claim ID | Claim friendly name             | Present in token        | Content                                                                          |
| :------- | :------------------------------ | :---------------------- | :------------------------------------------------------------------------------- |
| -75000   | ARM PSA Profile ID              | Always                  | ASCII 'SILABS_1'                                                                 |
| -75008   | ARM PSA nonce                   | Always                  | Copy of the nonce supplied as input to the token generation command.             |
| -75009   | ARM PSA/IETF EAT UEID           | Always                  | The device's EUI-64 pre-pended with 0x06 and zeroes.                             |
| -76000   | SE status                       | Always                  | Current SE status                                                                |
| -76001   | OTP configuration               | Always when provisioned | User configuration                                                               |
| -76002   | MCU Sign key                    | Always when provisioned | Public sign key                                                                  |
| -76003   | MCU Command key                 | Always when provisioned | Public command key                                                               |
| -76004   | Current applied tamper settings | Always                  | Currently applied tamper level per tamper signal (one nibble per tamper signal). |


最后，如以下示例所示验证证明 token 的签名。

**命令行语法**

```sh
$ commander security attestation
```

**命令行输入示例**

```sh
$ commander security attestation
```

**命令行输出示例**

```text
Certificate chain successfully validated up to Silicon Labs device root certificate.

-75008 ARM PSA nonce                   : 1799c9296ac44a854b74fe50dc6f1546a5c1e17de73584afcc478739161db7d0
-75000 ARM PSA Profile ID              : SILABS_1
-75009 ARM PSA/IETF EAT UEID           : 0614b457fffe0f7789
-76000 SE status                       : 000000010000000000000000000000000000002000010202ffffffff00000002ffffffff
-76002 MCU sign key                    : fb2470314c0710f5a72e89a30d2af607770187568f80cffa7fc6516f61e0dc258a8606fe664a097eb94d3ea29e1b87262babdb969842da31512bdc7
-76003 MCU command key                 : a218c9615321567527e94ac1f01230604e231f1eabe699fb1d751af3e28d00feaa3dd823540a2452baa40dfb3475d3bb786b41e7880881b5a5427e7
-76004 Current applied tamper settings : 05044440040004040000000014000440

Successfully validated signature of attestation token.
```

## 6.18 实用命令

### 6.18.1 密钥生成

生成用于加密和解密的密钥文件，并将密钥文件输出到指定的 filename。

**命令行语法**

```sh
$ commander util genkey --type aes-ccm --outfile <filename>
```

**命令行输入示例**

```sh
$ commander util genkey --type aes-ccm --outfile key.txt
```

**命令行输出示例**

```text
Using /dev/random for random number generation
Gathering sufficient entropy... (may take up to a minute)...
DONE
```

### 6.18.2 生成签署密钥

创建一个 EDCSA-P256 密钥对并将结果输出到指定的私钥和公钥文件。有关详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

**命令行语法**

```sh
$ commander util genkey --type ecc-p256 --privkey <filename> --pubkey <filename> [--tokenfile <filename>]
```

**命令行输入示例**

```sh
$ commander util genkey --type ecc-p256 --privkey signing_key.pem --pubkey signing_pubkey.pem
```

**命令行输出示例**

```text
Generating ECC P256 key pair...
Writing private key file in PEM format to signing_key.pem
Writing public key file in PEM format to signing_pubkey.pem
DONE
```

### 6.18.3 Token 密钥

创建一个 token 文本文件，其中包含适合刷写到设备的 ECC（Elliptic Curve Cryptography，椭圆曲线密码学）公钥。有关详细信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide for GSDK 3.x and Lower* 或 *UG489: Silicon Labs Gecko Bootloader User's Guide for GSDK 4.0 and Higher*。

**命令行语法**

```sh
$ commander util keytotoken <input file> --outfile <filename>
```

**命令行输入示例**

```sh
$ commander util keytotoken my_pubkey.pem --outfile keytokens.txt
```

**命令行输出示例**

```text
Writing EC tokens to keytokens.txt...
DONE
```

### 6.18.4 生成证书

签署文件的过程可以使用一个中间证书来完成。可以使用 `util gencert` 命令生成这些证书。当前有两种可用的证书类型：GBL 证书和安全启动证书。当启用了回滚保护时，如果设备已看到具有更高版本号的证书，则设备将不会启动。这是由 `--cert-version` 选项设置的。`--cert-pubkey` 对应的私钥用于对映像进行签署。证书可以通过使用 `--sign` 选项提供的签署密钥直接签署，也可以通过使用 `--extsign` 选项来未签署。

**命令行语法**

```sh
$ commander util gencert --cert-type <cert type> --cert-version <version> --cert-pubkey <key file> [--sign <key file>|--extsign] --outfile <filename>
```

**命令行输入示例**

```sh
$ commander gencert --cert-type secureboot --cert-version 1 --cert-pubkey cert_pubkey.pem --sign signing_key.pem --outfile secureboot_cert.bin
```

在此示例中，提供了签署密钥并直接对证书进行签署。

**命令行输出示例**

```text
Successfully signed certificate
DONE
```

**命令行输入示例**

```sh
$ commander gencert --cert-type gbl --cert-version 1 --cert-pubkey cert_pubkey.pem --extsign --outfile gbl_cert.bin
```

在此示例中，创建了一个未签署的证书。可以为证书创建一个签名，例如通过 HSM 来创建。可以通过将未签署的证书和 HSM 生成的签名传递给 [`util signcert`](#6185-签署证书) 命令来签署证书。

**命令行输出示例**

```text
DONE
```

### 6.18.5 签署证书

使用外部创建的签名签署证书。您可以使用可选的 `--verify` 选项通过提供与用于创建签名的私钥相对应的公钥来验证签名。

**命令行语法**

```sh
$ commander util signcert <cert filename> --cert-type <type> --signature <signature> [--verify <public key file>] --outfile <filename>
```

**命令行输入示例**

```sh
$ commander util signcert gbl_cert.bin.extsign --cert-type gbl --signature gbl_signature.bin --verify signing_pubkey.pem --outfile signed_cert.bin
```

**命令行输出示例**

```text
Successfully verified signature
Successfully signed certificate
DONE
```

### 6.18.6 验证签名

启用安全启动后，设备上运行的所有代码都必须已签署。此命令可用作验证文件是否已正确签署的检查，这可能有助于在安全启动失败的情况下进行调试，或用作刷写映像之前的验证。如果文件是使用中间证书签署的，则证书密钥用于检查文件的签名。`--verify` 选项给出的密钥用于验证证书的签名。

**命令行语法**

```sh
$ commander util verifysign <input file> --verify <public key file>
```

**命令行输入示例**

```sh
$ commander util verifysign my_application.bin --verify signing_pubkey.pem
```

**命令行输出示例**

```text
Parsing file my_application.bin...
Found application properties at 0x00000e78
Found certificate in image at location 0x0000b3a4
Successfully verified certificate signature with verification key.
Using certificate key to verify application signature.
Found signature at 0x0000b42c
Successfully verified application signature.
DONE
```

### 6.18.7 应用程序信息

通过解析映像中的 `ApplicationProperties_t` 结构来获取有关应用程序的所有可用信息。如果文件没有应用程序属性，则无法从文件中提取任何信息。

**命令行语法**

```sh
$ commander util appinfo <filename>
```

**命令行输入示例**

```sh
$ commander util appinfo my_application.bin
```

**命令行输出示例**

```text
Parsing file my_application.bin...
Found application properties in image.
Application protperties info:
Signature location          : 0x0000b42c
Signature type              : ECDSA-P256
Long token section address  : Not set (0x00000000)

Application data info :
If rollback prevention is enabled, the device will not boot if the device has seen an application with a higher version number.
App type                    : The application is an MCU application
App version                 : 0x00000000
Product ID                  : 0x53455f555047524144455f4150500000

Application certificate info:
If rollback prevention is enabled, the device will not boot if the device has seen a certificate with a higher version number.
Certificate located at      : 0x0000b3a4
Certificate version         : 0x00000001
Certificate key             : 0x249919c28b28156f19d2e03379b968c8a931aa9b195258e2741da28b686983dd71d0140e9a7b0d7e39de43f592163b8aa38d4e0871f5d2d88b575
Certificate signature       : 0x013f2adc310f10f1426db74b503f3612a46ab85c7ce86c967eb965b10f7d24267101192513d9481c49c0eb0b61c1f73392cc6f6d1cd1209a9d58e

DONE
```

### 6.18.8 从 ELF 文件打印 Section Header 信息

解析并打印 ELF 文件中的 section header 信息。

**命令行语法**

```sh
$ commander util elfinfo <filename>
```

**命令行输入示例**

```sh
$ commander util elfinfo my_bootloader.out
```

**命令行输出示例**

```text
Index   Name                    Size            Address         Type
    1   .shstrtab               0x00000111      0x00000000      STRTAB
    2   .strtab                 0x0001e169      0x00000000      STRTAB
    3   .symtab                 0x000243a0      0x00000000      SYMTAB
    4   HEADERS                 0x000000ac      0x00000000      PROGBITS
    5   APP ro                  0x0002ddf4      0x00000200      PROGBITS
    6   SIMEE&LOCKBITS          0x00009000      0x000f7000      NOBITS
    7   ResetHeap               0x00001490      0x20000000      NOBITS
    8   Guard                   0x00000030      0x20001490      NOBITS
    9   APP rw                  0x00002148      0x2003dce0      NOBITS
   10   .debug_abbrev           0x00006325      0x00000000      PROGBITS
   11   .debug_aranges          0x000037ac      0x00000000      PROGBITS
   12   .debug_frame            0x0003a2f5      0x00000000      PROGBITS
   13   .debug_info             0x00063435      0x00000000      PROGBITS
   14   .debug_line             0x00064f5c      0x00000000      PROGBITS
   15   .debug_loc              0x00010fe3      0x00000000      PROGBITS
   16   .debug_macinfo          0x00009941      0x00000000      PROGBITS
   17   .debug_pubnames         0x00007132      0x00000000      PROGBITS
   18   .debug_ranges           0x00003778      0x00000000      PROGBITS
   19   .iar.debug_frame        0x00015349      0x00000000      PROGBITS
   20   .iar.debug_line         0x00020199      0x00000000      PROGBITS
   21   .comment                0x001d394a      0x00000000      PROGBITS
   22   .iar.rtmodel            0x00000032      0x00000000      PROGBITS
   23   .ARM.attributes         0x0000002e      0x00000000
DONE
```

## 6.19 OTA 命令

### 6.19.1 创建 OTA 引导加载程序文件

从一个或多个 GBL 文件中创建一个 Zigbee OTA 引导加载程序文件，并将输出写到指定的 OTA 文件。

**命令行语法**

```sh
$ commander ota create --upgrade-image <filename> --manufacturer-id <ID> --image-type <image type> --firmwareversion <version> --string <text> -o <outfile> [--manufacture-tag <tag ID:filename> -stack-version <version> --credentials <credentials> --destinations <EUI64> --min-hw <version> --max-hw <version>]
```

**命令行输入示例**

```sh
$ commander ota create --upgrade-image example.gbl --manufacturer-id 0x1002 --image-type 0x5678 --firmware-version 0x00000005 --string "Example" -o example.ota
```

从 GBL 升级映像 *example.gbl* 创建 OTA 文件 *example.ota*。

**命令行输出示例**

```text
Initializing OTA file...
Writing header data...
Manufacturer ID : 0x1002
Image Type      : 0x5678
Firmware version: 0x00000005
Stack Version   : 0x0002
Header String   : Example
Writing OTA file ...
DONE
```

### 6.19.2 创建 Null OTA 文件

Zigbee Over-the-Air (OTA) Bootload cluster client 的认证过程需要厂商提供一个 NULL 升级文件给测试机构进行测试。NULL OTA 升级文件不包含其中的实际升级映像（GBL 文件）。它比完整升级映像小得多，但与普通 Zigbee OTA 文件相同。

您可以使用 `--null` 选项而不是 `--upgrade-image` 选项来创建 NULL 文件。`--null` 选项由 tag ID 和 tag length 组成。Tag ID 应该是非 0x0000 的，因为 Zigbee 已将其定义为“Upgrade Image”。Tag length 是字节的数量，通常是比较小，例如 10。此选项生成一个字节序列，从 0 开始并根据传入的 tag length 递增。

**命令行语法**

```sh
$ commander ota create --null <tag ID:tag length> --manufacturer-id <ID> --image-type <image type> --firmware-version <version> --string <text> -o <outfile> [--credentials <credentials> --destinations <EUI64> --min-hw <version> --max-hw <version>]
```

**命令行输入示例**

```sh
$ commander ota create --null 0xffff:10 --manufacturer-id 0x110c --image-type 0x5678 --firmware-version 0x0102 --string "NULL OTA file" -o ~/projects/Binaries/null.ota
```

创建一个 tag ID 为 `0xffff` 且 tag length 为 10 字节的 NULL OTA 文件。

**命令行输出示例**

```text
Initializing OTA file...
Writing OTA file ...
DONE
```

### 6.19.3 打印 OTA 文件信息

解析并打印 OTA 文件的内容。

**命令行语法**

```sh
$ commander ota parse <ota file>
```

**命令行输入示例**

```sh
$ commander ota parse example.ota
```

显示 OTA 文件 *example.ota* 的内容。

**命令行输出示例**

```text
Header Magic:           0x0beef11e
Header Version:         0x0100
Header Length:          56 bytes
Header Field Control:   0x0000
Manufacturer ID:        0x110c
ImageType:              0x0027 (Manufacture Specific)
Firmware Version:       0x01020509
Zigbee stack version:   0x0002 (ZigBee Pro)
Header String:          NULL
Image Size:             121680 bytes
Found 4 tags
    Tag ID:             0x0000 (Upgrade Image)
    Tag Length:         120572 bytes

    Tag ID:             0xff01 (Manufacturer Specific)
    Tag Length:         516 bytes

    Tag ID:             0xff3e (Manufacturer Specific)
    Tag Length:         504 bytes

    Tag ID:             0xff46 (Manufacturer Specific)
    Tag Length:         8 bytes
DONE
```

### 6.19.4 签署 OTA 文件

Zigbee Smart Energy Profile 要求制造商签署 OTA 文件。OTA client 必须在安装前验证下载的文件。映像使用 Certicom 颁发的证书进行签署。对映像进行签署后，签署者的证书将作为一个 tag 自动包含在 OTA 文件中，并添加一个 signature tag 作为 OTA 文件中的最后一个 tag。有关详细信息，请参阅 *AN714: Smart Energy ECC-Enabled Device Setup Process*。

**注意**：MacOS 不支持 OTA 签署。

**命令行语法**

```sh
$ commander ota create --sign --certificate <certificate> --upgrade-image <filename> --manufacturer-id <ID> --image-type <image type> --firmware-version <version> --string <text> -o <outfile> [--credentials <credentials> --destinations <EUI64> --min-hw <version> --max-hw <version>]
```

**命令行输入示例**

```sh
$ commander ota create --sign --certificate certificate.txt --upgrade-image example.gbl --manufacturer-id 0x0345 --image-type 0x4567 --firmware-version 0x00000002 --string "Signed OTA file" -o signed_file.ota
```

使用证书 `certificate.txt` 创建一个已签署的 OTA 文件。

**命令行输出示例**

```text
Creating OTA file...
Writing header data...
Manufacturer ID : 0x0345
Image Type      : 0x4567
Firmware version: 0x00000002
Stack Version   : 0x0002
Header String   : Signed OTA file
Digest: 8DFD32A4C6F3C39E6C152F33A16AEAD2
Signed file using certificate.
Successfully verified signature.
Writing OTA file signed_file.ota...
DONE
```

### 6.19.5 为外部签署创建 OTA 文件

创建要在外部签署的 OTA 映像。外部证书已添加到映像中。可以使用 [sign](#6196-外部签署-ota-文件) 命令将签名添加到映像中。

**命令行语法**

```sh
$ commander ota create --extsign --certificate <certificate> --upgrade-image <filename> --manufacturer-id <ID> --image-type <image type> --firmware-version <version> --string <text> -o <outfile> [--credentials <credentials> --destinations <EUI64> --min-hw <version> --max-hw <version>]
```

**命令行输入示例**

```sh
$ commander ota create --extsign --certificate certificate.txt --upgrade-image example.gbl --manufacturer-id 0x0345 --image-type 0x4567 --firmware-version 0x00000002 --string "Silicon Labs Ota Support" -o example_file.ota
```

**命令行输出示例**

```text
Creating OTA file...
Writing header data...
Manufacturer ID : 0x0345
Image Type      : 0x4567
Firmware version: 0x00000002
Stack Version   : 0x0002
Header String   : Silicon Labs Ota Support
Writing OTA file example_file.ota.extsign...
DONE
```

### 6.19.6 外部签署 OTA 文件

使用 Simplicity Commander 的 `sign` 命令将外部创建的签名附加到 OTA 文件。您必须使用 `--curve` 选项指定用于创建签名的曲线。可用曲线为 163k1 或 283k1。

**命令行语法**

```sh
$ commander ota sign <filename> --curve <curve (163k1|283k1)> --signature <filename> -o <outfile>
```

**命令行输入示例**

```sh
$ commander ota sign example.ota --curve <curve (163k1|283k1)> --signature signature.txt -o signed_file.ota
```

将外部创建的签名附加到 OTA 文件。

**命令行输出示例**

```text
DONE
```

### 6.19.7 验证 OTA 文件的签名

使用 Simplicity Commander 的 `verify` 命令验证 OTA 文件的签名。您必须提供用于签署文件的证书。

**注意**：MacOS 不支持 OTA 签名验证。

**命令行语法**

```sh
$ commander ota verify <filename> --certificate <certificate>
```

**命令行输入示例**

```sh
$ commander ota verify signed_file.ota --certificate certificate.txt
```

验证 OTA 文件的签名。

**命令行输出示例**

```text
Digest: 8ABB04618622595401AD45FA33C7D670
Successfully verified signature
DONE 
```

## 6.20 Post-Build 命令

### 6.20.1 执行项目 Post-Build 文件

Simplicity Commander 采用由 Simplicity Studio 生成的 YAML（Yaml Ain't Markup Language）格式的项目 post-build 描述文件，并按顺序执行文件中的指定任务。

**命令行语法**

```sh
$ commander postbuild <filename> [--parameter <name:value>]
```

**命令行输入示例**

```sh
$ commander postbuild project_name.slpb --parameter "build_dir:path_to_build_dir"
```

执行 *project_name.slpb* 中定义的 post-build pipeline 中的步骤。

**命令行输出示例**

```text
Parsing file project_name.slbp...
Running task copy...
Running task convert...
Running task GBL create...
Running task OTA create...
DONE
```

Post-build pipeline 由三个部分组成：

* 参数：已命名的变量，其值是在 pipeline 调用时从命令行获取的。
* 常量：已命名的变量，其值取自 post-build 文件或从继承常量的另一个 post-build 文件的路径。
* 步骤：要调用的任务列表，使用上面声明的变量。支持四种不同类型的任务。

下表总结了每项任务的必需选项和可选选项。

<table style="margin-left: auto; margin-right: auto;">
<caption style="white-space: nowrap;">Table 6.2. Copy Task</caption>
<tbody>
  <tr>
    <td><b>Required Options</b></td>
  </tr>
  <tr>
    <td>input: &lt;filename&gt;</td>
  </tr>
  <tr>
    <td>output: &lt;filename&gt;</td>
  </tr>
  <tr>
    <td><b>Optional Options</b></td>
  </tr>
  <tr>
    <td>export: &lt;constant value&gt;</td>
  </tr>
</tbody>
</table>

<table style="margin-left: auto; margin-right: auto;">
<caption style="white-space: nowrap;">Table 6.3. Convert Task</caption>
<tbody>
  <tr>
    <td><b>Required Options</b></td>
  </tr>
  <tr>
    <td>input: &lt;filename&gt;</td>
  </tr>
  <tr>
    <td>output: &lt;filename&gt;</td>
  </tr>
  <tr>
    <td><b>Optional Options</b></td>
  </tr>
  <tr>
    <td>export: &lt;constant value&gt;</td>
  </tr>
  <tr>
    <td>keyfile: &lt;key file&gt;</td>
  </tr>
  <tr>
    <td>crc: &lt;true&gt;</td>
  </tr>
  <tr>
    <td>certificate: &lt;certificate file&gt;</td>
  </tr>
  <tr>
    <td>include-section: &lt;ELF section&gt;</td>
  </tr>
  <tr>
    <td>exclude-section: &lt;ELF section&gt;</td>
  </tr>
  <tr>
    <td>signature: &lt;signature file&gt;</td>
  </tr>
  <tr>
    <td>verify: &lt;key file&gt;</td>
  </tr>
</tbody>
</table>

<table style="margin-left: auto; margin-right: auto;">
<caption style="white-space: nowrap;">Table 6.4. GBL Create Task</caption>
<tbody>
  <tr>
    <td><b>Required Options</b></td>
  </tr>
  <tr>
    <td>input: &lt;filename&gt;</td>
  </tr>
  <tr>
    <td>output: &lt;filename&gt;</td>
  </tr>
  <tr>
    <td><b>Optional Options</b></td>
  </tr>
  <tr>
    <td>export: &lt;constant value&gt;</td>
  </tr>
  <tr>
    <td>app: &lt;app image&gt;</td>
  </tr>
  <tr>
    <td>bootloader: &lt;bootloader image&gt;</td>
  </tr>
  <tr>
    <td>seupgrade: &lt;SE upgrade image&gt;</td>
  </tr>
  <tr>
    <td>metadata: &lt;metadata bin file&gt;</td>
  </tr>
  <tr>
    <td>compress: &lt;app compression algorithm&gt;</td>
  </tr>
  <tr>
    <td>certificate: &lt;certificate file&gt;</td>
  </tr>
  <tr>
    <td>sign: &lt;key file&gt;</td>
  </tr>
  <tr>
    <td>encrypt: &lt;AES key file&gt;</td>
  </tr>
  <tr>
    <td>extsign: &lt;true&gt;</td>
  </tr>
  <tr>
    <td>include-section: &lt;section&gt;</td>
  </tr>
  <tr>
    <td>exclude-section: &lt;section&gt;</td>
  </tr>
</tbody>
</table>

<table style="margin-left: auto; margin-right: auto;">
<caption style="white-space: nowrap;">Table 6.5. OTA Create Task</caption>
<tbody>
  <tr>
    <td><b>Required Options</b></td>
  </tr>
  <tr>
    <td>input: &lt;filename&gt;</td>
  </tr>
  <tr>
    <td>output: &lt;filename&gt;</td>
  </tr>
  <tr>
    <td>manufacturer-id: &lt;ID&gt;</td>
  </tr>
  <tr>
    <td>firmware-version: &lt;version&gt;</td>
  </tr>
  <tr>
    <td>image-type: &lt;image type&gt;</td>
  </tr>
  <tr>
    <td>string: &lt;text&gt;</td>
  </tr>

  <tr>
    <td><b>Optional Options</b></td>
  </tr>
  <tr>
    <td>export: &lt;constant value&gt;</td>
  </tr>
  <tr>
    <td>upgrade-image: &lt;filename&gt;</td>
  </tr>
  <tr>
    <td>manufacturer-tag: &lt;tag ID&gt;</td>
  </tr>
  <tr>
    <td>stack-version: &lt;version&gt;</td>
  </tr>
  <tr>
    <td>credentials: &lt;credentials&gt;</td>
  </tr>
  <tr>
    <td>destination: &lt;EUI64&gt;</td>
  </tr>
  <tr>
    <td>min-hw: &lt;version&gt;</td>
  </tr>
  <tr>
    <td>max-hw: &lt;version&gt;</td>
  </tr>
  <tr>
    <td>certificate: &lt;filename&gt;</td>
  </tr>
  <tr>
    <td>sign: &lt;true&gt;</td>
  </tr>
</tbody>
</table>

## 6.21 RPS 命令

Simplicity Commander 可用于将应用程序二进制文件转换为用于刷写 Si917 设备的 RPS 映像。Commander 会将一个 RPS style header 添加到所提供的应用程序映像中。支持的映像格式有 bin、hex、s37 和 ELF。可以使用 `--app-version` 选项提供应用程序版本号。

### 6.21.1 从二进制映像创建 RPS 文件

要从二进制映像创建 RPS 文件，您**必须**使用 `--address` 标志提供应用程序起始地址。

**命令行语法**

```sh
$ commander rps create <filename> --app <filename> --address <start address> [--app-version <version no.>]
```

**命令行输入示例**

```sh
$ commander rps create output.rps --app app.bin --address 0x08212000
```

此命令行使用闪存地址“0x08212000”和一个二进制映像创建一个 RPS 文件，并将其保存到名为“output.rps”的文件中。

**命令行输出示例**

```text
Parsing file app.bin...
RPS file successfully created at 'output.rps'
DONE
```

### 6.21.2 从 ELF 映像创建 RPS 文件

从 ELF 映像生成 RPS 文件时，您可以使用 `--include-section` 和 `--exclude-section` 选项在输出的 RPS 文件的应用程序映像中包含或排除某些 ELF section。如果这些选项均未提供，则 Simplicity Commander 将包含看起来属于应用程序一部分的所有 section。

您可以通过重复提供相应的选项来包含或排除多个 section。

**命令行语法**

```sh
$ commander rps create <filename> --app <filename> [--include-section <section> --exclude-section <section> --app-version <version no.>]
```

**命令行输入示例**

```sh
$ commander rps create output.rps --app app.axf --include-section .text --include-section .data
```

此命令行从一个 ELF 应用程序文件的“.text”和“.data” section 创建一个 RPS 文件，并将其保存到名为“output.rps”的文件中。

**命令行输出示例**

```text
Including ELF section(s):
    .text
    .data
Parsing file app.axf...
RPS file successfully created at 'output.rps'
DONE
```

### 6.21.3 从 Hex/s37 映像创建 RPS 文件


您可以从 Intel Hex（hex）映像或从 Motorola S-record（s37）映像创建 RPS 文件。

**命令行语法**

```sh
$ commander rps create <filename> --app <filename> [--app-version <version no.>]
```

**命令行输入示例**

```sh
$ commander rps create output.rps --app app.hex
```

此命令行从一个 hex 映像创建一个 RPS 文件，并将其保存到名为“output.rps”的文件中。

**命令行输出示例**

```text
Parsing file app.hex...
RPS file successfully created at 'output.rps'
DONE
```

# 7. 软件版本历史

> 注：内容见原文档。
