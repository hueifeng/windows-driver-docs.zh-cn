---
title: GUID_CLASS_KEYBOARD
description: GUID_CLASS_KEYBOARD
ms.assetid: 9e90d18f-5298-4234-8b05-38e9b8ec5076
keywords:
- GUID_CLASS_KEYBOARD 设备和驱动程序安装
topic_type:
- apiref
api_name:
- GUID_CLASS_KEYBOARD
api_location:
- Ntddkbd.h
api_type:
- HeaderDef
ms.localizationpriority: medium
ms.date: 10/17/2018
ms.openlocfilehash: 22a86c01144dd0df3df5471bfd9809cc490702a8
ms.sourcegitcommit: fb7d95c7a5d47860918cd3602efdd33b69dcf2da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2019
ms.locfileid: "67373509"
---
# <a name="guidclasskeyboard"></a>GUID_CLASS_KEYBOARD


GUID_CLASS_KEYBOARD 是已过时标识符[设备接口类](https://docs.microsoft.com/windows-hardware/drivers/install/device-interface-classes)键盘设备。 从 Microsoft Windows 2000 开始，使用[ **GUID_DEVINTERFACE_KEYBOARD** ](guid-devinterface-keyboard.md)此类的新实例的类标识符。

<a name="remarks"></a>备注
-------

WDK 中提供的 HID 示例包括在键盘类驱动程序。 在键盘类驱动程序使用 GUID_CLASS_KEYBOARD 注册此设备接口类的实例。

<a name="requirements"></a>要求
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>Version</p></td>
<td align="left"><p>已过时。 从 Windows 2000 开始，请改用 GUID_DEVINTERFACE_KEYBOARD。</p></td>
</tr>
<tr class="even">
<td align="left"><p>Header</p></td>
<td align="left">Ntddkbd.h （包括 Ntddkbd.h）</td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>请参阅


[**GUID_DEVINTERFACE_KEYBOARD**](guid-devinterface-keyboard.md)

 

 






