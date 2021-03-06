---
title: usbkd.usbdpc
description: Usbkd. usbdpc 命令显示 _XDPC_CONTEXT 结构中存储的信息。
ms.assetid: 51ED1BB0-416B-4B2B-9F4D-61F841224126
keywords:
- usbkd usbdpc Windows 调试
ms.date: 05/23/2017
topic_type:
- apiref
api_name:
- usbkd.usbdpc
api_type:
- NA
ms.localizationpriority: medium
ms.openlocfilehash: df23362fab8d5f2585926c483251ab1f9f2593c3
ms.sourcegitcommit: dadc9ced1670d667e31eb0cb58d6a622f0f09c46
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84534702"
---
# <a name="usbkdusbdpc"></a>!usbkd.usbdpc


**！ Usbkd. usbdpc**命令显示** \_ XDPC \_ 上下文**结构中存储的信息。

```dbgcmd
!usbkd.usbdpc StructAddr
```

## <a name="span-idddk__devobj_dbgspanspan-idddk__devobj_dbgspanparameters"></a><span id="ddk__devobj_dbg"></span><span id="DDK__DEVOBJ_DBG"></span>参数


<span id="_______StructAddr______"></span><span id="_______structaddr______"></span><span id="_______STRUCTADDR______"></span>*StructAddr*   
Usbport 的地址 **！ \_XDPC \_ 上下文**结构。 若要获取 USB 主机控制器的 XDPC 列表，请使用[**！ usbkd**](-usbkd-usbhcdext.md)命令。

## <a name="span-iddllspanspan-iddllspandll"></a><span id="DLL"></span><span id="dll"></span>.DLL


Usbkd

<a name="examples"></a>示例
--------

下面是查找 usbport 地址的一种方法 **！ \_XDPC \_ 上下文**结构。 首先输入[**！ usbkd. usb2tree**](-usbkd-usb2tree.md)。

```dbgcmd
0: kd> !usbkd.usb2tree
...
UHCI MINIPORT(s) dt usbport!_USBPORT_MINIPORT_DRIVER ffffe00001e77010
...
4)!uhci_info ffffe00001c7d1a0 !devobj ffffe00001c7d050 PCI: VendorId...
...
```

在上面的输出中，FDO 的设备扩展的地址显示为[DML](debugger-markup-language-commands.md) command **！ uhci \_ info ffffe00001c7d1a0**的参数。

单击 DML 命令或将设备扩展的地址传递给[**！ usbhcdext**](-usbkd-usbhcdext.md)以获取 XDPC 列表。

```dbgcmd
0: kd> !usbkd.usbhcdext ffffe00001c7d1a0
...
## XDPC List

01) dt USBPORT!_XDPC_CONTEXT ffffe00001c7df18
02) dt USBPORT!_XDPC_CONTEXT ffffe00001c7db88
03) dt USBPORT!_XDPC_CONTEXT ffffe00001c7dd50
04) dt USBPORT!_XDPC_CONTEXT ffffe00001c7e0e0
...
```

在上面的输出中， `ffffe00001c7df18` 是** \_ XDPC \_ 上下文**结构的地址。 将此地址传递给 **！ usbdpc**。

```dbgcmd
0: kd> !usbkd.usbdpc ffffe00001c7df18

dt USBPORT!_XDPC_CONTEXT ffffe00001c7df18

## XDPC HISTORY (latest at boottom)

##      EVENT                STATE                   NEXT

[01] Ev_Xdpc_End          XDPC_Running            XDPC_Enabled            
[02] Ev_Xdpc_Signal       XDPC_Enabled            XDPC_DpcQueued          
[03] Ev_Xdpc_Signal       XDPC_DpcQueued          XDPC_DpcQueued          
[04] Ev_Xdpc_Worker       XDPC_DpcQueued          XDPC_Running            
[05] Ev_Xdpc_Signal       XDPC_Running            XDPC_Signaled           
[06] Ev_Xdpc_End          XDPC_Signaled           XDPC_DpcQueued          
[07] Ev_Xdpc_Worker       XDPC_DpcQueued          XDPC_Running            
[08] Ev_Xdpc_End          XDPC_Running            XDPC_Enabled
```

## <a name="span-idsee_alsospansee-also"></a><span id="see_also"></span>另请参阅


[USB 2.0 调试器扩展](usb-2-0-extensions.md)

[通用串行总线（USB）驱动程序](https://docs.microsoft.com/windows-hardware/drivers/usbcon/)

 

 






