# AN1085: Using the Gecko Bootloader with Silicon Labs Connect (Rev. 0.4) <!-- omit in toc -->

- [1. Overview](#1-overview)
- [2. Using the Gecko Standalone Bootloaders](#2-using-the-gecko-standalone-bootloaders)
  - [2.1 Performing a Serial Upload – UART XMODEM Bootloader](#21-performing-a-serial-upload--uart-xmodem-bootloader)
  - [2.2 Errors and Status Codes](#22-errors-and-status-codes)
  - [2.3 Running the Application Image](#23-running-the-application-image)
  - [2.4 Performing a Bootloader Upgrade](#24-performing-a-bootloader-upgrade)
  - [2.5 Upload Recovery](#25-upload-recovery)
- [3. Using the Gecko Application Bootloader](#3-using-the-gecko-application-bootloader)
  - [3.1 Acquiring a New Image](#31-acquiring-a-new-image)
  - [3.2 Performing an Application Upgrade](#32-performing-an-application-upgrade)
  - [3.3 External Storage Application Bootloader](#33-external-storage-application-bootloader)
  - [3.4 Local Storage Application Bootloader](#34-local-storage-application-bootloader)

本应用笔记包含了将 Silicon Labs Gecko Bootloader 与 Silicon Labs Connect stack（Silicon Labs Flex SDK 的一部分）结合使用的相关信息。它补充了 *UG266: Silicon Labs Gecko Bootloader User's Guide* 中所提供的常规 Gecko Bootloader 的实现信息。如果您不熟悉执行固件升级的基本原理，或者想了解有关升级映像文件的更多信息，请参考 *UG103.6: Bootloading Fundamentals* 。

*Connect v3.x User's Guide* 是一系列文档，为使用 Silicon Labs Connect Stack 进行应用开发的开发者提供了深入的信息。请参阅 *UG235.06: Bootloading and OTA with Silicon Labs Connect* ，以了解可在 Connect-based 应用程序中使用的 bootloader 选项（standalone、application 和 OTA（Over the Air））。

所有 EFR32FG 设备均支持 Proprietary。对于其他产品，请在 Ordering Information > Protocol Stack 下查看设备的 datasheet，以了解是否支持 Proprietary。在 Proprietary SDK version 2.7.n 中，EFR32xG22 不支持 Connect。

# 1. Overview

Silicon Labs Gecko Bootloader 是适用于 Silicon Labs 的所有 newer MCU 和 wireless MCU 的通用 bootloader。可以将 Gecko Bootloader 配置为执行各种引导加载功能（从设备初始化到固件升级）。Gecko Bootloader 使用一种专有格式的升级映像，称为 GBL（Gecko Bootloader）。这些映像的文件扩展名为 “.gbl”。 *UG103.6: Bootloading Fundamentals* 提供了有关 GBL 文件格式的其他信息。

Gecko Bootloader 采用一个 two-stage 设计，其中最小的 first stage bootloader 用于升级 main bootloader。First stage bootloader 仅包含在内部闪存（flash）中读写固定地址的功能。要执行一个 main bootloader 升级，正在运行的 main bootloader 会验证 bootloader 升级映像文件的完整性和真实性。然后，正在运行的 main bootloader 会将升级映像写到闪存中的固定位置，并发出一个重启以进入到 first stage bootloader。First stage bootloader 在将升级映像复制到 main bootloader 位置之前会通过计算 CRC32 校验和来验证 main bootloader 固件升级映像的完整性。

可以将 Gecko Bootloader 配置为以 standalone mode（即 standalone bootloader）或 application mode（即 application bootloader）执行固件升级，具体取决于 plugin 配置。可以通过 Simplicity Studio IDE 启用和配置 plugin。

Standalone bootloader 使用通信通道来获取固件升级映像。NCP（network co-processor）设备始终使用 standalone bootloader。Standalone bootloader 以一个 single-stage 过程执行固件映像升级，该过程允许将 application 映像放入闪存中以覆盖现有的 application 映像，而无需 application 本身参与。通常，application 与 standalone bootloader 进行交互的唯一时刻是当其请求重新引导至 bootloader 时。Bootloader 运行后，它将通过物理连接（如 UART 或 SPI）接收包含固件升级映像的数据包。要用作一个 standalone bootloader，必须配置一个提供通信接口（如 UART 或 SPI）的 plugin。

Application bootloader 依靠 application 来获取固件升级映像。Application bootloader 通过使用存储在闪存区域（称为下载空间）中的固件升级映像对设备的闪存进行重新编程来执行固件映像升级。Application 可以以任何方便的方式（UART、OTA 等）将固件升级映像传输到下载空间（download space）。下载空间可以是外部存储器（如 EEPROM 或 dataflash），也可以是芯片内部闪存的一部分。Gecko Bootloader 可以将下载空间划分为多个存储槽（storage slot），并同时存储多个固件升级映像。要用作 application bootloader，必须配置一个提供 bootloader 存储实现的 plugin。

本文档介绍了如何将这两个模型与 Silicon Labs Connect 一起使用。

# 2. Using the Gecko Standalone Bootloaders

Gecko Bootloader-based standalone bootloader 通过 SPI 或 UART 的串行传输将 application 映像接收到目标设备上。如果使用 UART，则您可以在源设备和目标设备的串行接口之间建立一个串行连接，并使用 XModem 协议向其上传新的软件映像。如果您需要有关 XModem 协议的信息，那么一个很好的起点是 [http://en.wikipedia.org/wiki/XMODEM](http://en.wikipedia.org/wiki/XMODEM) ，其中应该有简短的描述和协议文档的最新链接。

## 2.1 Performing a Serial Upload – UART XMODEM Bootloader

可以使用提供预期串行接口方法的任何源设备执行串行上传。这可以是一个 Windows-based PC、Linux-based 或 Mac OS-based 设备，也可以是没有操作系统的嵌入式 MCU。UART 传输可以使用 Windows HyperTerminal 或 Linux“lrzsz” 等第三方串行终端程序或用户编译的主机代码来完成。但是，用于 SPI Master 或 UART 的驱动程序可能会因操作系统而异，并且串行终端程序的时序和性能可能会有所不同。因此，如果不确定在源码上使用哪种驱动程序或程序，请咨询 Silicon Labs 技术支持。

要通过 UART 打开一个串行连接，源设备将以 115,200 波特率、8 个数据位、无奇偶校验位和 1 个停止位（8-N-1）连接到目标设备，默认情况下没有流控制。可以使用 Bootloader 项目中的 plugin 选项来更改这些选项。

> 注意：默认情况下，UART-based serial bootloader 配置在通信通道中不使用任何流控制，因为用于映像传输的 XModem 协议已经具有内置的流控制机制。但是，Silicon Labs 通常提供的 NCP 固件确实使用 hardware-based（RTS/CTS）或 software-based（XON / XOFF）的流控制，因此 host 设备在将其 NCP 置于 serial bootloading mode 时必须注意暂时地禁用流控制。另外，应用程序设计者可以更改 bootloader 项目中提供的选项，并自定义 serial bootloader 对 UART 的处理，以自行决定是否添加硬件流控制。

与 UART-based serial bootloader 建立连接后：

1. 目标设备的 bootloader 在以预期的波特率从源设备接收到回车符之后，通过其串行端口发送输出。这样可以防止 bootloader 过早地（可能会被连接到串行端口的其他设备误解）发送命令。请注意，在等待通过回车返回的初始串行握手时，serial bootloader 通常不执行任何超时操作，因此 bootloader 将在此模式下无限期等待，直到由源设备指导或芯片复位为止。
2. Bootloader 从目标设备收到回车符后，它将显示一个菜单，其中包含以下 ASCII-based 输出（其中 X.Y.A 分别对应于 bootloader 版本号的 major、minor 和 sub-minor 字段）：
    ```
    Gecko Bootloader vX.Y.A
    1. upload gbl
    2. run
    3. ebl info
    BL >
    ```

> 注意：第三个菜单选项无效。尽管当前菜单选项在功能上应保持不变，但菜单标题和选项文本很容易会被更改，并且可能会添加新选项。

列出菜单选项后，将显示 bootloader 的“BL >”提示，然后源可以输入与每个选项编号相对应的 ASCII 字符来选择所描述的操作，例如 “2”（ASCII 代码 0x32）运行当前在应用程序区域中加载的固件。同样，bootloader 不会强制执行超时，因此它将无限期等待，直到接收到字符或芯片复位为止。请注意，尽管菜单界面是为人机交互而设计的，但是只要源设备在适当的时候将预期的 ASCII 字符发送到目标，仍然可以通过编程方式或通过脚本接口来执行传输。

> 注意：与 bootloader 交互的脚本应仅使用“BL >”提示来确定 bootloader 何时可以输入。

选择菜单选项 1 将开始将新的软件映像上传到目标设备，其展开如下：

1. 目标设备等待通过预期的串行接口（XModem CRC）上传 GBL 文件，这由其 bootloader 发送的 C 字符流指示。
2. 如果 60 秒内未发起任何事务，则 bootloader 超时并返回菜单。
3. 一旦开始上传（接收到第一个 XModem SOH 数据包），bootloader 就会期望每个连续的 XModem SOH 数据包间隔在 1 秒之内，否则将产生超时错误，并且会话将中止。
4. 成功上传映像后，XModem 事务完成，并且 bootloader 会在重新显示菜单之前显示“Serial upload complete”。

## 2.2 Errors and Status Codes

如果在上传过程中发生错误，则 UART serial bootloader 会显示消息“Serial upload aborted”，然后显示更详细的消息和十六进制错误码。下表展示了一些较常见的错误。UART serial bootloader 然后重新显示 bootloader 菜单。

下表描述了 Gecko Bootloader 使用的正常状态码、错误条件以及特殊字符或枚举。有关其他状态码，请参阅 `platform/bootloader/documentation` 文件夹中随 SDK 一起安装的 Gecko Bootloader API 文档。

<table title="Table 1. Serial Uploading Statuses, Error Messages, and Special Characters">
<thead>
  <tr>
    <th style="white-space: nowrap;">Hex code</th>
    <th style="white-space: nowrap;">Constant</th>
    <th style="white-space: nowrap;">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="white-space: nowrap;">0x00</td>
    <td style="white-space: nowrap;">BL_SUCCESS</td>
    <td>Default success status.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x01</td>
    <td style="white-space: nowrap;">BL_ERR</td>
    <td>General error processing packet.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x1C</td>
    <td style="white-space: nowrap;">BLOCK_TIMEOUT</td>
    <td>The bootloader timed out waiting for some part of the XModem frame.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x21</td>
    <td style="white-space: nowrap;">BLOCKERR_SOH</td>
    <td>The bootloader did not find the expected start of header (SOH) character at the beginning of the XModem frame.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x22</td>
    <td style="white-space: nowrap;">BLOCKERR_CHK</td>
    <td>The bootloader detected the sequence check byte of the XModem frame was not the inverse of the sequence byte.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x23</td>
    <td style="white-space: nowrap;">BLOCKERR_CRCH</td>
    <td>The bootloader encountered an error while comparing the high bytes of the received and calculated CRCx of the XModem frame.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x24</td>
    <td style="white-space: nowrap;">BLOCKERR_CRCL</td>
    <td>The bootloader encountered an error while comparing the low bytes of the received and calculated CRCs of the XModem frame.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x25</td>
    <td style="white-space: nowrap;">BLOCKERR_SEQUENCE</td>
    <td>The bootloader did not receive the expected sequence number in the current XModem frame.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x26</td>
    <td style="white-space: nowrap;">BLOCKERR_PARTIAL</td>
    <td>The frame that the bootloader was trying to parse was deemed incomplete (some bytes missing or lost).</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x27</td>
    <td style="white-space: nowrap;">BLOCKERR_DUPLICATE</td>
    <td>The bootloader encountered a duplicate of the previous XModem frame.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x40</td>
    <td style="white-space: nowrap;">BL_ERR_MASK</td>
    <td>Bitmask for any bootloader error codes returned in CAN or NAK frame.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x41</td>
    <td style="white-space: nowrap;">BL_ERR_HEADER_EXP</td>
    <td>No GBL header was received when expected.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x42</td>
    <td style="white-space: nowrap;">BL_ERR_HEADER_WRITE_CRC</td>
    <td>Failed to write header or CRC.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x43</td>
    <td style="white-space: nowrap;">BL_ERR_CRC</td>
    <td>File or written image failed CRC check.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x44</td>
    <td style="white-space: nowrap;">BL_ERR_UNKNOWN_TAG</td>
    <td>Unknown tag detected in GBL image.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x45</td>
    <td style="white-space: nowrap;">BL_ERR_SIG</td>
    <td>Invalid GBL header contents.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x46</td>
    <td style="white-space: nowrap;">BL_ERR_ODD_LEN</td>
    <td>Trying to flash odd number of bytes.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x47</td>
    <td style="white-space: nowrap;">BL_ERR_BLOCK_INDEX</td>
    <td>Indexed past end of block buffer.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x48</td>
    <td style="white-space: nowrap;">BL_ERR_OVWR_BL</td>
    <td>Attempt to overwrite bootloader flash.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x49</td>
    <td style="white-space: nowrap;">BL_ERR_OVWR_SIMEE</td>
    <td>Attempt to overwrite SIMEE flash.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x4A</td>
    <td style="white-space: nowrap;">BL_ERR_ERASE_FAIL</td>
    <td>Flash erase failed.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x4B</td>
    <td style="white-space: nowrap;">BL_ERR_WRITE_FAIL</td>
    <td>Flash write failed.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x4C</td>
    <td style="white-space: nowrap;">BL_ERR_CRC_LEN</td>
    <td>End tag CRC wrong length.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x4D</td>
    <td style="white-space: nowrap;">BL_ERR_NO_QUERY</td>
    <td>Received data before query request/response.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x4E</td>
    <td style="white-space: nowrap;">BL_ERR_BAD_LEN</td>
    <td>An invalid length was detected in the upgrade image file.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x4F</td>
    <td style="white-space: nowrap;">BL_ERR_TAGBUF</td>
    <td>Insufficient tag buffer size or an invalid length was found in the GBL image.</td>
  </tr>
  <tr>
    <td colspan="3"><strong>Special Characters Used in Packet Types</strong></td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x01</td>
    <td style="white-space: nowrap;">SOH</td>
    <td>Start of Header.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x03</td>
    <td style="white-space: nowrap;">CTRL_C</td>
    <td>Cancel (from sender).</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x04</td>
    <td style="white-space: nowrap;">EOT</td>
    <td>End of Transmission.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x06</td>
    <td style="white-space: nowrap;">ACK</td>
    <td>Acknowledged.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x15</td>
    <td style="white-space: nowrap;">NAK</td>
    <td>Not acknowledged.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x18</td>
    <td style="white-space: nowrap;">CAN</td>
    <td>Cancel</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x43</td>
    <td style="white-space: nowrap;">C</td>
    <td>ASCII ‘C’.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x51</td>
    <td style="white-space: nowrap;">QUERY</td>
    <td>ASCII ‘Q’.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x52</td>
    <td style="white-space: nowrap;">QRESP</td>
    <td>ASCII ‘R’.</td>
  </tr>
  <tr>
    <td colspan="3"><strong>Status Codes Returned in a Synchronous Response</strong></td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x16</td>
    <td style="white-space: nowrap;">TIMEOUT</td>
    <td>Bootloader timed out expecting characters.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x17</td>
    <td style="white-space: nowrap;">FILEDONE</td>
    <td>EOT process successfully.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x18</td>
    <td style="white-space: nowrap;">FILEABORT</td>
    <td>Transfer aborted prematurely.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x19</td>
    <td style="white-space: nowrap;">BLOCKOK</td>
    <td>Data block processed OK.</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;">0x1A</td>
    <td style="white-space: nowrap;">QUERYFOUND</td>
    <td>Successful query.</td>
  </tr>
</tbody>
</table>

## 2.3 Running the Application Image

对于使用交互式菜单的 standalone bootloader 变体，bootloader 菜单选项 2（run）会重置目标设备以进入上传的 application 映像。如果不存在 application 映像，或者在上一次上传期间发生错误，则 bootloader 将返回到菜单。对于不使用菜单的 SPI-based 变体，在确认源设备的 EOT 帧后立即运行该 application。

## 2.4 Performing a Bootloader Upgrade

要使用 standalone bootloader 执行 bootloader 升级，只需连续传输两个 GBL 文件。第一个 GBL 文件应包含 main bootloader 升级映像。上传完成后，设备将重置以升级 main bootloader。对于使用交互式菜单的 standalone bootloader 变体，bootloader 菜单选项 2（run）将重置目标设备以进入 first stage bootloader 来执行升级，然后返回升级的 main bootloader。然后可以上传包含 application 升级映像的第二个 GBL 文件。

## 2.5 Upload Recovery

如果映像上传失败，则目标节点将没有有效的 application 映像。如果 standalone bootloader 确定 application 映像无效，则它将重新进入固件升级模式。然而，application 映像可能具有有效的结构，但包含一个阻止正常运行的错误。无论 standalone bootloader 支持哪种串行接口，都可以使用 GPIO-based 触发器来通过串行上传进行恢复。

您可以通过在 Application Builder 框架中配置 GPIO Activation plugin，或将自定义代码添加到 bootloader 项目中，以将 standalone bootloader 配置为使用 software-based GPIO 引脚检查或其他恢复模式激活方案（如按键恢复）。

# 3. Using the Gecko Application Bootloader

## 3.1 Acquiring a New Image

Application bootloader 依赖 application code 来获取新的 code 映像。Bootloader 本身仅知道如何读取存储在下载空间中的 GBL 映像并将相关部分复制到 main flash 块中。这种方法意味着应用开发者可以自由地以任何有意义的方式（serial、OTA 等）获取新的 code 映像。

通常，应用开发者选择通过 OTA 方式获取新的 code 映像，因为这在所有设备上都可以很容易地使用。 *UG235 Connect Users Guide* 广泛地介绍了如何在 Connect 中获取新映像。

## 3.2 Performing an Application Upgrade

Application 和 bootloader 之间的间接联系受到限制。它们唯一的交互是通过跨模块重新引导来传递非易失性数据。

一旦 application 决定安装保存在下载空间中的新映像，它将调用 Ember HAL API `halAppBootloaderInstallNewImage()` 。此调用向 bootloader 指示它应尝试从存储槽 0 执行固件升级，然后重新引导设备。该 API 与 legacy Ember bootloader 向后兼容。如果需要从非存储槽 0 的其他存储槽执行固件升级，则应通过调用 `bootloader_setBootloadList()` 来使用 Gecko Bootloader API。如果 bootloader 无法安装新映像，则会将重置原因设置为 `RESET_BOOTLOADER_BADIMAGE` 并重置模块。启动后，application 应使用 `halGetExtendedResetInfo()` 读取重置原因。如果重置原因设置为 `RESET_BOOTLOADER_BADIMAGE` ，则 application 知道安装过程失败，可以尝试获取新映像。可通过调用 `halGetExtendedResetString()` 获得可打印的错误字符串。通常情况下，application bootloader 不会在串行线上打印任何内容。

## 3.3 External Storage Application Bootloader

有关 external storage bootloader 的存储配置的信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide* 。

Application bootloader 通常使用一个远程设备来存储下载的 application 映像。可以通过 I2C 或 SPI 串行接口访问该设备。有关受支持的 Dataflash/EEPROM 设备的列表，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide* 。重要的是选择一个大小至少为您的闪存大小的设备，以适合正在引导加载的 application。

External SPI serial flash 的默认建议是 MX25R，因为它以标准和更小封装提供，受标准驱动程序支持，并且具有非常低的 software-enabled 睡眠电流，而无需外部关机控制电路。Silicon Labs 的大多数无线电板都装有 MX25R8035F，用于评估目的。通常，客户应使用睡眠电流低，不需要外部关机控制电路且具有软件关机控制功能的部件。当使用具有高空闲/睡眠电流且没有软件关机控制的组件时，建议使用外部关机控制电路以降低睡眠电流。

对于使用 EFR32MG1 的 EFR32MG-based 设备，仅提供 serial dataflash 选项；当前没有提供 local storage application bootloader。但是，某些 EFR32MG IC 包含一个集成的 serial flash，可以像片外 serial dataflash 一样使用。使用 EFR32MG12 及以上的 EFR32MG-based 设备支持 local storage application bootloader。

> 注意：这些芯片中的一些具有与其他芯片兼容的引脚排列，但是有一些不兼容的变体。请联系 Silicon Labs，以获取有关将 I2C 或其他 SPI dataflash 芯片连接到 EFR32 的详细信息。

Read-Modify-Write 与某些 dataflash 芯片的特性有关，它们的相应驱动程序公开了该功能，并且被 bootloader 库利用。没有此功能的芯片需要在写页之前执行页擦除操作，这阻止了 application 的随机访问写入。

## 3.4 Local Storage Application Bootloader

Local storage bootloader 本质上是一个具有 dataflash 驱动程序的 application bootloader，该驱动程序使用片上闪存的一部分进行映像存储，而不是使用外部存储芯片。有关内部存储器的存储配置的信息，请参阅 *UG266: Silicon Labs Gecko Bootloader User's Guide* 。

由于 local storage application bootloader 会更改芯片的闪存布局，因此您必须在了解这一点的情况下构建您的 application。为此，您必须将适用的 EFR32 设备的 `LOCAL_STORAGE_GECKO_INFO_PAGE_BTL` 全局定义添加到 IAR 项目文件中。如果您是通过 Simplicity Studio 中的 AppBuilder 创建项目文件的，那么您只需从 bootloader 下拉菜单中选择 Local Storage，即可为您完成此操作。

<p>
    <img src="../images/AN1085/Figure%201.%20Main%20Flash%20Layout%20for%20Application%20Bootloader%20versus%20Local%20Storage%20Application%20Bootloader%20when%20Using%20Gecko%20Bootloader.png"
         alt="Figure 1. Main Flash Layout for Application Bootloader versus Local Storage Application Bootloader when Using Gecko Bootloader"
         title="Figure 1. Main Flash Layout for Application Bootloader versus Local Storage Application Bootloader when Using Gecko Bootloader">
</p>

> 注意：在 EFR32MG1 上，application 偏移了 16 kB，以将 bootloader 容纳在 main flash 的底部。在较新的 EFR32 设备上，Gecko Bootloader 位于 main flash 块外部的 Bootloader flash 区域中。
