# UG118: Bluetooth® Profile Toolkit Developer's Guide (Rev. 2.7) <!-- omit in toc -->

- [1. Understanding Profiles, Services, Characteristics, and the Attribute Protocol](#1-understanding-profiles-services-characteristics-and-the-attribute-protocol)
  - [1.1 GATT-Based Bluetooth Profiles and Services](#11-gatt-based-bluetooth-profiles-and-services)
  - [1.2 Services](#12-services)
  - [1.3 Characteristics](#13-characteristics)
  - [1.4 The Attribute Protocol](#14-the-attribute-protocol)
- [2. Building the GATT Database with Profile Toolkit](#2-building-the-gatt-database-with-profile-toolkit)
  - [2.1 General Limitations](#21-general-limitations)
  - [2.2 \<gatt\>](#22-gatt)
  - [2.3 \<capabilities\_declare\>](#23-capabilities_declare)
    - [2.3.1 \<capability\>](#231-capability)
      - [Inheritance of Capabilities](#inheritance-of-capabilities)
      - [Visibility](#visibility)
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
  - [2.6 GATT Examples](#26-gatt-examples)
- [3. Generating the Code Files](#3-generating-the-code-files)
  - [3.1 Using Simplicity Studio](#31-using-simplicity-studio)
  - [3.2 Using bgbuild](#32-using-bgbuild)

---

Bluetooth GATT Services 和 Characteristics 是 Bluetooth 数据交换的基础。它们用于描述设备（如心率监视器）公开数据的结构、访问类型和安全特性。Bluetooth Services 和 Characteristics 具有定义明确且结构化的格式，可以使用 XML 标记语言来描述它们。

Profile Toolkit 是一个基于 XML 的标记语言，用于以易于理解的形式和机器可读的形式描述 Bluetooth Services 和 Characteristics（也称为 GATT Database）。本指南将引导您逐步了解 Profile Toolkit 中使用的 XML 语法，并指导您如何描述自己的 Bluetooth Services 和 Characteristics、配置访问权限和安全属性以及如何包含 GATT Database 以作为固件的一部分。

本指南还包括一些实用示例，展示了标准化的 Bluetooth Services 和供应商特定的（Vendor-Specific）专用的 Services 的使用。这些示例为您自己的开发工作提供了良好的起点。

# 1. Understanding Profiles, Services, Characteristics, and the Attribute Protocol

本节对 Bluetooth Profiles、Services 和 Characteristics 进行了基本解释，并说明了如何在 GATT Server 和 Client 之间的数据交换中使用 Attribute Protocol。有关这些主题的更多信息，请访问 Bluetooth SIG 网站：[https://www.bluetooth.com/specifications/gatt/](https://www.bluetooth.com/specifications/gatt/)。

## 1.1 GATT-Based Bluetooth Profiles and Services

Bluetooth Profile 指定了数据交换的结构。Profile 定义了元素（如 Profile 中使用的 Services 和 Characteristics），但它也可能包含安全性和连接建立参数的定义。通常，一个 Profile 包含一个或多个完成高级用例（如心率或节奏监测）所需的 Services。标准化的 Profiles 允许设备和软件供应商构建可互操作的设备和应用。

## 1.2 Services

Service 是由一个或多个用于完成设备特定功能（如电池监视或温度数据）的 Characteristics 组成的数据集，而不是一个完整的用例。

## 1.3 Characteristics

Characteristic 是在 Service 中使用的值，用于公开数据和/或交换数据和/或控制信息。Characteristics 具有明确定义的已知格式。它还包含有关如何访问值、必须满足哪些安全要求以及（可选）如何显示或解释 Characteristic 值的信息。Characteristics 还可以包含 Descriptors（用于描述值或 Characteristic 数据指示或通知的许可配置）。

## 1.4 The Attribute Protocol

Attribute Protocol 使数据可以在 GATT Server 和 GATT Client 之间进行交换。该协议还提供了一组操作，即如何在两个 GATT 参与方之间查询（Query）、写入（Write）、指示（Indicate）或通知（Notify）数据和/或控制信息。

![Figure 1.1. Profile, Service, and Characteristic Relationships](images/Figure%201.1.%20Profile,%20Service,%20and%20Characteristic%20Relationships.png "Figure 1.1. Profile, Service, and Characteristic Relationships")

![Figure 1.2. Attribute Read Operation](images/Figure%201.2.%20Attribute%20Read%20Operation.png "Figure 1.2. Attribute Read Operation")

![Figure 1.3. Attribute Write Operation](images/Figure%201.3.%20Attribute%20Write%20Operation.png "Figure 1.3. Attribute Write Operation")

![Figure 1.4. Attribute Write without Response Operation](images/Figure%201.4.%20Attribute%20Write%20without%20Response%20Operation.png "Figure 1.4. Attribute Write without Response Operation")

![Figure 1.5. Attribute Indicate Operation](images/Figure%201.5.%20Attribute%20Indicate%20Operation.png "Figure 1.5. Attribute Indicate Operation")

![Figure 1.6. Attribute Notify Operation](images/Figure%201.6.%20Attribute%20Notify%20Operation.png "Figure 1.6. Attribute Notify Operation")

# 2. Building the GATT Database with Profile Toolkit

此部分描述了 Bluetooth Profile Toolkit 中使用的 XML 语法，并引导您完成构建 Bluetooth Services 和 Characteristics 时可以使用的不同选项。

还展示了一些实用的 GATT Database 示例。

## 2.1 General Limitations

下表展示了 EFR32BG 设备支持的 GATT Database 的局限性。

| Item                                                    | Limitation                                                                              | Notes                                                                                                                                                                                                                                                                                                                   |
| :------------------------------------------------------ | :-------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Maximum number of characteristics                       | Not limited; practically restricted by the overall number of attributes in the database | All characteristics which do NOT have the property **const="true"** are included in this count.                                                                                                                                                                                                                         |
| Maximum length of a **type="user"** characteristic      | 255 bytes                                                                               | These characteristics are handled by the application, which means that the amount of RAM available for the application will limit this.<br><br>Note: GATT procedures **Write Long Characteristic Values**, **Reliable Writes** and **Read Multiple Characteristic Values** are not supported for these characteristics. |
| Maximum length of a **type="utf-8/hex"** characteristic | 255 bytes                                                                               | If **const="true"** then the amount of free flash on the device defines this limit.<br><br>If **const="false"** then RAM will be allocated for the characteristic for storing its value. The amount of free flash available on the device used defines this.                                                            |
| Maximum number of attributes in a single GATT database  | 255                                                                                     | A single characteristic typically uses 3-5 attributes.                                                                                                                                                                                                                                                                  |
| Maximum number of notifiable characteristics            | 64                                                                                      |                                                                                                                                                                                                                                                                                                                         |
| Maximum number of capabilities                          | 16                                                                                      | The logic state of the capabilities will determine the visibility of each service/characteristic.                                                                                                                                                                                                                       |

## 2.2 \<gatt\>

必须在 XML 属性 **\<gatt\>** 内部描述 GATT Database 及 Services 和 Characteristics。

| Parameter                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| :-------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `out`                       | Filename for the GATT C source file<br><br>**Value**: Any UTF-8 string. Must be valid as a filename and end with '.c'<br><br>**Default**: gatt_db.c                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `header`                    | Filename for the GATT C header file<br><br>**Value**: Any UTF-8 string. Must be valid as a filename and end with '.h'<br><br>**Default**: gatt_db.h                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `db_name`                   | GATT database structure name and the prefix for data structures in the GATT C source file.<br><br>**Value**: Any UTF-8 string; must be valid in C.<br><br>**Default**: bg_gattdb_data                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `name`                      | Free text, not used by the database compiler<br><br>**Value**: Any UTF-8 string<br><br>**Default**: Nothing                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `prefix`                    | Prefix to add to each 'id' name for defining the handle macro that can be referenced from the C application.<br><br>**Value**: Any UTF-8 string. Must be valid in C.<br><br>**Default**: gattdb_<br><br>For example: If prefix="gattdb_" and id="temp_measurement" for a particular characteristic, then the following will be generated in the GATT C header file:<br><br>#define gattdb_temp_measurement X (where X is the handle number for the temp_measurement characteristic)                                                                                                                                                                                           |
| `generic_attribute_service` | If it is set to true, Generic Attribute service and its service_changed characteristic will be added in the beginning of the database. The Bluetooth stack takes care of database structure change detection and will send service_changed notifications to clients when a change is detected. In addition, this will enable the GATT-caching feature introduced in Bluetooth 5.1.<br><br>**Values**:<br><br>**true**: Generic Attribute service is automatically added to the GATT database and GATT caching is enabled.<br><br>**false**: Generic Attribute service is not automatically added to the GATT database and GATT caching is disabled.<br><br>**Default**: false |
| `gatt_caching`              | The GATT caching feature is enabled by default if generic_attribute_service is set to true. However, it can be disabled by setting this attribute to false.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

**示例**：一个 GATT Database 定义。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<gatt out="my_gatt_db.c" header="my_gatt_db.h" db_name="my_gatt_db_" prefix="my_gatt_" generic_attribute_service="true" name="My GATT database">
  …
</gatt>
```

## 2.3 \<capabilities_declare\>

通过使用 **capabilities**，可以使 GATT Database 的 Services 和 Characteristics 可见/不可见。Capability 必须在 \<capability\> 元素中声明，并且必须首先在由 \<capability\> 元素序列组成的 \<capabilities_declare\> 元素中声明 GATT Database 中的所有 Capabilities。数据库中 Capability 的最大数量为 16。

此新功能不会影响旧版的 GATT XML 数据库（Silicon Labs Bluetooth Stack version 2.4.x 之前的版本）。由于它们没有明确声明的任何 Capabilities，因此所有 Services 和 Characteristics 对于远程 GATT Client 将保持可见。

**示例**：Capabilities 声明。

```xml
<capabilities_declare>
  <capability enable="false">feature_1</capability>
  <capability enable="false">feature_2</capability>
</capabilities_declare>
```

### 2.3.1 \<capability\>

必须使用 \<capability\> 元素在 \<capabilities_declare\> 元素中分别声明每个 Capability。\<capability\> 元素具有一个名为 “enable” 的属性，该属性指示数据库初始化时 Capability 的默认状态。

\<capability\> 元素的文本值将用于在生成的数据库 C 头文件中标识（用作标识符）该 Capability。因此，它必须在 C 中有效。

#### Inheritance of Capabilities

Services 和 Characteristics 可以声明它们要使用的 Capabilities。如果未声明任何 Capabilities，则使用以下继承规则：

1. 不声明任何 Capabilities 的 Service 将具有 \<capabilities_declare\> 元素中的所有 Capabilities。
2. 不声明任何 Capabilities 的 Characteristics 将具有其所属 Service 中的所有 Capabilities。如果 Service 声明了 \<capabilities_declare\> 中的 Capabilities 的子集，则 Characteristic 将仅继承该子集。
3. Characteristic 的所有 Attributes 都继承了 Characteristic 的 Capabilities。

#### Visibility

可以根据以下逻辑启用/禁用 Capabilities，以使 Services 和 Characteristics 对 GATT Client 可见/不可见：

1. 当一个 Service **启用**其**至少一项** Capabilities 后，它及其所有 Characteristics 将**可见**。
2. 当一个 Service **禁用**其**所有** Capabilities 后，它及其所有 Characteristics 将**不可见**。
3. 当一个 Characteristic **启用**其**至少一项** Capabilities 后，它及其所有 Attributes 将**可见**。
4. 当一个 Characteristic **禁用**其**所有** Capabilities 后，它及其所有 Attributes 将**不可见**。

| Parameter | Description                                                                                                                                                                                      |
| :-------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| enable    | Sets the default state of a capability at database initialization.<br><br>**Values**:<br><br>**true**: Capability is enabled.<br><br>**false**: Capability is disabled.<br><br>**Default**: true |

**示例**：Capabilities 声明。

```xml
<capabilities_declare>
  <!-- This capability is enabled by default and the identifier is cap_light -->
  <capability enable="true">cap_light</capability>
  <!-- This capability is disabled by default and the identifier is cap_color -->
  <capability enable="false">cap_color</capability>
</capabilities_declare>
```

## 2.4 \<service\>

GATT Service 定义是通过 XML 属性 **\<service\>** 及其参数完成的。

下表描述了可用于定义相关值的参数。

| Parameter   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| :---------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `uuid`      | Universally Unique Identifier. The UUID uniquely identifies a service. 16-bit values are used for the services defined by the Bluetooth SIG and 128-bit UUIDs can be used for vendor specific implementations.<br><br>**Range**:<br><br>**0x0000 – 0xFFFF**: Reserved for Bluetooth SIG standardized services<br><br>**0x00000000-0000-0000-0000-000000000000 - 0xFFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFF**: Reserved for vendor specific services. |
| `id`        | The ID is used to identify a service within the service database and can be used as a reference from other services (include statement). Typically, this does not need to be used.<br><br>**Value**:<br><br>Any UTF-8 string                                                                                                                                                                                                                     |
| `type`      | The type field defines whether the service is a primary or a secondary service. Typically this does not need to be used.<br><br>**Values**:<br><br>**primary**: a primary service<br><br>**secondary**: a secondary service<br><br>**Default**: primary                                                                                                                                                                                          |
| `advertise` | This field defines if the service UUID is included in the advertisement data.<br><br>The advertisement data can contain up to 13 16-bit UUIDs or one (1) 128-bit UUID.<br><br>**Values**:<br><br>**true**: UUID included in advertisement data<br><br>**false**: UUID not included in advertisement data<br><br>**Default**: false<br><br>**Note**: You can override the advertisement data with the GAP API, in which case this is not valid.   |

**示例**：一个 GAP（Generic Access Profile）Service 定义。

```xml
<!-- Generic Access Service -->
<service uuid="1800">
  …
</service>
```

**示例**：一个供应商特定的 Service 定义。

```xml
<!-- A vendor specific service -->
<service uuid="25be6a60-2040-11e5-bd86-0002a5d5c51b">
  …
</service>
```

**示例**：一个具有 UUID 的 Heart Rate Service 定义，包含广告数据和 ID “hrs”。

```xml
<!-- Heart Rate Service -->
<service uuid="180D" id="hrs" advertise=”true”>
  …
</service>
```

**注意**：您可以生成自己的 128-bit UUID：[http://www.itu.int/en/ITU-T/asn1/Pages/UUID/uuids.aspx](http://www.itu.int/en/ITU-T/asn1/Pages/UUID/uuids.aspx)

### 2.4.1 \<capabilities\>

Service 可以使用一个 \<capabilities\> 元素来声明其具有的 Capabilities。该元素由一系列 \<capability\> 元素组成，其标识符**也必须是** \<capabilities_declare\> 元素的**一部分**。属性 “enable” 对在此上下文中声明的 Capabilities 无效，因此可以将其排除。

如果一个 Service 未声明任何 Capabilities，则根据**继承规则**，它将具有 **\<capabilities_declare\>** 中的**所有 Capabilities**。

当 Service **启用**其**至少一个** Capabilities 时，该 Service 及其所有 Characteristics 将变为**可见**；**禁用**其**所有** Capabilities 时，该 Service 及其所有 Characteristics 将变为**不可见**。

**示例**：Capabilities 声明。

```xml
<capabilities>
  <capability>cap_light</capability>
  <capability>cap_color</capability>
</capabilities>
```

### 2.4.2 \<informativeText\>

XML 元素 \<informativeText\> 可以用于提供信息（注释），并且不会在实际的 GATT Database 中公开。

### 2.4.3 \<include\>

可以使用 XML 属性 **\<include\>** 将一个 Service 包含在另一个 Service 中。

| Parameter | Description                                                               |
| :-------- | :------------------------------------------------------------------------ |
| `id`      | ID of the included service<br><br>**Value**:<br><br>ID of another service |

**示例**：将 Heart Rate Service 包含在 GAP Service 中。

```xml
<!-- Generic Access Service -->
<service uuid="1800">
  <!-- Include HR Service -->
  <include id="hrs" />
  …
</service>
```

## 2.5 \<characteristic\>

Service 公开的所有 Characteristics 都是使用 XML 属性 **\<characteristic\>** 及其参数定义的，必须在 **\<service\>** XML 属性标签内使用它们。

下表描述了可用于定义相关值的参数。

| Parameter | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `uuid`    | Universally Unique Identifier. The UUID uniquely identifies a characteristic.<br><br>16-bit values are used for the services defined by the Bluetooth SIG and 128-bit UUIDs can be used for vendor specific implementations.<br><br>**Range**:<br><br>**0x0000 – 0xFFFF**: Reserved for Bluetooth SIG standardized characteristics.<br><br>**0x00000000-0000-0000-0000-000000000000 to 0xFFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFF**:<br><br>Reserved for vendor specific characteristics. |
| `id`      | The ID is used to identify a characteristic. The ID is used within a C application to read and write characteristic values or to detect if notifications or indications are enabled or disabled for a specific characteristic.<br><br>When the project is built, the generated GATT C header file contains a macro with the characteristic 'id' and corresponding handle value.<br><br>**Value**:<br><br>Any UTF-8 string                                                             |
| `const`   | Defines if the value stored in the characteristic is a constant.<br><br>**Default**: false                                                                                                                                                                                                                                                                                                                                                                                            |
| `name`    | Free text, not used by the database compiler.<br><br>**Value**: Any UTF-8 string<br><br>Default: Nothing                                                                                                                                                                                                                                                                                                                                                                              |

**示例**：将 Device Name Characteristic 添加到 GAP Service 中。

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

**示例**：将一个供应商特定的 Characteristic 添加到具有 ID 的供应商特定的 Service 中。

```xml
<!-- A vendor specific service -->
<service uuid="25be6a60-2040-11e5-bd86-0002a5d5c51b">

  <!-- My proprietary data -->
  <characteristic uuid="59cd69c0-2043-11e5-a717-0002a5d5c51b" id="mydata">
    …
  </characteristic>
  …
</service>
```

### 2.5.1 \<capabilities\>

Characteristic 可以使用一个 \<capabilities\> 元素声明其具有的 Capabilities。该元素由一系列 \<capability\> 元素组成，这些元素的标识符**必须被其父 Service 声明（或完全继承）**。属性 “enable” 对在此上下文中声明的 Capabilities 无效，因此可以将其排除。

如果一个 Characteristics 未声明任何 Capabilities，则它将根据**继承规则**拥有其所属 **Service** 中的**所有 Capabilities**。Characteristic 的所有 Attributes 都继承了 Characteristic 的 Capabilities。

当 Characteristic **启用**其**至少一个** Capabilities 时，该 Characteristic 及其所有 Attributes 将**可见**；**禁用**其**所有 Capabilities** 时，该 Characteristic 及其所有 Attributes 将**不可见**。

**示例**：Capabilities 声明。

```xml
<capabilities>
  <capability>cap_light</capability>
  <capability>cap_color</capability>
</capabilities>
```

### 2.5.2 \<properties\>

Characteristic 的访问属性及其权限级别由 XML 属性 **\<properties\>** 的子属性定义，这必须在 **\<characteristic\>** XML 属性标签内使用。一个 Characteristic 可以同时具有多个访问属性，例如，它可以是可读的、可写的、或两者都可以。每个访问属性可以具有不同的权限级别（如加密的或认证的）。

下表列出了可能的访问属性。表中的每一行在 **\<properties\>** 属性下定义了一个新属性。

| Attribute           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| :------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `read`              | Characteristic can be read by a remote device.<br><br>**Values**:<br><br>**true**: Characteristic can be read<br><br>**false**: Characteristic cannot be read<br><br>**Default**: false                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `write`             | Characteristic can be written by a remote device<br><br>**Values**:<br><br>**true**: Characteristic can be written<br><br>**false**: Characteristic cannot be written<br><br>**Default**: false                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `write_no_response` | Characteristic can be written by a remote device. Write without response is not acknowledged over the Attribute Protocol.<br><br>**Values**:<br><br>**true**: Characteristic can be written<br><br>**false**: Characteristic cannot be written<br><br>**Default**: false                                                                                                                                                                                                                                                                                                                                                                                                        |
| `notify`            | Characteristic has the notify property and characteristic value changes are notified over the Attribute Protocol. Notifications are not acknowledged over the Attribute Protocol.<br><br>**Values**:<br><br>**true**: Characteristic has notify property.<br><br>**false**: Characteristic does not have notify property.<br><br>**Default**: false<br><br>**Note**: The `notify` attribute is stored in the SIG defined Client Characteristic Configuration Descriptor (a descriptor with the UUID 0x2902, which will be autogenerated when notifications are enabled). If you manually add a CCCD to the characteristic, the descriptor’s value will overwrite this setting.  |
| `indicate`          | Characteristic has the indicate property and characteristic value changes are indicated over the Attribute Protocol. Indications are acknowledged over the Attribute Protocol.<br><br>**Values**:<br><br>**true**: Characteristic has indicate property.<br><br>**false**: Characteristic does not have indicate property.<br><br>**Default**: false<br><br>**Note**: The `indicate` attribute is stored in the SIG defined Client Characteristic Configuration Descriptor (a descriptor with the UUID 0x2902, which will be autogenerated when indications are enabled). If you manually add a CCCD to the characteristic, the descriptor’s value will overwrite this setting. |
| `reliable_write`    | Allows using a reliable write procedure to modify an attribute; this is just a hint to a GATT client. The Bluetooth stack always allows the use of reliable writes to modify attributes.<br><br>**Values**:<br><br>**true**: Reliable write enabled.<br><br>**false**: Reliable write disenabled.<br><br>**Default**: false                                                                                                                                                                                                                                                                                                                                                     |

下表列出了权限级别或安全要求。任何安全要求都可以分配给任何访问属性。

| Attribute       | Description                                                                                                                                                                                                                                                                                                                                                  |
| :-------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `authenticated` | Accessing the characteristic value requires an authentication. To access the characteristic with this property the remote device has to be bonded using MITM protection and the connection must be also encrypted.<br><br>**Values**:<br><br>**true**: Authentication is required<br><br>**false**: Authentication is not required<br><br>**Default**: false |
| `encrypted`     | Accessing the characteristic value requires an encrypted link. Devices with iOS 9.1 or higher must also be bonded at least with Just Works pairing.<br><br>**Values**:<br><br>**true**: Encryption is required<br><br>**false**: Encryption is not required<br><br>**Default**: false                                                                        |
| `bonded`        | Accessing the characteristic value requires an encrypted link. Devices must also be bonded at least with Just Works pairing.<br><br>**Values**:<br><br>**true**: Bonding and encryption are required<br><br>**false**: Bonding is not required<br><br>**Default**: false                                                                                     |

**示例**：具有 `const` 和 `read` 属性的 Device Name Characteristic。

```xml
<!-- Device Name -->
<characteristic const = true uuid="2a00">
  <properties>
    <read authenticated="false" bonded="false" encrypted="false"/>
  </properties>
</characteristic>
```

**示例**：具有 `read` 和 `write` 属性的 Device Name Characteristic，以允许远程设备修改该值。

```xml
<!-- Device Name -->
<characteristic uuid="2a00">
  <properties>
    <read authenticated="false" bonded="false" encrypted="false"/>
    <write authenticated="false" bonded="false" encrypted="false"/>
  </properties>
</characteristic>
```

**示例**：具有 `notify` 属性的 Heart Rate Measurement Characteristic。

```xml
<!-- Heart Rate Measurement -->
<characteristic uuid="180D">
  <properties>
    <notify authenticated="false" bonded="false" encrypted="false"/>
  </properties>
</characteristic>
```

**示例**：具有 `encrypted read` 属性的 Characteristic。

```xml
<!-- Device Name -->
<characteristic uuid="1234">
  <properties>
    <read authenticated="false" bonded="false" encrypted="true"/>
  </properties>
</characteristic>
```

**示例**：具有 `authenticated write` 属性的 Characteristic。

```xml
<!-- Device Name -->
<characteristic uuid="1234">
  <properties>
    <write authenticated="true" bonded="false" encrypted="false"/>
  </properties>
</characteristic>
```

**示例**：具有 `authenticated indicate` 特性的 Characteristic。

```xml
<!-- Descriptor value changed -->
<characteristic uuid="2A7D">
  <properties>
    <indicate authenticated="true" bonded="false" encrypted="false"/>
  </properties>
</characteristic>
```

### 2.5.3 \<value\>

Characteristic 的数据类型和长度由 XML 属性 **\<value\>** 及其参数定义，这必须在 **\<characteristic\>** XML 属性标签内使用。

下表描述了可用于定义相关值的参数。

| Parameter         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| :---------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `length`          | Defines a fixed length for the characteristic or the maximum length if **variable_length** is true. If length is not defined and there is a value (e.g. data exists inside \<value\>\</value\>), then the value length is used to define the length.<br><br>If both **length** and value are defined, then the following rules apply:<ol><li>If **variable_length** is false and **length** is bigger than the value's length, then the value will be padded with 0's at the end to match the attribute's **length**.</li><li>If **length** is smaller than the value's length, then the value will be clipped to match **length**, regardless of whether **variable_length** is true or false.</li></ol>**Range**:<br><br>**0 – 255**: Length in bytes if **type** is 'hex', 'utf-8', or 'user'<br><br>**0 – 512**: Length in bytes if **type** is 'user'<br><br>**Default**: 0 |
| `variable_length` | Defines that the value is of variable length. The maximum length must also be defined with the **length** attribute or by defining a value. If both **length** and value are defined, then the rules described in **length** apply.<br><br>**Values**:<br><br>**true**: Value is of variable length<br><br>**false**: Value has a fixed length<br><br>**Default**: false                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `type`            | Defines the data type.<br><br>**Values**:<br><br>**hex**: Value type is hex<br><br>**utf-8**: Value is a string<br><br>**user**: When the characteristic type is marked as type="user", the application is responsible for initializing the characteristic value and also providing it, for example, when read operation occurs. The Bluetooth stack does not initialize the value or automatically provide the value when it is being read. When this is set, the Bluetooth stack generates **gatt_server_user_read_request** or **gatt_server_user_write_request**, which must be handled by the application.<br><br>**Default**: utf-8                                                                                                                                                                                                                                        |

**示例**：具有 `notify` 属性和固定长度为 2 字节的 Heart Rate Measurement Characteristic。

```xml
<!-- Heart Rate Measurement -->
<characteristic uuid="180D">
  <value length="2" type="hex" variable_length = "false"/>
  <properties>
    <notify authenticated="false" bonded="false" encrypted="false"/>
  </properties>
</characteristic>
```

**示例**：一个具有可变长度的供应商特定的 Characteristic，最大长度为 20 字节。

```xml
<!-- My proprietary data -->
<characteristic uuid="59cd69c0-2043-11e5-a717-0002a5d5c51b" id="mydata">
  <value variable_length="true" length="20" type="hex" />
  <properties>
    <notify authenticated="false" bonded="false" encrypted="false"/>
  </properties>
</characteristic>
```

**示例**：Characteristic 的值和长度也可以通过在 \<value\> 标签内键入实际值来定义。

```xml
<characteristic const="true" id="device_name" name="Device Name" sourceId="org.bluetooth.characteristic.gap.device_name" uuid="2A00">
  <value length="17" type="utf-8" variable_length="false">EFR32 BGM111</value>
  <properties>
    <read authenticated="false" bonded="false" encrypted="false"/>
  </properties>
</characteristic>
```

在上面的示例中，值是 “**EFR32 BGM111**”，长度是 17 byte。

**示例**：定义 length 和 value，且 length **大于** value 的长度。

```xml
<!-- Device name -->
<characteristic uuid="2a00">
  <properties read="true" />
  <value type="hex" length="4" variable_length="false">0102</value>
</characteristic>
```

在上面的示例中，值将为 “**01020000**”，因为 **length** 大于 value 的长度，所以将使用 0 进行填充。

**示例**：定义 length 和 value，且 length **小于** value 的长度。

```xml
<!-- Device name -->
<characteristic uuid="2a00">
  <properties read="true" />
  <value type="hex" length="2" variable_length="false">01020304</value>
</characteristic>
```

在上面的示例中，值将为 “**0102**”，因为 **length** 小于 value 的长度，因此值将被裁剪以匹配 **length**。

### 2.5.4 \<descriptor\>

XML 元素 \<descriptor\> 可用于定义一个通用的 Characteristic Descriptor。

Descriptor 属性由 \<properties\> 元素定义，并且仅允许读取和/或写入访问。值由 \<value\> 元素定义，方式与 Characteristic 的值相同。

**注意**：如果您手动添加一个 Client Characteristic Configuration Descriptor（UUID：0x2902），其值将覆盖其 Characteristic 的 `notify`/`indicate` 属性。如果没有手动添加 CCCD，那么如果该 Characteristic 启用了通知或指示，那么它将自动生成。

**示例**：添加一个具有 UUID 2908 的 Characteristic Descriptor。

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

可以使用 XML 属性 \<description\> 定义 Characteristic 用户描述值，该属性必须在 \<characteristic\> XML 属性标签内使用。

Characteristic 用户描述是一个可选值。它暴露给远程设备，可用于例如提供对应用程序用户界面中所示 Characteristic 的用户友好描述。

**示例**：常量字符串 “Heart Rate Measurement”。

```xml
<characteristic uuid="2a37">
  <properties>
    <notify authenticated="false" bonded="false" encrypted="false"/>
  </properties>
  <description> Heart Rate Measurement </description>
</characteristic>
```

properties 元素可用于允许远程修改一个 Attribute。

**示例**：允许远程读取，但需要绑定才能写入。

```xml
<characteristic uuid="2a37">
  <properties>
    <read authenticated="false" bonded="false" encrypted="true"/>
    <write authenticated="false" bonded="true" encrypted="false"/>
  </properties>
</characteristic>
```

**注意**：如果一个描述是可写的，则 GATT 解析器会自动添加扩展的 properties 属性，并将 `writable_auxiliaries` 位设置为与 Bluetooth 兼容。

### 2.5.6 \<aggregate\>

XML 元素 \<aggregate\> 通过自动将 ID 转换为 Attribute Handle 来启用聚合的 Characteristic Format Descriptor 的创建。

Attribute ID 应该引用 Characteristic Presentation Format Descriptor。

**示例**：添加一个 Characteristic 聚合。

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

## 2.6 GATT Examples

**示例**：一个完整的 GAP Service，其 Device Name 和 Appearance Characteristic 为具有 `read` 属性的常量值。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<gatt>
  <!--Generic Access-->
  <service advertise="false" name="Generic Access" requirement="mandatory" sourceId="org.bluetooth.service.generic_access" type="primary" uuid="1800">
    <informativeText>Abstract: The generic_access service contains generic information about the device. All available Characteristics are readonly. </informativeText>

    <!--Device Name-->
    <characteristic const="true" id="device_name" name="Device Name" sourceId="org.bluetooth.characteristic.gap.device_name" uuid="2A00">
      <informativeText/>
      <value length="17" type="utf-8" variable_length="false">EFR32 BGM111</value>
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

**示例**：Link Loss 和 Immediate Alert Service。

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

**示例**：具有 Capabilities 的 GATT Database。

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

* 如果启用了 Capabilities cap_light 和 cap_color，那么整个 light_service 将可见。
* 如果禁用了 Capability cap_light，那么 Characteristic light_control 将不可见。
* 如果禁用了 Capability cap_color，那么 Characteristic color_control 将不可见。

# 3. Generating the Code Files

本文档的该节描述了如何使用 Simplicity Studio 或 bgbuild Python 脚本和您的 IDE 来生成代码文件。

## 3.1 Using Simplicity Studio

在 Simplicity Studio 中使用 GATT Configurator 时，每次保存配置时都会自动生成代码文件（gatt_db.c/gatt_db.h）。

## 3.2 Using bgbuild

代码文件可以独立于 IDE 生成，使用 SDK 中提供的 bgbuild Python 脚本：`C:\SiliconLabs\SimplicityStudio\v5\developer\sdks\gecko_sdk_suite\<SDK_version>\protocol\bluetooth\bin\gatt`

\<SDK_version\> 是当前安装的 SDK 版本，例如 v3.1。

该脚本需要安装 Python 3 和通过调用 “pip install jinja2” 来安装 Jinja2 包。该脚本只能解析使用 Simplicity Studio 5 / SDK v3.x 创建的或根据本用户指南手动编写的 GATT 配置。GATT Configurator 可以导入旧的 gatt.xml 格式。

强制参数：

* GATT XML 文件的路径，或用于查找 XML 文件的目录。用 “;” 分隔输入

可选参数：

* -h, --help：显示帮助信息
* -o OUTDIR, --outdir OUTDIR：已生成文件的输出目录

```pwsh
PS C:\SiliconLabs\SimplicityStudio\v5\developer\sdks\gecko_sdk_suite\v3.1\protocol\bluetooth\bin\gatt> C:/Users/baleidec/AppDatt/Local/Programs/Python/Python37/python.exe bgbuild.py C:/Users/baleidec/SimplicityStudio/v5_workspace/BG13_soc_empty/config/btconf/gatt_configuration.btconf -o C:/Users/baleidec/SimplicityStudio/v5_workspace/BG13_soc_empty/
```
