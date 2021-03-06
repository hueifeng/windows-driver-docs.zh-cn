---
title: 面向连接的计时功能
description: 面向连接的计时功能
ms.assetid: 73b005c2-39bd-4931-89cd-7ea3db9068ca
keywords:
- 面向连接的 NDIS WDK，计时功能
- CoNDIS WDK 网络，计时功能
- 计时功能 WDK CoNDIS
- 方式
- 本地时钟 WDK CoNDIS
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 011fe81f1518d201e41a896ff85eeb64972108b1
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72838178"
---
# <a name="connection-oriented-timing-features"></a>面向连接的计时功能





面向连接的 NDIS 支持使用 NIC 的本地时间来计划数据包传输和时间戳发送和接收数据包。

**请注意**  这些面向连接的计时功能是可选的。 所有 CoNDIS Nic 均不支持这些功能。

 

面向连接的协议驱动程序可以调用[**NdisCoOidRequest**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndis/nf-ndis-ndiscooidrequest)来查询面向连接的微型端口驱动程序的本地计时功能，或使用[OID\_GEN\_CO\_获取\_时间\_cap](https://docs.microsoft.com/windows-hardware/drivers/network/oid-gen-co-get-time-caps)。 为响应此类查询，微型端口驱动程序或 MCM 驱动程序将返回以下信息：

-   NIC 上是否有可读的时钟。

-   NIC 是否从网络连接中派生出时间。

-   本地时钟的精度。

-   NIC 是否可以用时间戳来接收包含其本地时间的数据包。

-   NIC 是否可以根据其本地时间安排发送数据包以便进行传输。

-   NIC 是否可以用其本地时间来戳记传输的数据包。

为了获取 NIC 的本地时间，面向连接的协议可以调用**NdisCoOidRequest** ，以使用[OID\_GEN\_CO\_获取\_网卡\_时间](https://docs.microsoft.com/windows-hardware/drivers/network/oid-gen-co-get-netcard-time)来查询面向连接的微型端口驱动程序或 MCM 驱动程序。 面向连接的微型端口驱动程序或 MCM 驱动程序同步返回其本地时间，该时间是面向连接的协议可用于计划数据包传输的时间。

发送或接收数据包的时间信息包含在数据包的带外（OOB）数据中。 有关详细信息，请参阅[**NET\_BUFFER\_LIST**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndis/ns-ndis-_net_buffer_list)。

 

 





