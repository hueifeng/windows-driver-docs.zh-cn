---
title: 检测子设备
description: 检测子设备
ms.assetid: 36c0c4ef-7810-4e8a-b349-0b7c1f8c2f0c
keywords:
- 视频微型端口驱动程序 WDK Windows 2000，子设备
- 子设备 WDK 视频微型端口，检测
- 检测子设备 WDK 视频微型端口
- HwVidGetVideoChildDescriptor
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 259ac6757cd990a8bbb9233a63908db74ed6ecf3
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72839754"
---
# <a name="detecting-child-devices"></a>检测子设备


## <span id="ddk_detecting_child_devices_gg"></span><span id="DDK_DETECTING_CHILD_DEVICES_GG"></span>


你必须在微型端口驱动程序中实现[*HwVidGetVideoChildDescriptor*](https://docs.microsoft.com/windows-hardware/drivers/ddi/video/nc-video-pvideo_hw_get_child_descriptor) ，以便即插即用管理器能够检测图形适配器的子设备。

默认情况下，直到父设备启动之后，才能调用[*HwVidGetVideoChildDescriptor*](https://docs.microsoft.com/windows-hardware/drivers/ddi/video/nc-video-pvideo_hw_get_child_descriptor) ;也就是说，在[*HwVidFindAdapter*](https://docs.microsoft.com/windows-hardware/drivers/ddi/video/nc-video-pvideo_hw_find_adapter)完成后，才能调用*HwVidGetVideoChildDescriptor* 。 若要重写此默认值，从而允许子枚举随时发生，可以将[**视频\_硬件\_初始化\_** ](https://docs.microsoft.com/windows-hardware/drivers/ddi/video/ns-video-_video_hw_initialization_data)的**AllowEarlyEnumeration**成员设置为**TRUE**。

当新硬件连接到系统时，或者当现有硬件与系统断开连接时，某些设备会生成中断。 若要处理此类中断，微型端口驱动程序应执行以下操作：

-   实现调用[**VideoPortEnumerateChildren**](https://docs.microsoft.com/windows-hardware/drivers/ddi/video/nf-video-videoportenumeratechildren)的 DPC （[**HwVidDpcRoutine**](https://docs.microsoft.com/windows-hardware/drivers/ddi/video/nc-video-pminiport_dpc_routine)）。

-   实现一个中断处理程序（[*HwVidInterrupt*](https://docs.microsoft.com/windows-hardware/drivers/ddi/video/nc-video-pvideo_hw_interrupt)），该处理程序在设备上出现中断时调用[**VideoPortQueueDpc**](https://docs.microsoft.com/windows-hardware/drivers/ddi/video/nf-video-videoportqueuedpc)将 DPC 排队。

[**VideoPortEnumerateChildren**](https://docs.microsoft.com/windows-hardware/drivers/ddi/video/nf-video-videoportenumeratechildren)通过导致为父设备的每个子级调用微型端口驱动程序的[*HwVidGetVideoChildDescriptor*](https://docs.microsoft.com/windows-hardware/drivers/ddi/video/nc-video-pvideo_hw_get_child_descriptor)函数，来强制执行适配器的子设备的 reenumeration。 即插即用管理器将相应地更新父设备与其子项之间的关系。

 

 





