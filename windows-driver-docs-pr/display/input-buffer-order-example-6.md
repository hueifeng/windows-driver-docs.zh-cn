---
title: 输入缓冲区顺序示例 6
description: 输入缓冲区顺序示例 6
ms.assetid: e94ca05a-5089-460a-bf71-ee6d8cf52f17
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: c60fba42711eab5e330641883c50582846aa9033
ms.sourcegitcommit: 0cc5051945559a242d941a6f2799d161d8eba2a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "63350240"
---
# <a name="input-buffer-order-example-6"></a>输入缓冲区顺序示例 6


## <span id="ddk_input_buffer_order_example_6_gg"></span><span id="DDK_INPUT_BUFFER_ORDER_EXAMPLE_6_GG"></span>


**本部分仅适用于 Windows Server 2003 SP1 和更高版本和 Windows XP SP2 和更高版本。**

请考虑需要两个以前的输出帧、 单个向后引用示例，一个供将来参考示例中和当前的示例执行取消隔行扫描操作的更多复杂取消隔行扫描设备。 两个视频的子流也是与取消隔行扫描操作结合使用。 序列中的图面**lpBufferInfo**数组：

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">索引位置</th>
<th align="left">图面上的类型</th>
<th align="left">临时位置</th>
<th align="left">层位置</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>lpBufferInfo[0]</p></td>
<td align="left"><p>目标</p></td>
<td align="left"><p>T</p></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>lpBufferInfo[1]</p></td>
<td align="left"><p>以前的目标</p></td>
<td align="left"><p>T - 1</p></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>lpBufferInfo[2]</p></td>
<td align="left"><p>以前的目标</p></td>
<td align="left"><p>T - 2</p></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>lpBufferInfo[3]</p></td>
<td align="left"><p>交错的输入</p></td>
<td align="left"><p>T - 1</p></td>
<td align="left"><p>Z</p></td>
</tr>
<tr class="odd">
<td align="left"><p>lpBufferInfo[4]</p></td>
<td align="left"><p>交错的输入</p></td>
<td align="left"><p>T</p></td>
<td align="left"><p>Z</p></td>
</tr>
<tr class="even">
<td align="left"><p>lpBufferInfo[5]</p></td>
<td align="left"><p>交错的输入</p></td>
<td align="left"><p>T + 1</p></td>
<td align="left"><p>Z</p></td>
</tr>
<tr class="odd">
<td align="left"><p>lpBufferInfo[6]</p></td>
<td align="left"><p>子流</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>Z + 1</p></td>
</tr>
<tr class="even">
<td align="left"><p>lpBufferInfo[7]</p></td>
<td align="left"><p>子流</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>Z + 2</p></td>
</tr>
</tbody>
</table>

 

 

 





