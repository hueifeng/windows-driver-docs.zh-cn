---
title: .allow_image_mapping（允许映像映射）
description: .Allow_image_mapping 命令控制是否将映射图像文件。
ms.assetid: 3d216d17-f2af-48f7-9d91-e12c3c305a67
keywords:
- .allow_image_mapping （允许图像映射） Windows 调试
ms.date: 09/17/2018
topic_type:
- apiref
api_name:
- .allow_image_mapping (Allow Image Mapping)
api_type:
- NA
ms.localizationpriority: medium
ms.openlocfilehash: 7c1f0fe1fc270df9ee1212ce26848ecf37d139c0
ms.sourcegitcommit: 0cc5051945559a242d941a6f2799d161d8eba2a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "63334776"
---
# <a name="allowimagemapping-allow-image-mapping"></a>.allow\_image\_mapping （允许图像映射）


**.Allow\_image\_mapping**将映射控件是否图像文件的命令。

```dbgcmd
    .allow_image_mapping [/r] 0 
    .allow_image_mapping [/r] 1 
    .allow_image_mapping 
```

## <a name="span-idparametersspanspan-idparametersspanspan-idparametersspanparameters"></a><span id="Parameters"></span><span id="parameters"></span><span id="PARAMETERS"></span>参数


<span id="________r______"></span><span id="________R______"></span> **/r**   
重新加载所有模块的调试程序的模块列表中。 这相当于[ **.reload /d**](-reload--reload-module-.md)。

<span id="_______0______"></span> **0**   
防止图像文件正被映射。

<span id="_______1______"></span> **1**   
允许图像文件映射。

### <a name="span-idenvironmentspanspan-idenvironmentspanspan-idenvironmentspanenvironment"></a><span id="Environment"></span><span id="environment"></span><span id="ENVIRONMENT"></span>环境

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><strong>模式</strong></p></td>
<td align="left"><p>用户模式和内核模式</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>目标</strong></p></td>
<td align="left"><p>实时、 崩溃转储</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>平台</strong></p></td>
<td align="left"><p>全部</p></td>
</tr>
</tbody>
</table>

 

<a name="remarks"></a>备注
-------

不带任何参数， **.allow\_image\_mapping**是否当前允许图像文件映射将显示。 默认情况下，允许此映射。

在调试小型转储时，图像映射是最常见。 如果 DbgHelp 无法调试记录 （例如，在期间访问内核调试时内存已调出），也可能会出现图像映射。

 

 





