---
title: DispatchPower 例程
description: DispatchPower 例程
ms.assetid: e385064f-cbdb-432f-951a-743217891333
keywords:
- 调度例程 WDK 内核，DispatchPower 例程
- DispatchPower 例程
- 电源管理 WDK 内核，调度例程
- IRP_MJ_POWER i/o 函数代码
- 可移动设备电源派单例程 WDK 内核
ms.date: 06/16/2017
ms.localizationpriority: medium
ms.openlocfilehash: f74cc9d6ce51e119b8ca8e5a07b028212a2c5fd6
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72838727"
---
# <a name="dispatchpower-routines"></a>DispatchPower 例程





驱动程序的[*DispatchPower*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-driver_dispatch)例程通过处理[**irp\_MJ\_power**](https://docs.microsoft.com/windows-hardware/drivers/kernel/irp-mj-power) I/o 函数代码的 irp 来支持[电源管理](implementing-power-management.md)。 与**IRP\_MJ 关联\_power** function 代码是用于电源管理的几个次要 i/o 函数代码。 电源管理器使用这些次要函数代码来指示驱动程序更改电源状态、等待和响应系统唤醒事件，并查询有关其设备的驱动程序。

每个驱动程序的*DispatchPower*例程执行以下任务：

-   如果可能，请处理 IRP。

-   使用[**PoCallDriver**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-pocalldriver)将 IRP 传递到设备堆栈中的下一个较低的驱动程序。

-   如果是总线驱动程序，请在设备上执行请求的电源操作并完成 IRP。

设备的所有驱动程序都必须有机会处理设备的电源 Irp，但在某些情况下，允许使用函数或筛选器驱动程序使 IRP 失败。 大多数函数和筛选器驱动程序都执行某个处理或为每个电源 IRP 设置[*IoCompletion*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nc-wdm-io_completion_routine)例程，然后将 IRP 向下传递到下一个较低的驱动程序，而不完成。 该 IRP 最终会到达总线驱动程序，该驱动程序以物理方式更改设备的电源状态（如果需要）并完成 IRP。

完成 IRP 后，i/o 管理器将调用驱动程序设置的任何*IoCompletion*例程，因为 IRP 向下移动设备堆栈。 驱动程序是否需要设置完成例程取决于 IRP 的类型和驱动程序的各个要求。

支持设备的电源 Irp 必须首先由设备堆栈中的最低驱动程序（基础总线驱动程序）和每个连续的驱动程序堆栈处理。 电源关闭设备的电源必须首先由设备堆栈顶部的驱动程序处理，然后再按照堆栈上的每个连续驱动程序进行处理。

### <a name="special-handling-for-removable-devices"></a>适用于可移动设备的特殊处理

在*DispatchPower*例程中，可移动设备的驱动程序应进行检查以确定设备是否仍然存在。 如果设备已被删除，驱动程序不应将 IRP 传递到下一个较低版本的驱动程序。 相反，驱动程序应执行以下操作：

-   调用[**PoStartNextPowerIrp**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-postartnextpowerirp) ，开始处理下一个电源 IRP。

-   将**Irp&gt;IoStatus**设置为状态，\_删除\_"挂起"。

-   调用[**IoCompleteRequest**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-iocompleterequest)，指定 IO\_NO\_递增，以完成 IRP。

-   返回状态\_\_挂起删除。

 

 




