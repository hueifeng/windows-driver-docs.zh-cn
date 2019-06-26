---
title: Windows 内核模式即插即用管理器
description: Windows 内核模式即插即用管理器
ms.assetid: 43d06dbe-da66-4103-8be3-f27ff075a1b4
ms.localizationpriority: medium
ms.date: 10/17/2018
ms.openlocfilehash: e18d7d041d8ab26f7914165a88f0fe2c904b716a
ms.sourcegitcommit: 0cc5051945559a242d941a6f2799d161d8eba2a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "63357521"
---
# <a name="windows-kernel-mode-plug-and-play-manager"></a>Windows 内核模式即插即用管理器


插即用 (PnP) 是硬件技术的组合，并且使能够识别设备时的 PC 的软件技术添加到系统。 使用的即插即用，系统配置可以使用很少或没有用户输入更改。 例如，接通 USB 拇指驱动器，Windows 可以检测拇指驱动器，并将其自动添加到文件系统。 但是，若要执行此操作，硬件必须遵循特定的要求，因此必须驱动程序。

有关详细信息 PnP 驱动程序，请参阅[插](implementing-plug-and-play.md)。

PnP 管理器是实际 I/O 管理器的子系统。 有关 I/O 管理器的详细信息，请参阅[Windows 内核模式 I/O 管理器](windows-kernel-mode-i-o-manager.md)。

即插即用例程的列表，请参阅[插例程](https://msdn.microsoft.com/library/windows/hardware/ff558809)。

请注意，没有为 PnP 管理器; 提供直接的界面的例程也就是说，没有"**Pp**"例程。

Windows 驱动程序框架 (WDF) 提供一组库，以使即插即用的管理更加轻松。 WDF 的详细信息，请参阅[内核模式驱动程序框架概述](https://msdn.microsoft.com/library/windows/hardware/ff544296)。

 

 



