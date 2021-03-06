---
title: 处理 SRB_FUNCTION_EXECUTE_SCSI
description: 处理 SRB_FUNCTION_EXECUTE_SCSI
ms.assetid: 221e1070-12d8-4870-a23c-426ed4a25b84
keywords:
- SCSI 微型端口驱动程序 WDK 存储，HwScsiStartIo
- HwScsiStartIo
- SRB_FUNCTION_EXECUTE_SCSI
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 60ce582d44ec879b42c166c9bdc1fb42bd1a03cc
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72826239"
---
# <a name="handling-srb_function_execute_scsi"></a>处理 SRB\_函数\_执行\_SCSI


## <span id="ddk_handling_srb_function_execute_scsi_kg"></span><span id="DDK_HANDLING_SRB_FUNCTION_EXECUTE_SCSI_KG"></span>


加载较高级别的存储类驱动程序后，大多数发送到[**HwScsiStartIo**](https://docs.microsoft.com/previous-versions/windows/hardware/drivers/ff557323(v=vs.85))例程的 SRBs 会将**函数**成员设置为 SRB\_函数\_EXECUTE\_SCSI。

收到 SRB\_函数\_执行\_SCSI 请求时，微型端口驱动程序的*HwScsiStartIo*例程会执行以下操作：

-   获取和/或设置微型端口驱动程序在其设备、逻辑单元和/或 SRB 扩展中维护的任何上下文

    例如，微型端口驱动程序可能会设置一个逻辑单元扩展，其中包含指向 SRB 本身和 SRB **DataBuffer**指针的指针、SRB **DataTransferLength**值和驱动程序定义的值（或 CDB SCSIOP\_*XXX*值）指示要在 HBA 上执行的操作。

-   为请求的操作调用内部例程来编程由**SrbFlags**部分定向的 HBA

    对于设备 i/o 操作，此类内部例程通常会选择目标设备，并通过总线将 CDB 发送到目标逻辑单元。

如果微型端口驱动程序使用系统 DMA，则必须先调用[**ScsiPortIoMapTransfer**](https://docs.microsoft.com/windows-hardware/drivers/ddi/srb/nf-srb-scsiportiomaptransfer)，*然后再*对 HBA 进行编程以传输数据。 **ScsiPortIoMapTransfer**设置系统 DMA 控制器并调用微型端口驱动程序的*HwScsiDmaStarted*例程，稍后在[SCSI 微型端口驱动程序的 HwScsiDmaStarted 例程](scsi-miniport-driver-s-hwscsidmastarted-routine.md)中进行了介绍。

发送到基于 NT 的操作系统存储类驱动程序的所有系统定义的、所需的设备 i/o 控制请求将映射到 SRBs，并将**函数**成员设置为 SRB\_函数\_EXECUTE\_SCSI。

 

 




