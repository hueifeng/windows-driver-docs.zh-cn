---
title: 通信类接口的接口描述符
description: 通信类接口的接口描述符
ms.assetid: e3873a58-34fc-4ca0-8c45-197401cbf08b
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: e87c6169d447e5d49e316d589045d5d96ad7e02d
ms.sourcegitcommit: 0cc5051945559a242d941a6f2799d161d8eba2a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "63380730"
---
# <a name="interface-descriptor-for-communication-class-interface"></a>通信类接口的接口描述符





<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">偏移量</th>
<th align="left">字段</th>
<th align="left">大小</th>
<th align="left">ReplTest1</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>bDescriptorType</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0x04</p></td>
<td align="left"><p>接口描述符</p></td>
</tr>
<tr class="even">
<td align="left"><p>2</p></td>
<td align="left"><p>bInterfaceNumber</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0x00</p></td>
<td align="left"><p>此接口的索引</p></td>
</tr>
<tr class="odd">
<td align="left"><p>3</p></td>
<td align="left"><p>bAlternateSetting</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0x00</p></td>
<td align="left"><p>此设置的索引</p></td>
</tr>
<tr class="even">
<td align="left"><p>4</p></td>
<td align="left"><p>bNumEndpoints</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0x01</p></td>
<td align="left"><p>1 个终结点</p></td>
</tr>
<tr class="odd">
<td align="left"><p>5</p></td>
<td align="left"><p>bInterfaceClass</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0x02</p></td>
<td align="left"><p>通信类</p></td>
</tr>
<tr class="even">
<td align="left"><p>6</p></td>
<td align="left"><p>bInterfaceSubclass</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0x02</p></td>
<td align="left"><p>抽象控制模型</p></td>
</tr>
<tr class="odd">
<td align="left"><p>7</p></td>
<td align="left"><p>bInterfaceProtocol</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0xFF</p></td>
<td align="left"><p>特定于供应商的协议</p></td>
</tr>
<tr class="even">
<td align="left"><p>8</p></td>
<td align="left"><p>iInterface</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0x00</p></td>
<td align="left"><p>未使用</p></td>
</tr>
</tbody>
</table>

 

 

 





