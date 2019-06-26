---
title: Bug 检查 0xCE DRIVER_UNLOADED_WITHOUT_CANCELLING_PENDING_OPERATIONS
description: DRIVER_UNLOADED_WITHOUT_CANCELLING_PENDING_OPERATIONS bug 检查具有 0x000000CE 值。 这表示一个驱动程序无法在卸载之前取消挂起的操作。
ms.assetid: ade0d20d-25f0-45ad-a099-31f3521b91cd
keywords:
- Bug 检查 0xCE DRIVER_UNLOADED_WITHOUT_CANCELLING_PENDING_OPERATIONS
- DRIVER_UNLOADED_WITHOUT_CANCELLING_PENDING_OPERATIONS
ms.date: 05/23/2017
topic_type:
- apiref
api_name:
- DRIVER_UNLOADED_WITHOUT_CANCELLING_PENDING_OPERATIONS
api_type:
- NA
ms.localizationpriority: medium
ms.openlocfilehash: 258b8c3d03638b7fbe200e093223bcde615364c7
ms.sourcegitcommit: fb7d95c7a5d47860918cd3602efdd33b69dcf2da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2019
ms.locfileid: "67361584"
---
# <a name="bug-check-0xce-driverunloadedwithoutcancellingpendingoperations"></a>Bug 检查 0xCE：驱动程序\_UNLOADED\_WITHOUT\_正在取消\_PENDING\_操作


该驱动程序\_UNLOADED\_WITHOUT\_正在取消\_PENDING\_操作 bug 检查是否具有值为 0x000000CE。 这表示一个驱动程序无法在卸载之前取消挂起的操作。

> [!IMPORTANT]
> 本主题面向程序员。 如果你已使用计算机时收到一个蓝色的屏幕，错误代码的客户，请参阅[疑难解答蓝屏错误](https://support.microsoft.com/help/14238/windows-10-troubleshoot-blue-screen-errors)。


## <a name="driverunloadedwithoutcancellingpendingoperations-parameters"></a>驱动程序\_UNLOADED\_WITHOUT\_正在取消\_PENDING\_操作参数


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">参数</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>引用的内存地址</p></td>
</tr>
<tr class="even">
<td align="left"><p>2</p></td>
<td align="left"><p><strong>0:</strong>Read</p>
<p><strong>1:</strong>写入</p></td>
</tr>
<tr class="odd">
<td align="left"><p>3</p></td>
<td align="left"><p>（如果已知） 引用内存的地址</p></td>
</tr>
<tr class="even">
<td align="left"><p>4</p></td>
<td align="left"><p>保留</p></td>
</tr>
</tbody>
</table>

 

如果无法确定导致错误的驱动程序，其名称是蓝色屏幕上输出和位置在内存中存储 (PUNICODE\_字符串) **KiBugCheckDriver**。

<a name="cause"></a>原因
-----

未能取消后备链列表、 Dpc、 工作线程或其他此类项之前卸载此驱动程序。

 

 



