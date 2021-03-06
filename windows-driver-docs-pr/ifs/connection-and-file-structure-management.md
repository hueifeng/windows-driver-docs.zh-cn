---
title: 连接和文件结构管理
description: 连接和文件结构管理
ms.assetid: 3695cab3-6751-48ee-8b11-e70c2bceab29
keywords:
- 数据结构 WDK 文件系统
- RDBSS WDK 文件系统、连接和文件结构
- 重定向的驱动器缓冲子系统 WDK 文件系统、连接和文件结构
- 连接结构 WDK RDBSS
- 文件结构 WDK RDBSS
- 结构 WDK RDBSS
- 连接信息 WDK RDBSS
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 05810433778c748bc5996cebfaffcff5d6f06644
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72841462"
---
# <a name="connection-and-file-structure-management"></a>连接和文件结构管理


## <span id="ddk_connection_and_file_structure_management_if"></span><span id="DDK_CONNECTION_AND_FILE_STRUCTURE_MANAGEMENT_IF"></span>


RDBSS 使用六种基本数据结构来管理连接和文件结构。 这些数据结构由 RDBSS 和各种网络小型重定向器在内部使用。 这些数据结构有两种版本。 网络小型重定向程序版本包含可由网络微型重定向程序驱动程序操作的字段。 这些数据结构的网络微重定向程序版本以 MRX\_ 前缀开头。 RDBSS 版本包含只能由 RDBSS 操作的其他字段。

这六个基本数据结构如下所示：

-   SRV\_调用--服务器调用上下文。 此结构为远程服务器提供抽象。

-   NET\_根--net root。 此结构将抽象到共享的连接。

-   \_net\_net--net ROOT （也称为虚拟 netroots）的视图。

-   FCB--file control block。 此结构表示共享上打开的文件。

-   SRV\_打开--服务器端打开上下文。 此结构封装服务器上的打开句柄。

-   FOBX--file object extension。 此结构是[ **\_对象**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/ns-wdm-_file_object)结构的文件的 RDBSS 扩展。

这些数据结构按以下层次结构进行组织：

```cpp
                SRV_CALL 
     FCB   <------> NET_ROOT
        SRV_OPEN  <---> V_NET_ROOT
            FOBX
                FILE_OBJECT
```

为了响应内核文件系统调用，RDBSS 通常会为网络微型重定向程序驱动程序创建并完成所有前面提到的结构，但 FOBX 结构除外。 因此，网络小型重定向器驱动程序通常只会调用一些用于连接和文件结构管理的 RDBSS 例程。 大多数这些例程由 RDBSS 在内部调用。

所有这些数据结构都是引用计数的。 数据结构上的引用计数如下所示：

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">数据结构</th>
<th align="left">引用计数的说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>SRV_CALL</p></td>
<td align="left"><p>指向 SRV_CALL NET_ROOT 项的数目，以及一些动态值。</p></td>
</tr>
<tr class="even">
<td align="left"><p>NET_ROOT</p></td>
<td align="left"><p>指向 NET_ROOT 的 FCB 项和 V_NET_ROOT 项的数目，以及一些动态值。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>V_NET_ROOT</p></td>
<td align="left"><p>指向 V_NET_ROOT SRV_OPEN 项的数目，以及一些动态值。</p></td>
</tr>
<tr class="even">
<td align="left"><p>FCB</p></td>
<td align="left"><p>指向 FCB 的 SRV_OPEN 项的数目以及一些动态值。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SRV_OPEN</p></td>
<td align="left"><p>指向 SRV_OPEN 的 FOBX 项的数目以及一些动态值。</p></td>
</tr>
<tr class="even">
<td align="left"><p>FOBX</p></td>
<td align="left"><p>某个动态值。</p></td>
</tr>
</tbody>
</table>

 

在每种情况下，动态值都是指引用结构的调用方的数量，但不会对其进行引用。 引用计数的静态部分本身由例程维护。 例如， [**RxCreateNetRoot**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fcb/nf-fcb-rxcreatenetroot)将递增关联的 SRV\_调用结构的引用计数。

引用调用和成功查找递增引用计数;取消引用调用将减少计数。 创建例程调用分配结构并将引用计数设置为1。

与任何数据结构关联的引用计数至少为1加上与其关联的下一个较低级别的数据结构实例的数目。 例如，与 SRV\_调用关联的引用计数（具有两个与之关联的 NET\_根）至少为3。 除了**RDBSS 内部表示**结构和下一较低级别的数据结构所持有的引用以外，还有一些其他引用可能已获得。

这些限制可确保不能完成任何给定级别的数据结构（已释放并释放关联的内存块），直到下一级别的所有数据结构均已完成或已释放其引用。 例如，如果包含对 FCB 的引用，则可以安全地访问与之关联的 V\_NET\_ROOT、NET\_ROOT 和 SRV\_调用结构。

网络微重定向程序与 RDBSS 之间的接口中使用的两个重要抽象是 SRV\_调用和 NET\_根结构。 SRV\_调用结构对应于与已建立连接的服务器关联的上下文，并且 NET\_根结构对应于服务器上的共享（这也可以作为命名空间的一部分来查看，该部分已由网络小型重定向程序声明）。

创建 SRV\_调用和 NET\_根结构通常至少涉及到一次网络往返。 为了使异步操作继续进行，这些操作将建模为一个两阶段的活动。 每个向下调用一个网络微型重定向程序来创建 SRV\_调用和一个网络\_根结构，接下来是从网络微型重定向程序到 RDBSS 的调用，通知请求的完成状态。 此类当前是同步的。

由于 RDBSS 必须从多个网络小型重定向程序中进行选择来与服务器建立连接，因此创建 SRV\_调用结构更复杂。 若要为 RDBSS 提供最大的灵活性，以便选择它想要部署的网络微重定向程序，创建 SRV\_调用结构涉及第三个阶段，RDBSS 会在其中通知入选方的网络微型重定向程序。 所有丢失的网络微重定向器都销毁关联的上下文。

本部分包含以下主题：

[SRV\_调用结构](the-srv-call-structure.md)

[NET\_根结构](the-net-root-structure.md)

[\_NET\_根结构的 V](the-v-net-root-structure.md)

[FCB 结构](the-fcb-structure.md)

[SRV\_开放结构](the-srv-open-structure.md)

[FOBX 结构](the-fobx-structure.md)

[连接和文件结构锁定](connections-and-file-structure-locking.md)

[连接和文件控制块管理例程](connection-and-file-control-block-management-routines.md)

 

 




