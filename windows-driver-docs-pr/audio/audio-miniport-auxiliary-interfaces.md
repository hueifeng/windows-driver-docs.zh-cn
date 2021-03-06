---
title: 音频微型端口辅助接口
description: 音频微型端口辅助接口
ms.assetid: cda22e86-f3f7-430c-856d-a2c868caa975
ms.date: 11/28/2017
ms.localizationpriority: medium
ms.openlocfilehash: f3a0b3926661436a283853361fcb97a529e7f745
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72831355"
---
# <a name="audio-miniport-auxiliary-interfaces"></a>音频微型端口辅助接口


## <span id="ddk_audio_miniport_auxiliary_interfaces_ks"></span><span id="DDK_AUDIO_MINIPORT_AUXILIARY_INTERFACES_KS"></span>


某些微型端口驱动程序支持可选的辅助接口，并提供对专用微型端口驱动程序功能的访问权限。 本部分介绍由微型端口驱动程序实现并向端口驱动程序公开的辅助接口。

本节将讨论以下接口：

[IMusicTechnology](https://docs.microsoft.com/windows-hardware/drivers/ddi/portcls/nn-portcls-imusictechnology)-用于更改 dmu 微型端口驱动程序的 pin 的数据范围中指定的 DirectMusic 合成器技术。

[IPinCount](https://docs.microsoft.com/windows-hardware/drivers/ddi/portcls/nn-portcls-ipincount) -为微型端口驱动程序提供了一种动态监视和操作其 pin 计数的方法。

[IPinName](https://docs.microsoft.com/windows-hardware/drivers/ddi/portcls/nf-portcls-ipinname-getpinname) -允许端口驱动程序动态更新终结点的名称。

[IAdapterPnpManagement](https://docs.microsoft.com/windows-hardware/drivers/ddi/portcls/nn-portcls-iadapterpnpmanagement) -允许适配器注册以接收 PnP 管理消息。

 

 





