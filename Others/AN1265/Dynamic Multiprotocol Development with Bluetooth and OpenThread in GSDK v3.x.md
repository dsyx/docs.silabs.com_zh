# AN1265: Dynamic Multiprotocol Development with Bluetooth® and OpenThread in GSDK v3.x (Rev. 0.3) <!-- omit in toc -->

- [1 Introduction](#1-introduction)
  - [1.1 Hardware Requirements](#11-hardware-requirements)
- [2 Building the ot-ble-dmp Sample App](#2-building-the-ot-ble-dmp-sample-app)
- [3 CLI Commands](#3-cli-commands)
  - [3.1 Establishing a Bluetooth Connection Between Two Nodes](#31-establishing-a-bluetooth-connection-between-two-nodes)
- [4 Important Software Components and Files](#4-important-software-components-and-files)
  - [4.1 The Main Function and Initialization](#41-the-main-function-and-initialization)
  - [4.2 FreeRTOS Tasks](#42-freertos-tasks)
  - [4.3 Handling Bluetooth Events](#43-handling-bluetooth-events)
  - [4.4 Power Manager Integration](#44-power-manager-integration)
- [5 OpenThread Sleepy End Device Demo](#5-openthread-sleepy-end-device-demo)

---

本应用笔记提供了有关使用 Bluetooth 和 Silicon Labs OpenThread 开发 DMP（Dynamic Multiprotocol，动态多协议）应用程序的详细信息。它描述了如何使用 Silicon Labs OpenThread SDK 在 Simplicity Studio 5 中配置应用程序。然后，它提供了有关底层代码如何运行的详细演练。有关适用于所有协议组合的 DMP 应用程序开发的详细信息，请参阅 *UG305: Dynamic Multiprotocol User’s Guide*。

# 1 Introduction

本应用笔记提供了有关使用在 FreeRTOS 实时操作系统上运行的 Silicon Labs OpenThread 和 Bluetooth 开始 DMP 应用程序的说明。

示例应用程序 **ot-ble-dmp** 是一个测试应用程序，它演示了用于构建 DMP 应用程序的组件。它提供了一个 CLI（Command Line Interface，命令行接口），允许用户执行基本的 OpenThread 和 Bluetooth 命令。它还演示了如何使用电源管理器组件通过允许设备在活动之间进入低功耗（EM2）模式来节省电量。

DMP 中的术语“dynamic”指出了两个协议同时运行的事实。无线电调度器负责通过无线电复用传输和接收的数据包。有关无线电调度程序如何工作的更多信息，请参阅 *UG305: Dynamic Multiprotocol User’s Guide*。

本文档假定您已安装了 SSv5（Simplicity Studio 5）以及 OpenThread 和 Bluetooth 的 SDK，并且熟悉 SSv5 以及配置、构建和刷写应用程序。如果不是，请参阅 *QSG170: Silicon Labs OpenThread Quick Start Guide*。

## 1.1 Hardware Requirements

* 一个具有至少 512 kB 闪存的 EFR32 部件。

# 2 Building the ot-ble-dmp Sample App

Gecko SDK Suite 3.0 提供了预编译的演示应用映像，兼容：

* brd4161a 
* brd4166a 
* brd4168a 
* brd4180a 
* brd4304a

要快速开始，请在 SSv5 的 Launcher Perspective 中转到 DEMOS 选项卡。找到 **ot-ble-dmp** 演示并点击 **RUN**。这会将应用程序映像上传到您的板子。

要从源码开始构建 **ot-ble-dmp** 示例应用程序，您需要安装 SSv5 和安装了以下包的 Gecko SDK 3.x 开发环境：

* OpenThread SDK 1.x
* Bluetooth SDK 3.x

GNU ARM toolchain 随 SSv5 安装。IAR-EWARM toolchain 与 OpenThread 不兼容。

1. 连接目标开发硬件后，打开 SSv5 的 File 菜单并选择 New > Silicon Labs Project Wizard。这会打开 Target, SDK, and Toolchain Selection 对话框。您的目标硬件应该已被填充好。点击 **NEXT**。
2. Example Project Selection 对话框会被打开。使用 Technology Type 和 Keyword 过滤器搜索一个特定示例，在本例中为 **ot-ble-dmp**。选择它并点击 **NEXT**。<br>请注意，如果您没有看到该应用程序，则您所连接的硬件可能不兼容。要验证这个问题，在 Launcher Perspectives 的 My Products 视图中输入 EFR32MGxx 并选择其中一个板。转到 Examples 选项卡，按 Thread 技术过滤并确认您可以看到该应用程序。
3. Project Configuration 对话框会被打开。在这里您可以重命名您的项目、更改默认的项目文件位置，并确定您是否要链接到或复制项目文件。请注意，如果您更改了任何链接的资源，它也会为引用它的任何其他项目而更改。除非您清楚您要修改 SDK 资源，否则请使用默认选择。点击 **FINISH**。

Simplicity IDE 打开时会在 Project Configurator 中打开 ot-ble-dmp 项目。您现在可以构建该项目。对于习惯使用 Simplicity Studio 4 的用户，这不需要生成步骤，因为它是自动完成的。ot-ble-dmp.s37 映像将位于 GNU ARM 7.2.x 目录中，可以使用 flash programmer 或 Simplicity Commander 等 SSv5 工具将其上传到您的板子。

# 3 CLI Commands

如果您使用过 **ot-cli-ftd** 示例应用程序，那么其中可用的 OpenThread 命令也可用于 **ot-ble-dmp** 应用程序。在提示符下键入 help 以查看列表。此处提供了完整的 OpenThread CLI 参考：

[https://github.com/openthread/openthread/blob/master/src/cli/README.md](https://github.com/openthread/openthread/blob/master/src/cli/README.md)

此处提供了有关使用 CLI 建立一个双节点 OpenThread 网络并发送 ping 的快速教程：

[https://github.com/openthread/openthread/tree/master/examples/apps/cli](https://github.com/openthread/openthread/tree/master/examples/apps/cli)

ot-ble-dmp 应用程序添加了一组可用于测试 Bluetooth 栈的 Bluetooth 命令。在提示符下键入“ble”以查看子命令列表：

* `get_address`
* `create_adv_set`
* `set_adv_timing`
* `set_adv_random_address`
* `start_adv`
* `stop_adv`
* `start_discovery`
* `set_conn_timing`
* `conn_open`
* `conn_close`

这些命令在 bluetooth_cli.c 文件中实现，每一个都调用了相应的 Bluetooth C API 函数。有关底层功能的详细文档，请参阅 [https://docs.silabs.com/bluetooth/latest](https://docs.silabs.com/bluetooth/latest)。请注意，Bluetooth SDK 的 C API 前缀在 3.0 版中从 gecko\_ 更改为 sl\_bt\_。有关此更改和其他 BGAPI 更改的更多信息，请参阅 *AN1255: Transitioning from the v2.x to the v3.x Bluetooth® SDK*。

`ble get_address`

* 打印出公共 Bluetooth 地址。
* 示例：`ble get_address`
* 调用了 `sl_bt_system_get_identity_address()`

`ble create_adv_set`

* 创建一个广告集。必须调用它来获取一个在其它广告命令中使用的句柄。
* 示例：`ble create_adv_set`
* 调用了 `sl_bt_advertiser_create_set()`

`ble set_adv_timing <handle> <interval_min> <interval_max> <duration> <max_events>`

* 设置给定广告集的广告时间参数。
* 示例：`ble set_adv_timing 0 160 320 0 0`
* 调用了 `sl_bt_advertiser_set_timing()`

`ble set_adv_random_address <handle>`

* 设置广告集上的广告者以使用一个随机地址。
* 示例：`ble set_adv_random_address 1`
* 调用了 `sl_bt_advertiser_set_random_address()`

`ble start_adv <handle> <discoverableMode> <connectableMode>`

* 使用指定的可发现和可连接模式在给定的广告集上开始广告。
* 示例：`ble start_adv 0 2 2`
* 调用了 `sl_bt_advertiser_start()`

`ble stop_adv`

* 停止在给定句柄上的广告。
* 示例：`ble stop_adv`
* 调用了 `sl_bt_advertiser_stop()`

`ble start_discovery <mode>`

* 扫描广告设备。
* 示例：`ble start_discovery 1`
* 调用了 `sl_bt_scanner_start()`

`ble set_conn_timing <min_interval> <max_interval> <latency> <timeout>`

* 设置默认的 Bluetooth 连接参数。
* 示例：`ble set_conn_timing 6 400 0 800`
* 调用了 `sl_bt_connection_set_default_parameters()`

`ble conn_open <address> <address_type>`

* 连接到一个广告设备。地址类型 0=公共地址；1=随机地址。initiating_phy 参数硬编码为 1。
* 示例：`ble conn_open 80fd34a198bf 0`
* 调用了 `sl_bt_connection_open()`

`ble conn_close <handle>`

* 关闭一个 Bluetooth 连接。
* 示例：`ble conn_close 0`
* 调用了 `sl_bt_connection_close()`

## 3.1 Establishing a Bluetooth Connection Between Two Nodes

为了建立一个 Bluetooth 连接，Client 以可发现和可连接模式在广告集 0 上开始广告。Server 使用 client 的公共地址进行连接。

CLIENT:

```
> ble create_adv_set
ble create_adv_set
success handle=0
>
> ble start_adv 0 2 2
ble start_adv 0 2 2
success
>
> ble get_address
ble get_address
BLE address: 90fd9f7b5d39
```

SERVER:

```
> ble conn_open 90fd9f7b5d39 0
ble conn_open 90fd9f7b5d39 0
success
>
> BLE connection opened handle=1 address=90fd9f7b5d39 address_type=1 master=1 advertising_set=255
BLE connection parameters handle=1 interval=40 latency=0 timeout=100 security_mode=0 txsize=27
BLE event: 0x40800a0
BLE event: 0x900a0
BLE connection parameters handle=1 interval=40 latency=0 timeout=100 security_mode=0 txsize=251
```

# 4 Important Software Components and Files

在 Project Configurator 中打开 ot-ble-dmp 项目后，点击 Software Components 选项卡以查看组件库中的所有软件组件。筛选已安装的组件以查看项目中使用到的组件。与构建 DMP 应用程序相关的四个关键组件类别是：

* Bluetooth 组件 
* OpenThread 组件 
* Rail Library Multiprotocol 组件（在 Platform > Radio 下）
* FreeRTOS 组件（在 Third Party 下）

您需要在任何项目中包含这些组件以添加 OpenThread 和 Bluetooth 的 DMP 功能。

将 FreeRTOS 组件添加到项目时，Project Configurator 会自动地添加基于 CMSIS RTOS2 的适配层，这是在 FreeRTOS 上运行 OpenThread 和 Bluetooth 栈所必需的。OpenThread 和 Bluetooth 的适配文件位于以下的 Simplicity Studio 5 位置：

* developer/sdks/gecko\_sdk\_suite/\<version\>/protocol/openthread/platform-abstraction/rtos/sl\_ot\_rtos\_adaptation.c
* developer/sdks/gecko\_sdk\_suite/\<version\>/protocol/bluetooth/src/sl\_bt\_rtos\_adaptation.c

该项目的三个应用程序源文件（唯一不属于 Gecko Platform 组件的源文件）存储在项目的顶层并命名为：

* main.c
* app.c
* bluetooth\_event\_callback.c

## 4.1 The Main Function and Initialization

ot-ble-dmp 应用程序使用与其他 OpenThread 示例应用程序相同的 main 函数定义。对在 `sl_system_init.c` 中定义的 `sl_system_init()` 的调用会初始化整个系统，其包括了对负责创建 Bluetooth 和 OpenThread 线程的 `sl_bt_rtos_init()` 和 `sl_ot_rtos_init()` 的调用。

应用程序可以使用 `app_init()` 函数在启动内核之前执行任何必要的初始化步骤。对 `sl_system_kernel_start()` 的调用会启动 FreeRTOS 调度器。

OpenThread 实例化和 CLI 初始化由 OpenThread initialization thread 通过调用 `sl_ot_init.c` 中定义的 `sl_ot_init()` 来处理。下一节将提供有关不同线程及其优先级的更多详细信息。

## 4.2 FreeRTOS Tasks

ot-ble-dmp 应用程序默认创建以下五个 RTOS 线程：

* OpenThread initialization thread (priority 53)
* OpenThread main thread (priority 24)
* Bluetooth link layer thread (priority 52)
* Bluetooth stack event thread (priority 51)
* Bluetooth event handler thread (priority 50)

OpenThread 线程在 `sl_ot_rtos_adaptation.c` 中创建，Bluetooth 任务在 `sl_bt_rtos_adaptation.c` 中创建。

如前所述，OpenThread initialization thread 处理 OpenThread 实例化和 CLI 初始化。由于 Bluetooth 事件回调 `sl_bt_on_event()` 利用了 OpenThread CLI 进行打印（在 [4.3 Handling Bluetooth Events](#43-handling-bluetooth-events) 中讨论），所以 OpenThread initialization thread 以最高优先级启动。

初始化完成后，initialization thread 还会为 OpenThread 创建 main (operating) thread。默认情况下，main thread 使用一个比 Bluetooth 低的优先级，从而使 Bluetooth 线程接管。

各种 Bluetooth 线程和 OpenThread main thread 的优先级是可配置的，并在 `sl_bt_rtos_config.h` 和 `sl_openthread_rtos_config.h` 中进行定义。

Silicon Labs Bluetooth 有一个串行化的 API，允许以线程安全的方式在 RTOS 任务之间传递命令和事件。OpenThread 没有串行化的 API。因此，应用程序逻辑在 OpenThread 任务中运行最为方便。为此提供了一个应用程序滴答回调，并从 OpenThread 任务的运行循环中调用：`sl_ot_rtos_application_tick()`。ot-ble-dmp 应用程序在 app.c 文件中包含了该滴答回调的简单实现。

从应用程序滴答中调用 OpenThread API 是线程安全的，因为它们是在 OpenThread 任务中执行的。因为 Bluetooth API 是串行化的，所以可以从任何任务中调用 Bluetooth API。Bluetooth 任务负责消费和处理这些串行化事件。这对应用程序来说是透明的。

## 4.3 Handling Bluetooth Events

Bluetooth 事件通过 `sl_bt_on_event()` 回调分派到应用程序。对于 ot-ble-dmp 应用程序，此回调的实现位于 bluetooth\_event\_callback.c 中。此示例处理程序只是打印出有关事件的一些信息。在实际的应用程序中，这些事件将由应用处理程序处理。

Bluetooth 事件在专用的 Bluetooth Event Handler thread 中进行处理。这是一个单独的线程，其唯一目的是检查正在等待的 Bluetooth 事件，并在它们可用时调用 `sl_bt_on_event()`。该线程在初始化期间自动创建。

## 4.4 Power Manager Integration

ot-ble-dmp 应用程序还包含了 Power Manager 组件（在 Platform > Service > System 下），它负责在可能的情况下让系统进入睡眠状态。

Power Manager 组件包含了无缝的 FreeRTOS 集成。它从 FreeRTOS Idle Task 中自动运行。当所有线程都挂起（因为它们正在等待某个事件以继续处理）时，Idle Task 运行，然后电源管理器的代码可以让系统进入睡眠状态。

应用程序通过 API 调用 `sl_power_manager_add_em_requirement()` 和 `sl_power_manager_remove_em_requirement()` 添加和移除能量需求，从而通知电源管理器它想要什么睡眠级别。添加一个 EM1 要求会告诉电源管理器允许的最低能量级别是 EM1，这只会使处理器空闲并且不会进入睡眠状态。移除 EM1 要求允许电源管理器进入能量级别 EM2，这是深度睡眠。请参阅 [https://docs.silabs.com/](https://docs.silabs.com/) 上 MCU 参考的 Modules>Platform Services>Power Manager。

# 5 OpenThread Sleepy End Device Demo

ot-ble-dmp 应用程序首先在初始化期间在 `sl_ot_rtos_application_init()` 回调中添加 EM1 要求。这可以防止设备进入 EM2 睡眠模式，以便 CLI 响应并且用户可以输入命令。

要演示一个 OpenThread Sleepy End Device，首先按照以下说明形成一个双节点 OpenThread 网络：[https://github.com/openthread/openthread/tree/master/examples/apps/cli](https://github.com/openthread/openthread/tree/master/examples/apps/cli)。

接下来，在已加入网络的设备（非 leader）上，键入以下命令：

```
> mode s
> pollperiod 1000
```

`mode` 命令使设备进入睡眠孩子模式。`pollperiod` 命令告诉孩子每秒发送一次数据轮询。此时孩子还没有睡眠，CLI 仍然有响应。

按下 WSTK 开发板上的按钮 PB0 或 PB1 将切换能量模式要求。具体来说，第一次按下按钮时，EM1 要求将被移除，这允许 EM2 睡眠。孩子将在数据轮询之间以 EM2 模式开始睡眠，并且 CLI 将不再响应。您可以通过从 leader 节点发送 ping 来验证孩子是否仍然能够发送和接收消息。由于孩子的睡眠周期，最多会有一秒钟的延迟。再次按下任一按钮将添加回 EM1 要求，这将使设备脱离 EM2，以便可以使用 CLI。

要在执行上述步骤时监视设备的功耗，请使用 Simplicity Studio 中的 Energy Profiler 工具连接到设备并开始能量捕获。有关 Energy Profiler 的更多信息，请参阅 *UG343: Multi-Node Energy Profiler User’s Guide*。
