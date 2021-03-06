---
title: WlanAssert 规则（ndis）
description: WlanAssert 规则包括在 WDIWIFI 驱动程序中验证的一组检查。
ms.assetid: CCA68C4D-618A-4DE4-9DEB-2A03DCD38667
ms.date: 04/08/2020
keywords:
- WlanAssert 规则（ndis）
topic_type:
- apiref
api_name:
- WlanAssert
api_type:
- NA
ms.localizationpriority: medium
ms.openlocfilehash: 158a0752bedbe9f6b339270aa9bb131a93aa7ac3
ms.sourcegitcommit: 84be9e06fd0886598df77dffcbc75632d613c8f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/11/2020
ms.locfileid: "81219515"
---
# <a name="wlanassert-rule-ndis"></a>WlanAssert 规则（ndis）

**WlanAssert**规则包括在 WDIWIFI 驱动程序中验证的一组检查。

可能存在以下冲突：

- *TxPeerBacklogStub：在数据路径 deinitialization 后，IHV WDI 微型端口称为数据路径*-此规则仅适用于对等-排队模式。 当微型端口已停止或重置时，WDI 将调用 IHV 驱动程序的[CloseAdapterHandler](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-miniport_wdi_close_adapter)函数，该函数要求驱动程序清理其状态，而不会在之后调用任何数据回调。 如果驱动程序在关闭后调用[TxTransferCompleteIndication](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-ndis_wdi_tx_transfer_complete_ind)、 [TxSendPauseIndication](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-ndis_wdi_tx_send_pause_ind)或[TxReleaseFrameIndication](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-ndis_wdi_tx_release_frames_ind)等任何数据处理程序，或者关闭后仍有任何未完成的 Tx 帧，则会调用这些断言。

- *TxAbortStub：在数据路径 deinitialization 后，IHV WDI 微型端口称为数据路径*-此规则仅适用于对等-排队模式。 当微型端口已停止或重置时，WDI 将调用 IHV 驱动程序的[CloseAdapterHandler](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-miniport_wdi_close_adapter)函数，该函数要求驱动程序清理其状态，而不会在之后调用任何数据回调。 如果驱动程序在关闭后调用[TxTransferCompleteIndication](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-ndis_wdi_tx_transfer_complete_ind)、 [TxSendPauseIndication](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-ndis_wdi_tx_send_pause_ind)或[TxReleaseFrameIndication](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-ndis_wdi_tx_release_frames_ind)等任何数据处理程序，或者关闭后仍有任何未完成的 Tx 帧，则会调用这些断言。

- *卸载 WDIWIFI 驱动程序时，对 NdisMDeregisterWdiMiniportDriver 和 NdisMRegisterWdiMiniportDriver 的调用不匹配*-如果 ihv 驱动程序对[NdisMRegisterWdiMiniportDriver](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nf-dot11wdi-ndismregisterwdiminiportdriver)的调用失败，则会调用此断言，但 ihv 驱动程序仍调用[NdisMDeregisterWdiMiniportDriver](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nf-dot11wdi-ndismderegisterwdiminiportdriver)处理程序。

