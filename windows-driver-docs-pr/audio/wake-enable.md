---
title: 启用唤醒
description: 启用唤醒
ms.assetid: f4a2d4b1-d3a0-449a-ac65-a448d2bcab5c
keywords:
- HD 音频，启用唤醒
- 高清晰音频（HD 音频），唤醒启用
- 唤醒启用 WDK 音频
- 状态-更改事件 WDK 音频
- 电源管理 WDK 音频
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: c0b53df99b1ba7ef34893339156e848f8a279c81
ms.sourcegitcommit: 98930ca95b9adbb6e5e472f89e91ab084e67e31d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82925620"
---
# <a name="wake-enable"></a>启用唤醒


在关闭编解码器之前，编解码器函数驱动程序通常允许编解码器在编码解码器处于关闭状态的情况下唤醒系统。 对于音频编解码器，当用户插入输入插孔或从插孔中删除插头时，可以触发此类事件。 对于调制解调器编解码器，当电话响铃指示传入呼叫时，可能会发生状态更改事件。 有关状态更改事件的详细信息，请参阅[INTEL HD 音频](https://www.intel.com/content/www/us/en/standards/intel-standards-and-initiatives.html)网站上的*Intel 高质音频规范*。

若要准备关闭电源，函数驱动程序首先会将编解码器配置为在发生状态更改事件时向 HD 音频总线控制器发出信号。 接下来，函数驱动程序将[**irp\_MN\_等待\_唤醒**](https://docs.microsoft.com/windows-hardware/drivers/kernel/irp-mn-wait-wake)电源管理 irp 发送到 HD 音频总线驱动程序，告诉它从编解码器启用唤醒信号。 稍后，如果启用了唤醒信号，并且编解码器在编解码器的 SDI 行上传输状态更改事件，则控制器将为系统生成唤醒信号，并且总线驱动程序将通过完成 IRP\_MN\_等待\_唤醒 IRP 来通知函数驱动程序。

唤醒事件之后，总线驱动程序将确定哪些编解码器生成了唤醒信号，并在该编解码器\_上\_完成\_任何挂起的 IRP MN 等待唤醒 irp。 但是，如果编解码器同时包含音频和调制解调器功能组，则总线驱动程序无法确定哪个函数组是唤醒信号的源。 在这种情况下，函数驱动程序必须将自己的查询发送到编解码器来验证唤醒信号的源。

 

 




