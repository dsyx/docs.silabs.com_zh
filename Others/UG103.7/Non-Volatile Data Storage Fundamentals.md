# UG103.7: Non-Volatile Data Storage Fundamentals (Rev. 1.7) <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Implementations of Non-Volatile Data Storage](#2-implementations-of-non-volatile-data-storage)
  - [2.1 Basic Implementations](#21-basic-implementations)
  - [2.2 Handling Resets and Power Failures](#22-handling-resets-and-power-failures)
  - [2.3 Introducing Virtual Pages](#23-introducing-virtual-pages)
  - [2.4 Basic Storage](#24-basic-storage)
  - [2.5 FIFO Model](#25-fifo-model)
  - [2.6 Counter Objects](#26-counter-objects)
  - [2.7 Indexed Objects](#27-indexed-objects)
- [3. Dynamic Data Storage Implementations](#3-dynamic-data-storage-implementations)
  - [3.1 SimEEv1/v2](#31-simeev1v2)
  - [3.2 PS Store](#32-ps-store)
  - [3.3 NVM3](#33-nvm3)
- [4. Comparing Non-Volatile Data Storage Implementations](#4-comparing-non-volatile-data-storage-implementations)
  - [4.1 Flash Lifetime](#41-flash-lifetime)

---

本文档提供了使用闪存的非易失性数据存储的大致介绍，重点介绍了为 Silicon Labs 的 Microcontroller 和 Radio SoC（System on Chip）提供的三种不同的动态数据存储实现。提供了三种实现的比较，并提供了何时使用哪种实现的建议。有关使用各种数据存储实现的更多详细信息，请参阅以下文档：

* *AN1154: Using Tokens for Non-Volatile Data Storage*
* *AN703: Using Simulated EEPROM Version 1 and Version 2 for the EM35x and EFR32 SoC Series 1 Platforms*
* *AN1135: Using Third Generation Non-Volatile Memory (NVM3) Data Storage*

# 1. Introduction

NVM（Non-Volatile Memory，非易失性存储器）是一种即使设备重启仍然能保持数据的存储器。在 Silicon Labs 的 Microcontroller 和 Radio SoC 上，NVM 由闪存实现。在许多应用程序中，闪存不仅用于存储应用程序代码，还用于存储应用程序频繁读写的数据对象。由于闪存的擦除次数有限，因此存在多种方法来有效地降低闪存的磨损速度。

某些数据被认为是在制造时仅写入一次的制造数据。本文档关注的是在产品生命周期内频繁更改的动态数据。

本文档介绍了 Microcontroller 和 Radio SoC 中动态数据存储的主要设计选项，以及影响闪存寿命的因素。此外，它还介绍了 Silicon Labs 提供的主闪存数据存储实现：

* NVM3（Third Generation Non-Volatile Memory）
* SimEEv1（Simulated EEPROM version 1）和 SimEEv2（Simulated EEPROM version 2）
* PS Store（Persistent Store）

# 2. Implementations of Non-Volatile Data Storage

本章介绍了在闪存中实现非易失性数据存储时的一些挑战和设计选项。它描述了如何在闪存中以 PS Store、SimEEv1/v2 和 NVM3 方式实现非易失性数据存储。

## 2.1 Basic Implementations

闪存的一个特征是它可以在较小的块中写入，通常是 32-bit 字（Word），但是它只能以较大的块（通常是几千字节的页）擦除。使用闪存作为数据存储时，最直接的实现选项是将每个数据对象存储在各自的闪存页中，如下图所示。这样，每个对象都可以被擦除和重写，而不会影响其他数据对象。通常，数据对象比页的大小小得多，并且此解决方案不能很好地利用可用的闪存空间。

<figure>
  <img src="images/Figure%202.1.%20One%20Object%20Per%20Page.png"
       alt="Figure 2.1. One Object Per Page"
       style="display:block; margin: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 2.1. One Object Per Page</figcaption>
</figure>

为了避免浪费闪存空间，我们可以在一个闪存页中存储多个数据对象，如下图所示。当我们想要将一个新值写入到其中的一个数据对象时，该解决方案会引入一个挑战。在这种情况下，必须先擦除页，然后再将所有对象写回到页中，包括我们更改的对象。由于闪存只能承受有限数量的擦除，因此该解决方案限制了设备的寿命。

<figure>
  <img src="images/Figure%202.2.%20Multiple%20Objects%20in%20One%20Flash%20Page.png"
       alt="Figure 2.2. Multiple Objects in One Flash Page"
       style="display:block; margin: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 2.2. Multiple Objects in One Flash Page</figcaption>
</figure>

为了避免为每个对象的写入擦除闪存页，我们可以将每个对象的新版本写入到闪存页的新的空位置中。这是一种简单的磨损均衡方式，可减少页擦除次数。然而，这要求我们存储一些标识信息以及告诉我们数据属于哪个对象，这样我们就知道如何找到数据对象的最新版本。如下图所示，在每个版本的对象数据中都添加一个 key 来标识数据属于哪个对象。当访问对象时，我们需要在闪存页中搜索该对象的最新版本。在这种情况下，最新版本是具有最高地址的版本，因为我们从页的最低地址开始写入。

<figure>
  <img src="images/Figure%202.3.%20Object%20Versions%20With%20Keys.png"
       alt="Figure 2.3. Object Versions With Keys"
       style="display:block; margin: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 2.3. Object Versions With Keys</figcaption>
</figure>

## 2.2 Handling Resets and Power Failures

由于我们用新版本的数据对象填充闪存页，最终将没有空间来写入新的对象数据。此时我们需要擦除页并重新开始，只将每个对象的最新版本写回闪存页。然而，在许多应用中，电源故障或复位可能随时发生，如果发生这种情况，我们不应该冒丢失数据的风险。如果在擦除闪存页和写回数据对象之间发生复位，那么我们将丢失数据。为了处理这种情况，我们在删除原始页之前引入第二页，并将数据对象的最新版本复制到该页，如下图所示。然后我们可以开始用数据填充第二页。当第二页已满时，我们将最新数据移回第一页，依此类推。这种机制，即存储在两个闪存页之间交替，这就是 PS Store 的操作方式。

<figure>
  <img src="images/Figure%202.4.%20Latest%20Data%20Copied%20to%20New%20Page%20Before%20Erase.png"
       alt="Figure 2.4. Latest Data Copied to New Page Before Erase"
       style="display:block; margin: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 2.4. Latest Data Copied to New Page Before Erase</figcaption>
</figure>

## 2.3 Introducing Virtual Pages

在某些应用中，我们频繁地写入数据对象，因此也需要频繁地擦除闪存页。由于到目前为止实现中的数据对象仅分布在两个闪存页上，因此每个页将频繁地被擦除并且闪存寿命将受到限制。为了延长使用寿命，我们可以使用更多的闪存页来存储数据对象。在本例中，我们使用两个虚拟页（A 和 B）进行操作，每个虚拟页由多个物理闪存页组成。虚拟页被擦除和写入，就像它们是一个较大的闪存页一样。不同之处在于每个虚拟页都更大，我们可以在需要擦除虚拟页之前写入更多数据，从而延长了寿命。除了增加闪存寿命外，每个虚拟页使用多个闪存页还可以存储更多或更大的对象。SimEEv1 使用此设计，每个虚拟页由两个闪存页（A 和 B）组成，如下图所示。

<figure>
  <img src="images/Figure%202.5.%20Virtual%20Pages.png"
       alt="Figure 2.5. Virtual Pages"
       style="display:block; margin: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 2.5. Virtual Pages</figcaption>
</figure>

在某些应用中，必须最小化写入非易失性数据对象所花费的时间，以免干扰其他关键操作的时序。如果在虚拟页已满时触发了对象写入，则必须先将所有对象的最新版本复制到新的虚拟页，然后再将相关新对象版本写入到新页。必须立即复制所有对象以便快速擦除第一页，这样我们就可以在发生故障时将数据迁移到那里。一次复制所有对象会增加最坏情况下的对象写入时间。

为了减少写入时间，可以引入始终保持擦除的第三个虚拟页。当第一页已满时，我们可以只复制一些对象，而不是复制每个对象的最新版本。其余的对象将作为后续写入操作的一部分进行复制。这样，我们通过更多的写操作将复制过程扩展到新页，因此每次写操作只需要较少的时间来完成。通过这种方法，我们将实时对象数据分布在两个虚拟页上，并且第三页始终保持擦除，因此我们可以在发生故障时将数据迁移到某处。SimEEv2 将使用此带有 3 个虚拟页的实现，其中每个虚拟页由 6 个闪存页组成。

## 2.4 Basic Storage

基本存储定义为所有对象的所有最新版本的大小，包括与它们一起存储的任何开销。除 NVM3 外，每次擦除闪存页或虚拟页时，我们必须先将基本存储迁移到新页。基本存储大小很重要，因为它决定了在页中保留多少闪存空间来存储对象数据的任何新版本。如果基本存储占用了几乎整个页，那么在我们需要迁移到新页并擦除旧页之前，我们只能写入很少的对象版本。这会导致页擦除频繁从而降低闪存寿命。

NVM3 仅复制将被擦除的页中存储的唯一对象。如果其他页中存在该对象的较新版本，则不会复制该对象。

## 2.5 FIFO Model

闪存数据存储实现可以建模为一个 FIFO（First-In First-Out）缓冲区（见下图），其中我们向 FIFO 的输入写入新版本。当 FIFO 填满时，我们需要通过擦除 FIFO 末尾的一个或多个页来释放空间。在擦除页之前，我们需要复制其他闪存页中没有相同对象的较新版本的任何对象版本。因为存在较新的版本，所以可以丢弃其他对象数据。为了最大化闪存寿命，我们希望尽可能少地复制对象版本，以便大多数对闪存的写入都是对象的新版本。如果 FIFO 是在一个大的闪存空间上实现的，则更有可能是已经写入了对象的新版本，并且可以丢弃 FIFO 末尾的对象版本。在这种情况下，可以在很少或没有复制对象版本的情况下擦除闪存页。

<figure>
  <img src="images/Figure%202.6.%20FIFO%20Buffer.png"
       alt="Figure 2.6. FIFO Buffer"
       style="display:block; margin: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 2.6. FIFO Buffer</figcaption>
</figure>

使用少量虚拟页的缺点是 FIFO 的可用存储仅限于可以保存实时数据的虚拟页。对于使用两个虚拟页（如 SimEEv1）的实现，这意味着只有一半的存储空间用于 FIFO；而对于 SimEEv2，则有三分之二的存储空间可用于 FIFO。

为了允许更高比例的存储空间用于 FIFO，我们可以在分配的整个闪存存储空间上将 FIFO 实现为一个循环缓冲区，如下图所示。在此实现中，我们总是需要在缓冲区的前沿保留足够的已擦除页，以便在发生故障时写入最大的对象。当 FIFO 填充到要擦除闪存页的临界数量时，我们会复制任何尚未被取代的对象版本并擦除 FIFO 后面的页。这意味着，我们不需要擦除整个虚拟页面，只需擦除足够的空间来容纳最大的对象。我们可以将剩余的空间用于 FIFO。NVM3 在整个存储空间上实现为一个循环缓冲区，因此与使用较小虚拟页的实现相比，增加了闪存寿命。

<figure>
  <img src="images/Figure%202.7.%20Circular%20Buffer.png"
       alt="Figure 2.7. Circular Buffer"
       style="display:block; margin: auto;">
  <figcaption style="white-space: nowrap; text-align: center;">Figure 2.7. Circular Buffer</figcaption>
</figure>

## 2.6 Counter Objects

对于某些类型的数据，可以针对闪存介质优化存储格式。EFR32 Series 0 和 Series 1 设备每个字可以写两次（即可以更新半个字）。Series 2 设备每个字只允许写一次。

例如，计数器值通常在每次写入时递增 1 或其他一些小值。通常这意味着每次计数器递增时，除了标识字节之外，我们还必须写入整个计数器值。我们可以通过仅在第一次写入计数器时存储除标识值之外的起始值来优化这一点。然后，我们在写入增量的初始值之后保留一些字，以便写入增量。EFR32 Series 0 和 1 中的闪存字可以在每次擦除之间写入两次字，方法是将不变的位保持 1。

例如，假设我们要写入两个 16-bit 值，0xAAAA 和 0x5555。为了将它们安全地写入相同的闪存字中，可以使用以下方法：

* Write 0xFFFFAAAA (word in flash becomes 0xFFFFAAAA)
* Write 0x5555FFFF (word in flash becomes 0x5555AAAA)

请注意，由于闪存的物理限制，每次擦除之间最多可两次写入同一个字。

为了找到当前的计数器值，我们从初始值开始，然后将增量值添加到初始值的后半字中。这意味着我们只需要为每个增量写一个半字，而不是整个计数器值和标识值。NVM3 和 SimEEv1/v2 支持计数器对象。

## 2.7 Indexed Objects

在闪存中存储数组等数据时，我们通常一次只更新一个索引。如果我们将整个数组存储为一个常规对象，那么即使只更新了一个索引，我们也必须将整个数组写入闪存。我们可以将每个数据数组分配到多个对象上，而不是将整个数组存储在一个对象中，而只更新包含更改的数组索引的对象。虽然可以为所有 Silicon Labs 存储实现手动将数组拆分为多个对象，但 SimEEv1/v2 允许所有索引共享一个对象 key。然后，将要查找的数组中的索引条目作为单独的参数提供。

NVM3 不支持对象的部分读写。如果应用程序要单独访问索引，则必须使用唯一的 key 访问每个索引。

# 3. Dynamic Data Storage Implementations

本章介绍 Silicon Labs 提供的一些动态数据存储实现，包括 SimEEv1/SimEEv2、PS Store 和 NVM3。

## 3.1 SimEEv1/v2

SimEEv1/v2 可以与 EmberZNet PRO 和 Silicon Labs Connect（随 Flex SDK 一起安装）一起使用。SimEEv1 使用两个固定的总大小为 8 kB 的虚拟页，而 SimEEv2 使用三个固定的总大小为 36 kB 的虚拟页。

SimEEv1/v2 的一个特征是在编译时使用大小和类型定义所有对象，因此无法在运行时创建或删除新对象。

Silicon Labs 提供了一个用于将 SimEEv1 数据升级到 SimEEv2 的插件。

有关 SimEEv1/v2 实现的信息可在 *AN703: Using Simulated EEPROM Version 1 and Version 2 for the EM35x and EFR32 Series 1 SoC Platforms* 中找到。

## 3.2 PS Store

PS Store 可以与 Bluetooth 设备一起使用（EFR32 Series 2 除外）。PS Store API 命令用于管理 Bluetooth 设备闪存中的 PS key 中的用户数据。存储在闪存中的用户数据在设备的复位和重启期间是持久的。持久存储大小为 2048 byte，并使用两个闪存页进行存储。由于 Bluetooth Bonding 也存储在此区域中，因此可用于用户数据的空间还取决于设备当时的 Bonding 数量。一个 Bluetooth Bonding 的大小约为 150 byte。凭借其简单的实现和少量的存储闪存页，PS Store 是 Silicon Labs 中最小的非易失性存储选项。PS Store 允许在运行时创建和删除对象。

有关 PS Store API 的信息可在 *Bluetooth API Software Reference Manual* 中找到。

## 3.3 NVM3

NVM3 数据存储驱动程序是 SimEEv1/v2 和 PS Store 的替代品。NVM3 驱动程序提供了一种方法，以读写存储在闪存中的数据对象（Key/Value Pair）。应用磨损均衡算法来减少擦除和写入周期并最大化闪存寿命。驱动程序对断电和复位事件具有弹性，确保了从驱动程序检索的对象始终处于有效状态。由于 NVM3 可与多种协议（如 Bluetooth 和 EmberZNet PRO）一起使用，因此它允许在 DMP（Dynamic Multiprotocol，动态多协议）应用程序中共享单个数据存储实例。

NVM3 的一些主要特性如下：

* 在闪存中使用 Key/Value Pair 数据存储
* 运行时创建和删除对象
* 在断电和复位事件中保持持久性
* 磨损均衡可最大限度地延长闪存寿命
* 对象大小范围为 0 到 4096 byte
* 可配置的闪存大小（最少 3 个闪存页）
* 具有可配置大小的缓存，以便快速访问对象
* 数据和计数器对象类型
* 提供了 Token 和 PS Store API 的兼容层
* 多协议应用中的单个共享存储实例
* 重新包装 API，允许应用在 CPU 负载较低的时段运行待清除页的擦除

有关 NVM3 的详细信息，请参阅 [Gecko HAL & Driver API Reference Guide](http://devtools.silabs.com/dl/documentation/doxygen/) 的 EMDRV->NVM3 部分。通过其 Native API 访问 NVM3 的用户应参阅此 API 参考指南以获取信息。正在开发动态多协议应用的用户应参考 *AN1135: Using Third Generation Non-Volatile Memory (NVM3) Data Storage*。

# 4. Comparing Non-Volatile Data Storage Implementations

下表概述了各种 Silicon Labs 非易失性数据存储实现的主要特性。

<table style="margin: auto; white-space: nowrap;">
<caption style="white-space: nowrap;">Table 4.1. NV Data Storage Implementation Comparison</caption>
<thead>
  <tr>
    <th>Feature</th>
    <th>NVM3</th>
    <th>SimEEv1</th>
    <th>SimEEv2</th>
    <th>PS Store</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Compatible devices</td>
    <td>EFM32, EFR32</td>
    <td>EFR32 Series 1</td>
    <td>EFR32 Series 1</td>
    <td>EFR32 Series 1</td>
  </tr>
  <tr>
    <td>Compatible radio protocols</td>
    <td>EmberZNet, Connect,<br>Z-Wave, OpenThread,<br>Bluetooth</td>
    <td>EmberZNet, Connect</td>
    <td>EmberZNet, Connect</td>
    <td>Bluetooth</td>
  </tr>
  <tr>
    <td>Flash used for storage</td>
    <td>3 or more flash pages</td>
    <td>8 KB</td>
    <td>36 KB</td>
    <td>4 KB</td>
  </tr>
  <tr>
    <td>Virtual pages</td>
    <td>NA</td>
    <td>2</td>
    <td>3</td>
    <td>NA</td>
  </tr>
  <tr>
    <td>Max basic storage<br>(Sum of all object data with overhead)</td>
    <td>Variable<br>(see <em>AN1135</em> for details)</td>
    <td>2 KB</td>
    <td>8 KB</td>
    <td>2 KB</td>
  </tr>
  <tr>
    <td>Max number of objects</td>
    <td>Limited by basic storage</td>
    <td>254</td>
    <td>254</td>
    <td>Limited by basic storage</td>
  </tr>
  <tr>
    <td>Max object size (bytes)</td>
    <td>Configurable 208-4096</td>
    <td>254</td>
    <td>254</td>
    <td>56</td>
  </tr>
  <tr>
    <td>Object creation/deletion</td>
    <td>Runtime</td>
    <td>Compile time</td>
    <td>Compile time</td>
    <td>Runtime</td>
  </tr>
  <tr>
    <td>Compiled flash size excluding data storage</td>
    <td>9.1 KB</td>
    <td>3.5 KB</td>
    <td>5.4 KB</td>
    <td>1.6 KB</td>
  </tr>
  <tr>
    <td>Counter object support</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>No</td>
  </tr>
  <tr>
    <td>Indexed object support</td>
    <td>Partial<sup>1</sup></td>
    <td>Yes</td>
    <td>Yes</td>
    <td>No</td>
  </tr>
  <tr>
    <td>Overhead per object (bytes)</td>
    <td>4 (size &lt;= 128 bytes);<br>8 (size &gt; 128 bytes)</td>
    <td>2</td>
    <td>6</td>
    <td>10</td>
  </tr>
  <tr>
    <td>Counter object size including overhead<br>(bytes)</td>
    <td>212</td>
    <td>60</td>
    <td>56</td>
    <td>NA</td>
  </tr>
  <tr>
    <td>Counter increments before rewrite</td>
    <td>100 (EFR32 Series 1);<br>50 (EFR32 Series 2)</td>
    <td>25</td>
    <td>25</td>
    <td>NA</td>
  </tr>
  <tr>
    <td colspan="5"><sup>1</sup> When using NVM3, indexed objects are implemented by storing each index in a separate NVM3 object.</td>
  </tr>
</tbody>
</table>

## 4.1 Flash Lifetime

所有 Silicon Labs 闪存数据存储实现都使用某种形式的磨损均衡来延长闪存寿命。磨损均衡的有效性取决于实现，存储的数据类型以及更新的频率。影响磨损和闪存寿命的主要因素是：

* 用于数据存储的闪存大小：闪存区域越多，闪存寿命越长。对于 NVM3，可以配置用于数据存储的闪存页数，而其余实现使用固定存储大小。
* 每个对象存储的开销：将数据写入对象存储时，会添加一些开销字节以标识数据。较少开销的实现意味着数据对象在闪存中占用较少的空间，并提供更长的闪存寿命。
* 对齐到最小对象大小：对象以最小对象大小的倍数存储。如果数据大小与此大小不对齐，则会添加填充字节，这会增加存储的数据并缩短闪存生命周期。例如，当存储 16-bit Token 时，除了开销字节之外，NVM3 和 PS 存储还添加两个额外的填充字节。SimEEv1/v2 能够存储 16-bit 数据对象而无需填充。
* 基本存储后的剩余存储：对于使用虚拟页的实现，当切换到新的虚拟页时，每个对象的一个​​实例将写入页。然后，可以使用虚拟页的其余部分来存储对象的新写入。如果使用大量空间来存储每个对象的一个​​实例，则在虚拟页中留下很少的空间用于对后续对象写入进行磨损均衡。因此，当对象数据的总量相对于虚拟页大小较大时，闪存寿命将减少。即使对于未使用虚拟页的 NVM3，闪存寿命也受到整个 NVM3 存储空间的可用空间的限制。

为了帮助监控实际的闪存磨损，NVM3 和 SimEEv1/v2 包括了用于报告数据存储闪存页面的页面擦除次数的函数调用。在产品的加速寿命测试期间可以读取这些擦除计数器，以验证闪存是否以可接受的速率磨损。
