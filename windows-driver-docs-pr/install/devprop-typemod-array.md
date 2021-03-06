---
title: DEVPROP_TYPEMOD_ARRAY
description: 在 Windows Vista 和更高版本的 Windows，DEVPROP_TYPEMOD_ARRAY 标识符表示可与要创建一个表示数组的属性数据类型标识符的基本数据类型标识符结合使用的属性数据类型修饰符基本数据类型值。
ms.assetid: 33f12b66-c81a-451b-851a-b58a34a8fe9e
keywords:
- DEVPROP_TYPEMOD_ARRAY 设备和驱动程序安装
topic_type:
- apiref
api_name:
- DEVPROP_TYPEMOD_ARRAY
api_location:
- Devpropdef.h
api_type:
- HeaderDef
ms.localizationpriority: medium
ms.date: 10/17/2018
ms.openlocfilehash: 0ed8ef3a03c1d30a43543183b8cd65647ceff2c8
ms.sourcegitcommit: fb7d95c7a5d47860918cd3602efdd33b69dcf2da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2019
ms.locfileid: "67372741"
---
# <a name="devproptypemodarray"></a>DEVPROP_TYPEMOD_ARRAY


在 Windows Vista 和更高版本的 Windows，DEVPROP_TYPEMOD_ARRAY 标识符表示可与结合使用的属性数据类型修饰符[**基本数据类型标识符**](https://docs.microsoft.com/previous-versions/ff537793(v=vs.85))创建表示基本数据类型值的数组的属性数据类型标识符。

<a name="remarks"></a>备注
-------

可以仅与固定长度数据的基本类型标识符结合使用 DEVPROP_TYPEMOD_ARRAY 标识符 ([**DEVPROPTYPE** ](https://docs.microsoft.com/previous-versions/ff543546(v=vs.85))值) 与数据相关联的。 不能与组合 DEVPROP_TYPEMOD_ARRAY 标识符[ **DEVPROP_TYPE_EMPTY**](devprop-type-empty.md)， [ **DEVPROP_TYPE_NULL**](devprop-type-null.md)，或任何长度可变的基本数据类型标识符。

若要创建表示基本数据类型值的数组的属性数据类型标识符，请执行位或运算 DEVPROP_TYPEMOD_ARRAY 和相应 DEVPROP_TYPE_ 之间*Xxx*标识符。 例如，若要指定无符号字节数组，请执行位或以下：(DEVPROP_TYPEMOD_ARRAY |[ **DEVPROP_TYPE_BYTE**](devprop-type-byte.md))。

大小，单位为字节数组的大小 （字节） 的基本数据类型值数组。

有关如何创建一个表示属性数据类型标识符[REG_MULTI_SZ](https://docs.microsoft.com/windows/desktop/SysInfo/registry-value-types)列表的以 NULL 结尾的 Unicode 字符串，请参阅[ **DEVPROP_TYPEMOD_LIST** ](devprop-typemod-list.md).

<a name="requirements"></a>要求
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>Header</p></td>
<td align="left">Devpropdef.h （包括 Devpropdef.h）</td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>请参阅


[**DEVPROPTYPE**](https://docs.microsoft.com/previous-versions/ff543546(v=vs.85))

[**DEVPROP_TYPEMOD_LIST**](devprop-typemod-list.md)

 

 






