---
title: 实施版权保护
description: 实施版权保护
ms.assetid: 42d91ad3-615a-461a-846b-4876ac8decea
keywords:
- DVD 解码器微型驱动程序 WDK，版权保护
- 解码器微型驱动程序 WDK DVD，版权保护
- 版权保护 WDK DVD 解码器
- 内容编码系统 WDK DVD 解码器
- CSS WDK DVD 解码器
- DVD decrypters WDK
- 密钥交换 WDK DVD 解码器
- 解密 WDK DVD 解码器
- 加密 WDK DVD 解码器
- 加密 WDK DVD 解码器
- 身份验证 WDK DVD 解码器
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 40301eaf946f6306bca2f0911b4204a8708f44b0
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72844704"
---
# <a name="copyright-protection-implementation"></a>实施版权保护





Microsoft 提供的软件有助于内容编码系统（CSS）方案所需的身份验证过程，从而允许 DVD-ROM 驱动器使用 DVD decrypter 进行身份验证和传输密钥。 Microsoft 不提供 DVD decrypter。 相反，Microsoft 提供将充当代理以允许对硬件或软件 decrypters 进行身份验证的操作系统代码。

密钥交换过程由 DVD 导航器/拆分器筛选器启动和控制。 DVD 解码器微型驱动程序只需实现下一节中列出的属性。 其余组件由其他组件处理。

每个 DVD 输入流接收版权保护属性。 即使所有 DVD 流都由同一硬件控制，也是如此。

