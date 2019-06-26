---
title: 创建压缩缓冲区和解码渲染器目标
description: 创建压缩缓冲区和解码渲染器目标
ms.assetid: 4d386b72-24d9-424b-8d48-7eaf75aab76c
keywords:
- 视频解码 WDK DirectX VA，呈现器目标
- 解码视频 WDK DirectX VA，呈现器目标
- 视频解码 WDK DirectX VA，压缩缓冲区
- 解码视频 WDK DirectX VA，压缩的缓冲区
- 压缩的缓冲区 WDK DirectX VA
- 缓冲 WDK DirectX VA
- 呈现器目标 WDK DirectX VA
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 19c3b2e71211f1c06865ac7ffe5a585c635a7081
ms.sourcegitcommit: 0cc5051945559a242d941a6f2799d161d8eba2a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "63346865"
---
# <a name="creating-compressed-buffers-and-decode-render-targets"></a>创建压缩缓冲区和解码渲染器目标


Microsoft Direct3D 运行时将调用用户模式显示驱动程序[ **CreateResource** ](https://msdn.microsoft.com/library/windows/hardware/ff540688)函数来创建压缩的缓冲区，并呈现器目标用于解码。

每个压缩的缓冲区类型都有其自己图面上的格式，以及指示运行时创建的图面包含压缩的缓冲区的信息，用于加速的视频解码的特殊标志。 用户模式显示驱动程序确定要创建压缩的缓冲区，如果**DecodeCompressedBuffer**中的位域标志**标志**的成员[ **D3DDDIARG\_CREATERESOURCE** ](https://msdn.microsoft.com/library/windows/hardware/ff542963)结构的*pResource*参数[ **CreateResource** ](https://msdn.microsoft.com/library/windows/hardware/ff540688)设置到的点。 用户模式显示驱动程序确定压缩缓冲区中的格式值创建的类型**格式**D3DDDIARG 成员\_CREATERESOURCE。 定义以下格式：

```cpp
D3DDDIFMT_PICTUREPARAMSDATA       = 150
D3DDDIFMT_MACROBLOCKDATA          = 151
D3DDDIFMT_RESIDUALDIFFERENCEDATA  = 152
D3DDDIFMT_DEBLOCKINGDATA          = 153
D3DDDIFMT_INVERSEQUANTIZATIONDATA = 154
D3DDDIFMT_SLICECONTROLDATA        = 155
D3DDDIFMT_BITSTREAMDATA           = 156
```

Direct3D 运行时创建每个解码呈现器目标独立用户模式显示驱动程序的调用中[ **CreateResource** ](https://msdn.microsoft.com/library/windows/hardware/ff540688)函数。 每个目标是作为单个资源的子资源索引引用。 用户模式显示驱动程序确定创建解码呈现器目标，如果**DecodeRenderTarget**中的位域标志**标志**D3DDDIARG 成员\_CREATERESOURCE 设置。

 

 