- *IhvWdiVersion 对于传递的 MiniportDataHandler 修订版本太低*-WDI 将通过调用[OID_WDI_GET_ADAPTER_CAPABILITIES](https://docs.microsoft.com/windows-hardware/drivers/network/oid-wdi-get-adapter-capabilities)获取 IHV 驱动程序的 WDI 版本，然后它将调用驱动程序的[TalTxRxInitializeHandler](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-miniport_wdi_tal_txrx_initialize)处理程序以获取[WdiCharacteristics](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/ns-dot11wdi-_ndis_miniport_driver_wdi_characteristics)，其中，驱动程序可以根据需要更新 WDI 处理程序修订版。 如果驱动程序的 WDI 版本低于或等于 WDI_VERSION_1_1_0，但驱动程序的[WdiCharacteristics](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/ns-dot11wdi-_ndis_miniport_driver_wdi_characteristics)版本设置为大于 NDIS_OBJECT_TYPE_MINIPORT_WDI_DATA_HANDLERS_REVISION_1 版本，则会命中此断言。

- *MiniportDataHandler 修订版太小，IhvWdiVersion*将通过调用[OID_WDI_GET_ADAPTER_CAPABILITIES](https://docs.microsoft.com/windows-hardware/drivers/network/oid-wdi-get-adapter-capabilities)获取 IHV 驱动程序的 WDI 版本，然后它将调用驱动程序的[TalTxRxInitializeHandler](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-miniport_wdi_tal_txrx_initialize)处理程序以获取[WdiCharacteristics](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/ns-dot11wdi-_ndis_miniport_driver_wdi_characteristics)，其中，驱动程序可以根据需要更新 WDI 处理程序修订版。 如果驱动程序的 WDI 版本高于 WDI_VERSION_1_1_0，但驱动程序的[WdiCharacteristics](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/ns-dot11wdi-_ndis_miniport_driver_wdi_characteristics)版本设置为低于 NDIS_OBJECT_TYPE_MINIPORT_WDI_DATA_HANDLERS_REVISION_2，则会命中此断言。

将在 0xC4 bug 检查中将*冲突文本*作为参数2提供。

|              |      |
|--------------|------|
| 驱动程序模型 | NDIS |

|                                   |                                                                                                                                        |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| 使用此规则发现的错误检查 | [**Bug 检查0xC4：检测到\_冲突的驱动程序\_验证程序\_** ](https://docs.microsoft.com/windows-hardware/drivers/debugger/bug-check-0xc4--driver-verifier-detected-violation) （0x00093004） |

<a name="how-to-test"></a>如何测试
-----------

<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">在运行时</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>运行<a href="https://docs.microsoft.com/windows-hardware/drivers/devtest/driver-verifier" data-raw-source="[Driver Verifier](https://docs.microsoft.com/windows-hardware/drivers/devtest/driver-verifier)">驱动程序验证程序</a>，并选择<a href="https://docs.microsoft.com/windows-hardware/drivers/devtest/ndis-wifi-verification" data-raw-source="[NDIS/WIFI verification](https://docs.microsoft.com/windows-hardware/drivers/devtest/ndis-wifi-verification)">NDIS/WIFI 验证</a>选项。</p></td>
</tr>
</tbody>
</table>

<a name="applies-to"></a>适用于
----------

[TxTransferCompleteIndication](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-ndis_wdi_tx_transfer_complete_ind)

[TxSendPauseIndication](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-ndis_wdi_tx_send_pause_ind)

[TxReleaseFrameIndication](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nc-dot11wdi-ndis_wdi_tx_release_frames_ind) 

[OID_WDI_GET_ADAPTER_CAPABILITIES](https://docs.microsoft.com/windows-hardware/drivers/network/oid-wdi-get-adapter-capabilities)

[MINIPORT_HALT 回调函数](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndis/nc-ndis-miniport_halt)

[MINIPORT_SHUTDOWN 回调函数](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndis/nc-ndis-miniport_shutdown)

[NdisMRegisterWdiMiniportDriver](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nf-dot11wdi-ndismregisterwdiminiportdriver)

[NdisMDeregisterWdiMiniportDriver](https://docs.microsoft.com/windows-hardware/drivers/ddi/dot11wdi/nf-dot11wdi-ndismderegisterwdiminiportdriver)

<a name="see-also"></a>另请参阅
--------

[WDI IHV 驱动程序接口](https://docs.microsoft.com/windows-hardware/drivers/network/wdi-ihv-driver-interfaces)

[常规连接操作指导原则](https://docs.microsoft.com/windows-hardware/drivers/network/general-connection-operation-guidelines)

[OID\_DOT11\_RESET\_请求](https://docs.microsoft.com/windows-hardware/drivers/network/oid-dot11-reset-request)

[NDIS\_状态\_DOT11\_关联\_开始](https://docs.microsoft.com/windows-hardware/drivers/network/ndis-status-dot11-association-start)
