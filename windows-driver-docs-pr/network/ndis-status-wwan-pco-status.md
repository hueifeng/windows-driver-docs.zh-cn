---
title: NDIS_STATUS_WWAN_PCO_STATUS
description: 微型端口驱动程序使用 NDIS_STATUS_WWAN_PCO_STATUS 通知来通知 MB 服务完成了以前的 OID_WWAN_PCO 查询请求。
ms.assetid: E0F70FAE-B7C6-4BE4-B89A-88084463EEA5
keywords:
- NDIS_STATUS_WWAN_PCO_STATUS，PCO 状态通知，移动宽带 PCO 状态通知，MB PCO 状态通知
ms.date: 08/08/2017
ms.localizationpriority: medium
ms.openlocfilehash: a289ee894c9616d4c3775a81ec9b22005a062e5a
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72844770"
---
# <a name="ndis_status_wwan_pco_status"></a>NDIS_STATUS_WWAN_PCO_STATUS

**NDIS_STATUS_WWAN_PCO_STATUS**通知是通过调制解调器微型驱动程序发送的，用于将调制解调器中当前协议配置选项（PCO）状态的操作系统通知给操作系统。 在以下三种情况下，调制解调器小型端口驱动程序将发送此通知：

1.  新的 PCO 值到达激活的连接时。
2.  当主机上的连接被激活或桥接时，调制解调器具有 PCO 值。
3.  用于响应来自主机的[OID_WWAN_PCO](oid-wwan-pco.md)查询请求。

收到新的 PCO 值时，此通知将是未经请求的，并会从网络中的最新 PCO 值发送。 通知将提供与激活的连接的 PDN 相对应的 NDIS 端口号。

当某个连接被激活或从主机桥接时，该调制解调器应检查它是否已缓存 PCO 值。 如果是这样，则会将一个通知发送到主机，该主机的 NDIS 端口号对应于主机已激活或已桥接的 PDN。

此通知将用于通知主机**OID_WWAN_PCO**查询请求已完成，并且通知中包含 PCO 值。 主机需要调制解调器传递与端口号对应的 PDN 上的 PCO 值的完整结构。

如果调制解调器支持 PCO 功能，但当主机发送**OID_WWAN_PCO**查询请求时未从网络接收到 PCO 值，则调制解调器应返回带有空 WWAN_PCO_ 的**NDIS_STATUS_WWAN_PCO_STATUS**通知[值](https://docs.microsoft.com/windows-hardware/drivers/ddi/wwan/ns-wwan-_wwan_pco_value)负载。 

此通知使用[NDIS_WWAN_PCO_STATUS](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndiswwan/ns-ndiswwan-_ndis_wwan_pco_status)结构。

> [!NOTE]
> 目前，Windows 10 版本1709及更高版本中，某些调制解调器只能提供操作员特定的 PCO 元素。 如果调制解调器接收到 PCO 数据结构，但没有适用于操作员的特定 PCO 元素，则为了避免不必要的设备唤醒，调制解调器不应将 PCO 通知公布到操作系统。 

## <a name="requirements"></a>要求

| | |
| --- | --- |
| 版本 | Windows 10 版本 1709 |
| 标头 | Ndis。h |

## <a name="see-also"></a>另请参阅

[OID_WWAN_PCO](oid-wwan-pco.md)

[NDIS_WWAN_PCO_STATUS](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndiswwan/ns-ndiswwan-_ndis_wwan_pco_status)

[WWAN_PCO_VALUE](https://docs.microsoft.com/windows-hardware/drivers/ddi/wwan/ns-wwan-_wwan_pco_value)

[MB 协议配置选项（PCO）操作](mb-protocol-configuration-options-pco-operations.md)
