# Getting Started with Simplicity Studio 5 and the Gecko SDK (Rev. 5.4.2) <!-- omit in toc -->

- [先决条件](#先决条件)
  - [SSv5 系统要求](#ssv5-系统要求)
  - [硬件](#硬件)
    - [EFR32 Kits](#efr32-kits)
  - [客户帐户](#客户帐户)
- [安装 SSv5 和软件](#安装-ssv5-和软件)
  - [安装 SSv5](#安装-ssv5)
    - [在 Windows 和 MacOS 上](#在-windows-和-macos-上)
    - [在 Linux 上](#在-linux-上)
      - [注意事项](#注意事项)
  - [安装软件](#安装软件)
    - [按连接的设备安装软件](#按连接的设备安装软件)
    - [按技术类型安装软件](#按技术类型安装软件)
- [探索 SSv5](#探索-ssv5)
- [开始一个项目](#开始一个项目)
  - [项目创建](#项目创建)
    - [Target, SDK, and Toolchain Selection](#target-sdk-and-toolchain-selection)
    - [Examples](#examples)
    - [Configuration](#configuration)
  - [Project Configurator 项目](#project-configurator-项目)
    - [Pin Tool](#pin-tool)
    - [Bluetooth GATT Configurator](#bluetooth-gatt-configurator)
    - [Bluetooth Mesh Configurator](#bluetooth-mesh-configurator)
    - [Memory Editor](#memory-editor)
    - [Proprietary Radio Configurator](#proprietary-radio-configurator)
    - [Wi-SUN Configurator](#wi-sun-configurator)
    - [Zigbee Cluster Configurator](#zigbee-cluster-configurator)
    - [AppBuilder Projects](#appbuilder-projects)
- [安装 SDK 扩展](#安装-sdk-扩展)
- [项目迁移](#项目迁移)
  - [转移到 Component-Based 架构](#转移到-component-based-架构)
  - [从 Simplicity Studio 4 迁移到 Simplicity Studio 5](#从-simplicity-studio-4-迁移到-simplicity-studio-5)
    - [Bootloader、EFM32 和 EFM8 项目](#bootloaderefm32-和-efm8-项目)
    - [Z-Wave 项目](#z-wave-项目)
    - [Zigbee、Flex 和 Bluetooth/Bluetooth Mesh 项目](#zigbeeflex-和-bluetoothbluetooth-mesh-项目)

要开始使用 Simplicity Studio® 5（SSv5）：

<!-- no toc -->
1. [安装 SSv5 和开发软件](#先决条件)
2. [探索 SSv5 的主要特性](#安装-ssv5-和软件)
3. [开始一个项目](#开始一个项目)

安装 SSv5 及相关软件包或者探索针对 32-bit 设备的特定 SDK 的项目配置界面时不需要硬件。但是，在安装时连接着目标硬件可确保精确地配置 SSv5 的安装。

您还应该在 Silicon Labs Customer Support Portal 中建立一个帐户。您的客户资料决定了您对各种软件包的访问权限。

有关详细信息，请参阅[先决条件](#先决条件)。

如果您有作为 SDK 扩展提供的特性或功能，则需要在安装主 SDK 后单独安装。有关详细信息，请参阅[安装 SDK 扩展](#安装-sdk-扩展)。

# 先决条件

要简化您的 Simplicity Studio® 5（SSv5）安装：

* 将[开发硬件](#硬件)连接到您的 PC
* 登录您的[客户帐户](#客户帐户)

## SSv5 系统要求

操作系统

| Operating System | Version                                        |
| :--------------- | :--------------------------------------------- |
| Windows          | Windows 10 (x64)                               |
| macOS            | 10.24 Mojave or 10.25 Catalina (GNU toolchain) |
| Linux            | Ubuntu 20.04 LTS                               |

硬件

| Hardware Component | Item                                                                                       |
| :----------------- | :----------------------------------------------------------------------------------------- |
| CPU                | 1 GHZ or better                                                                            |
| Memory             | 1 GB RAM minimum, 8 GB recommended for Wireless Protocol development                       |
| Disk Space         | 600 MB disk space for minimum FFD installation; 7 GB for Wireless Dynamic Protocol support |

## 硬件

要开始使用 Simplicity Studio 并不需要 Silicon Labs 开发套件。但是，如果 Simplicity Studio 检测到官方开发套件，则可以使用它来帮助您选择合适的软件和工具进行安装。如果您没有开发套件，那么您可以在[此处](https://www.silabs.com/development-tools.p-microcontrollers.p-wireless)探索可用的选项。

### EFR32 Kits

如果您有 Silicon Labs Wireless Starter Kit（WSTK）或 Pro Kit，那么请您在主板（mainboard）上安装无线板（radio board），并使用套件随附的 USB 电缆将主板连接到您的 PC。

> 注意：为了在 SSv5 中获得最佳性能，请确保主板上的电源开关处于 Advanced Energy Monitoring 或 “AEM” 处。<br>![WSTK mainboard with the power switch in the AEM position](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-wstk-switch.png)

## 客户帐户

如果您还没有账户，那么请您在 Silicon Labs Customer Support Portal 上创建一个帐户。请务必记住账户的用户名和密码，因为您将使用它在 SSv5 上进行登录。您的帐户属性不仅决定了您可以下载哪些软件组件，还决定了您将收到哪些通知。

要查看或更改您的通知订阅，请登录门户，点击 **HOME** 以转到门户主页，然后点击 Manage Notifications 磁贴。请确保您勾选了 **Software/Security Advisory Notices & Product Change Notices (PCNs)** ，并且至少订阅了您的平台和协议。Silicon Labs 强烈建议您接收这些通知。点击 **Save** 以保存任何更改。

# 安装 SSv5 和软件

本节讨论[如何安装 SSv5](#安装-ssv5)，以及安装 SSv5 后[如何安装软件](#安装软件)。

> 注意：根据您的公司环境，您可能会遇到安装问题。例如，如果您无法登录 Simplicity Studio® 5（SSv5）但知道您输入的客户帐户用户名和密码正确，或者您在连接到更新服务器时收到错误消息并且您的计算机确实已连接到互联网，请参阅 [SDK installation or updates are not working](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-tips-and-tricks/#sdk-installation-or-updates-are-not-working) 以了解可能的原因及解决方案。

## 安装 SSv5

指示[在 Windows 和 MacOS](#在-windows-和-macos-上) 以及 [Linux 上](#在-linux-上)的安装说明。

### 在 Windows 和 MacOS 上

<ol>
    <li>
        <p>
            从 Silicon Labs 网站下载 SSv5 安装包。Windows 软件包是一个 “.iso” 磁盘映像。软件包下载完成后，双击它以将 iso 映像挂载为一个驱动器，然后双击驱动器内的 setup.exe 文件以启动安装程序。
        </p>
    </li>
    <li>
        <p>
            当 SSv5 安装程序首次启动时，它会显示一个 Simplicity Studio License Agreement 对话框。接受协议条款并点击 <strong>Next &gt;</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-license-1.png" alt="SSv5 License agreement with default do not accept selected">
        </p>
    </li>
    <li>
        <p>
            选择目标位置，点击 <strong>Next &gt;</strong>，然后点击 <strong>Install</strong>。
        </p>
    </li>
    <li>
        <p>
            当 SSv5 启动时，您会看到一个或多个附加许可协议。您可以单独地或全部一次性地接受它们。完成后点击 <strong>DONE</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-eclipse-license.png" alt="Review Licenses Dialog showing the Eclipse Foundation Software User Agreement">
        </p>
    </li>
    <li>
        <p>
            接下来，您将被邀请登录。使用您的 Silicon Labs 帐户用户名和密码登录。<strong>NOTE</strong>：虽然您可以在此处跳过登录，但您必须先登录才能访问某些访问受限的软件包。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-login.png" alt="Welcome to Simplicity Studio log in dialog">
        </p>
    </li>
    <li>
        <p>
            在登录完成之前，您必须同意 Experience Tracking。您可以稍后在 SSv5 Preferences &gt;&gt; Simplicity Studio &gt;&gt; User Experience 中更改状态。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-experience-tracking.png" alt="The experience tracking dialog with the OK control only">
        </p>
    </li>
    <li>
        <p>
            如果您连接了硬件，根据您的操作系统和安全设置，您可能需要允许 SSv5 对您的系统进行更改。SSv5 会提示您安装 Device Inspector 。点击 <strong>Yes</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-device-inspector.png" alt="Device inspector dialog">
        </p>
    </li>
</ol>

### 在 Linux 上

Simplicity Studio for Linux® OS 仅在 Ubuntu® LTS 发行版上得到官方支持。Simplicity Studio 可能可以在其他发行版上工作，但官方仅在 Ubuntu® LTS 上对其进行过测试和验证。

基本的 Simplicity Studio 平台（非安装程序）以 tar 文件的形式提供，该文件应展开到所需的安装目录中。推荐的安装目录是用户的根目录（`/home/USERNAME/SimplicityStudio_v5`）。此目录必须是可以在没有 root 权限的情况下更新的目录。

在第一次启动 Simplicity Studio 之前，请执行以下命令：

```bash
sudo apt-get update
sudo apt-get upgrade
cd SimplicityStudio_v5
sudo ./setup.sh
```

有关 setup.sh 脚本的更多信息，请参阅[注意事项](#注意事项)。

当 Simplicity Studio 可执行文件 “studio” 启动时，它会启动首次安装过程，以按[连接的调试适配器](#按连接的设备安装软件)或[技术类型](#按技术类型安装软件)添加支持。

#### 注意事项

**setup.sh** 会：

1. 尝试为 USB 设备连接安装 dev 规则
2. 通过系统的包管理器安装缺少的包（库）。

setup.sh 只会在 Ubuntu® 发行版上安装所需的外部 Linux® 软件包。对于其他发行版，应检查 setup.sh 脚本以获取可能需要安装以使 Simplicity Studio 工作的软件包列表。

如果正在使用 **Wayland 显示协议** ，请在终端窗口中使用 `./studiowayland.sh` 启动 Simplicity Studio。

**JxBrowser**：Simplicity Studio 使用第三方产品 JxBrowser 来渲染一些窗口，包括安装窗口、Launcher 透视图和 Project Configurator 窗口。默认情况下，JxBrowser 将 Chromium 浏览器展开到 /tmp 文件夹。如果对 /tmp 文件夹的访问权限不足，则可以通过在 studio.ini 文件中添加一行来指定不同的路径，例如：`-Djxbrowser.chromium.dir=/home/USERNAME/.jxbrowser`。

## 安装软件

SSv5 安装完成后会提示安装 Gecko SDK（GSDK）等软件包。SSv5 提供了两个选项：

* [按连接的设备安装](#按连接的设备安装软件)
* [按技术类型安装](#按技术类型安装软件)

您无需连接目标设备即可按技术类型进行安装。

![Installation Manager dialog with Install by connecting device(s) selected](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-method.png)

### 按连接的设备安装软件

<ol>
    <li>
        <p>
            确保您已连接设备，然后点击 <strong>Install by Connecting Devices</strong>。这将显示已连接的设备。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-by-device.png" alt="Select Products dialog with one device shown">
        </p>
        <p>
            如果您没有看到已连接的设备，或者在显示此对话框后连接了设备，请点击 <strong>Refresh</strong>。如果您没有连接设备，则无法继续。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-by-device-no-devices.png" alt="Select Products dialog with no devices found">
        </p>
    </li>
    <li>
        <p>
            选择要使用的设备。如果您选择了多个具有不同软件兼容性的设备，Studio 将下载与任何设备兼容的任何软件包。点击 <strong>NEXT</strong>。
        </p>
    </li>
    <li>
        <p>
            您有两个安装选项，<strong>Auto</strong> 和 <strong>Advanced</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-package-options.png" alt="Package installation options dialog with Auto selected">
        </p>
        <ul>
            <li>
                <p>选择 <strong>Auto</strong> 让 SSv5 自动下载与连接的硬件兼容的并且是您的账户拥有访问权限的所有包。</p>
            </li>
            <li>
                <p>选择 <strong>Advanced</strong> 以选择要安装的软件包或指定 Gecko SDK 的安装位置</p>
            </li>
        </ul>
        <p>
            如果您选择 <strong>Advanced</strong>，则必须安装所有 “Required” 包。<strong>注意</strong>：除非您了解该选择的全部影响，否则不要排除包。点击 <strong>Read More</strong> 以查看每个项目的发行说明。可以选择更改 Gecko SDK（GSDK）的默认安装位置。GSDK 将作为 GitHub 存储库的克隆来安装。不要选择已包含其他内容的现有文件夹，因为您的克隆存储库将不再是干净的。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-package-advanced-device.png" alt="Select Development Packages dialog with all recommended packages selected">
        </p>
    </li>
    <li>
        <p>
            在开始安装软件包之前，请接受一份或多份软件许可协议。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-software-license.png" alt="Review Licenses dialog showing the Master software license agreement">
        </p>
    </li>
    <li>
        <p>
            点击 <strong>NEXT</strong> 后，安装开始。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-status.png" alt="Installation dialog with video and progress bar">
        </p>
        <p>
            根据您的选择，安装可能需要一些时间。进度条可能会在安装大文件的地方暂停，但会在它们完成后恢复。如果安装过程中出现问题，SSv5 会显示错误。您必须重新启动 SSv5 才能恢复。
        </p>
    </li>
    <li>
        <p>
            安装完成后，点击 <strong>CLOSE</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-complete-first.png" alt="installation dialog with installation complete notice">
        </p>
    </li>
    <li>
        <p>
            点击 <strong>RESTART</strong> 开始使用项目进行工作。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-complete-restart.png" alt="installation complete with restart control">
        </p>
    </li>
</ol>

### 按技术类型安装软件

<ol>
    <li>
        <p>
            点击 <strong>Install by Technology Type</strong>
        </p>
    </li>
    <li>
        <p>
            选择要安装的技术并点击 <strong>NEXT</strong>。要安装 Gecko SDK Suite，请选择 <strong>32-Bit and Wireless MCUs</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-by-technology.png" alt="Select technology type dialog with GSDK selected">
        </p>
    </li>
    <li>
        <p>
            您有两个安装选项，<strong>Auto</strong> 和 <strong>Advanced</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-package-options.png" alt="Package installation options dialog with Auto selected">
        </p>
        <ul>
            <li>
                <p>选择 <strong>Auto</strong> 让 SSv5 自动下载与连接的硬件兼容的并且是您的账户拥有访问权限的所有包。</p>
            </li>
            <li>
                <p>选择 <strong>Advanced</strong> 以选择要安装的软件包或指定 Gecko SDK 的安装位置</p>
            </li>
        </ul>
        <p>
            如果您选择 <strong>Advanced</strong>，则必须安装所有 “Required” 包。<strong>注意</strong>：除非您了解该选择的全部影响，否则不要排除包。点击 <strong>Read More</strong> 以查看每个项目的发行说明。可以选择更改 Gecko SDK（GSDK）的默认安装位置。GSDK 将作为 GitHub 存储库的克隆来安装。不要选择已包含其他内容的现有文件夹，因为您的克隆存储库将不再是干净的。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-package-advanced-technology.png" alt="Select Development Packages dialog with all recommended packages selected">
        </p>
    </li>
    <li>
        <p>
            在开始安装软件包之前，请接受一份或多份软件许可协议。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-software-license.png" alt="Review Licenses dialog showing the Master software license agreement">
        </p>
    </li>
    <li>
        <p>
            点击 <strong>NEXT</strong> 后，安装开始。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-status.png" alt="Installation dialog with video and progress bar">
        </p>
        <p>
            根据您的选择，安装可能需要一些时间。进度条可能会在安装大文件的地方暂停，但会在它们完成后恢复。如果安装过程中出现问题，SSv5 会显示错误。您必须重新启动 SSv5 才能恢复。
        </p>
    </li>
    <li>
        <p>
            安装完成后，点击 <strong>CLOSE</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-complete-first.png" alt="installation dialog with installation complete notice">
        </p>
    </li>
    <li>
        <p>
            点击 <strong>RESTART</strong> 开始使用项目进行工作。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/install-complete-restart.png" alt="installation complete with restart control">
        </p>
    </li>
</ol>

# 探索 SSv5

当打开 Simplicity Studio® 5（SSv5）时，首先显示的是 Launcher 透视图中的欢迎页面。“透视图（perspective）” 是 Eclipse 术语，表示视图和编辑器区域的排列。本页概述了 Launcher 透视图的主要部分。可以在 [About the Launcher](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-about-the-launcher/) 参考部分中找到通过 Launcher 透视图访问的所有功能的详细参考。

![Launcher perspective with editor and views numbered](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/launcher-start-numbered.png)

1. **编辑器（Editor）**。编辑器以欢迎模式开始。在这里，您将从选择一个部件（part）开始，然后可以发现资源并基于该部件创建项目。请注意，当您下拉 Learn and Support 区域时，您可以访问技术支持、Silicon Labs 社区和教育资源。
2. **调试适配器视图（Debug Adapters view）**：通过调试适配器展示出物理连接到计算机或在本地网络上检测到的设备。选择设备可以开始一个项目。
3. **我的产品视图（My Product view）**：在这里您可以添加设备（device），板（board）或套件（kit），然后像已连接的套件一样选择它们。之后，您可以探索资源或为所选（目标）设备创建和配置项目。
4. **菜单（Menu）** 和 **工具栏（Toolbar）**：工具栏上提供了许多值得注意的主要功能。

![Launcher Toolbar - Home, Recent, Tools, Install, Preferences](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/launcher-toolbar.png)

* **Welcome** 用于返回到 Launcher Welcome 页面。
* **Recent** 用于显示最近项目的列表。在 Simplicity IDE Project Explorer 视图中选择一个以转到该项目。
* **Tools** 提供了可用工具的列表。
* **Install** 将弹出一个菜单，您可以在其中安装或卸载软件包和工具，或查看可用更新。
* **Preferences** 是首选项列表的快捷方式，与通过菜单选择 Window > Preferences 等价。

在整个 SSv5 中，launcher 和其他透视图的内容取决于您选择作为开发目标的设备以及您的软件环境。首次打开 SSv5 时，Launcher 透视图没有目标设备上下文。在连接套件时，它会同时出现在调试适配器视图和 **Get Started** 编辑器区域的设备选择器中。

![The selected device area of the Get Started editor showing a connected device](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/launcher-part-selected-focus.png)

如果您没有可连接的 Silicon Labs 套件，但想探索 SSv5 的更多功能，请点击 **All Products**，然后输入套件、板或设备的部件号。随着您的键入，您将看到一个列表。使用复选框可以过滤列表。

![The selected device area of the Get Started editor showing all products](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/launcher-virtual-part-focus.png)

您还可以将产品添加到 My Products 视图中，并在那里选择目标设备。如果您在 **Get Started** 下添加了产品，则点击 **Start** 后，所选产品将添加到 My Products 视图中，以便在 Welcome 页面或其他 Launcher 透视视图中轻松访问。

连接或选择目标设备后，点击 **Start** 开始移动到 Launcher 透视图的 OVERVIEW 选项卡，这将显示有关该设备的详细信息。

![launcher perspective with device-specific functions](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/launcher-selected.png)

您会在 OVERVIEW 选项卡上找到几张 “卡片”。第一个是 **General Information** 卡片，您可以在其中更新 Silicon Labs 套件的调试适配器固件、更改调试适配器的模式、管理目标设备的安全设置（如果适用）以及更改活动/首选 SDK。

**Recommended Quick Start Guides** 卡片突出显示了可用于所选目标设备的一些入门资源。点击 **All Quick-Start Guides** 可快速转到 DOCUMENTATION 选项卡，该选项卡已预先过滤为快速入门指南。所选目标的每个板和设备也有可用的卡片。每个硬件卡片上的 **View Documents** 下拉列表提供了该项目的硬件文档的过滤视图。

您可以使用 **Create New Project** 控件开始一个项目。有关创建项目的介绍，请参阅[开始一个项目](#开始一个项目)。

EXAMPLE PROJECTS & DEMOS 选项卡展示与所选设备兼容的示例项目和演示列表。演示是预构建的软件示例，可以加载到兼容设备中并用于展示和测试应用程序的功能。每个演示都附带一个相关的示例项目。有关提供的示例和演示的更多信息，请参阅您的 SDK 的 Quick Start Guide。

使用复选框和搜索框可以过滤列表。有许多不同的过滤器类别可用。显示的类别和过滤器取决于您所选择的设备。点击任何演示上的 **RUN** 可以将其安装在目标设备上。在任何项目上点击 **CREATE** 可以创建它。这等效于从 OVERVIEW 选项卡中创建项目，不同的只是该项目已被选中。

![the demos & example projects tab with technology filter applied](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/launcher-tab-demos-and-examples.png)

DOCUMENTATION 选项卡展示与所选设备兼容的所有文档。使用复选框或文本过滤器字段来查找感兴趣的资源。与您的开发环境相对应的技术过滤器将向您展示与该环境相关的大多数软件文档。

![the documentation tab with no filters applied](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/launcher-tab-documentation.png)

COMPATIBLE TOOLS 选项卡展示与所选设备兼容的工具。工具栏上的 Tools 按钮展示所有未过滤的工具。

![the compatible tools tab](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/launcher-tab-tools.png)

# 开始一个项目

Simplicity Studio® 5（SSv5）支持多种不同的项目类型，可以通过 **File > New** 创建。这些包括：

* Silicon Labs Project Wizard（创建 Project Configurator 项目，如本节所述）
* [Solution...](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-project-configurator/project-solutions)
* Project...（打开 New Project Wizards 对话框）
* Other（将上述所有选项与其他很少使用的选项结合起来）

选择 **Files > New > Project** 以打开 New Project Wizards 对话框。

![New project wizards](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-wizards.png)

最值得注意的是：

* Silicon Labs Project Wizard（两个实例）以及 Silicon Labs MCU Project Wizard：创建 Project Configurator 项目，如本节所述。
* Direction Finding Project Wizard：创建 [Bluetooth multi-locator direction-finding projects](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-direction-finding-tools/)。
* AppBuilder Project Wizard：创建 [AppBuilder Project](https://docs.silabs.com/d/0.4/ss-5-users-guide-developing-with-appbuilder)。

本页重点介绍 Project Configurator（\*.slcp）项目。

## 项目创建

通过一系列的三个对话框可以创建新的 Simplicity Studio® 5（SSv5）项目：

* [Target, SDK, and Toolchain](#target-sdk-and-toolchain-selection)
* [Examples](#examples)
* [Configuration](#configuration)

![The three project creation dialogs in sequence](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-3-dialogs-bigger.png)

对话框顶部的指示器显示您所在的位置。

![the sequence indicator positioned on target SDK](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-place-indicator.png)

您可以从 Launcher Perspective 中的三个不同位置开始一个项目。你从哪里开始将决定你将到达三个对话框中的哪一个。如果您需要进行更改，请点击 **BACK** 以移至先前的对话框。

* **从 OVERVIEW 选项卡中**：点击 **Create New Project**。从 Examples 对话框开始。
* **从 EXAMPLE PROJECTS 选项卡中**：根据需要使用过滤器，选择一个项目，然后点击 **CREATE**。从 Project Configuration 对话框开始。
* **从文件菜单中**：选择 New >> Silicon Labs Project Wizard。从 Target, SDK, and Toolchain Selection 对话框开始。

开始后，您可以保留默认值。

### Target, SDK, and Toolchain Selection

如果您已连接或选择了目标，则所有信息都已预先填充好了。否则，您可以在此处选择目标部件。点击 **NEXT**。

![Target, SDK, and Toolchain selection dialog, with the default GNU ARM toolchain](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-target.png)

请注意，如果您想在 SSv5 中使用 IAR，请在此处选择它。如果您在 Project Configurator 环境中进行开发，那么一旦创建了项目，就很难更改编译器了。

### Examples

使用复选框或关键字查找感兴趣的示例，然后选择它并点击 **NEXT**。

![New Project examples filtered by Proprietary Flex](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-examples.png)

### Configuration

如果需要，请重命名您的项目。“With project files” 下的三个选项控制复制哪些和链接哪些源。如果修改了链接源，则更改将应用​​于链接到该源的任何其他项目。

* 链接到源（Link to sources）：该项目将链接到 SDK 文件和项目文件，例如 app.c。
* 链接 SDK 并复制项目源（Link SDK and copy project sources）（默认）：将项目源复制到您的工作区。请注意，这将创建 SDK 文件夹结构，但如果您向下深探，您将会看到所有文件夹都是空的。
* 复制内容（Copy contents）：项目文件和 SDK 文件都复制到您的工作区。

点击 **FINISH**。

![New project configuration linking to the sdk and copying project source](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-config.png)

完成项目创建后，Simplicity IDE 透视图将打开。初次配置可能会稍有延迟。有关此透视图中可用的所有特性和功能的详细信息，请参阅 [About the Simplicity IDE](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-about-the-simplicity-ide/)。

![simplicity IDE with editor and views numbered](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-ide-numbered.png)

1. **编辑器区域（Editor area）**（取决于项目）。
2. **项目资源管理器视图（Project Explorer view）**：列出工作区中可用的项目和解决方案。
3. **调试适配器视图（Debug Adapters view）**：列出通过 USB 连接到您的计算机或在本地网络上检测到的套件或 SEGGER J-Link。
4. **开发者视图（Developer views）**：开发过程中使用的一组视图。

Simplicity IDE 透视图中的编辑器取决于项目：

* [Project Configurator](#project-configurator-项目)：用于从 Gecko SDK 4.0 开始的所有 Gecko SDK 协议和工具；项目文件以 .slcp 结尾。
* [8-bit Hardware Configurator](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-for-8bit-devices/using-hardware-configurator)：用于 8-bit 设备应用程序。
* [AppBuilder](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-getting-started/start-a-project#appbuilder-projects)：用于 Gecko SDK 3.2 及更低版本中的 Zigbee EmberZNet 和 Gecko Bootloader；项目文件以 .isc 结尾。

## Project Configurator 项目

本页除了介绍 Project Configurator 项目外，还介绍了：

* [Pin Tool](#pin-tool)
* Bluetooth 和 Bluetooth Mesh SDK 的 [GATT Configurator](#bluetooth-gatt-configurator)
* Bluetooth Mesh SDK 的 [Bluetooth Mesh Configurator](#bluetooth-mesh-configurator)
* Proprietary SDK 的 [Radio Configurator](#proprietary-radio-configurator)
* Zigbee EmberZNet SDK 的 [Zigbee Cluster Configurator](#zigbee-cluster-configurator)
* Wi-SUN SDK 的 [Wi-SUN Configurator](#wi-sun-configurator)

注意：[Memory Editor](#memory-editor) 是一个主要用于 [solutions](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-project-configurator/solutions) 的工具的 beta 版实现。

Project Configurator 项目在 **.slcp**（Silicon Labs Configurator project）文件中定义。用户可以通过在 Software Components 选项卡上添加、删除和配置组件来修改项目。有关详细信息，请参阅 [Developing with Project Configurator](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-project-configurator/)。

注意：从 Simplicity Studio 5.3 版开始，您可以通过导入 .slcp 文件在 Simplicity IDE 中创建项目。有关详细信息，请参阅 [Import and Export](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-about-the-simplicity-ide/import-and-export)。

项目通常在包含示例项目描述的 README 选项卡上或在 OVERVIEW 选项卡上打开。

![Project configurator with overview tab selected](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-project-config-general.png)

OVERVIEW 选项卡包含三张带有信息的卡片，其中一些带有可以更改的设置：

* **Target and Tool Settings**，您可以在其中更改开发目标、SDK 和项目生成器。向下滚动并点击 **Change Target/SDK/Generators** 可以编辑这些设置。项目生成器配置决定了 SSv5 在您配置项目时生成的 IDE 或构建系统项目文件。（注意：Simplicity IDE 使用的编译器/工具链可在 **Project > Build Configurations** 中配置。默认 IDE 可在 **Preferences > Simplicity Studio > Preferred IDE** 中配置。）
* **Project Details**，您可以在其中重命名项目，更改项目源导入模式，并在必要时强制生成项目和源文件（在 autogen 文件夹中）。
  * **Import mode** 控制复制哪些资源以及链接哪些资源。如果您修改了链接的源，您的更改将应用​​于链接到该源的任何其他项目。
  * **Force Generation** 在极少数未触发自动生成的情况下使用，这通常是因为在 SSv5 之外进行了一些更改，例如编辑 .slcp 文件。
* **Quick Links** 提供了指向通常用于修改项目或较低级别配置（例如无线电或外围连接）的工具的链接。链接因 SDK 和目标设备而异。

![Project Configurator Overview tab with the three cards](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-project-config-general-edit.png)

要通过组件库配置项目，请点击 SOFTWARE COMPONENTS 选项卡。许多过滤器和关键字搜索可帮助您探索各种组件类别。请注意，所有已安装 SDK 的组件均已显示。

![Project Configurator with Software Components tab open](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-project-config-components.png)

展开组件类别\子类别以查看各个组件。项目中已安装的组件会被选中（1），并且可以被卸载。可配置组件由齿轮符号（2）表示。

![Software Components tab with a checked project and a configurable project in the left panel indicated by numbers](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-project-config-component-detail.png)

点击组件名称旁边的齿轮符号或可配置组件描述中的 **Configure** 以打开 Component Editor。您可以在此处更改参数或直接编辑组件源。

![Component editor open on the Bluetooth Core component](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-project-config-component-editor.png)

在 Component Editor 中的变改会自动保存。

![Component editor with saved changes](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-config-component-saved.png)

当您在 Project Configurator 中进行更改时，例如安装或卸载组件，项目输出文件会自动生成。进度显示在透视图的右下方。

![Generation and indexing progress bars](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-generate.png)

速度根据您的系统而异。在构建应用程序映像之前，请确保生成已完成。

构建应用程序映像并将其刷写到您的目标设备，如 [Building and Flashing](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-building-and-flashing/) 中所述。

CONFIGURATION TOOLS 选项卡提供了一种在工具的选项卡尚未打开时打开工具的简便方法，作为常规选项卡上 Quick Links 卡片的替代方法。它展示了与项目类型相关的配置工具。 例如，Bluetooth Mesh 项目展示了许多工具，而 OpenThread 项目可能只显示 Pin Tool。点击工具卡上的 **Open** 以在单独的选项卡中打开它。

![Project Configurator Configuration tools tab](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-project-config-configuration-tools.png)

### Pin Tool

[Pin Tool](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-project-configurator/pin-tool) 允许您修改目标设备的引脚使用和参数。除了通过 CONFIGURATION TOOLS 选项卡打开 Pin Tool，您还可以在 Project Explorer 视图中双击 \<project\>.pintool 文件。

![Pin tool tab](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-pin-tool.png)

双击 Software Component 以打开 Component Editor 并配置该功能。Pin Tool 不会自动保存。

### Bluetooth GATT Configurator

Bluetooth 和 Bluetooth Mesh 项目都使用 [Bluetooth GATT Configurator](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-project-configurator/bluetooth-gatt-configurator) 进行配置。

![GATT configurator tab](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-gatt-configurator.png)

Bluetooth GATT Configurator 菜单允许您添加和删除服务（service）和特征（characteristic）。

![GATT configurator menu with numbered controls](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-gatt-configurator-menu.png)

1. 添加一个项。
2. 复制所选项。
3. 向上移动所选项。
4. 向下移动所选项。
5. 导入 Bluetooth GATT 数据库。
6. 添加预定义。
7. 删除所选项。

要添加自定义服务，请点击 **Profile (Custom BLE GATT)**，然后点击 **Add**（1）。要添加自定义特征，请选择一项服务，然后点击 **Add**（1）。要添加预定义的服务/特征，请点击添加预定义 **Add Predefined**（6）。

### Bluetooth Mesh Configurator

Bluetooth Mesh 项目 Device Composition Data 通过 [Bluetooth Mesh Configurator](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-project-configurator/bluetooth-mesh-configurator) 进行配置。Device Composition Data 呈现在三个方面：设备信息、元素和模型。

![Mesh configurator tab](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-mesh-configurator.png)

设备信息由在第一个字段中选择的公司确定。

每个节点至少有一个主要元素。要添加更多元素，请点击右下角的绿色 + 符号。

Bluetooth Mesh Configurator 具有 SIG-adopted 模型和 vendor 模型的编辑器。提供了 SIG-adopted 模型组件，但其无法编辑。然而，您可以删除这些模型，然后添加模型以满足您的需要。要删除模型，请选择它并点击红色 X 符号。要添加 SIG-adopted 模型，请将模型从左侧模型池拖到正确元素中的 SIG 模型表中。列表将显示所有 SIG-adopted 模型，您可以选择所需的模型。

![SIG-Adopted Model Editor](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/mesh-conf-sig-adopted-model-editor.png)

在开发 SIG-adopted 模型未涵盖的产品时，vendor 模型为您提供了更大的灵活性。供应商可以在这些 vendor 模型中定义自己的规范，包括状态、消息和相关行为。Vendor 模型编辑器如下图所示。

![Vendor Model Editor](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/mesh-conf-vendor-model.png)

### Memory Editor

Memory Editor 是一个图形工具，用于编辑工作区中应用程序的内存布局。它是该工具的 beta 版实现，旨在与 [solutions](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-project-configurator/project-solutions) 一起使用。在此版本中，它可以用于单个项目。大多数项目此时没有包含默认设置，这将导致出现下图所示的警告。将工具计算的默认值保存下来以作为起点。

![Memory Editor RAM view](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-memory-editor.png)

### Proprietary Radio Configurator

[Radio Configurator](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-project-configurator/proprietary-radio-configurator) 作为 Proprietary SDK 的一部分提供。使用 Radio Configurator 可为 RAIL-based 无线电应用程序创建标准或自定义的无线电配置。

![Software components tab with the Radio Configurator selected](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/radio-conf-start.png)

Radio Configurator 中的参数以卡片形式排列，其中一些组合在一起。不同的无线电配置文件提供不同的视图和参数集。

![Radio Configurator showing general settings and channels overview cards](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/radio-conf-parameter-tiles.png)

1. 在 General Settings 卡片中，可以在 **Select radio profile** 下拉菜单中选择一个无线电配置文件。无线电配置文件可以是任何受支持的无线电链路技术。这些技术可以受标准（例如 Sigfox 或 WMBus 协议）的约束，也可以完全自定义。完全可自定义的配置文件称为 “Base Profile”。
2. 在 **Select a radio PHY** 下拉列表中可以选择无线电 PHY（无线电配置）。每个配置文件都有可供使用的 “内置” 配置。
3. 查看并更新配置文件选项。默认情况下，是不允许更改的；字段显示为灰色。要启用自定义，请使用 General Settings 卡片上的 **Customized** 开关。这允许您访问配置文件定义的所有参数。

![General settings card showing the customized switch](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/radio-conf-customize-slider.png)

### Wi-SUN Configurator

[Wi-SUN Configurator](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-project-configurator/wi-sun-configurator) 作为 Wi-SUN SDK 的一部分提供。使用它通过三个面板配置 Wi-SUN 应用程序的主要设置：Application、Security 和 Radio。对于某些项目，只有 Radio 面板可用。Wi-SUN Configurator 选项卡在创建项目时可用，或者可以通过打开项目文件 /config/wisun/wisun_settings.wisunconf 来显示。

![Wi-SUN Configurator application tab](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-wisun-configurator.png)

### Zigbee Cluster Configurator

[Zigbee Cluster Configurator](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-project-configurator/zigbee-cluster-configurator) 作为 Zigbee EmberZNet SDK 的一部分提供。使用它来管理 Zigbee 端点（endpoint）、簇（cluster）和命令（command）。该接口基于添加或修改端点。

![Zigbee Cluster Configurator with an endpoint selected](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/zcc-endpoint.png)

您可以根据需要添加和配置簇。簇配置界面由三个选项卡组成：

* Attributes
* Attribute Reporting
* Commands

![Zigbee Cluster Configurator Clusters interface](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/zcc-cluster-tabs.png)

通过 Zigbee Cluster Configurator 所做的配置更改会被保存到 zcl_config.zap 文件中。当您保存文件时，Zigbee Cluster Configurator 不仅会将 .zap 文件保存到您的项目中，还会自动生成您的 Zigbee 应用程序所需的所有 .c 和 .h 文件。

### AppBuilder Projects

AppBuilder Projects 是通过修改各种选项卡中的参数来配置的，尤其是 PLUGINS 选项卡。有关详细信息，请参阅使用 [Developing with AppBuilder](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-appbuilder/)。

![Simplicity IDE with the AppBuilder editor](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-appbuilder.png)

配置项目后，点击 **Generate** 以创建项目文件。如 [Building and Flashing](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-building-and-flashing/) 中所述构建应用程序映像并将其刷写到您的目标设备。

![AppBuilder editor with Generate control highlighted](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-appbuilder-generate.png)

要修改目标设备的引脚使用和参数，请使用 HAL 选项卡上提供的 [Hardware Configurator](https://docs.silabs.com/simplicity-studio-5-users-guide/latest/ss-5-users-guide-developing-with-appbuilder/configuring-peripherals)。请注意，虽然界面类似于 8-bit Hardware Configurator，但这是一个不同的工具。

![general tab with Hardware Configurator Interface highlighted](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/new-project-hardware-configurator.png)

通过 Hardware Configurator 所做的更改存储在一个特定于板的 .hwconf 文件中。

# 安装 SDK 扩展

SDK 扩展是特定于使用 Project Configurator 和其他 Silicon Labs Configurator (SLC)-based 工具开发 32-bit 设备的实体。它是组件和其他项目（例如示例文件）的集合。SDK 扩展依赖于父 SDK（必须先安装好）。SDK 扩展可用于控制对某些功能的访问，或包含客户创建的组件和其他项目，与 Silicon Labs SDK 分开维护。

**注意**：Silicon Labs' HomeKit SDK 需要作为 SDK 扩展安装，因为它仅适用于已签署 Made For iPhone (MFi) 协议的已授权 Apple 开发人员。您必须登录到您的 Silicon Labs 客户帐户才能查看和下载 HomeKit SDK。

如果您有可用的扩展，请使用以下过程安装它。

<ol>
    <li>
        <p>
            从工具栏上的 Preferences 打开 Preferences &gt; Simplicity Studio &gt; SDKs 或从 Launcher 透视图的 OVERVIEW 选项卡中选择 <strong>Manage SDKs</strong>。选择父 SDK 并点击 <strong>Add Extensions</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/sdk-extension-preferences.png" alt="Add extensions highlighted on the preferences dialog">
        </p>
    </li>
    <li>
        <p>
            在 Add SDK Extensions 对话框中，浏览到扩展目录。如果它具有有效的 SDK 扩展，SSv5 会检测到它。点击 <strong>OK</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/sdk-extension-add.png" alt="Add extensions dialog with a valid extension">
        </p>
    </li>
    <li>
        <p>
            这可能会要求您信任该 SDK 扩展。如果信任，请点击 <strong>Trust</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/sdk-extension-trust.png" alt="Trust extension dialog">
        </p>
    </li>
    <li>
        <p>
            该扩展现在会出现在 SDK 列表中，正如 HomeKit 在本示例中的那样。点击 <strong>Apply and Close</strong>。<br>
            <img src="../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/sdk-extension-apply.png" alt="HomeKit highlighted on the preferences dialog">
        </p>
    </li>
</ol>

添加 SDK 扩展后，SSv5 会像对待任何其他 SDK 一样对待它，例如在示例项目的过滤器中显示它。

![HomeKit highlighted on the preferences dialog](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/sdk-extension-filter.png)

# 项目迁移

## 转移到 Component-Based 架构

在 SSv5 version 5.3 中，Gecko Bootloader、Zigbee 和 Z-Wave 更改为 Silicon Labs Configurator component-based 架构。有关从早期版本的 SSv5 转移到这些项目的更多信息，请参阅：

* Zigbee [AN1301: Transitioning from Zigbee EmberZNet SDK 6.x to SDK 7.x](https://www.silabs.com/documents/public/application-notes/an1301-zigbee-v6x-to-v7x-transition-guide.pdf)
* Gecko Bootloader [AN1326: Transitioning to the Updated Gecko Bootloader in GSDK 4.0 and Higher](https://www.silabs.com/documents/public/application-notes/an1326-gecko-bootloader-transitioning-guide.pdf)

## 从 Simplicity Studio 4 迁移到 Simplicity Studio 5

将项目从 Simplicity Studio® 4（SSv4）迁移到 SSv5 的过程取决于项目的类型。

如果要迁移 [Bootloader、EFM32 或 EFM8 项目](#bootloaderefm32-和-efm8-项目)，请使用 **Migrate Project** 工具。如果您要迁移 [Z-Wave](#z-wave-项目)、[Bluetooth/Bluetooth Mesh、Proprietary Flex 或 Zigbee 项目](#zigbeeflex-和-bluetoothbluetooth-mesh-项目)，请按照指定文档中的说明进行操作。

### Bootloader、EFM32 和 EFM8 项目

**Bootloader 注意**：将此过程用作迁移的第一步。

点击 **Tools** 工具栏按钮以打开 Tools 对话框。选择 **Migrate Projects** 并点击 **OK**。选择要迁移的项目，点击 **Next**。

![migrate start](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/migrate-start.png)

验证显示的信息，然后点击 **Next**。

![migrate verify](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/migrate-verify.png)

决定是否要复制项目（推荐）并点击 **Finish**。

![migrate copy](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/migrate-copy.png)

项目将从 SSv4 迁移到 SSv5。

![migrate success](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/migrate-success.png)

### Z-Wave 项目

（通过 Simplicity Studio 5.2）按照知识库文章 [Migrating a Z-Wave project from GSDK 2.7.6 to GSDK 3.0.0](https://www.silabs.com/community/software/simplicity-studio/knowledge-base.entry.html/2020/07/17/migrating_a_z-waveprojectfromgsdk276togsdk-opdc) 中的指示进行操作。

### Zigbee、Flex 和 Bluetooth/Bluetooth Mesh 项目

无法使用该工具迁移这些项目。相反，请参阅以下文档：

* Zigbee (Through Simplicity Studio 5.2): [QSG106: Getting Started with EmberZNet PRO](https://www.silabs.com/documents/public/quick-start-guides/qsg106-efr32-zigbee-pro.pdf)
* Flex: [AN1254: Transitioning from the v2.x to the v3.x Proprietary Flex SDK](https://www.silabs.com/documents/public/application-notes/an1254-transitioning-from-proprietary-flex-sdk-v2-to-v3.pdf)
* Bluetooth: [AN1255: Transitioning from the v2.x to the v3.x Bluetooth SDK](https://www.silabs.com/documents/public/application-notes/an1255-transitioning-from-bluetooth-sdk-v2-to-v3.pdf)
* Bluetooth Mesh: [AN1298: Transitioning from the v1.x to the v2.x Bluetooth Mesh SDK](https://www.silabs.com/documents/public/application-notes/an1298-transitioning-from-bluetooth-mesh-1x-to-2x.pdf)

如果您尝试使用该工具，在大多数情况下，该工具会将您指向迁移过程的文档。

![migrate incompatible](../images/Getting%20Started%20with%20Simplicity%20Studio%205%20and%20the%20Gecko%20SDK/migrate-incompatible.png)
