# AN1154: Using Tokens for Non-Volatile Data Storage (Rev. 0.9) <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. About Tokens](#2-about-tokens)
  - [2.1 Concept of Tokens](#21-concept-of-tokens)
  - [2.2 Types of Tokens: Manufacturing Tokens and Dynamic Tokens](#22-types-of-tokens-manufacturing-tokens-and-dynamic-tokens)
    - [2.2.1 Types of Dynamic Tokens: Basic Tokens and Indexed Tokens](#221-types-of-dynamic-tokens-basic-tokens-and-indexed-tokens)
  - [2.3 Types of Tokens: Default Tokens and Custom Tokens](#23-types-of-tokens-default-tokens-and-custom-tokens)
    - [2.3.1 Default Tokens](#231-default-tokens)
    - [2.3.2 Custom Tokens](#232-custom-tokens)
- [3. Creating and Accessing Tokens](#3-creating-and-accessing-tokens)
  - [3.1 Token Header Files](#31-token-header-files)
  - [3.2 Creating Dynamic Tokens](#32-creating-dynamic-tokens)
    - [3.2.1 Define the Token Name](#321-define-the-token-name)
    - [3.2.2 Define the Token Type](#322-define-the-token-type)
    - [3.2.3 Define the Token Storage](#323-define-the-token-storage)
  - [3.3 Accessing Dynamic Tokens With HAL APIs](#33-accessing-dynamic-tokens-with-hal-apis)
    - [3.3.1 Accessing Basic (Non-indexed) Tokens](#331-accessing-basic-non-indexed-tokens)
      - [3.3.1.1 Accessing Counter Tokens](#3311-accessing-counter-tokens)
    - [3.3.2 Accessing Indexed Tokens](#332-accessing-indexed-tokens)
  - [3.4 Accessing Dynamic Tokens with Token Manager APIs](#34-accessing-dynamic-tokens-with-token-manager-apis)
    - [3.4.1 Accessing Basic (Non-Indexed) Tokens](#341-accessing-basic-non-indexed-tokens)
      - [3.4.1.1 Accessing Counter Tokens](#3411-accessing-counter-tokens)
    - [3.4.2 Accessing Indexed Tokens](#342-accessing-indexed-tokens)
  - [3.5 Manufacturing Tokens](#35-manufacturing-tokens)
    - [3.5.1 Accessing Manufacturing Tokens](#351-accessing-manufacturing-tokens)
  - [3.6 Accessing Default and Custom Tokens](#36-accessing-default-and-custom-tokens)
    - [3.6.1 Where to Find Default Token Definitions](#361-where-to-find-default-token-definitions)
    - [3.6.2 Add Custom Tokens](#362-add-custom-tokens)
- [4. Token Manager Component](#4-token-manager-component)

本应用笔记介绍了 token，并展示如何在 EmberZNet PRO 和 Silicon Labs Connect（Flex SDK 的一部分）中将使用它来进行非易失性数据存储。

# 1. Introduction

本应用笔记说明了如何将 token 用于非易失性数据存储。首先，介绍 token 的概念、token 的不同类型以及如何从多个角度对 token 进行分类。然后，提供了有关如何从应用程序中创建和访问（即读取和写入）token 的实用说明。

阅读本应用笔记后，您应该了解什么是 token、使用 token 的原因和时机以及如何从某些类型的应用程序（当前为 EmberZNet PRO 和 Silicon Labs Connect（Flex SDK 的一部分））中创建和访问 token。

Gecko SDK Suite (GSDK) v3.x 引入了新的基础设施。作为该基础设施更新的一部分，用于访问和管理 token 的 HAL API 已被 Token Manager API 取代。在这整个过程中都会存在差异。

# 2. About Tokens

## 2.1 Concept of Tokens

一个 token 是一个对应用程序中具有特殊持久化含义的抽象数据常量。Token 用于在重新引导期间和断电期间保留某些重要数据。这些 token 存储在非易失性存储器中。Token 含有两个部分：token key 和 token data。Token key 是用于存储和检索 token data 的唯一标识符。在多数情况下，单词“token”被广泛地用于表示 token key、token data 或 key 与 data 的组合。通常，从上下文中可以清楚地看出要使用的含义。在本应用笔记中，“token”始终是指 key + data pair。

本节的其余部分从几个不同的角度审视 token 的分类。下图是 token 的“universe”概念说明，以及本应用笔记如何与非易失性数据存储上的其他文档相关。理解 token 的类型与理解如何创建和使用 token 有关，如 [3. Creating and Accessing Tokens](#3-Creating-and-Accessing-Tokens) 中所述。

<p>
    <img src="../images/AN1154/Figure%202.1.%20Non-volatile%20Data%20Storage%20and%20Tokens.png"
         alt="Figure 2.1. Non-volatile Data Storage and Tokens"
         title="Figure 2.1. Non-volatile Data Storage and Tokens">
</p>

## 2.2 Types of Tokens: Manufacturing Tokens and Dynamic Tokens

根据 token 的使用方式，token 的编写方式有所不同。

Manufacturing token 在芯片的生命周期内仅被写入一次或很少被写入，并且它们被存储在闪存（flash）的绝对地址中。有关制造和 token 编程的更多信息，请参阅文档 *AN710: Bringing up Custom Devices for the Ember® EM35x SoC or NCP Platform* 和 *AN961: Bringing up Custom Devices for the EFR32MG and EFR32FG Families* 。

Dynamic token 可以被频繁访问（读取和写入）。它们存储在闪存的一个专用区域中，该区域使用 memory-rotation 算法来防止闪存的过度使用。Silicon Labs 提供了三种不同的 dynamic token 实现：SimEEv1（Simulated EEPROM Version 1）、SimEEv2（Simulated EEPROM Version 2）和 NVM3（Third Generation Non-Volatile Memory）。有关非易失性数据存储概念的概述以及这三种实现的说明，请参阅 *UG103.7: Non-Volatile Data Storage Fundamentals* 。

与通用的 RAM 使用相比，dynamic token 系统的基本目的是允许 token data 在重新引导期间以及断电期间得以持久化。通过使用 token key 来标识正确的 data，请求 token data 的应用程序不需要知道数据的确切存储位置。这简化了应用程序的设计和代码重用。

因为 EM3x 和 EFR32 处理技术没有提供一个内部 EEPROM，所以实现了用于 dynamic token 的存储机制，以将内部闪存的一部分用于 stack 和 application token 的存储。对于 SimEEv1，EM35x 和 EFR32 Series 1 使用 8 kB 的上部（upper）闪存用于非易失性数据存储。SimEEv2 需要 36 kB 的上部闪存。使用 SimEEv2 需要 Silicon Labs 的 special key。这样做的目的是防止从 v1 到 v2 的意外升级。降级的唯一方法要求完全丢失数据，并且升级可能不会保留每个 token。使用 NVM3，可从 3 个闪存页（及以上）中配置存储大小。

使用 dynamic token 存储非易失性数据的器件具有不同的闪存性能（就保证的擦除周期而言）。EM35x-I 闪存可以保证 2,000 次擦除周期；其他 EM35x 闪存可以保证 20,000 次擦除周期；EFR32 闪存可以保证 10,000 次擦除周期，具体情况请参阅器件的数据手册。由于擦除周期的限制，dynamic token 存储机制实现了一种磨损平衡（wear-leveling）算法，该算法有效地扩展了单个 token 的擦除周期数。


Silicon Labs 建议应用设计者熟悉不同的 dynamic token 存储机制，以便他们设计的应用程序对 token 的使用达到最佳闪存擦除周期。有关 SimEEv1/v2 的更多信息，请参考文档 *AN703: Using Simulated EEPROM Version 1 and Version 2 for the EM35x and EFR32 Series 1 SoC Platforms* 。请注意，EFR32 Series 2 上未实现 SimEEv1/v2。有关 NVM3 的更多信息，请参考 *AN1135: Using Third Generation Non-Volatile Memory (NVM3) Data Storage* 。如果您不确定从何处开始并且正在进行 EFR32 开发，Silicon Labs 建议使用 NVM3。NVM3 更具可配置性（允许更好地平衡 token 容量和保留的闪存），并与 DMP 兼容（如果将来应用程序需要）。以下是有关机制特点的一些非常通用的准则：

* Simulated EEPROM version1：低数据需求，单一协议，EM35x 和 EFR32 Series 1。
* Simulated EEPROM version2：高数据需求，单一协议，EM35x 和 EFR32 Series 1。
* NVM3：高数据需求，动态多协议，EFR32 Series 2。

### 2.2.1 Types of Dynamic Tokens: Basic Tokens and Indexed Tokens

Dynamic token 有两种类型，其类型由其格式区分：

* **Non-indexed or basic** dynamic token。可以将它们视为简单的 char 变量类型。它们可以用于存储数组，但是如果一个元素发生更改，则必须重写整个数组。
  * Counter token 是一种特殊类型的 non-indexed dynamic token，用于存储每次递增 1 的数字。
* **Indexed** dynamic token。可以看作是 char 变量的链接数组，其中每个元素都希望独立于其他元素进行更改，因此可以在内部作为独立 token 存储并通过 token API 进行显式访问。

有关 basic token 和 indexed token 的更多信息，请参阅 *AN703: Using Simulated EEPROM Version 1 and Version 2 for the EM35x and EFR32 Series 1 SoC Platforms* 和 *AN1135: Using Third Generation Non-Volatile Memory (NVM3) Data Storage* 。

## 2.3 Types of Tokens: Default Tokens and Custom Tokens

根据 token 是由 Silicon Labs 作为网络协议栈的一部分提供还是由用户创建的，token 也可以分为 default token 和 custom token。

### 2.3.1 Default Tokens

网络栈包含按其软件用途分组的 default token：

* **Manufacturing Token** 是在制造时设置的，不能由应用程序更改。
* **Stack Token** 是协议栈设置的运行时配置选项。这些 dynamic token 不应由应用程序更改。
* **Application Framework Token** 是 Application Framework 使用的 application token，由 AppBuilder 在 Connect v2.7.x 和 EmberZNet SDK v6.7.x/v6.8.x 中生成。在项目生成后，应用程序不应更改这些 dynamic token。这些的示例包括 ZCL attribute token 和 plugin token。

### 2.3.2 Custom Tokens

除了 default token，用户可以添加特定于其应用程序的以下类型的 token：

* **Custom Manufacturing Token** 由用户定义，并在制造时设置。
* **Custom Application Token** 是用户可以添加的额外的 application token，以满足其对非易失性数据存储的独特应用程序需求。

# 3. Creating and Accessing Tokens

现在您已经了解 token 是什么，以及根据不同角度对 token 进行分类的几种方法，您可以学习如何使用 token。这包括知道在哪里可以找到 default token，如何创建新 token 以及如何读取和可能地修改 token。请记住，这些方法可能会因所涉及的 token 类型而异。

## 3.1 Token Header Files

Token 头文件是包含 token 定义的 .h 文件。Manufacturing token 和 dynamic token 具有单独的 token 头文件。一个应用程序中可能有多个用于 dynamic token 的头文件：一个用于 stack token、可变数量个用于 application framework token、可能一个用于 custom application token。

## 3.2 Creating Dynamic Tokens

将一个 dynamic token 添加到头文件涉及三个步骤：

1. 定义 token 名。
2. 如果 token 使用的是一个 application-defined 类型，则需要添加该 token 所需的所有 typedef。
3. 定义 token 存储。

本节的其余部分将一次性查看每个步骤。

### 3.2.1 Define the Token Name

在定义名称时，不要在前面添加单词 `TOKEN` 。对于 SimEEv1/v2 dynamic token，使用单词 `CREATOR` ：

```c
/**
 * Custom Application Tokens
 */
// Define token names here
#define CREATOR_DEVICE_INSTALL_DATA  (0x000A)
#define CREATOR_HOURLY_TEMPERATURES  (0x000B)
#define CREATOR_LIFETIME_HEAT_CYCLES (0x000C)
```

对于 NVM3 dynamic token，使用单词 `NVM3KEY` 。请注意，以下示例假定为一个 Zigbee 应用程序。对于不同的协议栈，NVM3 域（domain）是不同的。另请注意，`HOURLY_TEMPERATURES` 的 `NVM3KEY` 值被设置为一个值，其后面的 0x7F 个值未被使用，因为这是一个 indexed token。有关 NVM3 默认实例键空间的详情以及为 indexed token 选择 `NVM3KEY` 值的限制条件，请参阅 *AN1135: Using Third Generation Non-Volatile Memory (NVM3) Data Storage* 。

```c
/**
 * Custom Zigbee Application Tokens
 */
// Define token names here
#define NVM3KEY_DEVICE_INSTALL_DATA  (NVM3KEY_DOMAIN_USER | 0x000A)
// This key is used for an indexed token and the subsequent 0x7F keys are also reserved
#define NVM3KEY_HOURLY_TEMPERATURES  (NVM3KEY_DOMAIN_USER | 0x1000)
#define NVM3KEY_LIFETIME_HEAT_CYCLES (NVM3KEY_DOMAIN_USER | 0x000C)
```

这些示例定义了 token key 并将其链接到一个程序化变量。Token 的名称实际上是 `DEVICE_INSTALL_DATA` 、 `HOURLY_TEMPERATURES` 和 `LIFETIME_HEAT_CYCLES` 。根据使用情况，将不同的标签添加到名称的前面。因此，在示例代码中将它们参考为 `TOKEN_DEVICE_INSTALL_DATA` 等。

Token key 值在此设备中必须是唯一的。Token key 是应用程序的使用与对应数据的链接的关键，因此在定义新 token 甚至在更改现有 token 的结构时，应该始终保证 token key 的唯一性。 `CREATOR` 代码值为 16-bit， `NVM3KEY` 代码值为 20-bit。对于 SimEEv1/v2，第一位预留给 manufacturing token、stack token 和 Application Framework 定义的 application token，因此所有 custom token 的 token key 都应该小于 0x8000。

对于 NVM3，custom token 应该使用 `NVM3KEY_DOMAIN_USER` 范围，以避免与 `NVM3KEY_DOMAIN_ZIGBEE` 范围内的 stack token 发生冲突。

### 3.2.2 Define the Token Type

Token 类型可以是内置的 C 数据类型，也可以是使用 `typedef` 为自定义数据结构定义的类型。请注意，token 类型只能在一个地方定义，如果定义了两次相同的数据结构，编译器就会抱怨。

[3.2 Creating Dynamic Tokens](#3-2-Creating-Dynamic-Tokens) 中的代码示例中的每个 token 都是不同的类型。 `HOURLY_TEMPERATURES` 和 `LIFETIME_HEAT_CYCLES` 的类型是 C 中的内置类型，而 `DEVICE_INSTALL_DATA` 的类型是自定义数据结构：

```c
#ifdef DEFINETYPES
// Include or define any typedef for tokens here
typedef struct {
    int8u install_date[11] /** YYYY-mm-dd + NULL */
    int8u room_number;     /** The room where this device is installed */
} InstallationData_t;
#endif //DEFINETYPES
```

### 3.2.3 Define the Token Storage

在定义了任何自定义类型后，可以定义 token 的存储。这将告知 token 管理软件已定义的 token。每个 token，无论是 custom 还是 default，在这一部分中都拥有自己的条目：

```c
#ifdef DEFINETOKENS
// Define the actual token storage information here
DEFINE_BASIC_TOKEN(DEVICE_INSTALL_DATA,
                   InstallationData_t,
                   {0, {0,...}})
DEFINE_INDEXED_TOKEN(HOURLY_TEMPERATURES, int16u, HOURS_IN_DAY, {0,...})
DEFINE_COUNTER_TOKEN(LIFETIME_HEAT_CYCLES, int32u, 0}
#endif //DEFINETOKENS
```

下面将详细介绍此过程中的每个步骤。

```c
DEFINE_BASIC_TOKEN(DEVICE_INSTALL_DATA,
                   InstallationData_t,
                   {0, {0,...}})
```

`DEFINE_BASIC_TOKEN` 有三个参数：名称（ `DEVICE_INSTALL_DATA` ）、数据类型（ `InstallationData_t` ）以及 token 的默认值（ `{0, {0, ...}}` ）（如果它从来不被应用程序写入）。

默认值采用与 C 默认初始化相同的语法。在这种情况下，第一个值（ `room_number` ）被初始化为 0，下一个值（ `install_date` ）被设置为全 0，因为 `{0, ...}` 语法用 0 填充数组的其余部分。

`DEFINE_COUNTER_TOKEN` 的语法与 `DEFINE_BASIC_TOKEN` 相同。

`DEFINE_INDEXED_TOKEN` 需要数组的长度（在本例中为 `HOURS_IN_DAY` 或 24）。其最终参数是数组中每个元素的默认值。同样，在本例中，它被初始化为全 0。

## 3.3 Accessing Dynamic Tokens With HAL APIs

如果要使用 GSDK v2.7x 中的任何 EmberZNet 或 Connect 以及 GSDK v3.0 中的 EmberZNet 进行开发，请参考此信息。

网络栈提供了一组简单的 API，用于访问 token data。根据 token 的类型，API 略有不同。本节的其余部分讨论如何根据类型访问 token。您可以在 stack API Reference 中找到完整的文档。

### 3.3.1 Accessing Basic (Non-indexed) Tokens

Non-indexed/basic token API 函数包括：

```c
void halCommonGetToken(data, token)
void halCommonSetToken(token, data)
```

在这种情况下，“token”是 token key，“data”是 token data。请注意， `halCommonGetToken()` 和 `halCommonSetToken()` 是可用于 basic dynamic token 和 manufacturing token 的 general token API（请参阅 [3.5.1 Accessing Manufacturing Tokens](#3-5-1-Accessing-Manufacturing-Tokens) ）。


以下示例说明了这些 API 的用法。假设应用程序需要在安装时存储配置数据。已经定义了一个 basic token 以使用 token key `DEVICE_INSTALL_DATA` ，数据结构如下所示：

```c
typedef struct {
    int8u install_date[11] /** YYYY-mm-dd + NULL */
    int8u room_number;     /** The room where this device is installed */
} InstallationData_t;
```

然后您可以使用这样的代码片段来访问它：

```c
InstallationData_t data;
// Read the stored token data
halCommonGetToken(&data, TOKEN_DEVICE_INSTALL_DATA);
// Set the local copy of the data to new values
data.room_number = < user input data >
MEMCOPY(data.install_date, < user input data>, 0, sizeof(data.install_date));
// Update the stored token data with the new values
halCommonSetToken(TOKEN_DEVICE_INSTALL_DATA, &data);
```

#### 3.3.1.1 Accessing Counter Tokens

有一个 special API 可递增 counter token（这是 non-indexed token 的一种特殊类型）：

```c
void halCommonIncrementCounterToken(token)
```

请注意，尽管您可以使用通用的 `halCommonSetToken()` 调用来写入 counter token，但这样做效率低下，并且违背了使用 counter token 的目的。

以下示例说明了 counter token 和 special API 递增 counter token 的用法。计算恒温器已启动的加热循环次数非常适合使用 counter token。假设它名为 `LIFETIME_HEAT_CYCLES` ，并且它是一个 int32u 数据类型。

```c
void requestHeatCycle(void) {
    /// < application logic to initiate heat cycle >
    halCommonIncrementCounterToken(TOKEN_LIFETIME_HEAT_CYCLES);
}

int32u totalHeatCycles(void) {
    int32u heatCycles;
    halCommonGetToken(&heatCycles, TOKEN_LIFETIME_HEAT_CYCLES);
    return heatCycles;
}
```

请注意，要读取 counter token，请使用 `halCommonGetToken()` ，就像读取常规 basic token 一样。

### 3.3.2 Accessing Indexed Tokens

Indexed token API 函数包括：

```c
void halCommonGetIndexedToken(data, token, index)
void halCommonSetIndexedToken(token, index, data)
```

以下示例说明了这些 API 用于 indexed token 的用法。要存储一组相似的值，例如一天中的首选温度设置数组，可以使用默认数据类型 int16s 来存储所需的温度并定义一个称为 `HOURLY_TEMPERATURES` 的 indexed token。

整个数据集的本地副本如下所示：

```c
int16s hourlyTemperatures[HOURS_IN_DAY]; /** 24 hours per day */
```

在应用代码中，您可以使用 indexed token 函数在一天中仅访问或更新一个值：

```c
int16s getCurrentTargetTemperature(int8u hour) {
    int16s temperatureThisHour = 0; /** Stores the temperature for return */
    if (hour < HOURS_IN_DAY) {
        halCommonGetIndexedToken(&temperatureThisHour,
        TOKEN_HOURLY_TEMPERATURES, hour);
    }
    return temperatureThisHour;
}

void setTargetTemperature(int8u hour, int16s targetTemperature) {
    if (hour < HOURS_IN_DAY) {
        halCommonSetIndexedToken(TOKEN_HOURLY_TEMPERATURE, hour,
                                 &temperatureThisHour);
    }
}
```

## 3.4 Accessing Dynamic Tokens with Token Manager APIs

如果要在 GSDK v3.0 中使用 Connect 进行开发，请参考此信息。

网络栈提供了一组简单的 API，用于访问 token data。根据 token 的类型，API 略有不同。本节的其余部分讨论如何根据类型访问 token。您可以在 stack API Reference 中找到完整的文档。

### 3.4.1 Accessing Basic (Non-Indexed) Tokens

Non-indexed/basic token API 函数包括：

```c
Ecode_t sl_token_get_data(uint32_t token,
                          uint32_t index,
                          void *data,
                          uint32_t length);

Ecode_t sl_token_set_data(uint32_t token,
                          uint32_t index,
                          void *data,
                          uint32_t length);
```

在这种情况下，“token”是 token key，non-indexed/basic token 的“index”是 1，“data”是 token data，“length”是数据字节的大小。请注意， `sl_token_get_data()` 和 `sl_token_set_data()` 专用于 basic dynamic token。有关 manufacturing token，请参阅 [3.5.1 Accessing Manufacturing Tokens](#3-5-1-Accessing-Manufacturing-Tokens) 。

以下示例说明了这些 API 的用法。假设应用程序需要在安装时存储配置数据。已经定义了一个 basic token 以使用 token key `DEVICE_INSTALL_DATA` ，数据结构如下所示：

```c
typedef struct {
    int8u install_date[11] /** YYYY-mm-dd + NULL */
    int8u room_number;     /** The room where this device is installed */
} InstallationData_t;
```

然后，您可以使用如下代码段访问它：

```c
InstallationData_t data;
// Read the stored token data
sl_token_get_data(TOKEN_DEVICE_INSTALL_DATA, 1, &data, sizeof(data));
// Set the local copy of the data to new values data.room_number = < user input data >
MEMCOPY(data.install_date, < user input data>, 0, sizeof(data.install_date));
// Update the stored token data with the new values
sl_token_set_data(TOKEN_DEVICE_INSTALL_DATA, 1, &data, sizeof(data));
```

#### 3.4.1.1 Accessing Counter Tokens

有一个 special API 可递增 counter token（这是 non-indexed token 的一种特殊类型）：

```c
Ecode_t sl_token_increment_counter(uint32_t token);
```

请注意，尽管您可以使用通用的 `sl_token_set_data()` 调用来写入 counter token，但这样做效率低下，并且违背了使用 counter token 的目的。

以下示例说明了 counter token 和 special API 递增 counter token 的用法。计算恒温器已启动的加热循环次数非常适合使用 counter token。假设它名为 `LIFETIME_HEAT_CYCLES` ，并且它是一个 int32u 数据类型。

```c
void requestHeatCycle(void) {
    /// < application logic to initiate heat cycle >
    sl_token_increment_counter(TOKEN_LIFETIME_HEAT_CYCLES);
}

int32u totalHeatCycles(void) {
    int32u heatCycles;
    sl_token_get_data(TOKEN_LIFETIME_HEAT_CYCLES, 1, &heatCycles, sizeof(heatCycles));
    return heatCycles;
}
```

请注意，要读取 counter token，请使用 `sl_token_get_data()` ，就像读取常规 basic token 一样。

### 3.4.2 Accessing Indexed Tokens

Indexed token API 函数与 non-indexed/basic token API 函数相同。唯一的区别是“index”参数可以不是 1。

以下示例说明了这些 API 用于 indexed token 的用法。要存储一组相似的值，例如一天中的首选温度设置数组，可以使用默认数据类型 int16s 来存储所需的温度并定义一个称为 `HOURLY_TEMPERATURES` 的 indexed token。

整个数据集的本地副本如下所示：

```c
int16s hourlyTemperatures[HOURS_IN_DAY]; /** 24 hours per day */
```

在应用代码中，您可以使用 indexed token 函数在一天中仅访问或更新一个值：

```c
int16s getCurrentTargetTemperature(int8u hour) {
    int16s temperatureThisHour = 0; /** Stores the temperature for return */
    if (hour < HOURS_IN_DAY) {
        sl_token_get_data(TOKEN_HOURLY_TEMPERATURES, hour, &temperatureThisHour, sizeof(temperatureThisHour));
    }
    return temperatureThisHour;
}

void setTargetTemperature(int8u hour, int16s targetTemperature) {
    if (hour < HOURS_IN_DAY) {
        sl_token_set_data(TOKEN_HOURLY_TEMPERATURE, hour, &temperatureThisHour, sizeof(temperatureThisHour));
    }
}
```

## 3.5 Manufacturing Tokens

Manufacturing token 的定义与 basic (non-indexed) dynamic token 的定义类似，但是在 manufacturing token 头文件中使用 `DEFINE_MFG_TOKEN` ，而不是其他 `DEFINE_*_TOKEN` 宏。请注意，即使将 NVM plugin 用于非易失性存储，也应始终将 `CREATOR_*` 前缀（而非 `NVM3KEY_*` ）与 manufacturing token 一起使用。有关更多信息，请参阅 [3.2 Creating Dynamic Tokens](#3-2-Creating-Dynamic-Tokens) 和 `DEFINE_MFG_TOKEN` 宏上的 API 文档。

Manufacturing token 的主要差异在于存储位置和访问方式上。Manufacturing token 位于 manufacturing token 的专用闪存页中（具有固定的绝对地址）。本节的其余部分描述访问 manufacturing token 的注意事项。

### 3.5.1 Accessing Manufacturing Tokens

顾名思义，manufacturing token 通常在制造时一次性写入到专用闪存页的固定位置中。由于它们的地址是固定的，因此可以轻松地从外部编程工具中读取它们。但是请注意，当芯片或专用闪存页上启用了 Read
Protection 特性时，就只能从片上代码读取 manufacturing token。

Manufacturing token 不应该经常被写入或使用片上代码写入，因为它们位于固定位置。如果写入之间没有擦除操作，则无法重复写入同一闪存单元。仅当 token 当前处于已擦除状态时，才可以从片上代码写入 manufacturing token。这意味着 manufacturing token 只能用外部编程工具覆写，而不能用片上代码覆写。覆写所有已写入的 manufacturing token 需要擦除 manufacturing token 的专用闪存页。如果启用了 Read Protection，则必须先将其禁用，否则会擦除芯片中的内容。Manufacturing token 没有磨损均衡，因此应谨慎覆写。

Manufacturing token 具有自己专用的 API，这些 API 与 basic token API 的参数相同：

* HAL： `halCommonGetMfgToken()` 和 `halCommonSetMfgToken()`
* Token Manager： `sl_token_get_manufacturing_data()` 和 `sl_token_set_manufacturing_data()`

使用专用的 manufacturing token access API 的两个主要目的是：

1. 稍微加快访问速度；
2. 在引导过程（调用 `emberInit()` 或 `sl_token_init()` 之前）的早期进行访问。

如果您使用的是 HAL API，则还可以通过 basic token API： `halCommonGetToken()` 和 `halCommonSetToken()` 访问 manufacturing token。

## 3.6 Accessing Default and Custom Tokens

### 3.6.1 Where to Find Default Token Definitions

要查看 stack token，请参考文件：

```
<install-dir>/stack/config/token-stack.h
```

若要查看 Application Framework token，请参考文件 `<project_name>_tokens.h` 以及在 Connect v2.7 和 EmberZNet SDK v6.7.x/v6.8.x 中的 AppBuilder 中生成项目后，项目目录下的 protocol-specific token 文件（如 `znet-token.h` ）。 `<project_name>_tokens.h` 包含应用程序已选择存储在非易失性存储器中的 ZCL attribute 的 token。Protocol-specific token 文件包括 plugin token 头文件和 custom application token 头文件。

要查看 EFR32 或 EM3x 系列芯片的 manufacturing token，请分别参考以下文件：

```
<install-dir>/hal/micro/cortexm3/efm32/token-manufacturing.h
<install-dir>/hal/micro/cortexm3/token-manufacturing.h
```

搜索 `CREATOR` 以查看定义的名称。如果整个文件看起来很巨大，那么请仅关注描述 token 的部分。创建板时，制造商可能会设置一些固定的 manufacturing token。例如，供应商可以设置自定义的 EUI-64 地址，以覆盖 Silicon Labs 提供的内部 EUI-64 地址。其他 token（如内部 EUI-64）不能被覆写。

### 3.6.2 Add Custom Tokens

有关用于创建 custom token 的方法和 API，请参阅 [3.2 Creating Dynamic Tokens](#3-2-Creating-Dynamic-Tokens) 。您还可以参考 [3.6.1 Where to Find Default Token Definitions](#3-6-1-Where-to-Find-Default-Token-Definitions) 顶部附近提到的 stack token 定义文件，以作为创建 custom application token 的指南。

在 custom token 头文件中创建 token 后，您还需要执行一个步骤：使用 Simplicity Studio 中（.isc 文件） **Includes** 选项卡下的“Token Configuration”部分，将头文件添加到应用程序中。如果您同时定义了 custom manufacturing token 和 custom application token，Silicon Labs 建议您将它们分成两个不同的头文件。

> 注意：对于 custom token 文件，不应使用在 `token-stack.h` 中看到的包含保护（ `#ifndef` ）。

# 4. Token Manager Component

要使用 Token Manager，应将 token_manager 组件（token_manager.slcc）添加到项目中。token_manager 组件将默认使用 token_manager_nvm3 组件，该组件将引入 NVM3 系统。强烈建议使用 NVM3 进行存储。

要使用 Simulated EEPROM v1 或 v2（SimEEv1、SimEEv2）进行存储，组件 token_manager_simee1 或 token_manager_simee2 应该与 token_manager 一起成为项目的一部分。

要将 SimEEv2 升级到 NVM3，应将 sim_eeprom2_to_nvm3_upgrade 组件与 token_manager 一起使用。此升级组件将引入必要的 SimEE 和 NVM3 代码。