视频端口属性集的 GUID 是[KSPROPSETID\_CopyProt](https://docs.microsoft.com/windows-hardware/drivers/stream/kspropsetid-copyprot)。 以下属性可用。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>属性</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-chlg-key" data-raw-source="[&lt;strong&gt;KSPROPERTY_DVDCOPY_CHLG_KEY&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-chlg-key)"><strong>KSPROPERTY_DVDCOPY_CHLG_KEY</strong></a></p></td>
<td><p>此属性支持 get 和 set。 Get 属性请求解码器提供其总线质询密钥。 Set 属性使用 DVD 驱动器中的总线质询密钥提供解码器。 此属性中传递的数据是<a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_chlgkey" data-raw-source="[&lt;strong&gt;KS_DVDCOPY_CHLGKEY&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_chlgkey)"><strong>KS_DVDCOPY_CHLGKEY</strong></a>类型的结构。</p></td>
</tr>
<tr class="even">
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-dvd-key1" data-raw-source="[&lt;strong&gt;KSPROPERTY_DVDCOPY_DVD_KEY1&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-dvd-key1)"><strong>KSPROPERTY_DVDCOPY_DVD_KEY1</strong></a></p></td>
<td><p>仅设置属性。 此属性为解码器提供 DVD 驱动器总线密钥1。 传递的数据是<a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_buskey" data-raw-source="[&lt;strong&gt;KS_DVDCOPY_BUSKEY&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_buskey)"><strong>KS_DVDCOPY_BUSKEY</strong></a>类型的结构。</p></td>
</tr>
<tr class="odd">
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-dec-key2" data-raw-source="[&lt;strong&gt;KSPROPERTY_DVDCOPY_DEC_KEY2&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-dec-key2)"><strong>KSPROPERTY_DVDCOPY_DEC_KEY2</strong></a></p></td>
<td><p>获取属性。 此属性请求解码器的总线键2被传输到 DVD 驱动器。 传递的数据是<a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_buskey" data-raw-source="[&lt;strong&gt;KS_DVDCOPY_BUSKEY&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_buskey)"><strong>KS_DVDCOPY_BUSKEY</strong></a>类型的结构。</p></td>
</tr>
<tr class="even">
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-title-key" data-raw-source="[&lt;strong&gt;KSPROPERTY_DVDCOPY_TITLE_KEY&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-title-key)"><strong>KSPROPERTY_DVDCOPY_TITLE_KEY</strong></a></p></td>
<td><p>仅设置属性。 这会提供当前内容的标题键。 该键是<a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_titlekey" data-raw-source="[&lt;strong&gt;KS_DVDCOPY_TITLEKEY&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_titlekey)"><strong>KS_DVDCOPY_TITLEKEY</strong></a>类型的结构。</p></td>
</tr>
<tr class="odd">
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-disc-key" data-raw-source="[&lt;strong&gt;KSPROPERTY_DVDCOPY_DISC_KEY&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-disc-key)"><strong>KSPROPERTY_DVDCOPY_DISC_KEY</strong></a></p></td>
<td><p>仅设置属性。 这会提供光盘密钥。</p>
<div>
 
</div>
该键是<a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_disckey" data-raw-source="[&lt;strong&gt;KS_DVDCOPY_DISCKEY&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_disckey)"><strong>KS_DVDCOPY_DISCKEY</strong></a>类型的结构。</td>
</tr>
<tr class="even">
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-copy-macrovision" data-raw-source="[&lt;strong&gt;KSPROPERTY_COPY_MACROVISION&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-copy-macrovision)"><strong>KSPROPERTY_COPY_MACROVISION</strong></a></p></td>
<td><p>仅设置属性。 该键是<a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_copy_macrovision" data-raw-source="[&lt;strong&gt;KS_COPY_MACROVISION&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_copy_macrovision)"><strong>KS_COPY_MACROVISION</strong></a>类型的结构。 这是模拟 NTSC 视频流，不久将处理 NTSC macrovision 属性。</p></td>
</tr>
<tr class="odd">
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-region" data-raw-source="[&lt;strong&gt;KSPROPERTY_DVDCOPY_REGION&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-region)"><strong>KSPROPERTY_DVDCOPY_REGION</strong></a></p></td>
<td><p>获取属性。 DVD 微型驱动程序仅适用于一个区域位。 该键是<a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_region" data-raw-source="[&lt;strong&gt;KS_DVDCOPY_REGION&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_region)"><strong>KS_DVDCOPY_REGION</strong></a>类型的结构。</p></td>
</tr>
<tr class="even">
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-set-copy-state" data-raw-source="[&lt;strong&gt;KSPROPERTY_DVDCOPY_SET_COPY_STATE&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/stream/ksproperty-dvdcopy-set-copy-state)"><strong>KSPROPERTY_DVDCOPY_SET_COPY_STATE</strong></a></p></td>
<td><p>仅获取和设置属性。 该键是<a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_set_copy_state" data-raw-source="[&lt;strong&gt;KS_DVDCOPY_SET_COPY_STATE&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_set_copy_state)"><strong>KS_DVDCOPY_SET_COPY_STATE</strong></a>类型的结构。 此属性使用</p>
<p>KS_DVDCOPYSTATE_AUTHENTICATION_NOT_REQUIRED,</p>
<p>KS_DVDCOPYSTATE_AUTHENTICATION_REQUIRED,</p>
<p>KS_DVDCOPYSTATE_INITIALIZE 和</p>
<p>KS_DVDCOPYSTATE_INITIALIZE_TITLE.</p></td>
</tr>
</tbody>
</table>

 

以下顺序在解码器上每个打开的 DVD 输入插针上重复出现。 解码器会按以下顺序接收密钥：

获取 KSPROPERTY\_DVDCOPY\_CHLG\_键

将 KSPROPERTY\_DVDCOPY\_DVD\_KEY1

设置 KSPROPERTY\_DVDCOPY\_CHLG\_键

\_DEC\_KEY2 获取 KSPROPERTY\_DVDCOPY

将 KSPROPERTY\_DVDCOPY\_光盘\_密钥

然后，会收到以下密钥：

获取 KSPROPERTY\_DVDCOPY\_CHLG\_键

将 KSPROPERTY\_DVDCOPY\_DVD\_KEY1

设置 KSPROPERTY\_DVDCOPY\_CHLG\_键

\_DEC\_KEY2 获取 KSPROPERTY\_DVDCOPY

设置 KSPROPERTY\_DVDCOPY\_标题\_键

对于解码器上的每个打开的 DVD 输入插针，也会重复此序列。 DVD 光盘密钥成功建立后，可能会在任何时间发生，并且每个光盘密钥可能出现一次以上。 只要读取包含标题键的扇区，身份验证过程就必须成功完成。 如果身份验证失败，则会阻止读取并返回相应的错误消息。

 

 




