# AN1135: Using Third Generation Non-Volatile Memory (NVM3) Data Storage (Rev. 1.0) <!-- omit in toc -->

- [1. 引言](#1-引言)
- [2. NVM3 默认实例](#2-nvm3-默认实例)
  - [2.1 NVM3 默认实例 Key Space](#21-nvm3-默认实例-key-space)
- [3. Simplicity Studio 5 Project Configurator 中的 NVM3](#3-simplicity-studio-5-project-configurator-中的-nvm3)
- [4. 将 NVM3 与 Silicon Labs Connect 一起使用](#4-将-nvm3-与-silicon-labs-connect-一起使用)
- [5. 将 NVM3 与 Silicon Labs OpenThread 一起使用](#5-将-nvm3-与-silicon-labs-openthread-一起使用)
- [6. 将 NVM3 与 Silicon Labs Bluetooth 一起使用](#6-将-nvm3-与-silicon-labs-bluetooth-一起使用)
  - [6.1 在 Bluetooth SDK 中配置 NVM3](#61-在-bluetooth-sdk-中配置-nvm3)
    - [使用 Simplicity Studio 5 和 Bluetooth SDK v3.x](#使用-simplicity-studio-5-和-bluetooth-sdk-v3x)
    - [使用 Simplicity Studio 4 和 Bluetooth SDK v2.x](#使用-simplicity-studio-4-和-bluetooth-sdk-v2x)
  - [6.2 从 PS Store 切换到 NVM3](#62-从-ps-store-切换到-nvm3)
    - [使用 Simplicity Studio 5 和 Bluetooth SDK v3.x](#使用-simplicity-studio-5-和-bluetooth-sdk-v3x-1)
    - [使用 Simplicity Studio 4 和 Bluetooth SDK v2.x](#使用-simplicity-studio-4-和-bluetooth-sdk-v2x-1)
  - [6.3 从 NVM3 切换到 PS Store](#63-从-nvm3-切换到-ps-store)
    - [使用 Simplicity Studio 5 和 Bluetooth SDK v3.x](#使用-simplicity-studio-5-和-bluetooth-sdk-v3x-2)
    - [使用 Simplicity Studio 4 和 Bluetooth SDK v2.x](#使用-simplicity-studio-4-和-bluetooth-sdk-v2x-2)
- [7. 在 AppBuilder-Based 应用程序中使用 NVM3](#7-在-appbuilder-based-应用程序中使用-nvm3)
  - [7.1 NVM3 Library Plugin](#71-nvm3-library-plugin)
  - [7.2 SimEEv2 to NVM3 Upgrade Plugin](#72-simeev2-to-nvm3-upgrade-plugin)
- [8. 将 NVM3 与 Z-Wave 一起使用](#8-将-nvm3-与-z-wave-一起使用)
- [9. NVM3 API 选项](#9-nvm3-api-选项)
  - [9.1 Token API](#91-token-api)
    - [9.1.1 删除 Token](#911-删除-token)
    - [9.1.2 Indexed Token 的特殊注意事项](#912-indexed-token-的特殊注意事项)
  - [9.2 Bluetooth NVM API](#92-bluetooth-nvm-api)
  - [9.3 Native NVM3 API](#93-native-nvm3-api)
- [10. Simplicity Commander and NVM3](#10-simplicity-commander-and-nvm3)

NVM3 驱动器提供了一种读写存储在 flash 中的 data object（key/value pair）的方法。磨损均衡（wear-leveling）用于减少擦写周期，以最大限度地延长 flash 的寿命。该驱动器对断电和重启事件是可还原的，这确保从驱动器中检索的对象（object）总是处于有效状态。单个 NVM3 实例可以在多个无线协议栈和应用代码之间共享，这使得它非常适合多协议应用。本应用笔记解释了如何在 Zigbee（EmberZNet）、OpenThread、Z-Wave、Bluetooth 和 Connect 中使用 NVM3 进行非易失性数据存储。

与 Simplicity Studio 5 一起使用的 Gecko SDK Suite v3.x 引入了一种新的 component-based 项目架构，该架构取代了 AppBuilder。最终，所有无线协议都将转向这种架构。本文档介绍了新架构和仍在使用的 AppBuilder 中的 NVM3 配置。

# 1. 引言

NVM3（Non-Volatile Memory）数据存储驱动器用于在 flash 中存储持久性数据。

NVM3 被设计成可在所有 Silicon Labs 无线协议栈（在 EFR32 上运行）以及 MCU 应用程序（在 EFM32 上运行）上工作。

NVM3 的一些主要特性是：

* 在 flash 中使用 key/value pair 数据存储
* 运行时创建和删除 object
* 在断电和复位事件间持久化
* 磨损均衡，以最大化 flash 寿命
* 对象大小最多可配置为 4096 byte
* 可配置 flash 存储大小（minimum 3 flash page）
* 具有可配置大小的缓存（cache），用于加速对象访问
* 数据和计数器对象类型
* 包括多个 Silicon Labs 持久化存储 API 的兼容性层
* 在多协议应用程序中实现单个共享存储实例
* 重新封装 API，以允许应用程序在 CPU 负载较低的时间段内运行 clean-up page 擦除

在 [https://docs.silabs.com/](https://docs.silabs.com/) 上的 NVM3 Documentation 对 NVM3 进行了详细描述。本文档的其余部分假定您熟悉该内容。

虽然 NVM3 API 可以直接使用，但 NVM3 还可以作为 Silicon Labs 提供的其他几种持久性存储 API 的底层存储机制：

* EmberZnet 和 Connect 应用程序中使用的 Token API
* Silicon Labs Bluetooth 应用程序中使用的 Persistent Storage API

# 2. NVM3 默认实例

可以在一个设备上创建多个 NVM3 实例，并且它们彼此独立地运行，但是为了节省内存，通常会仅使用一个 NVM3 实例，因为每个实例都会增加一些开销。对于基于 Silicon Labs 无线协议栈的应用程序，其使用一个通用的默认实例。这允许 DMP（Dynamic Multiprotocol）应用程序将多个无线协议栈组合在一起以共享同一个 NVM3 实例。

用于 NVM3 默认实例的 flash page 数量是可配置的，如果应用程序包含了多个都使用默认实例的协议栈，则此设置必须作出匹配。

> 重点：当为设备（已在 flash 中包含 NVM3 实例）创建包含 NVM3 实例的应用程序时，为 NVM3 实例配置的 flash page 数量必须与设备上已找到的 NVM3 实例的 flash page 数量匹配。因此，一旦将 NVM3 实例安装到设备上，如果不先擦除保存 NVM3 实例和 NVM3 对象的 flash page，就无法更改其大小。

NVM3 具有缓存以加快对 NVM3 对象的访问。缓存大小必须设置为大于或等于在 NVM3 中对象数的值。这包括通过 native NVM3 API 创建的 NVM3 对象的数量和通过更高级的 API（如 Token API）创建的任何对象。缓存还必须足够大以容纳任何已删除的 NVM3 对象。`nvm3_countObjects()` ~~和~~ 等函数可用于在任何给定点中查找 NVM3 中的存活对象和已删除对象的数量。Silicon Labs 建议在所有 NVM3 对象初始化之后，通过 native NVM3 API 和更高级的 API（如 Token API）检查这些函数，以找出 NVM3 默认缓存大小的正确大小。

## 2.1 NVM3 默认实例 Key Space

NVM3 使用一个 20-bit key 来标识每个对象。为了避免对多个对象使用相同的 key，下表中概述了默认 NVM3 实例的 NVM3 key space。例如，在 EmberZNet 协议栈中定义的 NVM3 对象应使用 0x10000 到 0x1FFFF 范围内的 NVM3 key，而用户应用程序 token 应使用 0x10000 以下的 key。请注意，任何用户定义的 NVM3 对象都应放置在 0x10000 以下。

<table title="Table 2.1. NVM3 Default Instance Key Space" id="Table-2.1">
<thead>
  <tr>
    <th style="white-space: nowrap;">Domain</th>
    <th style="white-space: nowrap;">NVM3 Key</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="white-space: nowrap;">User</td>
    <td style="white-space: nowrap;">0x00000 - 0x0FFFF</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">EmberZNet stack</td>
    <td style="white-space: nowrap;">0x10000 - 0x1FFFF</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">OpenThread stack</td>
    <td style="white-space: nowrap;">0x20000 - 0x2FFFF</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">Connect stack</td>
    <td style="white-space: nowrap;">0x30000 - 0x3FFFF</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">Bluetooth stack</td>
    <td style="white-space: nowrap;">0x40000 - 0x4FFFF</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">Z-Wave stack</td>
    <td style="white-space: nowrap;">0x50000 - 0x5FFFF</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">Reserved</td>
    <td style="white-space: nowrap;">0x60000 - 0xFFFFF</td>
  </tr>
</tbody>
</table>

# 3. Simplicity Studio 5 Project Configurator 中的 NVM3

与 Silicon Labs Bluetooth v3.x、Connect v3.x 和 OpenThread SDK 一起使用的 Simplicity Studio 5 Project Configurator 包含一个 **NVM3 Driver** 组件，其位于 Software Components 视图的 Platform->Driver 下。其还为 NVM3 默认实例提供了一个单独的组件，该组件将初始化该实例。

<p>
    <img src="../images/AN1135/3-1.png">
</p>

**NVM3 Default Instance** 组件提供以下配置：

* **Size** ：用于 NVM3 数据存储的 byte page 数量。必须将其设置为与整数个 flash page 匹配，最少 3 page。
* **Cache Size** ：要缓存的对象数量。为了减少访问时间，该数量应等于或大于任何时候存储在 NVM3 中的存活对象和已删除对象的数量。
* **Max Object Size** ：允许的最大 NVM3 对象大小，以 byte 为单位。必须介于 208 ~ 4096 byte。
* **User Repack Headroom** ：保留的余量，用户重新填充限制低于强制重新填充限制的 byte 数。默认值为 0，这表示强制的限制与用户重新填充限制相同。

如果项目中包含 **NVM3 Default Instance** 组件，那么如果项目中包含 Platform->Service->System 下的 **System Init** 组件，则默认实例将在 `sl_system_init()` 期间自动初始化。

# 4. 将 NVM3 与 Silicon Labs Connect 一起使用

本章适用于将 Proprietary Flex SDK v3.x 中的 Connect 与 Simplicity Studio 5 中 Project Configurator 结合使用的用例。Proprietary Flex SDK v2.x 中的 Connect 与 Simplicity Studio 4 中的 AppBuilder 结合使用。[7. 在 AppBuilder-Based 应用程序中使用 NVM3](#7-在-AppBuilder-Based-应用程序中使用-NVM3) 中描述了 AppBuilder 的用法。

对于 Silicon Labs Connect v3.x 应用程序，在 Simplicity Studio Project Configurator Software Component 视图的 Platform->Driver 下有一个可用的 **Token Manager** 组件。当需要 token 管理器提供 [9.1 Token API](#9-1-Token-API) 时，必须为 token 存储后端选择一个额外的组件，其可以是 **Token Manager using Sim EEPROM 1, Token Manager using Sim EEPROM 2**，或者是 **Token Manager using NVM3** 。如果选择 NVM3 作为存储后端，则 **NVM3** 和 **NVM3 Default Instance** 组件将包含在项目中。

# 5. 将 NVM3 与 Silicon Labs OpenThread 一起使用

本部分适用于将 Silicon Labs OpenThread SDK v1.x 与 Simplicity Studio 5 中的 Project Configurator 结合使用的用例。默认情况下，GSDK 中的所有 OpenThread 示例应用均配置为使用 NVM3 以将数据存储在非易失性存储器中。这样做时，这些应用程序：

* 利用通用的默认 NVM3 实例。
* 在项目中包含 **NVM3 Driver** 和 **NVM3 Default Instance** 组件。
* 使用 native NVM3 API 访问 NVM3 对象

OpenThread 协议栈使用的 NVM3 key space 是 0x20000 - 0x2FFFF。

# 6. 将 NVM3 与 Silicon Labs Bluetooth 一起使用

传统上，Bluetooth 协议栈使用其专有的解决方案将数据存储在称为 **Persistent Store (PS Store)** 的非易失性存储器中。PS Store 既存储由协议栈处理的数据（如临时 Bluetooth 地址、绑定密钥等），又存储在复位设备时必须保留的用户数据（如设备状态）。要了解有关 PS Store 的更多信息，请阅读 [Bluetooth API Reference Guide](https://www.silabs.com/documents/login/reference-manuals/bluetooth-api-reference.pdf) 的相关部分。

Bluetooth SDK 中的一些示例应用仍配置为使用 PS Store，而其他示例应用则已配置为使用 NVM3。这意味着：

* 在 Series 1 设备上，除了 Bluetooth Mesh NCP 示例项目（默认情况下使用 NVM3）外，示例应用均配置为使用 PS Store。
* 在 Series 2 设备上，所有示例应用都配置为使用 NVM3。

Series 2 设备仅支持 NVM3，而在 Series 1 设备上 PS Store 和 NVM3 都是可用的。本节介绍了如何在 Bluetooth SDK 中配置 NVM3，以及如何在 PS Store 和 NVM3 之间进行切换。

## 6.1 在 Bluetooth SDK 中配置 NVM3

### 使用 Simplicity Studio 5 和 Bluetooth SDK v3.x

NVM3 可以使用 Project Configurator 进行配置，如 [3. Simplicity Studio 5 Project Configurator 中的 NVM3](#3-Simplicity-Studio-5-Project-Configurator-中的-NVM3) 中所述。

### 使用 Simplicity Studio 4 和 Bluetooth SDK v2.x

Project Configurator 不可用于配置 NVM3 参数。因此，必须手动定义参数。要覆盖默认参数：

1. 打开项目设置。
2. 查找定义的符号：
   1. 如果使用 GCC 作为编译器，请转到 C/C++ Build>Settings>GNU ARM C Compiler>Symbols>Defined Symbols
   2. 如果使用 IAR 作为编译器，请转到 C/C++ Build>Settings>IAR C/C++ compiler for ARM>Preprocessor>Defined Symbols
3. 添加以下任何定义以覆盖默认参数：
  * `NVM3_DEFAULT_NVM_SIZE`
  * `NVM3_DEFAULT_CACHE_SIZE`
  * `NVM3_DEFAULT_MAX_OBJECT_SIZE`
  * `NVM3_DEFAULT_REPACK_HEADROOM`

您可以在 [3. Simplicity Studio 5 Project Configurator 中的 NVM3](#3-Simplicity-Studio-5-Project-Configurator-中的-NVM3) 中找到每个参数的描述。提供 NVM3 的大小时请务必小心，因为它必须是 flash page 大小的倍数。注意：在 Series 1 设备上，flash page 大小通常为 2 kB；而在 Series 2 设备上，flash page 大小通常为 8 kB。请检查设备的 datasheet。NVM 的大小必须至少为 3 flash page。

## 6.2 从 PS Store 切换到 NVM3

从 Bluetooth SDK v2.13.0 起，PS Store 和 NVM3 都可以作为 Series 1 设备上的非易失性存储解决方案。默认情况下，大多数示例应用都配置为使用 PS Store，但是对于某些应用程序（需要更大的非易失性存储），NVM3 可能是更好的解决方案。

> 注意：PS Store 和 NVM3 彼此不兼容，因此将已有的应用程序从 PS Store 升级到 NVM3 将导致设备上存储的所有数据丢失。如果您有一个正在现场运行的应用程序，那么最好还是停留在 PS Store 上。如果仍要升级，请参见 *AN1086: Using the Gecko Bootloader with the Silicon Labs Bluetooth® Application* 以了解更多信息。

> 注意：PS Store 仅使用 2 flash page（=4 kB on an EFR32BG1/12/13 device）。因此，更改为 NVM3 时将影响 flash 中的可用空间。升级固件时，必须特别小心，以免将应用程序覆盖到 NVM3 区域。

### 使用 Simplicity Studio 5 和 Bluetooth SDK v3.x

要将项目配置从 PS Store 更改为 NVM3，只需在 [3. Simplicity Studio 5 Project Configurator 中的 NVM3](#3-Simplicity-Studio-5-Project-Configurator-中的-NVM3) 中讨论的那样，在 Project Configurator 中安装 **NVM3 Default Instance** 组件。这将自动卸载 PS Store 组件。

### 使用 Simplicity Studio 4 和 Bluetooth SDK v2.x

要将项目配置从 PS Store 更改为 NVM3，请使用以下过程：

1. 复制以下文件夹及其所有内容：
    ```
    C:\SiliconLabs\SimplicityStudio\v4\developer\sdks\gecko_sdk_suite\<version>\platform\emdrv\nvm3
    ```
    到您的项目的 `/platform/emdrv` 文件夹下。
2. 从项目中移除以下文件：
  * `/platform/emdrv/nvm3/src/nvm3_hal_extflash.c`
  * `/platform/emdrv/nvm3/src/nvm3_default_extflash.c` (NVM3 use with external flash is deprecated)
3. 如果您在项目中使用 Apploader，请从以下位置复制 NVM3 版本的 Apploader：
    ```
    C:\SiliconLabs\SimplicityStudio\v4\developer\sdks\gecko_sdk_suite\<version>\protocol\bluetooth\lib\<device>\<compiler>\binapploader_nvm3.o
    ```
    到您的项目的 `/protocol/bluetooth/lib/<device>/<compiler>` 文件夹下。
4. 如果您使用的是 GCC 编译器：
   1. 转到 `Project > Properties > C/C++ Build > Settings > GNU ARM C Compiler > Includes`。
   2. 添加 `${workspace_loc:/${ProjName}/platform/emdrv/nvm3/inc}` 到包含目录中。
   3. 转到 `Project > Properties > C/C++ Build > Settings > GNU ARM C Linker > Miscellaneous`。
   4. 移除 `${workspace_loc:/${ProjName}/protocol/bluetooth/lib/<device>/<compiler>/libpsstore.a}`。
   5. 添加 `${workspace_loc:/${ProjName}/platform/emdrv/nvm3/lib/libnvm3_CM4_gcc.a}`。

   如果您使用的是 IAR 编译器：
   1. 转到 `Project > Properties > C/C++ Build > Settings > IAR C/C++ Compiler for ARM > Preprocessor`。
   2. 添加 `${workspace_loc:/${ProjName}/platform/emdrv/nvm3/inc}` 到包含目录中。
   3. 转到 `Project > Properties > C/C++ Build > Settings > IAR Linker for ARM > Library`。
   4. 移除 `${workspace_loc:/${ProjName}/protocol/bluetooth/lib/<device>/<compiler>/libpsstore.a}`。
   5. 添加 `${workspace_loc:/${ProjName}/platform/emdrv/nvm3/lib/libnvm3_CM4_iar.a}`。
5. 如果您使用 Apploader，则还要修改：
    ```
    ${workspace_loc:/${ProjName}/protocol/bluetooth/lib/<device>/<compiler>/binapploader.o}
    ```
    为
    ```
    ${workspace_loc:/${ProjName}/protocol/bluetooth/lib/<device>/<compiler>/binapploader_nvm3.o}
    ```
6. 如 [6.1 在 Bluetooth SDK 中配置 NVM3](#6-1-在-Bluetooth-SDK-中配置-NVM3) 所述配置 NVM3。

## 6.3 从 NVM3 切换到 PS Store

由于诸如向后兼容性之类的原因，可能必须将配置从 NVM3 更改为 PS Store。

### 使用 Simplicity Studio 5 和 Bluetooth SDK v3.x

要将项目配置从 NVM3 更改为 PS Store，只需卸载 **NVM3 Default Instance** 组件。这将自动安装 PS Store 组件。请注意，这只在 Series 1 设备上可用，因为 Series 2 设备不支持 PS Store。

### 使用 Simplicity Studio 4 和 Bluetooth SDK v2.x

要将项目配置从 NVM3 更改为 PS Store，请使用以下过程：

1. 从您的项目中移除 `/platform/emdrv/nvm3`。
2. 从 `C:\SiliconLabs\SimplicityStudio\v4\developer\sdks\gecko_sdk_suite\<version>\protocol\bluetooth\lib\<device>\<compiler>\libpsstore.a` 中复制 PS Store 库到您项目中的 `/protocol/bluetooth/lib/<device>/<compiler>` 目录。
3. 如果您在项目中使用 Apploader，请从以下位置复制 PS Store 版本的 Apploader：
    ```
    C:\SiliconLabs\SimplicityStudio\v4\developer\sdks\gecko_sdk_suite\<version>\protocol\bluetooth\lib\<device>\<compiler>\binapploader.o
    ```
    到您的项目的 `/protocol/bluetooth/lib/<device>/<compiler>` 文件夹下。

    > 注意：Series 2 设备（EFR32xG2x）不支持 PS Store，因此这些设备只有 NVM3 版本的 Apploader。
4. 如果您使用的是 GCC 编译器：
   1. 转到 `Project > Properties > C/C++ Build > Settings > GNU ARM C Compiler > Includes`。
   2. 从包含目录中移除 `${workspace_loc:/${ProjName}/platform/emdrv/nvm3/inc}`。
   3. 转到 `Project > Properties > C/C++ Build > Settings > GNU ARM C Linker > Miscellaneous`。
   4. 添加 `${workspace_loc:/${ProjName}/protocol/bluetooth/lib/<device>/<compiler>/libpsstore.a}`。
   5. 移除 `${workspace_loc:/${ProjName}/platform/emdrv/nvm3/lib/libnvm3_CM4_gcc.a}`。

   如果您使用的是 IAR 编译器：
   1. 转到 `Project > Properties > C/C++ Build > Settings > IAR C/C++ Compiler for ARM > Preprocessor`。
   2. 从包含目录中移除 `${workspace_loc:/${ProjName}/platform/emdrv/nvm3/inc}`。
   3. 转到 `Project > Properties > C/C++ Build > Settings > IAR Linker for ARM > Library`。
   4. 添加 `${workspace_loc:/${ProjName}/protocol/bluetooth/lib/<device>/<compiler>/libpsstore.a}`。
   5. 移除 `${workspace_loc:/${ProjName}/platform/emdrv/nvm3/lib/libnvm3_CM4_iar.a}`。
5. 如果您使用 Apploader，则还要修改：
    ```
    ${workspace_loc:/${ProjName}/protocol/bluetooth/lib/<device>/<compiler>/binapploader_nvm3.o}
    ```
    为
    ```
    ${workspace_loc:/${ProjName}/protocol/bluetooth/lib/<device>/<compiler>/binapploader.o}
    ```

# 7. 在 AppBuilder-Based 应用程序中使用 NVM3

本章介绍了如何在 Appbuilder-based 应用程序（如 EmberZNet 6.7.n 和 6.8.n，基于 EmberZNet 的 DMP 应用程序以及 Gecko SDK Suite 2.7 及更低版本中的 Connect 应用程序）中将 NVM3 用于非易失性存储。

## 7.1 NVM3 Library Plugin

要将 NVM3 与 AppBuilder-based 示例应用一起使用， **NVM3 Library** plugin 应包含在项目中。所有 PS Store 和 SimEE plugin 均应取消选中。

<p>
    <img src="../images/AN1135/Figure%207.1.%20NVM3%20Library%20Plugin%20in%20AppBuilder.png"
         alt="Figure 7.1. NVM3 Library Plugin in AppBuilder"
         title="Figure 7.1. NVM3 Library Plugin in AppBuilder">
</p>

**NVM3 Library** plugin 提供了四个 plugin 选项：

* **Flash Pages** ：用于 NVM3 数据存储的 flash page 数量。必须为 3 或更高。对于 EFR32 Series 1 设备，默认值为 18（36 KB）；对于 EFR32 Series 2 设备，默认值为 4（32 KB）。
* **Cache Size** ：要缓存的对象数量。为了减少访问时间，该数量应等于或大于任何时候存储在 NVM3 中的对象的数量，包括 token 和已删除的对象。
* **Max Object Size** ：允许的最大 NVM3 对象大小，以 byte 为单位。必须介于 208 ~ 4096 byte。请注意，token API 仅可用于访问 254 byte 或更小的对象。访问较大的对象时，必须使用 native NVM3 API。
* **User Repack Headroom** ：保留的余量，用户重新填充限制低于强制重新填充限制的 byte 数。默认值为 0，这表示强制的限制与用户重新填充限制相同。

当使用 NVM3 Library plugin 时，必须包含 **Simulated EEPROM version 2 to NVM3 Upgrade Library** 或 **Simulated EEPROM version 2 to NVM3 Upgrade Stub Library**，如 [7.2 SimEEv2 to NVM3 Upgrade Plugin](#7-2-SimEEv2-to-NVM3-Upgrade-Plugin) 中所述。

## 7.2 SimEEv2 to NVM3 Upgrade Plugin

为 EmberZNet 应用程序提供了一个 AppBuilder plugin（ **Simulated EEPROM version 2 to NVM3 Upgrade Library** ），以将存储在 SimEEv2 中的 token 升级到 NVM3。为了将 token 成功升级到 NVM3，必须为所有 token 添加 `CREATOR_*` 和 `NVM3KEY_*` 定义，如 [9.1 Token API](9-1-Token-API) 所述。该 plugin 将使用 NVM3 存储实例替换 SimEEv2 存储。该 plugin 通过将 SimEEv2 存储空间压缩至 12 kB 来实现此目的，然后在原来的 36 kB SimEEv2 存储空间的剩余 24 kB 中创建一个 NVM3 实例。将 token 数据从 SimEEv2 复制到 NVM3 之后，将擦除 SimEEv2 存储，并调整 NVM3 实例的大小以使用整个 36 kB 存储空间。除了升级库代码所需的代码空间外，升级不需要向 36 kB 存储区增加任何 flash 空间。该 plugin 要求现有的 SimEEv2 存储空间和新的 NVM3 存储空间位于相同的地址并具有相同的大小。

应当包含 **Simulated EEPROM version 2 to NVM3 Upgrade Library** plugin，以启用升级，如下图所示。如果未找到 SimEEv2 token 数据，则该 plugin 将查找 NVM3 数据，如果未找到，则将创建一个新的 NVM3 实例，并将 token 设置为其默认值。对于不需要升级任何 SimEEv2 token 的应用程序，应改为包含 **Simulated EEPROM version 2 to NVM3 Upgrade Stub** plugin。

<p>
    <img src="../images/AN1135/Figure%207.2.%20SimEEv2%20to%20NVM3%20Upgrade%20Library%20and%20Stub%20Plugins%20in%20AppBuilder.png"
         alt="Figure 7.2. SimEEv2 to NVM3 Upgrade Library and Stub Plugins in AppBuilder"
         title="Figure 7.2. SimEEv2 to NVM3 Upgrade Library and Stub Plugins in AppBuilder">
</p>

# 8. 将 NVM3 与 Z-Wave 一起使用

For details on using NVM3 with Z-Wave applications, see [INS14250-10: Z-Wave Plus V2 Application Framework SDK7](https://www.silabs.com/documents/login/reference-manuals/INS14259.pdf), section 6.4.

# 9. NVM3 API 选项

本章介绍可用于访问 NVM3 对象的三种不同的 API。

* Token API
* Persistent Store API
* Native NVM3 API

## 9.1 Token API

Token API 与 EmberZNet、Connect 以及多协议应用程序一起使用，以访问存储在 SimEEv1 和 SimEEv2 中的数据。有关如何定义和访问 token 的信息，请参见 *AN1154: Using Tokens for Non-Volatile Data Storage* ，用户应在使用 token API 之前阅读此文档。当选择中 NVM3 Library plugin 而不是一个 SimEE plugin 时，NVM3 默认实例将代替 SimEE 用来存储 token 数据。仍然可以使用相同的 token API 访问存储在 NVM3 中的 token，但是 token 的定义需要进行一些修改才能与 NVM3 一起工作，如下所述。

定义与 SimEE 一起使用的 token 时，creator code 必须定义为 token 的标识符。同样地，在定义与 NVM3 一起使用的 token 时，必须为 token 定义 NVM3 key。与 NVM3 和 SimEE 都兼容的 token 定义将同时包含 NVM3 key 和 creator code，如下所示：

```c
#define CREATOR_name    16bit_value
#define NVM3KEY_name    20bit_value
#ifdef DEFINETYPES
    typedef data_type type
#endif
#ifdef DEFINETOKENS
    DEFINE_*_TOKEN(name, type, ... ,defaults)
#endif
```

根据 [Table 2.1 NVM3 Default Instance Key Space](#Table-2.1) 中的 Domain，为 token 选择一个 20-bit NVM3 key。每个 token 必须具有唯一的 NVM3 key，但 indexed token 除外，其中必须保留更多 NVM3 key 如 [9.1.2 Indexed Token 的特殊注意事项](#9-1-2-Indexed-Token-的特殊注意事项) 所述。

### 9.1.1 删除 Token

由于 token 是在编译时创建的，因此无法在运行时创建或删除 token。但是，NVM3 对象是可以在运行时创建和删除的，并且 token 初始化函数会为每个已定义的 token 创建 NVM3 对象（如果尚不存在）。Token 初始化通常不会删除 NVM3 对象（没有与之关联的相应 token）。因此，如果一个 token 不再包含在应用程序中，则应用程序应使用 [9.3 Native NVM3 API](#9-3-Native-NVM3-API) 中所述的 NVM3 Native API 手动删除其关联的 NVM3 对象。然而，对于 indexed token，token 初始化会检查 indexed token 的索引是否大于或小于 indexed token 的 NVK3KEY 范围中找到的 NVM3 对象的数量。如果索引较少，则 token 初始化将删除多余的 NVM3 对象。如果索引数量增加了，则将创建新的 NVM3 对象来保存这些索引。

删除 NVM3 对象时，实际的对象数据保留在 NVM3 中，但标记为已删除。删除的对象数据保留在 NVM3 中，并占用高速缓存空间，直到 NVM3 重新填充已擦除包含这些对象所有版本的 page 为止。

### 9.1.2 Indexed Token 的特殊注意事项

NVM3 没有对 indexed token 的 native 支持。因此，对 indexed token 的 NVM3 key 选择提出了额外要求。使用 NVM3，可通过将每个索引存储在单独的对象中来实现 indexed token，该索引从 0（存储在所定义的 `NVM3KEY_name` key value）开始到 127（使用 key `NVM3KEY_name` + 127 存储）。在此实现中，必须为每个 indexed token 保留 128 个 NVM3 key。用户仍然只需定义一个 `NVM3KEY_name` key 值，但不应使用此定义的 key 之后的 127 个值中的 key 值来定义其他 token。即使 token 的定义少于 128 个索引，也应保留 128 个索引，以便后续的扩展。

下面的示例展示了在 user key domain 中定义的两个 indexed token：

```c
// This key is used for an indexed token and the subsequent 0x7F keys are also reserved
#define NVM3KEY_MY_INDEXED_TOKEN_A 0x00000
// This key is used for an indexed token and the subsequent 0x7F keys are also reserved
#define NVM3KEY_MY_INDEXED_TOKEN_B 0x00080
```

<table>
<thead>
  <tr>
    <th style="white-space: nowrap; text-align: center;">NVM3KEY</th>
    <th style="white-space: nowrap;">NVM3 Objects Contents</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="white-space: nowrap; text-align: center;">0x00000</td>
    <td style="white-space: nowrap;">Reserved for TOKEN_MY_INDEXED_TOKEN_A index 0</td>
  </tr>
  <tr>
    <td style="white-space: nowrap; text-align: center;">0x00001</td>
    <td style="white-space: nowrap;">Reserved for TOKEN_MY_INDEXED_TOKEN_A index 1</td>
  </tr>
  <tr>
    <td style="white-space: nowrap; text-align: center;">0x00002</td>
    <td style="white-space: nowrap;">Reserved for TOKEN_MY_INDEXED_TOKEN_A index 2</td>
  </tr>
  <tr>
    <td style="white-space: nowrap; text-align: center;">…</td>
    <td style="white-space: nowrap;"></td>
  </tr>
  <tr>
    <td style="white-space: nowrap; text-align: center;">0x0007F</td>
    <td style="white-space: nowrap;">Reserved for TOKEN_MY_INDEXED_TOKEN_A index 127</td>
  </tr>
  <tr>
    <td style="white-space: nowrap; text-align: center;">0x00080</td>
    <td style="white-space: nowrap;">Reserved for TOKEN_MY_INDEXED_TOKEN_B index 0</td>
  </tr>
  <tr>
    <td style="white-space: nowrap; text-align: center;">0x00081</td>
    <td style="white-space: nowrap;">Reserved for TOKEN_MY_INDEXED_TOKEN_B index 1</td>
  </tr>
  <tr>
    <td style="white-space: nowrap; text-align: center;">…</td>
    <td style="white-space: nowrap;"></td>
  </tr>
  <tr>
    <td style="white-space: nowrap; text-align: center;">0x000FF</td>
    <td>Reserved for TOKEN_MY_INDEXED_TOKEN_B index 127</td>
  </tr>
</tbody>
</table>

## 9.2 Bluetooth NVM API

最初为 PS Store 设计的 Bluetooth NVM API 可以在使用 NVM3 和 PS Store 时以相同的方式使用。Bluetooth 协议栈会在背后自动将其 NVM API 调用转换为 PS Store API 调用或 NVM3 API 调用，具体取决于项目所使用的组件。Zigbee + Bluetooth DMP 项目也是如此，其中 NVM3 被用作存储机制，但是 Bluetooth NVM API 仍然可以使用。Bluetooth API Reference Manual 中记录了 Bluetooth API。在 Bluetooth SDK v2.x 中，可以在 Flash 分类下找到该命令（以 `gecko_cmd_flash_` 开头的命令），而在 Bluetooth SDK v3.x 中，可以在 NVM 分类下找到它（以 `sl_bt_nvm` 开头的命令）。

Bluetooth NVM API 使用 16-bit key，然后在将 NVM3 用作存储机制时将其映射到 20-bit NVM3 key。四个最高有效位设置为 0x4，以将这些对象放置在 NVM3 默认实例 key space 的 Bluetooth domain 中。由于将 Bluetooth NVM API 固定为仅使用 Bluetooth domain，因此必须使用 native NVM3 API 来创建和访问放置在其他 domain（如 User domain）中的任何对象。

如果要在同一应用程序中使用 Bluetooth NVM API 和 native NVM3 API，则：

1. 调用 `gecko_init(pconfig)` 初始化 Bluetooth 协议栈并打开其自己的 NVM3 实例。这是在所有 Bluetooth 示例应用中完成的。
2. 通过使用默认参数调用 `nvm3_open()` 函数来打开 NVM3，以打开您的 NVM3 实例：
    ```c
    nvm3_open(nvm3_defaultHandle, nvm3_defaultInit);
    ```

用户数据现在可以保存到：

* PS key 范围 0x4000 - 0x407F。所有其他 PS key（0x0000 - 0xFFFF）都为协议栈保留（如用于存储绑定数据）。
* NVM3 key 范围 0x00000 - 0x0FFFF（NVM3 user data area）和 0x44000 - 0x4407F（PS Store user data area）。

例如，以下 API 调用将具有相同的效果（写入相同的区域）：

* `nvm3_writeData(nvm3_defaultHandle,0x44000,(void*)data,len);`
* `gecko_cmd_flash_ps_save(0x4000,len,data); //in Bluetooth SDK v2.x`
* `sl_bt_nvm_save(0x4000,len,data); //in Bluetooth SDK v3.x`

同样地，您可以通过以下 API 调用读取相同的数据：

* `nvm3_readData(nvm3_defaultHandle,0x44000,(void*)read_buffer,maxlen);`
* `gecko_cmd_flash_ps_load(0x4000); //in Bluetooth SDK v2.x`
* `sl_bt_nvm_load(0x4000,maxlen,&read_len,(uint8_t*)read_buffer); //in Bluetooth SDK v3.x`

## 9.3 Native NVM3 API

对于不需要与 token 或 PS Store API 兼容的 NVM3 对象的代码访问，建议使用 native NVM3 API 访问 NVM3 数据，以减少代码大小并允许使用 NVM3 的全部特性。也可以使用同一 NVM3 key 通过 native NVM3 API 访问任何 PS Store 对象或 token。有关此 API 的完整文档，请参阅 [Gecko HAL & Driver API Reference Guide](http://devtools.silabs.com/dl/documentation/doxygen/) 的 EMDRV->NVM3 部分。

# 10. Simplicity Commander and NVM3

Simplicity Commander 是在生产环境中使用的一个通用工具。也可以编写脚本或通过 CLI（Command Line Interface）来调用它。Simplicity Commander 支持从设备读取 NVM3 数据区域并解析 NVM3 数据以提取存储的值。这在调试场景中很有用，在这种情况下，您可能需要找出已经运行了一段时间的应用程序的存储状态。

*UG162: Simplicity Commander Reference Guide* 中提供了有关如何将 Simplicity Commander 与 NVM3 一起使用的更多信息。
