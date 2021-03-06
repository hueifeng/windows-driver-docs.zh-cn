---
title: Bug 检查 0x11C ATTEMPTED_WRITE_TO_CM_PROTECTED_STORAGE
description: ATTEMPTED_WRITE_TO_CM_PROTECTED_STORAGE bug 检查具有 0x0000011C，该值指示写入尝试访问 configuration manager 的受保护的存储值。
ms.assetid: 5a322457-51d7-4832-8eeb-1fdc99f313e8
keywords:
- Bug 检查 0x11C ATTEMPTED_WRITE_TO_CM_PROTECTED_STORAGE
- ATTEMPTED_WRITE_TO_CM_PROTECTED_STORAGE
ms.date: 05/23/2017
topic_type:
- apiref
api_name:
- ATTEMPTED_WRITE_TO_CM_PROTECTED_STORAGE
api_type:
- NA
ms.localizationpriority: medium
ms.openlocfilehash: 412847e1ea8b56badf9c719616e7a2a08ac9e207
ms.sourcegitcommit: d03b44343cd32b3653d0471afcdd3d35cb800c0d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2019
ms.locfileid: "67521020"
---
# <a name="bug-check-0x11c-attemptedwritetocmprotectedstorage"></a>Bug 检查 0x11C：尝试\_编写\_TO\_CM\_受保护\_存储


已尝试\_编写\_TO\_CM\_受保护\_存储 bug 检查的值为 0x0000011C。 检查此错误表示尝试写入只读的受保护存储的配置管理器。

> [!IMPORTANT]
> 本主题面向程序员。 如果你已使用计算机时收到一个蓝色的屏幕，错误代码的客户，请参阅[疑难解答蓝屏错误](https://www.windows.com/stopcode)。


## <a name="attemptedwritetocmprotectedstorage-parameters"></a>尝试\_编写\_TO\_CM\_受保护\_存储参数


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
<td align="left"><p>尝试的写入的虚拟地址</p></td>
</tr>
<tr class="even">
<td align="left"><p>2</p></td>
<td align="left"><p>PTE 内容</p></td>
</tr>
<tr class="odd">
<td align="left"><p>3</p></td>
<td align="left"><p>保留</p></td>
</tr>
<tr class="even">
<td align="left"><p>4</p></td>
<td align="left"><p>保留</p></td>
</tr>
</tbody>
</table>

 

<a name="remarks"></a>备注
-------

如果可能，如 bug 上的 Unicode 字符串检查屏幕，然后保存在 KiBugCheckDriver 打印正尝试写入操作的驱动程序的名称。

 

 




