---
title: 文件排队函数
description: 文件排队函数
ms.assetid: ad777cd9-99ca-4468-b1fa-608503f96cd4
keywords:
- 安装程序 Api 函数 WDK，文件队列
- 文件队列 WDK SetupAPI
- 队列文件 WDK SetupAPI
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 49c45a5f8e5dd104583926e4fb386c31690fddd4
ms.sourcegitcommit: fb7d95c7a5d47860918cd3602efdd33b69dcf2da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2019
ms.locfileid: "67360305"
---
# <a name="file-queuing-functions"></a>文件排队函数





使用常规的安装程序函数，可以为各种操作的文件进行排队。 可以复制、 重命名和删除文件建立文件队列。 通常情况下，应用程序队列整个安装所需的所有文件操作，然后"提交"队列以便在单个批处理中执行的操作。

下表提供了队列函数的文件的摘要。 有关详细的功能描述，请参阅 Microsoft Windows SDK 文档。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">函数</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupclosefilequeue" data-raw-source="[&lt;strong&gt;SetupCloseFileQueue&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupclosefilequeue)"><strong>SetupCloseFileQueue</strong></a></p></td>
<td align="left"><p>销毁文件队列以及任何未提交的文件操作。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupcommitfilequeuea" data-raw-source="[&lt;strong&gt;SetupCommitFileQueue&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupcommitfilequeuea)"><strong>SetupCommitFileQueue</strong></a></p></td>
<td align="left"><p>提交 （执行） 所有排队操作。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupopenfilequeue" data-raw-source="[&lt;strong&gt;SetupOpenFileQueue&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupopenfilequeue)"><strong>SetupOpenFileQueue</strong></a></p></td>
<td align="left"><p>创建并返回的句柄文件队列。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setuppromptreboot" data-raw-source="[&lt;strong&gt;SetupPromptReboot&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setuppromptreboot)"><strong>SetupPromptReboot</strong></a></p></td>
<td align="left"><p>如有必要，会提示用户重新启动其计算机。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuecopya" data-raw-source="[&lt;strong&gt;SetupQueueCopy&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuecopya)"><strong>SetupQueueCopy</strong></a></p></td>
<td align="left"><p>排队复制指定的文件。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuecopyindirecta" data-raw-source="[&lt;strong&gt;SetupQueueCopyIndirect&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuecopyindirecta)"><strong>SetupQueueCopyIndirect</strong></a></p></td>
<td align="left"><p>队列指定的文件进行复制，并提供文件的位置和安全信息。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuecopysectiona" data-raw-source="[&lt;strong&gt;SetupQueueCopySection&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuecopysectiona)"><strong>SetupQueueCopySection</strong></a></p></td>
<td align="left"><p>队列中指定的 INF 文件的文件将复制的部分。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuedefaultcopya" data-raw-source="[&lt;strong&gt;SetupQueueDefaultCopy&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuedefaultcopya)"><strong>SetupQueueDefaultCopy</strong></a></p></td>
<td align="left"><p>队列指定的文件进行复制，请使用 INF 文件中包含的默认源和目标设置。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuedeletea" data-raw-source="[&lt;strong&gt;SetupQueueDelete&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuedeletea)"><strong>SetupQueueDelete</strong></a></p></td>
<td align="left"><p>队列为待删除指定的文件。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuedeletesectiona" data-raw-source="[&lt;strong&gt;SetupQueueDeleteSection&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuedeletesectiona)"><strong>SetupQueueDeleteSection</strong></a></p></td>
<td align="left"><p>队列中的 INF 文件的文件为删除的部分。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuerenamea" data-raw-source="[&lt;strong&gt;SetupQueueRename&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuerenamea)"><strong>SetupQueueRename</strong></a></p></td>
<td align="left"><p>队列指定的文件进行重命名。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuerenamesectiona" data-raw-source="[&lt;strong&gt;SetupQueueRenameSection&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupqueuerenamesectiona)"><strong>SetupQueueRenameSection</strong></a></p></td>
<td align="left"><p>队列中 INF 部分重命名的文件。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupscanfilequeuea" data-raw-source="[&lt;strong&gt;SetupScanFileQueue&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupscanfilequeuea)"><strong>SetupScanFileQueue</strong></a></p></td>
<td align="left"><p>扫描文件队列，并执行每个队列项上指定的操作。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupsetplatformpathoverridea" data-raw-source="[&lt;strong&gt;SetupSetPlatformPathOverride&lt;/strong&gt;](https://docs.microsoft.com/windows/desktop/api/setupapi/nf-setupapi-setupsetplatformpathoverridea)"><strong>SetupSetPlatformPathOverride</strong></a></p></td>
<td align="left"><p>设置用于重写默认特定于平台的源路径的值。</p></td>
</tr>
</tbody>
</table>

 

 

 





