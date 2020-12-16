# UG118: Blue Gecko Bluetooth® Profile Toolkit Developer's Guide (Rev. 2.3) <!-- omit in toc -->

- [1. 了解 Profile、Service、Characteristic 和 Attribute Protocol](#1-了解-profileservicecharacteristic-和-attribute-protocol)
  - [1.1 GATT-Based Bluetooth Profiles and Services](#11-gatt-based-bluetooth-profiles-and-services)
  - [1.2 Services](#12-services)
  - [1.3 Characteristics](#13-characteristics)
  - [1.4 Attribute Protocol](#14-attribute-protocol)
- [2. 使用 Profile Toolkit 构建 GATT 数据库](#2-使用-profile-toolkit-构建-gatt-数据库)
  - [2.1 一般限制](#21-一般限制)
  - [2.2 \<gatt\>](#22-gatt)
  - [2.3 \<capabilities_declare\>](#23-capabilities_declare)
    - [2.3.1 \<capability\>](#231-capability)
      - [Capability 的继承](#capability-的继承)
      - [可见性](#可见性)
  - [2.4 \<service\>](#24-service)
    - [2.4.1 \<capabilities\>](#241-capabilities)
    - [2.4.2 \<informativeText\>](#242-informativetext)
    - [2.4.3 \<include\>](#243-include)
  - [2.5 \<characteristic\>](#25-characteristic)
    - [2.5.1 \<capabilities\>](#251-capabilities)
    - [2.5.2 \<properties\>](#252-properties)
    - [2.5.3 \<value\>](#253-value)
    - [2.5.4 \<descriptor\>](#254-descriptor)
    - [2.5.5 \<description\>](#255-description)
    - [2.5.6 \<aggregate\>](#256-aggregate)
  - [2.6 GATT 示例](#26-gatt-示例)

Bluetooth GATT service 和 characteristic 是 Bluetooth 数据交换的基础。它们用于描述设备（如心率监视器）公开数据的结构、访问类型和安全特性。Bluetooth service 和 characteristic 具有定义明确且结构化的格式，可以使用 XML 标记语言来描述它们。

Profile Toolkit 基于 XML 的标记语言，用于以易于理解的形式和机器可读的形式描述 Bluetooth service 和 characteristic，也称为 GATT 数据库（database）。本指南将引导您逐步了解 Profile Toolkit 中使用的 XML 语法，并指导您如何描述自己的 Bluetooth service 和 characteristic、配置访问和安全特性以及如何将 GATT 数据库作为固件的一部分。

本指南还包含一些实用示例，展示了标准化的 Bluetooth service 和 vendor-specific proprietary service 的使用。这些示例为您自己的开发工作提供了良好的起点。

# 1. 了解 Profile、Service、Characteristic 和 Attribute Protocol

本节对 Bluetooth profile、service 和 characteristic 进行了基本解释，并说明了如何在 GATT 服务端（server）和客户端（client）之间的数据交换中使用 Attribute Protocol。还提供了有关这些主题的详情链接。

## 1.1 GATT-Based Bluetooth Profiles and Services

Bluetooth profile 指定了数据交换的结构。Profile 定义了元素（如 profile 中使用的 service 和 characteristic），但它也可能包含安全性和连接建立参数的定义。通常，profile 包含一个或多个完成高级用例所需的 service（如心率或节奏监测）。标准化的 profile 允许设备和软件供应商构建可互操作的设备和应用。

Bluetooth SIG 标准化的 profile 位于：

[https://developer.bluetooth.org/gatt/profiles/Pages/ProfilesHome.aspx](https://developer.bluetooth.org/gatt/profiles/Pages/ProfilesHome.aspx)

## 1.2 Services

Service 是由一个或多个用于完成设备特定功能的 characteristic 组成的数据集（如电池监视或温度数据），而不是完整的用例。

Bluetooth SIG 标准化的 service 规范位于：

[https://developer.bluetooth.org/gatt/services/Pages/ServicesHome.aspx](https://developer.bluetooth.org/gatt/services/Pages/ServicesHome.aspx)

## 1.3 Characteristics

Characteristic 是在 service 中使用的值，用于公开数据和/或交换数据和/或控制信息。Characteristic 具有明确定义的已知格式。它还包含有关如何访问值、必须满足哪些安全要求以及（可选）如何显示或解释 characteristic 值的信息。Characteristic 还可以包含 descriptor（用于描述值或 characteristic 数据指示或通知的许可配置）。

Bluetooth SIG 标准化的 characteristic 位于：

[https://developer.bluetooth.org/gatt/characteristics/Pages/CharacteristicsHome.aspx](https://developer.bluetooth.org/gatt/characteristics/Pages/CharacteristicsHome.aspx)

## 1.4 Attribute Protocol

Attribute Protocol 使数据可以在 GATT 服务端和 GATT 客户端之间进行交换。该协议还提供了一组操作，即如何在两个 GATT 参与方之间查询（query）、写入（write）、指示（indicate）或通知（notify）数据和/或控制信息。

<p>
  <img src="../images/UG118/Figure%201.1.%20Profile,%20Service,%20and%20Characteristic%20Relationships.png"
       alt="Figure 1.1. Profile, Service, and Characteristic Relationships"
       title="Figure 1.1. Profile, Service, and Characteristic Relationships">
</p>

<p>
  <img src="../images/UG118/Figure%201.2.%20Attribute%20Read%20Operation.png"
       alt="Figure 1.2. Attribute Read Operation"
       title="Figure 1.2. Attribute Read Operation">
</p>

<p>
  <img src="../images/UG118/Figure%201.3.%20Attribute%20Write%20Operation.png"
       alt="Figure 1.3. Attribute Write Operation"
       title="Figure 1.3. Attribute Write Operation">
</p>

<p>
  <img src="../images/UG118/Figure%201.4.%20Attribute%20Write%20without%20Response%20Operation.png"
       alt="Figure 1.4. Attribute Write without Response Operation"
       title="Figure 1.4. Attribute Write without Response Operation">
</p>

<p>
  <img src="../images/UG118/Figure%201.5.%20Attribute%20Indicate%20Operation.png"
       alt="Figure 1.5. Attribute Indicate Operation"
       title="Figure 1.5. Attribute Indicate Operation">
</p>

<p>
  <img src="../images/UG118/Figure%201.6.%20Attribute%20Notify%20Operation.png"
       alt="Figure 1.6. Attribute Notify Operation"
       title="Figure 1.6. Attribute Notify Operation">
</p>

# 2. 使用 Profile Toolkit 构建 GATT 数据库

此部分描述了 Blue Gecko Bluetooth Profile Toolkit 中使用的 XML 语法，并引导您完成构建 Bluetooth service 和 characteristic 时可以使用的不同选项。

还展示了一些实用的 GATT 数据库示例。

## 2.1 一般限制

下表展示了 Blue Gecko 设备支持的 GATT 数据库的局限性。

<table>
<thead>
  <tr>
    <th>Item</th>
    <th>Limitation</th>
    <th>Notes</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Maximum number of characteristics</td>
    <td>Not limited; practically restricted by the overall number of attributes in the database</td>
    <td>All characteristics which do NOT have the property <strong>const="true"</strong> are included in this count.</td>
  </tr>
  <tr>
    <td>Maximum length of a <strong>type="user"</strong> characteristic</td>
    <td>255 bytes</td>
    <td>These characteristics are handled by the application, which means that the amount of RAM available for the application will limit this.<br><br>Note: GATT procedures <strong>Write Long Characteristic Values</strong>, <strong>Reliable Writes</strong> and <strong>Read Multiple Characteristic Values</strong> are not supported for these characteristics.</td>
  </tr>
  <tr>
    <td>Maximum length of a <strong>type="utf-8/hex"</strong> characteristic</td>
    <td>255 bytes</td>
    <td>If <strong>const="true"</strong> then the amount of free flash on the device defines this limit.<br><br>If <strong>const="false"</strong> then RAM will be allocated for the characteristic for storing its value. The amount of free flash available on the device used defines this.</td>
  </tr>
  <tr>
    <td>Maximum number of attributes in a single GATT database</td>
    <td>255</td>
    <td>A single characteristic typically uses 3-5 attributes.</td>
  </tr>
  <tr>
    <td>Maximum number of notifiable characteristics</td>
    <td>64</td>
    <td></td>
  </tr>
  <tr>
    <td>Maximum number of capabilities</td>
    <td>16</td>
    <td>The logic state of the capabilities will determine the visibility of each service/characteristic.</td>
  </tr>
</tbody>
</table>

## 2.2 \<gatt\>

必须在 XML 属性 `<gatt>` 内部描述 GATT 数据库及 service 和 characteristic。

<table>
<thead>
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><code>out</code></td>
    <td>Filename for the GATT C source file<br><br><strong>Value</strong>: Any UTF-8 string. Must be valid as a filename and end with '.c'<br><br><strong>Default</strong>: gatt_db.c</td>
  </tr>
  <tr>
    <td><code>header</code></td>
    <td>Filename for the GATT C header file<br><br><strong>Value</strong>: Any UTF-8 string. Must be valid as a filename and end with '.h'<br><br><strong>Default</strong>: gatt_db.h</td>
  </tr>
  <tr>
    <td><code>db_name</code></td>
    <td>GATT database structure name and the prefix for data structures in the GATT C source file.<br><br><strong>Value</strong>: Any UTF-8 string; must be valid in C.<br><br><strong>Default</strong>: bg_gattdb_data</td>
  </tr>
  <tr>
    <td><code>name</code></td>
    <td>Free text, not used by the database compiler<br><br><strong>Value</strong>: Any UTF-8 string<br><br><strong>Default</strong>: Nothing</td>
  </tr>
  <tr>
    <td><code>prefix</code></td>
    <td>Prefix to add to each 'id' name for defining the handle macro that can be referenced from the C application.<br><br><strong>Value</strong>: Any UTF-8 string. Must be valid in C.<br><br><strong>Default</strong>: gattdb_<br><br>For example: If prefix="gattdb_" and id="temp_measurement" for a particular characteristic, then the following will be generated in the GATT C header file:<br><br>#define gattdb_temp_measurement X (where X is the handle number for the temp_measurement characteristic)</td>
  </tr>
  <tr>
    <td><code>generic_attribute_service</code></td>
    <td>If it is set to true, Generic Attribute service and its service_changed characteristic will be added in the beginning of the database. The Bluetooth stack takes care of database structure change detection and will send service_changed notifications to clients when a change is detected. In addition, this will enable the GATT-caching feature introduced in Bluetooth 5.1.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Generic Attribute service is automatically added to the GATT database and GATT caching is enabled.<br><br><strong>false</strong>: Generic Attribute service is not automatically added to the GATT database and GATT caching is disabled.<br><br><strong>Default</strong>: false</td>
  </tr>
  <tr>
    <td><code>gatt_caching</code></td>
    <td>The GATT caching feature is enabled by default if generic_attribute_service is set to true. However, it can be disabled by setting this attribute to false.</td>
  </tr>
</tbody>
</table>

示例：一个 GATT 数据库定义。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<gatt out="my_gatt_db.c" header="my_gatt_db.h" db_name="my_gatt_db_" prefix="my_gatt_" generic_attribute_service="true" name="My GATT database">
    …
</gatt>
```

## 2.3 \<capabilities_declare\>

通过使用 **capability**，可以使 GATT 数据库的 service 和 characteristic 可见/不可见（visible/invisible）。Capability 必须在 `<capability>` 元素中声明，并且必须首先在由 `<capability>` 元素序列组成的 `<capabilities_declare>` 元素中声明 GATT 数据库中的所有 capability。数据库中 capability 的最大数量为 16。

此新功能不会影响旧版（legacy）GATT XML 数据库（Silicon Labs Bluetooth stack version 2.4.x 之前的版本）。由于它们没有明确声明的任何 capability，因此所有 service 和 characteristic 对于远程 GATT 客户端将保持可见性。

示例：capability 声明。

```xml
<capabilities_declare>
    <capability enable="false">feature_1</capability>
    <capability enable="false">feature_2</capability>
</capabilities_declare>
```

### 2.3.1 \<capability\>

必须使用 `<capability>` 元素在 `<capabilities_declare>` 元素中分别声明每个 capability。`<capability>` 元素具有一个名为“enable”的属性，该属性指示数据库初始化时 capability 的默认状态。

`<capability>` 元素的文本值将用于在生成的数据库 C 头文件中标识（用作标识符）该 capability。因此，它必须在 C 中有效。

#### Capability 的继承

Service 和 characteristic 可以声明它们要使用的 capability。如果未声明任何 capability，则适用以下继承规则：

1. 不声明任何 capability 的 service 将具有 `<capabilities_declare>` 元素中的所有 capability。
2. 不声明任何 capability 的 characteristic 将具有其所属 service 中的所有 capability。如果 service 在 `<capabilities_declare>` 中声明了 capability 的子集，则 characteristic 将仅继承该子集。
3. Characteristic 的所有 attribute 都继承了 characteristic 的 capability。

#### 可见性

可以根据以下逻辑启用/禁用 capability，以使 service 和 characteristic 对 GATT 客户端可见/不可见：

1. 当一个 service 启用至少一项 capability 后，它及其所有 characteristic 将可见。
2. 当一个 service 禁用所有 capability 后，它及其所有 characteristic 将不可见。
3. 当一个 characteristic 启用至少一项 capability 后，它及其所有 attribute 将可见。
4. 当一个 characteristic 禁用所有 capability 后，它及其所有 attribute 将不可见。

<table>
<thead>
  <tr>
    <th style="white-space: nowrap">Parameter</th>
    <th style="white-space: nowrap">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="white-space: nowrap"><code>enable</code></td>
    <td style="white-space: nowrap">Sets the default state of a capability at database initialization.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Capability is enabled.<br><br><strong>false</strong>: Capability is disabled.<br><br><strong>Default</strong>: true</td>
  </tr>
</tbody>
</table>

示例：capability 声明。

```xml
<capabilities_declare>
    <!-- This capability is enabled by default and the identifier is cap_light -->
    <capability enable="true">cap_light</capability>
    <!-- This capability is disabled by default and the identifier is cap_color -->
    <capability enable="false">cap_color</capability>
</capabilities_declare>
```

## 2.4 \<service\>

GATT service 定义是通过 XML 属性 `<service>` 及其参数完成的。

下表描述了可用于定义相关值的参数。

<table>
<thead>
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><code>uuid</code></td>
    <td>Universally Unique Identifier. The UUID uniquely identifies a service. 16-bit values are used for the services defined by the Bluetooth SIG and 128-bit UUIDs can be used for vendor specific implementations.<br><br><strong>Range</strong>:<br><br><strong>0x0000 – 0xFFFF</strong>: Reserved for Bluetooth SIG standardized services<br><br><strong>0x00000000-0000-0000-0000-000000000000 - 0xFFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFF</strong>: Reserved for vendor specific services.</td>
  </tr>
  <tr>
    <td><code>id</code></td>
    <td>The ID is used to identify a service within the service database and can be used as a reference from other services (include statement). Typically, this does not need to be used.<br><br><strong>Value</strong>:<br><br>Any UTF-8 string</td>
  </tr>
  <tr>
    <td><code>type</code></td>
    <td>The type field defines whether the service is a primary or a secondary service. Typically this does not need to be used.<br><br><strong>Values</strong>:<br><br><strong>primary</strong>: a primary service<br><br><strong>secondary</strong>: a secondary service<br><br><strong>Default</strong>: primary</td>
  </tr>
  <tr>
    <td><code>advertise</code></td>
    <td>This field defines if the service UUID is included in the advertisement data.<br><br>The advertisement data can contain up to 13 16-bit UUIDs or one (1) 128-bit UUID.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: UUID included in advertisement data<br><br><strong>false</strong>: UUID not included in advertisement data<br><br><strong>Default</strong>: false<br><br><strong>Note</strong>: You can override the advertisement data with the GAP API, in which case this is not valid.</td>
  </tr>
</tbody>
</table>

示例：Generic Access Profile（GAP）service 定义。

```xml
<!-- Generic Access Service -->
<service uuid="1800">
    …
</service>
```

示例：vendor-specific service 定义。

```xml
<!-- A vendor specific service -->
<service uuid="25be6a60-2040-11e5-bd86-0002a5d5c51b">
    …
</service>
```

示例：具有 UUID 的 Heart Rate service 定义，包含广告数据和 ID “hrs”。

```xml
<!-- Heart Rate Service -->
<service uuid="180D" id="hrs" advertise=”true”>
    …
</service>
```

> 注意：您可以生成自己的 128-bit UUID：[http://www.itu.int/en/ITU-T/asn1/Pages/UUID/uuids.aspx](http://www.itu.int/en/ITU-T/asn1/Pages/UUID/uuids.aspx)

### 2.4.1 \<capabilities\>

Service 可以使用 `<capabilities>` 元素声明其具有的 capability。该元素由一系列 `<capability>` 元素组成，其标识符也必须是 `<capabilities_declare>` 元素的一部分。属性“enable”对在此上下文中声明的 capability 无效，因此可以将其排除。

如果 service 未声明任何 capability，则根据继承规则，它将具有 `<capabilities_declare>` 中的所有 capability。

启用 service 的任一 capability 后，该 service 及其所有 characteristic 将变为可见；禁用其所有 capability 时，该 service 及其所有 characteristic 将变为不可见。

示例：capability 声明。

```xml
<capabilities>
    <capability>cap_light</capability>
    <capability>cap_color</capability>
</capabilities>
```

### 2.4.2 \<informativeText\>

XML 元素 `<informativeText>` 可以用于提供信息（注释），并且不会在实际的 GATT 数据库中公开。

### 2.4.3 \<include\>

可以使用 XML 属性 `<include>` 将 service 包含在另一个 service 中。

<table>
<thead>
  <tr>
    <th style="white-space: nowrap">Parameter</th>
    <th style="white-space: nowrap">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="white-space: nowrap"><code>id</code></td>
    <td style="white-space: nowrap">ID of the included service<br><br><strong>Value</strong>:<br><br>ID of another service</td>
  </tr>
</tbody>
</table>

示例：将 Heart Rate service 包含在 GAP service 中。

```xml
<!-- Generic Access Service -->
<service uuid="1800">
    <!-- Include HR Service -->
    <include id="hrs” />
    …
</service>
```

## 2.5 \<characteristic\>

Service 公开的所有 characteristic 都是使用 XML 属性 `<characteristic>` 及其参数定义的，必须在 `<service>` XML 属性标签内使用它们。

下表描述了可用于定义相关值的参数。

<table>
<thead>
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><code>uuid</code></td>
    <td>Universally Unique Identifier. The UUID uniquely identifies a characteristic.<br><br>16-bit values are used for the services defined by the Bluetooth SIG and 128-bit UUIDs can be used for vendor specific implementations.<br><br><strong>Range</strong>:<br><br><strong>0x0000 – 0xFFFF</strong>: Reserved for Bluetooth SIG standardized characteristics.<br><br><strong>0x00000000-0000-0000-0000-000000000000</strong> to <strong>0xFFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFF</strong>: Reserved for vendor specific characteristics.</td>
  </tr>
  <tr>
    <td><code>id</code></td>
    <td>The ID is used to identify a characteristic. The ID is used within a C application to read and write characteristic values or to detect if notifications or indications are enabled or disabled for a specific characteristic.<br><br>When the project is built the generated GATT C header file contains a macro with the characteristic 'id' and corresponding handle value.<br><br><strong>Value</strong>:<br><br>Any UTF-8 string</td>
  </tr>
  <tr>
    <td><code>const</code></td>
    <td>Defines if the value stored in the characteristic is a constant.<br><br><strong>Default</strong>: false</td>
  </tr>
  <tr>
    <td><code>name</code></td>
    <td>Free text, not used by the database compiler.<br><br><strong>Value</strong>: Any UTF-8 string<br><br><strong>Default</strong>: Nothing</td>
  </tr>
</tbody>
</table>

示例：将 Device Name characteristic 添加到 GAP service 中。

```xml
<!-- Generic Access Service -->
<service uuid="1800">
    <!-- Device name -->
    <characteristic uuid="2a00">
    …
    </characteristic>
    …
</service>
```

示例：将 vendor-specific characteristic 添加到具有 ID 的 vendor-specific service 中。

```xml
<!-- A vendor specific service -->
<service uuid="25be6a60-2040-11e5-bd86-0002a5d5c51b">
    <!-- My proprietary data -->
    <characteristic uuid="59cd69c0-2043-11e5-a717-0002a5d5c51b" id="mydata”>
    …
    </characteristic>
    …
</service>
```

### 2.5.1 \<capabilities\>

Characteristic 可以使用 `<capabilities>` 元素声明其具有的 capability。该元素由一系列 `<capability>` 元素组成，这些元素的标识符必须被其父 service 声明（或完全继承）。属性“enable”对在此上下文中声明的 capability 无效，因此可以将其排除。

如果 characteristic 未声明任何 capability，则它将根据继承规则拥有其所属 service 中的所有 capability。Characteristic 的所有 attribute 都继承了 characteristic 的 capability。

当启用至少一个 capability 时，characteristic 及其所有 attribute 将可见；而在禁用所有 capability 时，characteristic 及其所有 attribute 将不可见。

<table>
<thead>
  <tr>
    <th align="center">Parameter</th>
    <th align="center">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td align="center">-</td>
    <td align="center">-</td>
  </tr>
</tbody>
</table>

示例：capability 声明。

```xml
<capabilities>
    <capability>cap_light</capability>
    <capability>cap_color</capability>
</capabilities>
```

### 2.5.2 \<properties\>

Characteristic 的访问特性（property）及其权限级别由 XML 属性 `<properties>` 的子属性定义，必须在 `<characteristic>` XML 属性标签内使用。Characteristic 可以同时具有多个访问特性，例如，它可以是可读的、可写的、或两者都可以。每个访问特性可以具有不同的权限级别（如加密或认证）。

下表列出了可能的访问特性。表中的每一行在 `<properties>` 属性下定义了一个新属性。

<table>
<thead>
  <tr>
    <th>Attribute</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><code>read</code></td>
    <td>Characteristic can be read by a remote device.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Characteristic can be read<br><br><strong>false</strong>: Characteristic cannot be read<br><br><strong>Default</strong>: false</td>
  </tr>
  <tr>
    <td><code>write</code></td>
    <td>Characteristic can be written by a remote device<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Characteristic can be written<br><br><strong>false</strong>: Characteristic cannot be written<br><br><strong>Default</strong>: false</td>
  </tr>
  <tr>
    <td><code>write_no_response</code></td>
    <td>Characteristic can be written by a remote device. Write without response is not acknowledged over the Attribute Protocol.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Characteristic can be written<br><br><strong>false</strong>: Characteristic cannot be written<br><br><strong>Default</strong>: false</td>
  </tr>
  <tr>
    <td><code>notify</code></td>
    <td>Characteristic has the notify property and characteristic value changes are notified over the Attribute Protocol. Notifications are not acknowledged over the Attribute Protocol.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Characteristic has notify property.<br><br><strong>false</strong>: Characteristic does not have notify property.<br><br><strong>Default</strong>: false</td>
  </tr>
  <tr>
    <td><code>indicate</code></td>
    <td>Characteristic has the indicate property and characteristic value changes are indicated over the Attribute Protocol. Indications are acknowledged over the Attribute Protocol.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Characteristic has indicate property.<br><br><strong>false</strong>: Characteristic does not have indicate property.<br><br><strong>Default</strong>: false</td>
  </tr>
  <tr>
    <td><code>reliable_write</code></td>
    <td>Allows using a reliable write procedure to modify an attribute; this is just a hint to a GATT client. The Bluetooth stack always allows the use of reliable writes to modify attributes.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Reliable write enabled.<br><br><strong>false</strong>: Reliable write disenabled.<br><br><strong>Default</strong>: false</td>
  </tr>
</tbody>
</table>

<table>
<thead>
  <tr>
    <th>Attribute</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td colspan="2">The following table lists the permission levels or security requirements. Any security requirement can be assigned to any access property.</td>
  </tr>
  <tr>
    <td><code>authenticated</code></td>
    <td>Accessing the characteristic value requires an authentication. To access the characteristic with this property the remote device has to be bonded using MITM protection and the connection must be also encrypted.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Authentication is required<br><br><strong>false</strong>: Authentication is not required<br><br><strong>Default</strong>: false</td>
  </tr>
  <tr>
    <td><code>encrypted</code></td>
    <td>Accessing the characteristic value requires an encrypted link. Devices with iOS 9.1 or higher must also be bonded at least with Just Works pairing.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Encryption is required<br><br><strong>false</strong>: Encryption is not required<br><br><strong>Default</strong>: false</td>
  </tr>
  <tr>
    <td><code>bonded</code></td>
    <td>Accessing the characteristic value requires an encrypted link. Devices must also be bonded at least with Just Works pairing.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Bonding and encryption are required<br><br><strong>false</strong>: Bonding is not required<br><br><strong>Default</strong>: false</td>
  </tr>
</tbody>
</table>

示例：具有 `const` 和 `read` 特性的 Device Name characteristic。

```xml
<!-Device Name-->
<characteristic const = true uuid="2a00">
    <properties>
        <read authenticated="false" bonded="false" encrypted="false"/>
    </properties>
</characteristic>
```

示例：具有 `read` 和 `write` 特性的 Device Name characteristic，以允许远程设备修改该值。

```xml
<!-Device Name-->
<characteristic uuid="2a00">
    <properties>
        <read authenticated="false" bonded="false" encrypted="false"/>
        <write authenticated="false" bonded="false" encrypted="false"/>
    </properties>
</characteristic>
```

示例：具有 `notify` 特性的 Heart Rate Measurement characteristic。

```xml
<!-Heart Rate Measurement -->
<characteristic uuid="180D">
    <properties>
        <notify authenticated="false" bonded="false" encrypted="false"/>
    </properties>
</characteristic>
```

示例：具有 `encrypted read` 特性的 characteristic。

```xml
<!-Device Name-->
<characteristic uuid="1234">
    <properties>
        <read authenticated="false" bonded="false" encrypted="true"/>
    </properties>
</characteristic>
```

示例：具有 `authenticated write` 特性的 characteristic。

```xml
<!-Device Name-->
<characteristic uuid="1234">
    <properties>
        <write authenticated="true" bonded="false" encrypted="false"/>
    </properties>
</characteristic>
```

示例：具有 `authenticated indicate` 特性的 characteristic。

```xml
<!-Descriptor value changed -->
<characteristic uuid="2A7D">
    <properties>
        <indicate authenticated="true" bonded="false" encrypted="false"/>
    </properties>
</characteristic>
```

### 2.5.3 \<value\>

Characteristic 的数据类型和长度由 XML 属性 `<value>` 及其参数定义，这必须在 `<characteristic>` XML 属性标签内使用。

下表描述了可用于定义相关值的参数。

<table>
<thead>
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><code>length</code></td>
    <td>Defines a fixed length for the characteristic or the maximum length if <code>variable_length</code> is true. If length is not defined and there is a value (e.g. data exists inside ), then the value length is used to define the length.<br><br>If both <code>length</code> and value are defined, then the following rules apply:
    <ol>
        <li>If <code>variable_length</code> is false and <code>length</code> is bigger than the value's length, then the value will be padded with 0's at the end to match the attribute's <code>length</code>.</li>
        <li>If <code>length</code> is smaller than the value's length, then the value will be clipped to match <code>length</code>, regardless of whether <code>variable_length</code> is true or false.</li>
    </ol>
    <br>
    <strong>Range</strong>:<br><br><strong>0 – 255</strong>: Length in bytes if <code>type</code> is 'hex', 'utf-8', or 'user'<br><br><strong>0 - 512</strong>: Length in bytes if <code>type</code> is 'user'<br><br><strong>Default</strong>: 0</td>
  </tr>
  <tr>
    <td style="white-space: nowrap"><code>variable_length</code></td>
    <td>Defines that the value is of variable length. The maximum length must also be defined with the <code>length</code> attribute or by defining a value. If both <code>length</code> and value are defined, then the rules described in <code>length</code> apply.<br><br><strong>Values</strong>:<br><br><strong>true</strong>: Value is of variable length<br><br><strong>false</strong>: Value has a fixed length<br><br><strong>Default</strong>: false</td>
  </tr>
  <tr>
    <td><code>type</code></td>
    <td>Defines the data type.<br><br><strong>Values</strong>:<br><br><strong>hex</strong>: Value type is hex<br><br><strong>utf-8</strong>: Value is a string<br><br><strong>user</strong>: When the characteristic type is marked as type="user", the application is responsible for initializing the characteristic value and also providing it, for example, when read operation occurs. The Bluetooth stack does not initialize the value or automatically provide the value when it is being read. When this is set, the Bluetooth stack generates <code>gatt_server_user_read_request</code> or <code>gatt_server_user_write_request</code>, which must be handled by the application.<br><br><strong>Default</strong>: utf-8</td>
  </tr>
</tbody>
</table>

示例：具有 `notify` 特性和固定长度为 2 byte 的 Heart Rate Measurement characteristic。

```xml
<!-Heart Rate Measurement -->
<characteristic uuid="180D">
    <value length="2" type="hex" variable_length = "false"/>
    <properties>
        <notify authenticated="false" bonded="false" encrypted="false"/>
    </properties>
</characteristic>
```

示例：具有可变长度的 vendor-specific characteristic，最大长度为 20 byte。

```xml
<!-My proprietary data -->
<characteristic uuid="59cd69c0-2043-11e5-a717-0002a5d5c51b" id="mydata">
    <value variable_length="true" length="20" type="hex" />
    <properties>
        <notify authenticated="false" bonded="false" encrypted="false"/>
    </properties>
</characteristic>
```

示例：也可以通过在 `<value>` 标签内键入实际值来定义 characteristic 的值和长度。

```xml
<characteristic const="true" id="device_name" name="Device Name" sourceId="org.bluetooth.characteristic.gap.device_name" uuid="2A00">
    <value length="17" type="utf-8" variable_length="false">Blue Gecko BGM111</value>
    <properties>
        <read authenticated="false" bonded="false" encrypted="false"/>
    </properties>
</characteristic>
```

在上面的示例中，值是“Blue Gecko BGM111”，长度是 17 byte。

示例：定义 `length` 和 `value`，且 `length` 大于 `value` 的长度。

```xml
<!-- Device name -->
<characteristic uuid="2a00">
    <properties read="true" />
    <value type="hex" length="4" variable_length="false">0102</value>
</characteristic>
```

在上面的示例中，值将为“01020000”，因为 `length` 大于 `value` 的长度，所以将使用 0 进行填充。

示例：定义 `length` 和 `value`，且 `length` 小于 `value` 的长度。

```xml
<!-- Device name -->
<characteristic uuid="2a00">
    <properties read="true" />
    <value type="hex" length="2" variable_length="false">01020304</value>
</characteristic>
```

在上面的示例中，值将为“0102”，因为 `length` 小于 `value` 的长度，因此值将被裁剪以匹配 `length`。

### 2.5.4 \<descriptor\>

XML 元素 `<descriptor>` 可用于定义通用的 characteristic descriptor。

Descriptor 特性由 `<properties>` 元素定义，并且仅允许读取和/或写入访问。值由 `<value>` 元素定义，方式与 characteristic 值相同。

示例：添加类型为 UUID 2908 的 characteristic descriptor。

```xml
<characteristic uuid="2a4d" id="hid_input">
    <properties notify="true" read="true" />
    <value length="3" />
    <descriptor const="false" discoverable="true" id="" name="Custom Descriptor" sourceId="" uuid="2908">
        <properties>
            <read authenticated="false" bonded="false" encrypted="false"/>
        </properties>
        <value length="0" type="hex" variable_length="false">00</value>
    </descriptor>
</characteristic>
```

### 2.5.5 \<description\>

使用 XML 属性 `<description>` 定义 characteristic user description 值，该属性必须在 `<characteristic>` XML 属性标签内使用。

Characteristic user description 是一个可选值。它暴露给远程设备，可用于例如提供对应用程序用户界面中所示 characteristic 的用户友好描述。

示例：常量字符串“Heart Rate Measurement”。

```xml
<characteristic uuid="2a37">
    <properties>
        <notify authenticated="false" bonded="false" encrypted="false"/>
    </properties>
    <description> Heart Rate Measurement </description>
</characteristic>
```

`properties` 元素可用于允许远程修改 attribute。

示例：允许远程读取，但需要绑定才能写入。

```xml
<characteristic uuid="2a37">
    <properties>
        <read authenticated="false" bonded="false" encrypted="true"/>
        <write authenticated="false" bonded="true" encrypted="false"/>
    </properties>
</characteristic>
```

> 注意：如果描述是可写的，则 GATT 解析器会自动添加扩展特性属性，并将 `writable_auxiliaries` 位设置为与 Bluetooth 兼容。

### 2.5.6 \<aggregate\>

XML 元素 `<aggregate>` 通过自动将 ID 转换为 attribute 句柄（handle）来启用聚合的 characteristic format descriptor 的创建。

Attribute ID 应该引用 characteristic presentation format descriptor。

示例：添加 characteristic 聚合。

```xml
<characteristic uuid="da8a80c0-829d-498f-b70b-e85c95e0f839">
    <properties notify="true" read="true"/>
    <value length="10" />
    <aggregate>
        <attribute id="format1" />
        <attribute id="format2" />
    </aggregate>
</characteristic>
```

## 2.6 GATT 示例

示例：一个完整的 GAP service，其 Device Name 和 Appearance characteristic 为具有 `read` 特性的常量值。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<gatt>
    <!--Generic Access-->
    <service advertise="false" name="Generic Access" requirement="mandatory" sourceId="org.bluetooth.service.generic_access" type="primary" uuid="1800">
        <informativeText>Abstract: The generic_access service contains generic information about the device. All available Characteristics are readonly. </informativeText>
        <!--Device Name-->
        <characteristic const="true" id="device_name" name="Device Name" sourceId="org.bluetooth.characteristic.gap.device_name" uuid="2A00">
            <informativeText/>
            <value length="17" type="utf-8" variable_length="false">Blue Gecko BGM111</value>
            <properties>
                <read authenticated="false" bonded="false" encrypted="false"/>
            </properties>
        </characteristic>

        <!--Appearance-->
        <characteristic const="true" name="Appearance" sourceId="org.bluetooth.characteristic.gap.appearance" uuid="2A01">
            <informativeText>Abstract: The external appearance of this device. The values are composed of a category (10-bits) and sub-categories (6-bits). </informativeText>
            <value length="2" type="hex" variable_length="false">0000</value>
            <properties>
                <read authenticated="false" bonded="false" encrypted="false"/>
            </properties>
        </characteristic>
    </service>
</gatt>
```

示例： Link Loss 和 Immediate Alert service。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<gatt>
    <!--Link Loss-->
    <service advertise="false" id="link_loss" name="Link Loss" requirement="mandatory" sourceId="org.bluetooth.service.link_loss" type="primary" uuid="1803">
        <!--Alert Level-->
        <characteristic const="false" id="alert_level" name="Alert Level" sourceId="org.bluetooth.characteristic.alert_level" uuid="2A06">
            <value length="1" type="hex" variable_length="false"/>
            <properties>
                <read authenticated="false" bonded="false" encrypted="false"/>
                <write authenticated="false" bonded="false" encrypted="false"/>
            </properties>
        </characteristic>
    </service>

    <!--Immediate Alert-->
    <service advertise="false" id="immediate_alert" name="Immediate Alert" requirement="mandatory" sourceId="org.bluetooth.service.immediate_alert" type="primary" uuid="1802">
        <!--Alert Level-->
        <characteristic const="false" id="alert_level" name="Alert Level" sourceId="org.bluetooth.characteristic.alert_level" uuid="2A06">
            <value length="1" type="hex" variable_length="false"/>
            <properties>
                <write_no_response authenticated="false" bonded="false" encrypted="false"/>
            </properties>
        </characteristic>
    </service>
</gatt>
```

示例：具有 capability 的 GATT 数据库。

```xml
<gatt db_name="light_gattdb" out="gatt_db.c" header="gatt_db.h" generic_attribute_service="true">
    <capabilities_declare>
        <capability enable="true">cap_light</capability>
        <capability enable="false">cap_color</capability>
    </capabilities_declare>

    <!--Light Service-->
    <service advertise="false" id="light_service" name="Light Service" requirement="mandatory" sourceId="" type="primary" uuid="257f993d-756e-baa6-e69c-8b101e4e6b3f">
        <informativeText>Info about custom service</informativeText>
        <capabilities>
            <capability>cap_light</capability>
            <capability>cap_color</capability>
        </capabilities>

        <!--Ligth Control-->
        <characteristic const="false" id="light_control" name="Ligth Control" sourceId="" uuid="85e82a1c-8423-610b-9fea-5ad999445231">
            <description>User description</description>
            <informativeText/>
            <capabilities>
                <capability>cap_light</capability>
            </capabilities>
            <value length="0" type="user" variable_length="false"/>
            <properties>
                <read authenticated="false" bonded="false" encrypted="false"/>
                <write authenticated="false" bonded="false" encrypted="false"/>
                <write_no_response authenticated="false" bonded="false" encrypted="false"/>
                <reliable_write authenticated="false" bonded="false" encrypted="false"/>
                <indicate authenticated="false" bonded="false" encrypted="false"/>
                <notify authenticated="false" bonded="false" encrypted="false"/>
            </properties>
        </characteristic>

        <!--Color control-->
        <characteristic const="false" id="color_control" name="Color control" sourceId="" uuid="16b90591-c54ae7c9-413e-a82748a1e783">
            <informativeText/>
            <capabilities>
                <capability>cap_color</capability>
            </capabilities>
            <value length="0" type="user" variable_length="false"/>
            <properties>
                <read authenticated="false" bonded="false" encrypted="false"/>
                <write authenticated="false" bonded="false" encrypted="false"/>
            </properties>
        </characteristic>

    </service>
</gatt>
```

* 如果启用 capability `cap_light` 和 `cap_color`，则整个 `light_service` 将可见
* 如果禁用 capability `cap_light`，则 characteristic `light_control` 将不可见
* 如果禁用 capability `cap_color`，则 characteristic `color_control` 将不可见
