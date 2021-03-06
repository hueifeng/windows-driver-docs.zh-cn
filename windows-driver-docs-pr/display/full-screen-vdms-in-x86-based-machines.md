---
title: 基于 x86 的计算机中的全屏 VDM
description: 基于 x86 的计算机中的全屏 VDM
ms.assetid: 5be4919d-d46f-430f-9d4f-670134379268
keywords:
- 视频微型端口驱动程序-基于 x86 的计算机中的 WDK Windows 2000、VGA 和全屏幕 VDMs
- 基于 x86 的计算机中的 VGA WDK 视频微型端口、全屏 VDMs
- 基于 x86 的计算机中的全屏 VDMs WDK 视频微型端口
- 基于 x86 的计算机 WDK VGA 兼容视频微型端口
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: d3832bd85c6e470b10810458518c6d3885691a95
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72838940"
---
# <a name="full-screen-vdms-in-x86-based-machines"></a>基于 x86 的计算机中的全屏 VDM


## <span id="ddk_full_screen_vdms_in_x86_based_machines_gg"></span><span id="DDK_FULL_SCREEN_VDMS_IN_X86_BASED_MACHINES_GG"></span>


出于性能原因，当用户在基于 x86 的计算机上将 MS-DOS 应用程序切换到全屏模式时，显示驱动程序会控制适配器。 然后，系统 VGA 或 VGA 兼容的微型端口驱动程序将从 V86 模拟器挂钩到模拟器，如应用程序发出**的 IN**、**代表 INSB/INSW/INSD**、 **OUT**和**代表 OUTSB/OUTSW**说明，到视频 I/O 个端口。 这些挂钩 i/o 操作将转发到与 VGA 兼容的微型端口驱动程序的*SvgaHwIoPortXxx*函数。

但是，为了提高性能，微型端口驱动程序可以调用[**VideoPortSetTrappedEmulatorPorts**](https://docs.microsoft.com/windows-hardware/drivers/ddi/video/nf-video-videoportsettrappedemulatorports) ，以允许应用程序直接访问某些 i/o 端口。 微型端口驱动程序继续将其他 i/o 端口与*SvgaHwIoPortXxx*挂钩，以将应用程序发出的指令流验证到这些端口。

若要防止全屏应用程序发出可能挂起计算机的指令序列， *SvgaHwIoPortXxx*函数会将应用程序指令流监视到驱动程序确定的适配器寄存器集。 微型端口驱动程序必须仅对完全安全的 i/o 端口启用直接访问。 例如，sequencer 和杂项输出寄存器的端口应始终与 V86 模拟器挂钩，并捕获到微型端口驱动程序提供的用于验证的*SvgaHwIoPortXxx*函数。

直接访问应用程序的 i/o 端口取决于 VGA 兼容的微型端口驱动程序通过调用**VideoPortSetTrappedEmulatorPorts**来设置的 IOPM （名为 x86 i/o 权限映射）。 请注意，微型端口驱动程序可以通过调用此函数来对 IOPM 进行调整，以使应用程序或 retrapped 访问*SvgaHwIoPortXxx*函数时释放访问范围（描述 i/o 端口）。 当前 IOPM 确定哪些端口可由应用程序直接访问，哪些端口仍与 V86 模拟器挂钩，并捕获到*SvgaHwIoPortXxx*函数进行验证。

默认情况下，在这种微型端口驱动程序的仿真器访问范围中设置的所有 i/o 端口都将捕获到相应的*SvgaHwIoPortXxx*函数。 但是，与 VGA 兼容的微型端口驱动程序通常会在收到\_视频的 IOCTL 时调用**VideoPortSetTrappedEmulatorPorts**\_启用\_vdm 请求来重置*vdm*的 IOPM，以允许直接访问其中的某些 i/o端口. 通常，此类驱动程序允许直接访问除 VGA sequencer 寄存器和杂项输出寄存器之外的所有视频适配器注册，以及驱动程序编写器已确定的任何 SVGA 适配器特定寄存器应始终由*SvgaHwIoPortXxx*函数。

 

 





