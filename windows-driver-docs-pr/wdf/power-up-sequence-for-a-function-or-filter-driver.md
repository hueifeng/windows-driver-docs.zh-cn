---
title: 函数或筛选器驱动程序的启动顺序
description: 函数或筛选器驱动程序的启动顺序
ms.assetid: 3E904641-A1E2-400C-A201-2D1D2D359657
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: b966bdc1091be2d9393cb5798a6bdf74c59f7e8c
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72842239"
---
# <a name="power-up-sequence-for-a-function-or-filter-driver"></a>函数或筛选器驱动程序的启动顺序


下图显示了在设备处于完全操作状态（从图底部的设备插入状态开始）时，框架调用 WDF （KMDF 和 UMDF V2）函数或筛选器驱动程序的事件回调函数的顺序：

![函数或筛选器驱动程序的设备枚举和开机序列](images/fdo-fido-powerup.png)

大水平行用于标记启动设备所涉及的步骤。 图左侧的列描述了步骤，右侧的列列出了完成该步骤的事件回调。

在图的底部，设备上不存在设备。 当用户插入设备时，框架首先调用驱动程序的[*EvtDriverDeviceAdd*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfdriver/nc-wdfdriver-evt_wdf_driver_device_add)回调，以便驱动程序可以创建设备对象来表示设备。 此框架通过在整个序列中向上遍历来继续调用驱动程序的回调例程，直到设备正常运行。 请记住，框架按下图所示的下向下顺序调用事件回调，因此在[*EvtDeviceFilterAddResourceRequirements*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdffdo/nc-wdffdo-evt_wdf_device_filter_resource_requirements)之前调用[*EvtDeviceFilterRemoveResourceRequirements*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdffdo/nc-wdffdo-evt_wdf_device_filter_resource_requirements) 。 如果设备已停止重新平衡资源，或者物理上存在但处于低功耗状态，则不需要执行所有步骤，如图所示。

 

 





